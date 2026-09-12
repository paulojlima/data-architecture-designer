---
name: data-architecture-designer
description: Design and review modern data architectures by translating business requirements, technical constraints, and quality attributes into justified architectural decisions and trade-offs.
---

# Data Architecture Designer

Design data architectures through explicit architectural reasoning rather
than defaulting to a predefined technology stack.

The objective is not merely to produce an architecture diagram or a list
of technologies. The objective is to produce an architecture whose major
decisions can be traced back to requirements, constraints, quality
attributes, and trade-offs.

## When to use this skill

Use this skill when the user asks to:

- design a new data architecture;
- define a target architecture for a data platform;
- modernize or migrate an existing data platform;
- compare architectural approaches;
- review an existing data architecture;
- select appropriate architectural patterns;
- evaluate technology choices for a data platform.

## Core principles

Always follow these principles:

1. **Requirements before technology**
   Do not select technologies before understanding the problem.

2. **Architecture before products**
   Define architectural capabilities and patterns before mapping them
   to vendor-specific services.

3. **Make trade-offs explicit**
   Every significant architectural decision has benefits and costs.

4. **Avoid unnecessary complexity**
   Do not introduce distributed systems, streaming, multiple engines,
   or additional platform components unless requirements justify them.

5. **Separate facts from assumptions**
   Clearly identify missing information and assumptions.

6. **Remain vendor-neutral by default**
   Do not default to Azure, AWS, GCP, Databricks, Snowflake, Fabric,
   or another platform unless requirements or constraints justify it.

7. **Design for operability**
   Consider observability, data quality, governance, security,
   maintainability, and cost as part of the architecture.

## Architecture workflow

Follow the workflow below.

### 1. Understand the business context

Identify:

- business objectives;
- primary use cases;
- consumers of the data;
- expected business outcomes;
- regulatory or organizational constraints.

Do not begin technology selection yet.

### 2. Characterize the data

Determine where possible:

- source systems;
- data formats;
- approximate volume;
- velocity and ingestion frequency;
- data variety;
- retention requirements;
- historical requirements;
- expected growth.

### 3. Identify non-functional requirements

Evaluate the relevant quality attributes, including:

- scalability;
- availability;
- reliability;
- latency;
- security;
- privacy;
- governance;
- lineage;
- data quality;
- observability;
- maintainability;
- interoperability;
- cost efficiency.

Consult:

`references/quality-attributes.md`

### 4. Identify constraints

Determine constraints such as:

- cloud or on-premises requirements;
- existing technology ecosystem;
- team skills;
- budget;
- licensing;
- regulatory requirements;
- legacy dependencies;
- delivery timeline;
- vendor restrictions.

### 5. Select architectural patterns

Evaluate suitable architectural patterns based on the information gathered.

Examples may include:

- Data Warehouse;
- Data Lake;
- Lakehouse;
- Medallion Architecture;
- Lambda Architecture;
- Kappa Architecture;
- Event-Driven Architecture;
- Data Mesh;
- Hybrid architectures.

Consult:

`references/architecture-patterns.md`

Do not select a pattern simply because it is popular.

### 6. Make architectural decisions

For each significant decision:

1. State the decision.
2. Explain the requirement or constraint driving it.
3. Identify viable alternatives.
4. Explain the trade-offs.
5. State why the recommended option is preferred.

Use the framework defined in:

`references/decision-framework.md`

### 7. Design the logical architecture

Describe the required capabilities and layers.

Consider where applicable:

- data sources;
- ingestion;
- messaging/event streaming;
- raw storage;
- processing and transformation;
- curated storage;
- semantic or serving layer;
- APIs;
- analytics and BI;
- AI/ML consumption;
- metadata and catalog;
- governance and lineage;
- security and access control;
- orchestration;
- monitoring and observability.

Not every architecture requires every layer.

### 8. Map capabilities to technologies

Only after the logical architecture has been established, propose
appropriate technologies.

For every important technology choice:

- explain why it fits;
- mention relevant alternatives;
- identify vendor lock-in implications where applicable;
- avoid adding technologies without a clear architectural purpose.

### 9. Identify risks

Highlight relevant risks such as:

- unnecessary architectural complexity;
- scalability bottlenecks;
- single points of failure;
- poor data quality;
- governance gaps;
- excessive operational overhead;
- uncontrolled cloud costs;
- vendor lock-in;
- skills gaps;
- migration complexity.

### 10. Produce the architecture output

Structure the final response using:

`template/architecture-output.md`

The output should make the reasoning behind the architecture understandable
to both technical stakeholders and engineering teams.

## Handling missing information

Do not silently invent requirements.

If critical information is missing:

- identify what is unknown;
- state reasonable assumptions when progress is still possible;
- explain how those assumptions affect the architecture;
- ask focused questions when the missing information could materially
  change the architectural decision.

## Architecture quality check

Before presenting the final recommendation, verify:

- Can every major component be justified by a requirement?
- Are unnecessary components present?
- Are assumptions explicitly identified?
- Are important quality attributes addressed?
- Are security and governance considered?
- Are operational concerns considered?
- Are technology choices justified?
- Are alternatives and trade-offs visible?
- Could a simpler architecture satisfy the same requirements?

If the answer to the last question is yes, prefer the simpler architecture.
