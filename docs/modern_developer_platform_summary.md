# Building a Modern Developer Platform for Infrastructure Management

## Executive framing

Build an **internal developer platform (IDP) that is agent-ready by
design, infrastructure-aware by default, and governed by evidence rather
than trust alone**.

The platform should help engineers and AI agents discover capabilities,
understand constraints, execute infrastructure changes, and demonstrate
that the resulting work meets required standards.

Agentic capabilities should not crowd out the fundamentals: excellent
documentation, discoverability, clear release communications, consistent
experiences across platform teams, and dependable support.

## 1. Six pillars of the platform

### 1. Developer knowledge and discoverability

*The interface for understanding the platform.*

-   Documentation written for humans and structured for agents.
-   Searchable service catalogs, examples, templates, troubleshooting,
    and decision guides.
-   Clear ownership, support paths, prerequisites, and limitations.
-   A single source of truth rather than duplicated instructions across
    wikis and repositories.

### 2. Infrastructure capabilities and paved roads

*The things developers can actually do.*

-   Standardized provisioning, access management, connectivity,
    deployment, and teardown.
-   Reusable modules, CLI commands, APIs, templates, and workflows.
-   Opinionated defaults with documented extension and exception paths.
-   Clear contracts between platform teams and cloud, network, security,
    and database owners.

### 3. Agent interoperability

*The interface for coding agents and future autonomous workflows.*

-   Machine-readable capability definitions, schemas, examples, and
    errors.
-   Discoverable APIs and tool contracts that do not require an agent to
    reverse-engineer a CLI.
-   Explicit permissions, execution boundaries, and context-efficient
    documentation.
-   Safe handling of retries, long-running jobs, partial failures, and
    resumable workflows.

### 4. Governance, evidence, and trust

*The controls that make infrastructure automation safe.*

-   Policy evaluation, identity, least privilege, and approval gates.
-   Evidence of tests, policy checks, and deployment outcomes.
-   Human attestation when required, with traceable identity and
    timestamp.
-   Immutable or tamper-evident audit records linking intent, changes,
    approvals, and outcomes.

### 5. Platform product operations

*How developers learn what is available and what has changed.*

-   Consistent release notes and announcements across platform teams.
-   Deprecation notices, migration guides, and breaking-change warnings.
-   Feature discovery, adoption tracking, and feedback loops.
-   A common release calendar and standard for communicating changes.

### 6. Reliability and continuous improvement

*How the platform proves its value.*

-   Provisioning time, workflow success rates, failure recovery, and
    support burden.
-   Agent task success, unsafe-action prevention, and evidence
    completeness.
-   Developer satisfaction, adoption, and time to first successful
    deployment.
-   Service ownership, incident communications, and operational
    readiness.

These are product capabilities, not necessarily six teams. Distributed
teams can own individual capabilities while sharing common standards and
a consistent developer experience.

## 2. Treat the platform as a product ecosystem

Use the [CNCF Platform Engineering Maturity
Model](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/)
as a starting point for considering investment, adoption, interfaces,
operations, and measurement.

Organize the platform into three layers:

### Layer 1 --- The experience

How a person or agent discovers and understands the platform.

-   Documentation and search
-   Service catalog
-   CLI help and examples
-   Announcements and release notes
-   Troubleshooting and support

### Layer 2 --- The capability interface

How a person or agent requests and manages infrastructure.

-   APIs and CLI
-   Infrastructure modules and templates
-   Schemas and workflow engines
-   Status and events

### Layer 3 --- The control plane

How the organization enforces policy and operates the platform.

-   Identity and authorization
-   Policy evaluation and approvals
-   Orchestration and lifecycle management
-   Audit evidence and observability

**Architectural principle: multiple experiences, common capability
contracts, centralized enforcement, and distributed ownership.**

For example, an engineer or agent requests a temporary test environment
through the supported interface. Both use the same provisioning
workflow. The control plane evaluates permissions and policies, the
workflow reports status, and the platform retains the evidence needed
for audit and release decisions.

The portal, AI assistant, and CLI are interfaces to the platform; none
of them is the platform itself.

## 3. Make documentation, releases, and communication first-class capabilities

### A. Human-readable and agent-readable documentation

Treat documentation as a product contract, not merely a writing
exercise. Every supported capability should use a consistent template.

#### Suggested capability documentation template

1.  **Purpose and fit:** Problem solved, intended users, supported use
    cases, and non-goals.
2.  **Getting started:** Prerequisites, permissions, first successful
    example, and expected result.
