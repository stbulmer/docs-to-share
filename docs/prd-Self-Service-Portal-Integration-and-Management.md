# PRD: Self-Service Portal Integration and Management

Nov 24, 2026 · Stephen

The portal becomes a journey-based hub that renders every offering from a contract the service owner publishes and maintains. The portal team stops being on the critical path for forms, content or rollout.

## Problem and evidence

The portal gates rollout for service teams and does not give engineers a clear path. Five problems drive this.

- **Integration takes months.** Every form, wording change and new flow goes through a small, frontend-heavy portal team maintaining a static site. Two major onboarding flows and one offboarding flow are currently held up, and one or two more platforms are approaching.
- **The portal carries an unwritten standard.** A team that designed offboarding with portal engineers was blocked in May once its API contract was shared. Requirements such as an order-style ID exist only in the portal, while governance already lives in the API layer.
- **External APIs are interwoven into the portal experience.** Existing flows mix provider APIs and portal logic so tightly that they are hard to understand or undo. This is the reason for the new consolidated API version.
- **Registration is a directory entry.** Adding a service captures a description and a link. There is no integration contract and no accountability for standards.
- **The site is organized around things, not decisions.** The home page behaves like a management page. Navigation is crowded with release schedules and semi-related items. Categories such as compute, object and database were designed around private cloud and do not match what is offered today.

Engineers who cannot find an answer are routed by the Slack bot to operations staff, whose knowledge of each offering varies. Once an offering is rolled out, both application teams and service owners stop using the portal.

## Goals and non-goals

**Goals**

- A service owner can publish and change an offering, its forms, its instructional text and its news without work from the portal team.
- An engineer lands on a home page organized by decision and reaches the right next step in one click.
- The portal, the CLI and agents all read the same contract, so a capability appears on every surface when its API ships.
- News, roadmap and release content become a first-class part of the portal, owned by the teams that ship the changes.
- Standards are enforced by automated guardrails at publish time, with no manual review queue.

**Non-goals**

- Rebuilding usage or management views that already exist in cloud and internal consoles.
- Improving the legacy on-prem, ticket-backed request flows owned by a separate organization. They stay listed and labeled.
- Making the portal the required path for any action. The CLI and API remain valid on their own.
- Preserving portal-only requirements such as the order-style ID.

## Product principles

1. **The portal contains no business logic.** Validation, entitlement and orchestration live behind the API. The portal renders and submits.
2. **The contract is led by the shared metadata API.** All public cloud already shares one metadata API. The offering contract extends that capability, and the portal conforms to it. The portal does not define its own contract.
3. **Service owners own their contract and content.** Fields, help text, docs and announcements are theirs to change at any time.
4. **Every surface is a client, and no surface gates a rollout.** A capability ships when its API contract exists. This applies to the CLI as much as to the portal.
5. **Guardrails are built in.** Anything that would have been a review comment becomes an automated check with a clear error.
6. **One vocabulary across surfaces.** Learn, Get access (Onboard or Order) and Manage name the portal's entry points, the CLI's command groups and the agent's intents.
7. **Organize by decision, not by type of thing.** Categories are a filter inside the catalog.

## At a glance

*Diagram: target architecture · contract, three surfaces, one API.*

The owner publishes once. The portal, CLI and agents read the same contract and call the same API, which holds all the logic.

## Users and their journeys

| User | What they need | Journey in this PRD |
| --- | --- | --- |
| Application engineer | Find the right offering, get access, then manage what they have | Home, Learn, Get access, Manage |
| Service owner | Publish an offering and keep it current without waiting on another team | Admin: register, define forms, write docs, post news, watch scorecard |
| Portal team | Maintain one renderer and a design system, not a form per offering | Renderer, admin tooling, guardrail engine |
| Leadership | See what is shipping and whether teams adopt it | What's new, roadmap, usage by client |

The site already knows the signed-in user and requires a tenant selection. Every page in this PRD is scoped to that user and tenant.

## Capability 1: Home and navigation

The home page becomes a hub of decisions. Management moves to its own page.

