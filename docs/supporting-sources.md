### Source materials

**Platform engineering foundations**

- CNCF Platforms White Paper and the [Platform Engineering Maturity Model](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/). A refresh of both started in Q1 2026, so look for the newest version. [netlify](https://cnpe.netlify.app/blog/2026-initiatives-announcement)
- _Team Topologies_ (Skelton & Pais): the clearest way to think about platform teams versus the teams they serve, and about how much complexity a team can carry.
- Google’s _Site Reliability Engineering_ and _SRE Workbook_ (free online): especially the chapters on release engineering and managing change.

**Docs for humans and agents**

- [Diátaxis](https://diataxis.fr): sorts docs into tutorials, how-to guides, reference, and explanation. Agents benefit from this split as much as people do.
- [llmstxt.org](https://llmstxt.org): the spec. Look at platform.claude.com/llms.txt and docs.stripe.com/llms.txt as real examples.
- [agents.md](https://agents.md): the spec and examples.
- [MCP specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/changelog): read the changelog first.
- OpenAPI 3.1 and JSON Schema: the source your API reference and agent tools should both be generated from.
- _Docs for Developers_ (Bhatti et al.): a practical book on writing technical docs.

**Release communication**

- [Keep a Changelog](https://keepachangelog.com), [Semantic Versioning](https://semver.org), and [Conventional Commits](https://www.conventionalcommits.org): these three together let you generate changelogs automatically.
- CloudEvents (a CNCF spec): for publishing release and deprecation events that other teams’ tools can subscribe to.
- Stripe’s API versioning and upgrade docs, and Kubernetes’ deprecation policy: two strong real-world examples of managing breaking changes at scale.


Sources:

- [llms.txt Explained (May 2026), Codersera](https://codersera.com/blog/llms-txt-complete-guide-2026/)
- [llms.txt: Discovery for AI Documentation, Emergent Mind](https://www.emergentmind.com/topics/llms-txt)
- [Documentation stack for AI agents, Ravi Chaganti](https://ravichaganti.com/blog/documentation-stack-for-ai-agents/)
- [MCP spec 2026-07-28 release, GitHub](https://github.com/modelcontextprotocol/specification/releases/tag/2026-07-28)
- [MCP removes sessions, PPC Land](https://ppc.land/mcp-forces-ad-tech-to-rebuild-agent-servers-as-sessions-disappear/)
- [Announcing the Platform Engineering Maturity Model, CNCF](https://tag-app-delivery.cncf.io/blog/announcing-the-platform-engineering-maturity-model/)
- [2026 refresh of the whitepaper and maturity model, CNPE](https://cnpe.netlify.app/blog/2026-initiatives-announcement)
- [agents.md](https://agents.md/)

## Source materials: a focused reading path

Rather than trying to read everything about platform engineering, I'd study the following in order. The goal is to develop a coherent product and architecture model before choosing tools.

[Free Cloud Native Computing Foundation by Linux Logo Vector Download | Logowik](https://images.openai.com/static-rsc-4/YEAtnxkyTh3UY7rdqLD9pYaO4HHO35C9xKeyHFTTh7N0Uy7Lo3zRkdOOzhL3EOnBYOfwIXGn4LGUtv6H7imqqApvMbQaS5_sJeYugSY0u5BeFgBh2J8q7nKEcWKDT7XynEVIQhjn9nH9QnqWMQzyxS-lQAG3y8xl3bqcOUV8hp4?purpose=inline)

FIRST · PRODUCT STRATEGY

CNCF Platform Engineering Maturity Model

Use this to assess where your platform stands across five dimensions and to identify what maturity should mean for your organization. It helps prevent architecture from becoming the entire strategy.

[Read the framework](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/?utm_source=chatgpt.com)

[Designing an internal developer platform architecture - AWS Prescriptive Guidance](https://images.openai.com/static-rsc-4/8VCJdBwSwDJbraIxgc8Mm2l0Zhg3d1II9psYePawccgKO6TQXd6f_C71Kf7ZFRYo9Zncp2WltPdXLAmt6wgXW4hFLzm1XNy0m8VjWKhPsl9C9bQHMNG1SN2ORZ80TsO5Nz10PFXAnhMZ0K9Tt5bmvPLSf3lc7ibuSM9-_TTphBo?purpose=inline)

SECOND · ARCHITECTURE

Internal Developer Platform reference architectures

Study the control, resource, delivery, security and observability planes. Compare the patterns against your infrastructure environment and existing cloud-owner boundaries. Treat vendor-sponsored architecture recommendations as options to evaluate, not prescriptions.

[Explore reference architectures](https://platformengineering.org/reports?utm_source=chatgpt.com)

[Announcing the 2024 DORA report | Google Cloud Blog](https://images.openai.com/static-rsc-4/ypf3ukq_fncG-GyhViErrNnZ029x8t_kXIaIh6sPqRIuMhAsftWbbU5eoP8FTOcJM4V8Bv3OQh83LQBnamY3meyWDz9790shcZQtuJ0R0fS876JuueF1hv5SAAUboh8FQbXpYLheOixWGHPZ0PTqsVeYz13zVs0FHRB3GZehVhI?purpose=inline)

THIRD · PRODUCT MANAGEMENT

DORA platform engineering guidance

Focus on user-centered journeys, reducing cognitive load, extensibility and platform outcomes. This provides a strong counterbalance to infrastructure-first thinking.

[Read DORA's guidance](https://dora.dev/capabilities/platform-engineering/?utm_source=chatgpt.com)

[Demo TechDocs Basics - KodeKloud](https://images.openai.com/static-rsc-4/Ex04AUxGEmoPzyhaEiBI9hMNBo5MgzUrKG0efOY_Y9O_gOf4tRaw8wagz0N1K1P_cHrUXes0PwqfYPH2klL0EfFiHfkENzzecvE0ZfcgSzOQ17ev74DpYqRm8lLRJHbK6vE8YB7XFjGVASK07u4R2yx88U3cQXKodFl3git9Yzw?purpose=inline)

FOURTH · DOCUMENTATION AND DISCOVERY

Backstage TechDocs and Software Catalog

Study documentation-as-code, ownership metadata, catalog relationships and software templates. Borrow the patterns even if your portal is custom-built or CLI-first.

[TechDocs documentation](https://backstage.io/docs/features/techdocs/?utm_source=chatgpt.com) · [Backstage documentation](https://backstage.io/docs/?utm_source=chatgpt.com)

[The NIST Framework (CSF) 2.0 por National Institute of Standards and Technology - Audiolibro - Audible.com](https://images.openai.com/static-rsc-4/Rc-4E7IRTUlB1VGT-f1v7WCcboCtkUOEJRFu6NzKuey3pX3XanP3oDsYvhx8e2VMcHGm1_97OVdLNeNs0zzq1YwAz0fUnDd5iNCwN7ZRissgnaFuJdKwmbePNHHFf2Do1bT0D6C4Tov4JYENrdcWTOlWtb-V6GhQwwdYX7cOMnc?purpose=inline)

FIFTH · GOVERNANCE AND EVIDENCE

NIST SSDF and SP 800-204D

Use these to develop your security vocabulary around traceability, secure development, verification and provenance. Map their practices to your organization's existing regulatory and control requirements rather than assuming compliance is achieved by adopting a framework.

[NIST SSDF](https://csrc.nist.gov/projects/ssdf?utm_source=chatgpt.com) · [NIST SP 800-204D](https://csrc.nist.gov/pubs/sp/800/204/d/final?utm_source=chatgpt.com)

[Why developer portals matter more in the age of AI agents](https://images.openai.com/static-rsc-4/UDkp5V-UYBDZEXOmZSiseUKl3Zhbf_9XcccL9mOuhE9VUOkWXaFh1T4DVLPKhJQd0CrufXITdF644jPfTMOmlCfwWytd1sqjfVt2o8ncgZW077KjTdtQuR9F7CbjpwUzC0TrA1YaPgDVF4qX95rVWDugQPAP4VtkHVbIZ0mfASk?purpose=inline)

SIXTH · AGENTIC EVOLUTION

The Four Levels of Agentic Software Development in the Enterprise

Use this as a hypothesis for discussing the progression from coding assistance to more autonomous execution, particularly the need for deterministic validation around probabilistic agents. It is an industry perspective, not a universal standard.

[Explore the report](https://platformengineering.org/reports/the-four-levels-of-agentic-software-development-in-the-enterprise?utm_source=chatgpt.com)
