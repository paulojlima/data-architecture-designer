# Data Architecture Designer

> A reusable AI Agent Skill for designing modern data architectures through
> requirements, constraints, quality attributes, and explicit architectural
> trade-offs.

## Why this project?

AI can generate impressive architecture diagrams and technology stacks in
seconds.

The harder problem is determining whether those architectural decisions are
actually justified.

**Data Architecture Designer** is an experimental reusable Agent Skill that
encodes a structured architecture decision process.

Instead of starting with:

> Which technologies should I use?

the Skill starts with:

> What problem are we solving, what constraints exist, and which quality
> attributes actually matter?

The goal is not to generate the most sophisticated architecture.

The goal is to recommend the **simplest architecture that satisfies the
requirements with acceptable trade-offs**.

---

## What it does

The Skill guides an AI agent through a structured architecture workflow:

```text
Business Context
       ↓
Data Characteristics
       ↓
Quality Attributes
       ↓
Constraints
       ↓
Architecture Patterns
       ↓
Alternatives
       ↓
Trade-offs
       ↓
Architecture Decisions
       ↓
Technology Mapping
       ↓
Risks & ADRs
```

It can be used to help:

- design a new data platform;
- review an existing data architecture;
- evaluate modernization approaches;
- compare architectural patterns;
- reason about batch vs streaming;
- evaluate Warehouse vs Lakehouse;
- identify unnecessary architectural complexity;
- document Architecture Decision Records (ADRs).

---

## Design Principles

### Requirements before technology

Technology selection should follow requirements analysis, not precede it.

### Architecture before products

Define the logical architecture and required capabilities before mapping
them to vendor-specific services.

### Explicit trade-offs

Architectural decisions should explain both what is gained and what is
sacrificed.

### Simplicity is a feature

Streaming, distributed processing, Kubernetes, Data Mesh, or multiple
processing engines should only appear when requirements justify them.

### Vendor-neutral by default

The Skill does not default to Azure, AWS, GCP, Databricks, Snowflake,
Microsoft Fabric, or another platform.

### Assumptions must be visible

Missing information should be identified rather than silently invented.

---

## Repository Structure

```text
data-architecture-designer/
│
├── .github/
│   └── skills/
│       └── data-architecture-designer/
│           │
│           ├── SKILL.md
│           │
│           ├── references/
│           │   ├── decision-framework.md
│           │   ├── architecture-patterns.md
│           │   └── quality-attributes.md
│           │
│           ├── template/
│           │   └── architecture-output.md
│           │
│           └── examples/
│               └── batch-analytics-platform.md
│
├── README.md
└── LICENSE
```

The Skill follows the project-level Agent Skills structure used by compatible
AI coding agents.

### [`SKILL.md`](.github/skills/data-architecture-designer/SKILL.md)

The main entry point for the Agent Skill.

Defines when the Skill should be activated, the mandatory execution rules,
the architecture workflow, the complexity test, and the quality gate the
agent should apply before producing a recommendation.

### [`references/decision-framework.md`](.github/skills/data-architecture-designer/references/decision-framework.md)

Defines how architectural decisions should be evaluated, including
alternatives, trade-offs, complexity checks, architecture smells, and
Architecture Decision Records (ADRs).

### [`references/architecture-patterns.md`](.github/skills/data-architecture-designer/references/architecture-patterns.md)

Provides guidance for evaluating patterns such as:

- Data Warehouse;
- Data Lake;
- Lakehouse;
- Medallion Architecture;
- Batch Processing;
- Streaming;
- Lambda;
- Kappa;
- Event-Driven Architecture;
- Data Mesh;
- centralized, federated, and hybrid approaches.

### [`references/quality-attributes.md`](.github/skills/data-architecture-designer/references/quality-attributes.md)

Provides guidance for evaluating architecturally significant quality
attributes such as:

- latency and data freshness;
- scalability;
- reliability and recoverability;
- security and privacy;
- governance;
- lineage;
- observability;
- maintainability;
- interoperability;
- cost efficiency.