| ID | Requirement | Priority |
| --- | --- | --- |
| HOME-1 | Home presents four entry points: Learn, Get access, Manage, What's new | P0 |
| HOME-2 | What's new is a large, front-and-center component on home, showing recent releases and upcoming roadmap items for the selected tenant | P0 |
| HOME-3 | Usage and management content is removed from home and lives under Manage | P0 |
| HOME-4 | Top navigation carries only the four entry points plus search and tenant selection. Release schedules and other items move into What's new | P0 |
| HOME-5 | Private-cloud type tiles (compute, object, database) are removed from home. Categories become a filter in Learn | P0 |
| HOME-6 | Home adapts to state: a tenant with no offerings sees Learn and Get access first, a tenant with offerings sees Manage and What's new first | P1 |
| HOME-7 | Service owners see an Admin entry in navigation | P0 |

## Capability 2: Learn and the catalog

Every offering page is generated from the owner's contract, so the catalog is as detailed as the contract requires.

| ID | Requirement | Priority |
| --- | --- | --- |
| LEARN-1 | Each offering has a page rendered from its contract: what it is, who owns it, how to get access, how to get support | P0 |
| LEARN-2 | A how-to document is required for an offering to be published | P0 |
| LEARN-3 | The catalog is searchable and filterable by category, cloud, access type (Onboard or Order) and lifecycle | P0 |
| LEARN-4 | The category taxonomy spans public and private cloud and reflects current offerings | P0 |
| LEARN-5 | Each offering page shows its recent news and upcoming roadmap items | P1 |
| LEARN-6 | Legacy ticket-backed offerings are listed with a legacy label and a pointer to the modern alternative where one exists | P1 |
| LEARN-7 | The same catalog is available through the API for the CLI and agents | P0 |

## Capability 3: Get access

Get access offers two actions and explains the difference with examples, because the distinction is ours and not the engineer's.

| Action | What it is | What happens next | Example |
| --- | --- | --- | --- |
| Onboard | Join a platform and receive broad access to it | Immediate and self-service. Resources are then created through the platform's own tooling | Onboarding an application to a cloud platform |
| Order | Request a discrete resource from an owning team | A request with lead time, and sometimes a consultation before use | Requesting a managed resource from an offering team |

| ID | Requirement | Priority |
| --- | --- | --- |
| ACCESS-1 | Get access shows Onboard and Order side by side with a one-line description and an example of each | P0 |
| ACCESS-2 | Each offering declares its access type in its contract, so the engineer is routed to the right action from the offering page | P0 |
| ACCESS-3 | Onboard pages show the CLI command and an agent prompt, ready to copy, beside any rendered form | P0 |
| ACCESS-4 | Forms for both actions are rendered from the owner's form schema. The portal adds no fields and no rules | P0 |
| ACCESS-5 | Order pages show expected lead time and whether a consultation is required, as declared by the owner | P0 |
| ACCESS-6 | Lifecycle actions beyond access, including offboarding, use the same schema-driven mechanism | P0 |
| ACCESS-7 | Submissions carry the user's session identity to the API so that the API can authorize and attribute them | P0 |

## Capability 4: Manage and activity history

Manage shows what the tenant has and links to where it is operated. It does not rebuild consoles.

| ID | Requirement | Priority |
| --- | --- | --- |
| MANAGE-1 | Manage lists the tenant's onboarded platforms and ordered resources, read from the API | P0 |
| MANAGE-2 | Each item deep-links to the console or tool where it is managed | P0 |
| MANAGE-3 | An activity history shows actions taken for the tenant, built from real events such as access grants and Terraform runs | P0 |
| MANAGE-4 | The order-style ID is retired. Activity entries are identified by the event and resource they describe | P0 |
| MANAGE-5 | Activity history is the same whether the action came from the portal, the CLI or an agent | P1 |
| MANAGE-6 | Open Order requests show status and owner contact | P1 |

## Capability 5: What's new, roadmap and releases

The portal becomes the place to learn what changed and what is coming, across every offering. Today the notification section only reports changes to the portal itself.

