# Data Architecture Designer

> A reusable Agent Skill for designing data architectures through requirements,
> quality attributes, explicit trade-offs, and justified architectural decisions.

[![Release](https://img.shields.io/github/v/release/paulojlima/data-architecture-designer)](https://github.com/paulojlima/data-architecture-designer/releases)
[![License](https://img.shields.io/github/license/paulojlima/data-architecture-designer)](LICENSE)

**Complexity is acceptable. Unjustified complexity is not.**

---

## Why this project?

AI agents can generate impressive architecture diagrams and technology stacks
in seconds.

The harder problem is determining whether those architectural decisions are
actually justified.

**Data Architecture Designer** encodes a structured architecture decision
process into a reusable Agent Skill.

Instead of starting with:

> Which technologies should I use?

the Skill starts with:

> What problem are we solving, what constraints exist, and which quality
> attributes actually matter?

The objective is not to generate the most sophisticated architecture.

It is to recommend the **simplest architecture that satisfies the requirements
with acceptable trade-offs**.

---

## What it does

The Skill guides an AI agent through a requirements-first architecture workflow:

```text
Business Context
        ↓
Data Characteristics
        ↓
Architecturally Significant Quality Attributes
        ↓
Constraints
        ↓
Candidate Architecture Patterns
        ↓
Logical Architecture
        ↓
Alternatives & Trade-offs
        ↓
Complexity Test
        ↓
Technology Mapping
        ↓
Risks
        ↓
Architecture Decision Records
```

It can help with:

- designing new data platforms;
- reviewing existing data architectures;
- defining target architectures;
- evaluating modernization approaches;
- comparing architectural patterns;
- reasoning about batch vs streaming;
- evaluating Data Warehouse vs Lakehouse;
- evaluating centralized vs federated approaches;
- identifying unnecessary architectural complexity;
- documenting Architecture Decision Records (ADRs).

---

## Core Design Principles

### Requirements before technology

Technology selection follows requirements analysis, not the other way around.

### Architecture before products

Define logical capabilities before mapping them to vendor-specific services.

### Explicit trade-offs

Architectural decisions should explain what is gained, what is sacrificed,
and why the trade-off is acceptable.

### Simplicity is a feature

Streaming, distributed processing, Kubernetes, Data Mesh, multiple processing
engines, or additional storage platforms should only appear when requirements
justify them.

### Vendor-neutral reasoning

The Skill does not default to Azure, AWS, GCP, Databricks, Snowflake,
Microsoft Fabric, or another platform.

A strategic platform may be a valid constraint, but it should influence
technology mapping rather than replace architectural reasoning.

### Assumptions must be visible

Missing information should be identified rather than silently invented.

---

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/paulojlima/data-architecture-designer.git
cd data-architecture-designer
```

### 2. Open it in VS Code

```bash
code .
```

The project-level Agent Skill is located at:

```text
.github/skills/data-architecture-designer/
```

Compatible AI coding agents can discover the Skill from this workspace
structure.

### 3. Use your coding agent

Open your AI coding agent in the repository and ask for a data architecture.

For example:

```text
Design a data architecture for an e-commerce platform.

We have PostgreSQL, Salesforce and application events.

Approximately 5 TB of historical data exists today and we expect
100 GB of new data per day.

Finance reporting can tolerate a 4-hour delay, but fraud detection
requires relevant events within 10 seconds.

The platform must support BI, data science and regulatory lineage.

Compare viable architectural approaches and explain the trade-offs.
```

The agent should determine which requirements justify capabilities such as
batch processing, streaming, storage patterns, governance, and technology
choices.

You should not need to tell the agent which architecture or technologies
to select.

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

### [`SKILL.md`](.github/skills/data-architecture-designer/SKILL.md)

The main entry point for the Agent Skill.

Defines activation guidance, mandatory execution rules, the architecture
workflow, the Complexity Test, and the architecture quality gate.

### [`references/decision-framework.md`](.github/skills/data-architecture-designer/references/decision-framework.md)

Defines how architectural decisions should be evaluated, including
alternatives, trade-offs, complexity checks, architecture smells, and ADRs.

### [`references/architecture-patterns.md`](.github/skills/data-architecture-designer/references/architecture-patterns.md)

Provides guidance for evaluating patterns such as:

- Data Warehouse;
- Data Lake;
- Lakehouse;
- Medallion Architecture;
- Batch Processing;
- Streaming;
- Lambda and Kappa;
- Event-Driven Architecture;
- Data Mesh;
- centralized, federated, and hybrid approaches.

### [`references/quality-attributes.md`](.github/skills/data-architecture-designer/references/quality-attributes.md)

Provides guidance for evaluating architecturally significant attributes such as:

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

Defines the output contract for a full architecture recommendation, including:

- business context;
- requirements and assumptions;
- quality attributes;
- logical architecture;
- technology mapping;
- data flows;
- architectural decisions;
- ADRs;
- alternatives;
- trade-offs;
- risks and mitigations.

### [`examples/`](.github/skills/data-architecture-designer/examples/)

Contains scenarios used to test whether the Skill produces architecture
proportional to the actual requirements.

---

## The Complexity Test

Before adding significant architectural complexity, the Skill explicitly asks:

```text
Streaming
└── What requirement cannot be satisfied with batch, incremental batch, or CDC?

Distributed Processing
└── What workload characteristic actually requires distributed processing?

Kubernetes
└── What deployment or operational requirement requires it?

Multiple Processing Engines
└── What distinct workload requires each engine?

Multiple Serving Engines
└── Why are overlapping analytical serving technologies necessary?

Multiple Storage Platforms
└── What requirement requires each persistent storage technology?

Data Mesh
└── What organizational scaling or ownership problem requires it?
```

If a component cannot justify its existence, the Skill should remove it.

---

## Example: resisting over-engineering

Consider a company with:

- ERP, CRM, and CSV sources;
- approximately 500 GB of historical data;
- approximately 2 GB of daily changes;
- dashboards refreshed once per day;
- a small data engineering team;
- predominantly structured analytical workloads.

A technology-first architecture might immediately introduce:

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

For this scenario, scheduled ingestion and a simpler analytical platform may
satisfy the requirements with significantly lower operational complexity.

See the full
[`Batch Analytics Platform example`](.github/skills/data-architecture-designer/examples/batch-analytics-platform.md).

---

## Validation: does the Skill actually change agent behavior?

The Skill has been tested with **GitHub Copilot Agent in VS Code** using the
same insurance data-platform scenario before and after strengthening the
Skill's execution rules.

The scenario deliberately contained mixed requirements:

```text
Regulatory / Management Reporting
Freshness requirement: up to 4 hours
             │
             ├── Batch / incremental processing is sufficient
             │
             │
             └──────────────┐
                            │
                    Governed analytical
                       foundation
                            │
             ┌──────────────┘
             │
             ├── Low-latency event processing required
             │
IoT Fraud Detection
Freshness requirement: 15 seconds
```

### Initial behavior

The agent identified the mixed latency requirements, but moved too quickly
into vendor-specific technologies and introduced several overlapping
platform capabilities.

Logical architecture and technology selection were not sufficiently separated.

### After strengthening the Skill

The same prompt produced a materially more structured recommendation:

| Architecture behavior | Initial | v0.2 |
|---|:---:|:---:|
| Requirements-first reasoning | Partial | Yes |
| Explicit quality attributes | Partial | Yes |
| Logical architecture before products | No | Yes |
| Batch vs streaming justified separately | Yes | Yes |
| Complexity challenge | Partial | Yes |
| Technology mapping separated | No | Yes |
| Alternatives evaluated | Partial | Yes |
| Explicit trade-offs | Partial | Yes |
| ADRs | Partial | Yes |
| Assumptions surfaced | Partial | Yes |
| Structured architecture output | No | Yes |

The resulting recommendation used:

- batch / micro-batch for policy, claims, and CRM workloads;
- a dedicated low-latency path for IoT fraud detection;
- a shared governed analytical foundation;
- explicit lineage, privacy, security, and data-quality controls;
- technology mapping only after the logical architecture had been established.

This is not intended as a formal benchmark.

It is a practical validation that encoding architecture reasoning as an Agent
Skill can materially influence how an AI agent approaches the same problem.

---

## Architecture Decision Records

Significant decisions are documented using ADRs.

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

The purpose is not merely to record the final technology choice.

It is to preserve **why the decision was made**.

---

## Anti-Patterns

The Skill explicitly challenges:

### Resume-Driven Architecture

Selecting technologies because they are fashionable, interesting, or useful
for a CV and then searching for requirements to justify them.

### Vendor Catalogue Architecture

Representing an architecture as a collection of cloud services without
demonstrating why each service exists.

### Diagram-Driven Complexity

Adding components because a more complex diagram appears more architectural.

### Real-Time by Default

Using streaming because low latency is technically possible rather than
because the business requires it.

### Distributed by Default

Using distributed processing because a dataset is described as large without
evaluating the actual workload.

### Governance by Product

Assuming that deploying a catalog or governance product creates ownership,
accountability, or data governance.

### Future-Proofing Without Evidence

Adding substantial complexity for hypothetical future requirements without
credible evidence that they are likely.

---

## Status

**v0.2.0 — Experimental**

The current release establishes the first validated version of the architecture
reasoning framework.

The project is intentionally experimental. Architecture decisions remain
context-dependent and should be reviewed by qualified practitioners,
particularly for regulated, safety-critical, or high-impact systems.

See the
[latest release](https://github.com/paulojlima/data-architecture-designer/releases/latest).

---

## Roadmap

Potential future improvements:

- [ ] additional architecture test scenarios;
- [ ] real-time / event-driven scenario;
- [ ] regulated financial-services scenario;
- [ ] architecture review mode;
- [ ] cloud-specific technology mappings;
- [ ] automated architecture quality checks;
- [ ] additional ADR templates;
- [ ] improved architecture diagram generation;
- [ ] validation with additional AI coding agents.

---

## Contributing

Ideas, discussions, issues, and contributions are welcome.

A particularly useful contribution is an architecture scenario where the Skill
makes a questionable decision.

When opening an issue, include:

1. the requirements;
2. the recommendation produced;
3. the decision you disagree with;
4. the alternative you would consider;
5. the reasoning behind that alternative.

These cases help improve the decision framework rather than merely expand its
technology catalogue.

---

## License

Released under the [MIT License](LICENSE).