3.  **Inputs and outputs:** Parameters, types, defaults, constraints,
    outputs, and side effects.
4.  **Examples:** Copyable CLI, API, and infrastructure-as-code examples
    for common and advanced use cases.
5.  **Security and compliance:** Required roles, data boundaries, policy
    constraints, approvals, and evidence.
6.  **Lifecycle:** Create, inspect, update, recover, rotate,
    decommission, and destroy.
7.  **Failure handling:** Error catalog, retryability, recovery
    instructions, escalation, and correlation IDs.
8.  **Ownership and support:** Owning team, supported versions, service
    status, contact path, and escalation expectations.
9.  **Change history:** Release notes, breaking changes, deprecation
    dates, and migration instructions.

#### Agent-readiness requirements

-   **Structured capability metadata:** Stable identifier, owner,
    version, supported operations, input/output schemas, and required
    permissions.
-   **Executable examples:** Examples validated against the current
    implementation.
-   **Structured errors:** Stable error codes, clear causes, retry
    safety, and the next permitted action.
-   **Discoverability:** Stable URLs, API specifications, and
    machine-readable references from catalog entries.
-   **Freshness controls:** Version alignment, documentation checks in
    CI, and explicit ownership for stale content.

Markdown is a useful common authoring format.
[OpenAPI](https://www.openapis.org/) can describe HTTP interfaces, and
[JSON Schema](https://json-schema.org/) can describe structured inputs
and outputs. These formats complement each other; none replaces all
documentation.

**Rule of thumb:** An agent should not need to infer a security
boundary, invent a parameter, or guess whether a failed operation can
safely be retried.

Explore [Backstage
TechDocs](https://backstage.io/docs/features/techdocs/) and its
documentation-as-code model. The patterns can be adopted without
adopting Backstage itself.

### B. A unified release and announcement model

Distributed platform teams can release independently, while developers
experience the platform as one product. Establish a common communication
model without forcing all teams onto one release train.

  -----------------------------------------------------------------------
  Communication type      Purpose                 Example
  ----------------------- ----------------------- -----------------------
  Feature announcement    Help users discover     Temporary environments
                          something new           are now available
                                                  through the CLI

  Release notes           Explain what changed    New flags, module
                                                  versions, fixes, and
                                                  limitations

  Migration/deprecation   Prevent disruption      An old IAM role
  notice                                          configuration retires
                                                  on a stated date

  Operational notice      Explain current impact  Provisioning delays in
                                                  a particular cloud
                                                  region
  -----------------------------------------------------------------------

Every user-facing release should include:

-   Capability and owning team
-   What changed and who benefits
-   Why a developer should care
-   How to adopt it, with a working example
-   Prerequisites and required permissions
-   Compatibility and breaking-change implications
-   Availability, limitations, and rollout status
-   Links to documentation, migration steps, and support

Create a **single platform change feed** assembled from team-owned
release records. Filter it by capability, cloud, environment, audience,
and severity. Domain teams remain responsible for content; platform
product leadership defines the format, publishing requirements, and
communication policy.

Distinguish between **a release existing** and **a release being
adopted**. Publishing notes does not prove users know about a feature,
understand it, or have successfully used it.

### C. Discoverability and onboarding

Design for the questions developers actually ask:

-   What can I provision?
-   What is the recommended approach for my use case?
-   What prerequisites am I missing?
-   What am I permitted to do?
-   What happens after I run this command?
-   Where do I see status, logs, and evidence?
-   What has changed since I last used this capability?

A searchable capability catalog and well-designed CLI may be more useful
than a portal that simply exposes many forms. The portal should guide
people to action; CLI, APIs, and code-based workflows should remain
first-class interfaces.

## 4. Make agent actions governable

An AI agent can generate Terraform, modify Kubernetes manifests, and
invoke APIs. But plausible configuration is not the same as safe
infrastructure management. Infrastructure operations can have
long-running effects, irreversible consequences, and regulatory
implications.

Design around a clear distinction between **the agent's ability to
reason and the platform's authority to act**.

### Governed infrastructure lifecycle

1.  **Discover:** Find a supported capability, documentation, version,
    constraints, and required permissions.
2.  **Propose:** Produce a change plan or pull request with intended
    outcomes, acceptance criteria, and resource impact.
3.  **Validate:** Run deterministic checks for schemas, policy,
    security, tests, cost limits, and environment constraints.
4.  **Authorize:** An independent policy and authorization layer
    determines whether the action is allowed automatically or needs
    approval.
5.  **Execute:** Apply the approved change using scoped identity and
    record the actual operations performed.
6.  **Verify:** Check postconditions, including resource state,
    connectivity, configuration, tests, and applicable acceptance
    criteria.
7.  **Attest and retain evidence:** A person attests when required; the
    platform records identity, decision, versioned evidence, and
    resulting state.

The agent should not be able to manufacture its own proof of compliance.
Obtain evidence from the systems that actually executed tests, evaluated
policy, and applied infrastructure changes.

### Design rules for regulated infrastructure

-   **Separate permissions from intelligence.** The model does not
    decide what its own credentials permit.
-   **Make approvals contextual.** Show the change plan, affected
    resources, test results, policy outcomes, and material risks.
-   **Bind approval to the exact change.** If the plan, commit,
    artifacts, or relevant configuration changes, invalidate the
    approval or require revalidation.
-   **Distinguish test completion from acceptance.** Passing checks
    proves only that those checks ran and passed; it does not
    automatically prove every business acceptance criterion was met.
-   **Handle partial success.** An operation can time out after a cloud
    provider has applied a change. Reconcile actual state before
    retrying.
-   **Keep an auditable execution record.** Capture initiating identity,
    agent/workflow identity, requested intent, approved plan, policy
    decisions, execution results, and verification evidence.

Useful references:

-   [NIST Secure Software Development Framework
    (SSDF)](https://csrc.nist.gov/projects/ssdf)
-   [NIST SP 800-204D: Software Supply Chain Security in DevSecOps CI/CD
    Pipelines](https://csrc.nist.gov/pubs/sp/800/204/d/final)

These provide security-development and supply-chain assurance
foundations, but neither alone defines a complete control framework for
autonomous infrastructure operations.

## 5. Design the operating model for distributed platform teams

Use a **federated product model**: common experience and control
standards, with implementation and domain roadmaps owned by the relevant
teams.

### Platform product leadership

Own or coordinate:

-   North-star outcomes
-   Common standards
-   Portfolio priorities
-   Experience consistency
-   Adoption measurement

### Shared platform standards

Establish standards for:

-   Documentation contracts
-   Release metadata
-   API and CLI conventions
-   Security controls
-   Lifecycle semantics
-   Service health

### Domain product teams

Own:

-   Capability roadmaps and implementation
-   Operational health
-   Domain expertise
-   Release content
-   Support and incident response

### Shared developer-facing experience

Provide consistent:

-   Discovery and documentation
-   CLI/API conventions
-   Release feed
-   Status and feedback

#### Suggested decision rights

  -----------------------------------------------------------------------
  Decision                            Suggested owner
  ----------------------------------- -----------------------------------
  Platform strategy and experience    Platform product leadership
  principles                          

  Capability implementation and       Domain product and engineering
  roadmap                             owners

  Security policy and mandatory       Authorized security/control owners
  controls                            

  API and documentation standards     Shared platform standards owners

  Release readiness for a capability  Owning team against common criteria

  Cross-team dependencies and         Portfolio leadership with domain
  sequencing                          owners

  Risk acceptance and exceptional     Designated authorized approvers
  access                              
  -----------------------------------------------------------------------

Make shared standards enforceable where appropriate. For example, a
release pipeline could reject a capability release without published
documentation, a declared owner, or supported-version information.

Avoid making every team wait on a central committee for routine
releases. Establish minimum requirements, automated checks, and explicit
exception paths.

## 6. Measure outcomes, not activity

Avoid measuring success solely by the number of tools, agents, or
infrastructure modules launched.

  -----------------------------------------------------------------------
  Category                            Example measures
  ----------------------------------- -----------------------------------
  Developer experience                Time to first successful provision,
                                      task completion rate, support
                                      tickets per workflow

  Platform operations                 Provisioning success rate, median
                                      and tail latency, recovery time,
                                      change failure rate

  Agent readiness                     Capability discovery success, agent
                                      task completion, schema/tool
                                      errors, safe recovery from partial
                                      failures

  Governance                          Evidence completeness, policy
                                      violations prevented, approval
                                      integrity, successful audit
                                      sampling

  Documentation and communication     Broken examples, stale pages, time
                                      to publish release notes, feature
                                      awareness, adoption after
                                      announcements
  -----------------------------------------------------------------------

A high agent task completion rate is not enough if the agent used
excessive permissions, bypassed required approvals, or left
infrastructure in an unknown state. Measure **successful,
policy-compliant outcomes**.

Explore [DORA's platform engineering
guidance](https://dora.dev/capabilities/platform-engineering/) for
user-centered platform principles and outcome-oriented measurement.

## 7. Focused reading path

Read these in sequence to develop a coherent product and architecture
model before selecting tools.

### 1. Product strategy

**[CNCF Platform Engineering Maturity
Model](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/)**

Assess maturity across investment, adoption, interfaces, operations, and
measurement. Use it to identify what maturity should mean for your
organization.

### 2. Architecture

**[Platform Engineering reports and reference
materials](https://platformengineering.org/reports)**

Study control, resource, delivery, security, and observability planes.
Compare the patterns against your infrastructure environment and
cloud-owner boundaries. Treat vendor-sponsored recommendations as
options to evaluate, not prescriptions.

### 3. Product management

**[DORA platform engineering
guidance](https://dora.dev/capabilities/platform-engineering/)**

Focus on user-centered journeys, cognitive load, extensibility, and
platform outcomes. This counterbalances infrastructure-first thinking.

### 4. Documentation and discovery

**[Backstage TechDocs](https://backstage.io/docs/features/techdocs/) and
[Backstage documentation](https://backstage.io/docs/)**

Study documentation-as-code, ownership metadata, catalog relationships,
and software templates. Borrow the patterns even if the portal is
custom-built or CLI-first.

### 5. Governance and evidence

**[NIST SSDF](https://csrc.nist.gov/projects/ssdf) and [NIST SP
800-204D](https://csrc.nist.gov/pubs/sp/800/204/d/final)**

Develop a shared vocabulary for traceability, secure development,
verification, and provenance. Map practices to existing regulatory and
control requirements rather than assuming compliance comes from adopting
a framework.

### 6. Agentic evolution

**[The Four Levels of Agentic Software Development in the
Enterprise](https://platformengineering.org/reports/the-four-levels-of-agentic-software-development-in-the-enterprise)**

Use this as a discussion hypothesis for progression from coding
assistance to more autonomous execution, with deterministic validation
around probabilistic agents. It is an industry perspective, not a
universal standard.

## 8. A practical 30-day preparation plan

### Week 1 --- Define the product

-   Write the platform mission and identify target users.
-   Map the top five infrastructure journeys and their friction.
-   Agree on outcome measures and a platform maturity baseline.

### Week 2 --- Define the contracts

-   Create a standard capability documentation template.
-   Define common API, CLI, error, and lifecycle conventions.
-   Draft a minimum release metadata schema.

### Week 3 --- Design a governed workflow

-   Map a representative infrastructure lifecycle end to end.
-   Define policy, approval, verification, and evidence boundaries.
-   Identify failure, timeout, retry, and reconciliation cases.

### Week 4 --- Validate with a thin vertical slice

-   Select one repeatable infrastructure workflow.
-   Implement discovery, documentation, execution, and verification
    together.
-   Test it with an engineer and a coding agent; capture gaps and
    feedback.

A strong first slice would be a bounded infrastructure workflow, such as
provisioning a temporary test environment. It is concrete enough to
validate documentation, CLI/API experience, permissions, orchestration,
evidence, and cleanup without requiring every platform capability to be
built first.

## 9. Leadership framing

Summarize the strategy in five principles:

1.  **Platform as product:** Optimize for developer outcomes, not the
    number of services exposed.
2.  **One platform, multiple interfaces:** Humans and agents use shared
    capabilities and contracts rather than separate implementations.
3.  **Documentation and communication are product capabilities:** Every
    supported capability is discoverable, understandable, and
    maintained.
4.  **Automation is governed by evidence:** Agents may propose and
    execute permitted work, but independent controls enforce policy and
    verify outcomes.
5.  **Federated ownership, consistent experience:** Domain teams retain
    implementation ownership while the platform establishes shared
    standards, lifecycle conventions, and outcome measurement.

Do not treat "agentic" as a separate layer bolted onto a traditional
platform. It should influence capability contracts, documentation,
authorization, execution, and verification from the start---without
displacing the fundamentals that make a platform dependable.

### Recommended first deliverables

1.  **Platform product strategy** --- mission, users, journeys,
    principles, outcomes, and operating model.
2.  **Capability contract standard** --- documentation structure,
    metadata, APIs/CLI, errors, lifecycle, ownership, and release
    requirements.
3.  **Agent-governance reference architecture** --- permissions, policy,
    approvals, execution, verification, evidence, and failure recovery.

Together, these provide a durable foundation before committing to
specific technologies.
