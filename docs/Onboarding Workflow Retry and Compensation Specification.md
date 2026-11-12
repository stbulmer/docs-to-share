# Onboarding Workflow: Retry and Compensation Specification

If a failure might be fixed by retrying, keep what was created and fix forward. Only compensate once a request is finally abandoned (cancelled, rejected, or past the agreed retry window). Undo runs in reverse order of creation.

This specification defines how the system applies the Temporal Workflow Design Principles to the onboarding workflow.

## Creation order

Security ID → PRC group → Terraform team → Terraform project → cloud branch (AWS or GCP) → Terraform workspace with variable sets.

## Shared and AWS resources

| # | Resource | Idempotent creation | On a failure retries might fix | On final abandonment |
| --- | --- | --- | --- | --- |
| 1 | Security ID | Tie it to the onboarding request ID, so a retry returns the same ID | Keep it and retry against the same ID | Soft delete, which is permanent: last step only, ideally with human approval or delayed cleanup. Track status in the workflow, since the registry has no inactive state |
| 2 | PRC group (entitlements) | Deterministic name from the security ID; adding members (owner or director, plus requester) is idempotent | Keep the group and its members; it grants nothing until roles are linked | Remove members straight away, then flag the group for the daily batch deletion |
| 3 | Terraform team (API) | Find or create by name; "name taken" counts as success; save the team ID | Keep it; an empty team can do nothing | Remove project and workspace access, then delete it |
| 4 | Terraform project (API, team access in the same call) | Check that the project exists *and* the team has access; fix whichever is missing | Keep it; an empty project is harmless | Delete or move workspaces, remove team access, then delete the project |
| 5 | AWS policy collections (IaC and A to A) | Deterministic names; compare the document and create a new version only if it changed; prune versions (max 5 per policy) | Keep them; a policy that isn't attached grants nothing | Detach from roles, delete non-default versions, then delete. Delete after the roles |
| 6 | SSO / interactive role (Identity Center) | Split into activities: permission set (find by name) → provision to the account (poll) → assign the PRC group (poll; "already exists" counts as success) | Keep and fix forward | Remove the account assignment **first** (it gives humans access), then deprovision and delete the permission set |
| 7 | A to A and IaC roles | Find or create, then reconcile the trust policy and attached policies on every retry. Reserve the workspace name the IaC trust policy depends on | Keep and fix forward | Detach policies, remove inline policies, delete the role. Do this before the policy collections, after the workspace |
| 8 | Terraform workspace + variable sets | Find or create by its reserved name in the project; reconcile the variable set attachments; team access comes from the project | Keep and fix forward | If it has never applied: detach variable sets, then delete. If state has resources: safe delete only, which means a destroy run first or escalation to a human |

**AWS undo order on abandonment:** workspace → SSO assignment and permission set → roles → policy collections → project → team → PRC group (members now, deletion in the batch) → security ID soft delete.

## GCP branch

Each application team gets its own GCP project, which the platform manages; application teams don't get direct access to it. The project is created through an API call, then IAM inside it is applied through Terraform. All names are derived from the security ID, so a retry during onboarding lands on the same project, service accounts and roles.

| Step | Idempotent creation | On a failure retries might fix | On final abandonment |
| --- | --- | --- | --- |
| GCP project (API) | Deterministic, globally unique project ID from the security ID; look it up before creating, and treat "exists and ours" as success; check the billing link on every retry | Keep and fix forward | Last step only, ideally with approval. Deletion is pending for 30 days and the ID can never be used again. Remove IAM first |
| IAM: roles, service accounts, bindings, enabled APIs (Terraform) | A re-apply reconciles. Use additive member-style IAM resources, so platform bindings in the project can't be wiped | Re-apply. Treat IAM conflict errors and "not found" / "permission denied" just after creation as retryable with backoff; verify before dependent steps | Remove config and apply, or run a destroy. Leave APIs enabled |

**GCP undo order:** workspace → Terraform IAM → project deletion → Terraform project → team → PRC group → security ID soft delete.