| ID | Requirement | Priority |
| --- | --- | --- |
| NEWS-1 | Service owners publish release notes, announcements and roadmap items for their own offerings, with no approval step | P0 |
| NEWS-2 | Each item is tied to an offering and typed as released, upcoming or deprecation | P0 |
| NEWS-3 | The home component and a full What's new page show a combined feed, filterable by offering, category and type | P0 |
| NEWS-4 | A roadmap view shows upcoming items by expected period, as stated by each owner | P1 |
| NEWS-5 | Engineers receive notifications for offerings their tenant uses, and can follow others | P1 |
| NEWS-6 | Items past their stated period are flagged to the owner to update or close | P1 |
| NEWS-7 | The feed is available through the API for the CLI and agents | P1 |

## Capability 6: Service owner admin

The current "add a service" form grows into an admin area where an owner runs their offering end to end.

| ID | Requirement | Priority |
| --- | --- | --- |
| ADMIN-1 | An owner registers an offering by supplying its contract, through the admin UI or as a file in source control | P0 |
| ADMIN-2 | An owner edits form fields, labels, help text and instructional content, and the change is live on publish | P0 |
| ADMIN-3 | A preview shows the rendered offering page and forms before publish | P0 |
| ADMIN-4 | Publish runs guardrail checks and reports each failure with the field and the fix | P0 |
| ADMIN-5 | Contracts are versioned, with history and rollback | P0 |
| ADMIN-6 | An owner manages docs, news and roadmap items for the offering in the same place | P0 |
| ADMIN-7 | An owner assigns co-maintainers from their team | P1 |
| ADMIN-8 | A scorecard shows contract completeness, doc freshness and stale roadmap items | P1 |
| ADMIN-9 | An owner sees usage of the offering by client: portal, CLI and agent | P1 |
| ADMIN-10 | Offerings that outgrow generic rendering can host their own page built on the portal design system | P2 |

## Capability 7: Offering contract and schemas

The contract is the product. It extends the shared metadata API and is the single source for every surface.

| Part of the contract | Contents | Used by |
| --- | --- | --- |
| Identity | Name, owning team, support route, category, cloud, lifecycle | Catalog, search, agents |
| Access | Access type (Onboard or Order), lead time, consultation required | Get access routing |
| Actions | For each lifecycle action (onboard, order, change, offboard): the API endpoint and its input schema | Forms, CLI, agent tools |
| Presentation | Labels, help text, field order and grouping for each input | Forms, CLI help, agent tool descriptions |
| Docs | How-to document and links | Learn, Slack bot, vector store |
| Announcements | Release, upcoming and deprecation items | What's new, roadmap |

| ID | Requirement | Priority |
| --- | --- | --- |
| CONTRACT-1 | The contract schema is defined as an extension of the shared metadata API and owned by the API team | P0 |
| CONTRACT-2 | Input schemas for actions are derived from the API's own payload definitions, so form and API cannot drift | P0 |
| CONTRACT-3 | Instructional text is stored once in the contract and reused by the portal, CLI help and agent tool descriptions | P0 |
| CONTRACT-4 | Lifecycle actions use a common set of verbs across all offerings | P0 |
| CONTRACT-5 | The contract schema is versioned, and changes to it are backward compatible | P0 |
| CONTRACT-6 | The contract supports declarative resources, including a reference to a Terraform module or provider where one exists | P1 |
| CONTRACT-7 | A contract can be validated locally and in CI before publish | P1 |

## Capability 8: Backend and integration layer

A configuration management layer replaces the static site. The API team owns the contract and API. Portal backend engineers build the configuration store and renderer with them.

