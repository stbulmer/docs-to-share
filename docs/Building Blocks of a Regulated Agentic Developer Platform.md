# Building Blocks of a Regulated Agentic Developer Platform

Oct 28, 2026 · @Stephen

## Overview

The agent proposes, the platform proves, and an accountable human attests. The agent never generates its own evidence of compliance: the platform produces that evidence independently, and a named, qualified person signs off before anything reaches production.

Every change moves through five stages, and each stage writes signed records to an immutable evidence ledger:

&#91;embedded content: release flow · 5 stages, 1 evidence ledger\]

Humans own the start and the sign-off; the agent and platform handle everything between, and nothing ships without the ledger holding every required attestation.

The ten building blocks below supply the controls behind these stages.

## 1. Requirements and traceability

- **Structured acceptance criteria.** Each criterion gets a unique ID and is written so it can be tested, for example Given/When/Then. Vague criteria cannot produce clean evidence.
- **Risk classification per change.** Tier changes (low, medium, high or regulated) to decide how much review, testing, and approval each one needs.
- **End-to-end traceability.** Link requirement → criteria → commits → tests → build → deployment, so an auditor can follow any production change back to its intent.

## 2. Identity, delegation, and separation of duties

- **Agent identities.** Agents authenticate as themselves, acting on behalf of a named sponsor, using workload identity and short-lived, scoped credentials.
- **Separation of duties enforced by the platform.** The person who sponsored the agent's work cannot be the sole approver of it. Author, reviewer, and releaser roles stay distinct, enforced by policy rather than convention.
- **Privileged actions require humans.** Production access, secrets, and configuration changes go through just-in-time, approved elevation.

## 3. Controlled execution environment

- Isolated sandboxes with restricted network egress and no production or unmasked regulated data.
- Data classification and redaction before anything enters an agent's context, respecting data residency and model-provider terms.
- Reproducible, pinned build environments so results can be recreated for an audit.

## 4. Agent governance

- **Approved model and tool registry.** Record which models, versions, MCP servers, and tools are allowed for each risk tier.
- **Versioned configuration.** Track prompts, system instructions, and agent configuration like code, so you can show exactly what the agent was running at any point.
- **Session logging.** Capture prompts, tool calls, file changes, and outputs, linked to the change record.
- **Model risk management.** Validate agent performance periodically, monitor drift, and document limitations, in line with your organization's model risk framework.

## 5. Independent verification and evidence generation

- **Tests mapped to acceptance criteria.** Every criterion ID has at least one test, and the platform reports coverage per criterion, not just per line.
- **Independence of tests.** An agent writing both the code and its tests can grade its own homework. Mitigate with human-reviewed or human-authored tests for high-risk criteria, tests derived directly from criteria, mutation testing, and protected test suites the agent cannot edit.
- **Full scanning suite.** SAST, dependency/SCA, secrets, IaC, and license scans, plus DAST and performance tests where relevant.
- **Platform-generated, signed evidence.** CI, not the agent, produces test reports, SBOMs, and scan results as signed attestations (for example in-toto or SLSA provenance with Sigstore). An agent's self-reported "tests passed" never counts as evidence.

## 6. Human attestation

- **An attestation interface built for real review.** Show the reviewer each criterion next to its linked evidence, the diff, the risk tier, scan results, and a summary of the agent session.
- **Explicit, criterion-level sign-off.** The reviewer confirms each criterion, or the set, with a statement of what they are attesting to.
- **Electronic signatures that hold up.** Tie them to a strong identity (re-authentication at signing), and record the signer's name, date and time, and the meaning of the signature, the model used by regimes like FDA 21 CFR Part 11.
- **Guard against rubber-stamping.** Track review time and depth, require reviewer qualification per domain, and sample attestations for quality.

## 7. Policy engine and release gates

- **Policy as code** (OPA, Kyverno, or similar) defines what a releasable artifact must have: required attestations, signatures from approved roles, passing scans, and complete criteria coverage.
- **Enforcement at deploy time.** Admission control verifies signatures and attestations on the actual artifact, so nothing unsigned or unattested can run, however it was triggered.
- **Change management integration.** Generate change records in your ITSM tool automatically with the evidence attached, and keep CAB approval only for the tiers that need it.
- **Controlled rollout.** Progressive delivery, feature flags, and automated rollback, with rollback decisions logged too.

## 8. Immutable evidence ledger

- WORM or append-only storage, with hash chaining or a transparency log so tampering is detectable.
- Retention periods matched to each applicable regulation.
- Queryable by change, requirement, system, or time period, so "show me everything about this release" takes minutes.

## 9. Runtime assurance and feedback

- Observability linked to change IDs, so incidents trace back to the change, its evidence, and its attesters.
- Drift detection between attested configuration and what is actually running.
- Post-incident reviews feed back into policy, tests, and agent configuration.

## 10. Audit and compliance reporting

- **Control mapping.** Map platform controls to your frameworks, for example SOX, SOC 2, ISO 27001, PCI DSS, HIPAA, GxP, or EU DORA for financial entities.
- **Self-service auditor portal.** Give auditors read-only access to evidence packages instead of screenshot hunts.
- **Continuous compliance dashboards.** Show criteria coverage, attestation completion, policy violations, and exceptions.

## Design principles

- **Evidence by construction.** Compliance artifacts are byproducts of the pipeline, not documents assembled afterward.
- **Trust the pipeline, not the actor.** The same gates apply whether a human or an agent wrote the change.
- **Accountability stays human.** Agents can do the work, but a named person owns the decision to release.
- **Proportionality.** Scale controls to risk tier so low-risk changes stay fast and high-risk changes get scrutiny.

Map these building blocks to your specific regulatory obligations with your compliance and legal teams, since requirements vary by industry and jurisdiction.