### [`template/architecture-output.md`](.github/skills/data-architecture-designer/template/architecture-output.md)

Defines the output contract for a full architecture recommendation,
including logical architecture, technology mapping, decisions, alternatives,
trade-offs, risks, and ADRs.

### [`examples/`](.github/skills/data-architecture-designer/examples/)

Contains scenarios used to test whether the Skill produces architecture
proportional to the actual requirements.

The first example deliberately tests whether the agent can resist unnecessary
streaming, distributed processing, Kubernetes, Data Mesh, and other forms of
architectural over-engineering.

---

## Example

Consider a company with:

- ERP, CRM, and CSV data sources;
- approximately 500 GB of historical data;
- approximately 2 GB of daily changes;
- dashboards refreshed once per day;
- a small data engineering team;
- predominantly structured analytical workloads.

A technology-first approach might immediately propose:

```text
Kafka
+ Spark
+ Kubernetes
+ Lakehouse
+ Streaming
+ multiple processing engines
```

The Skill instead asks:

> What requirement actually requires each of these components?

For this scenario, daily batch ingestion and a simpler analytical platform
may satisfy the requirements with significantly lower operational
complexity.

See:

[`examples/batch-analytics-platform.md`](.github/skills/data-architecture-designer/examples/batch-analytics-platform.md)

---

## Architecture Decision Records

Significant decisions can be expressed as ADRs.

Example:

```text
ADR-001: Batch vs Streaming Ingestion

Context:
Business dashboards must be refreshed every morning.

Decision:
Use scheduled incremental batch ingestion.

Alternatives considered:
- Change Data Capture
- Event streaming

Rationale:
The required freshness can be satisfied without continuous processing
infrastructure.

Trade-off:
Lower operational complexity in exchange for non-real-time availability.

Revisit when:
Business requirements require materially lower data latency.
```

This makes architecture decisions easier to understand, challenge, and
revisit.

---

## Using the Skill

Clone the repository or copy the Skill into a location supported by your
AI coding agent.

The core entry point is:

```text
SKILL.md
```

Example request:

```text
Design a data architecture for an e-commerce platform.

We have PostgreSQL, Salesforce and application events.

Approximately 5 TB of historical data is stored today and we expect
100 GB of new data per day.

Finance reporting can tolerate a 4-hour delay, but fraud detection
requires events within 10 seconds.

The platform must support BI, data science and regulatory lineage.

Compare viable architectural approaches and explain the trade-offs.
```

The agent should use the Skill to determine which requirements justify
batch processing, streaming, storage patterns, governance capabilities,
and technology choices.

---

## What this Skill intentionally avoids

This project is designed to challenge:

- technology-first architecture;
- unnecessary real-time processing;
- premature distributed systems;
- tool duplication;
- governance as an afterthought;
- unacknowledged vendor lock-in;
- architecture driven primarily by technology trends.

A complex architecture can be correct.

**Complexity simply needs a reason.**

---

## Status

🚧 **Experimental / v0.1**

This project is an experiment in encoding architectural decision-making into
reusable AI Agent Skills.

It is expected to evolve through testing against different architecture
scenarios.

Feedback, issues, and contributions are welcome.

---

## Roadmap

Potential future improvements include:

- [ ] additional architecture test scenarios;
- [ ] real-time / event-driven example;
- [ ] regulated financial-services example;
- [ ] architecture review mode;
- [ ] cloud-specific technology mappings;
- [ ] automated architecture quality checks;
- [ ] additional ADR templates;
- [ ] architecture diagram generation improvements.

---

## Contributing

Ideas, discussions, issues, and contributions are welcome.

If you find an architecture scenario where the Skill makes a questionable
decision, please open an issue and describe:

1. the requirements;
2. the recommendation produced;
3. why you disagree with it;
4. what alternative you would consider.

Those cases are particularly useful for improving the decision framework.

---

## License

Released under the [MIT License](LICENSE).