| ID | Requirement | Priority |
| --- | --- | --- |
| BACKEND-1 | A contract registry stores published contracts and serves them to the portal, CLI and agents | P0 |
| BACKEND-2 | A generic renderer turns any valid contract into an offering page and its forms, with no per-offering code | P0 |
| BACKEND-3 | The portal calls the consolidated API only. Direct calls from portal code to provider or external APIs are removed | P0 |
| BACKEND-4 | The portal passes the user's session identity to the API on every call | P0 |
| BACKEND-5 | Every API call carries a client identifier (portal, CLI, agent) for usage measurement | P0 |
| BACKEND-6 | Existing frozen onboarding flows keep working against the backward-compatible endpoints until their offering moves to the contract | P0 |
| BACKEND-7 | The portal design system (styles and components) is published for use by owner-hosted pages | P2 |
| BACKEND-8 | Contract changes emit events that the vector store and Slack bot can consume | P2 |

## Guardrails

Self-service only works if the checks are automatic. Each check below replaces something that is a manual review or an unwritten standard today.

| Check at publish | Replaces |
| --- | --- |
| Contract validates against the schema | Portal team reviewing a form design |
| Every action's input schema matches the API's payload definition | Back-and-forth once the API contract is shared |
| Owner, support route and how-to doc are present | Directory entries with only a description and a link |
| Access type, and lead time for Order, are declared | Engineers discovering the process after clicking |
| Publisher is a maintainer of the offering | Central approval of content changes |
| Text and links pass basic content checks (length, broken links) | Wording review by the portal team |

All publishes are logged with author, version and time. A failed check blocks the publish and names the field. No check requires a person on the portal team.

## Success metrics

Baselines and targets are open and need to be set from current data.

| Metric | What it shows | Source |
| --- | --- | --- |
| Integration lead time: contract submitted to live on the portal | Whether the portal still gates rollout | Contract registry |
| Share of offering changes published without portal team work | Whether owners are self-sufficient | Contract registry, portal team backlog |
| API usage by client (portal, CLI, agent) | Where to invest in each surface | API metrics |
| Offerings on the contract, of all listed | Coverage of the new model | Contract registry |
| Onboarding and how-to questions reaching operations | Whether docs and clearer journeys deflect support | Slack bot, ops channels |
| News and roadmap items published per offering | Whether What's new is alive | Contract registry |

## Phasing

1. **Contract and registry.** Define the contract schema on the metadata API. Stand up the registry. Add client identifiers to API calls. Launch the CLI onboarding experimentally against the same endpoints.
2. **Renderer and first offerings.** Build the generic renderer. Move the stuck onboarding and offboarding flows onto the contract as the first offerings, with the private cloud team as design partner.
3. **Admin and guardrails.** Ship the admin area, preview, versioning and publish-time checks. Owners take over their own content.
4. **Home and navigation.** Replace the home page and navigation with the four entry points. Move management content to Manage.
5. **What's new.** Ship owner-published news and roadmap, the home component and notifications.
6. **Migrate and retire.** Move remaining offerings to the contract. Remove direct provider API calls from portal code. Retire the order-style ID.

Phases 4 and 5 depend only on phase 1 and can run in parallel with 2 and 3.

## Risks and open questions

**Risks**

- **The renderer becomes the new bottleneck.** If a contract needs a field type the renderer lacks, owners wait again. Mitigation: owner-hosted pages on the design system (ADMIN-10) as the escape hatch.
- **Owners publish poor content.** Removing review removes a quality filter. Mitigation: guardrails, scorecards and visible ownership on every page.
- **Two paths diverge during migration.** Frozen flows and contract-based flows run side by side. Mitigation: a dated migration list in phase 6.
- **The portal team does not adopt the model.** Mitigation: a ruling from shared leadership that contract conformance is sufficient to ship.

**Open questions**

- [ ] Does the shared metadata API cover private cloud offerings today, or only public cloud?
- [ ] Who owns and staffs the renderer: portal backend engineers, the API team, or both?
- [ ] What prevents the portal from passing the session identity today, and does the fix need a security review?
- [ ] What are the baseline lead times for the stuck flows, including dates for the May offboarding case?
- [ ] Has change management confirmed that nothing depends on the order-style ID?
- [ ] Which usage thresholds from the experimental CLI release decide whether portal onboarding forms stay?
- [ ] Do engineers understand Onboard versus Order as labeled, or does it need testing with a few users?
- [ ] Which common lifecycle verbs apply to every offering?
