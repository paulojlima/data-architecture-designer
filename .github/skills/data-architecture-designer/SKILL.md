---
name: data-architecture-designer
description: Design and review modern data architectures by translating business requirements, data characteristics, constraints, and quality attributes into justified architectural decisions, alternatives, and trade-offs. Use for new data platforms, target architectures, modernization, architecture reviews, and decisions such as batch vs streaming, warehouse vs lakehouse, centralized vs federated, and technology selection.
---

# Data Architecture Designer

Design data architectures through explicit architectural reasoning rather
than defaulting to a predefined technology stack.

The objective is not merely to produce an architecture diagram or a list
of technologies.

The objective is to produce an architecture whose major decisions can be
traced back to:

- business requirements;
- data characteristics;
- quality attributes;
- constraints;
- alternatives;
- trade-offs.

The preferred architecture is the simplest architecture that satisfies the
requirements with acceptable trade-offs.

---

## Mandatory Execution Rules

When this skill is activated, you MUST:

1. Read and apply:
   - `references/decision-framework.md`
   - `references/architecture-patterns.md`
   - `references/quality-attributes.md`
   - `template/architecture-output.md`

2. Establish the logical architecture before recommending vendor-specific
   technologies or products.

3. Follow the required reasoning order defined in this skill.

4. Structure a full architecture recommendation using
   `template/architecture-output.md`.

5. For every major architectural component, be able to identify the
   requirement, constraint, or quality attribute that justifies it.

6. For every significant technology recommendation, explicitly identify:
   - the logical capability it implements;
   - the requirement or constraint that justifies it;
   - realistic alternatives;
   - the important trade-off introduced.

7. Apply the Complexity Test before adding:
   - streaming;
   - distributed processing;
   - Kubernetes;
   - multiple processing engines;
   - multiple serving engines;
   - multiple storage platforms;
   - Data Mesh;
   - additional orchestration or integration layers.

8. If two technologies provide overlapping capabilities, explicitly justify
   why both are required.

9. Do not silently invent:
   - requirements;
   - data volumes;
   - latency targets;
   - availability targets;
   - cost assumptions;
   - regulatory obligations;
   - team capabilities;
   - operational constraints.

10. Do not use terms such as "modern", "enterprise-grade", "cloud-native",
    "scalable", "real-time", or "best practice" as architectural justification
    by themselves.

---

## Required Reasoning Order

For a full architecture design, follow this order:

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

Do not reverse this process by selecting products first and constructing
architectural justification afterwards.

For narrower requests, such as reviewing one architecture decision, apply
only the relevant stages while preserving the same reasoning principles.

---

## When to Use This Skill

Use this skill when the user asks to:

- design a new data architecture;
- define a target architecture for a data platform;
- modernize or migrate an existing data platform;
- review an existing data architecture;
- compare architectural approaches;
- select appropriate data architecture patterns;
- evaluate architecture alternatives;
- evaluate technology choices for a data platform;
- reason about batch vs streaming;
- reason about Data Warehouse vs Lakehouse;
- reason about centralized vs federated data architectures;
- identify unnecessary architectural complexity;
- document significant data architecture decisions.

---

## Core Principles

### Requirements Before Technology

Do not select technologies before understanding the problem they need to
solve.

A product name is not an architectural requirement.

---

### Architecture Before Products

Define capabilities and architectural relationships before mapping them to
vendor-specific services.

For example, establish that the architecture requires:

```text
Low-latency event ingestion
Stream processing
Long-term analytical storage
Governed analytical serving
Metadata and lineage
```

before deciding whether those capabilities should be implemented using
specific Azure, AWS, GCP, Databricks, Snowflake, Microsoft Fabric, or other
services.

---

### Make Trade-offs Explicit

Every significant architectural decision has benefits and costs.

Do not present an option as universally superior.

Explain what is gained, what is sacrificed, and why the trade-off is
acceptable for the specific problem.

---

### Simplicity Is an Architectural Quality

Do not introduce distributed systems, streaming, additional engines, or
platform components unless requirements justify them.

Complexity has costs in:

- operations;
- reliability;
- observability;
- security;
- skills;
- testing;
- troubleshooting;
- cloud consumption;
- long-term maintainability.

---

### Separate Facts From Assumptions

Clearly distinguish:

- confirmed requirements;
- assumptions;
- unknowns;
- recommendations.

If an assumption could materially change the architecture, make that
explicit.

---

### Vendor-Neutral Reasoning, Constraint-Aware Selection

Remain vendor-neutral during logical architecture design.

A vendor or cloud preference may be a legitimate constraint.

For example:

```text
Azure is the strategic cloud platform.
```

This should influence technology mapping.

It should not eliminate architectural reasoning or justify unnecessary
Azure services.

---

### Design for Operability

Architecture is not complete when data reaches storage.

Consider where relevant:

- security;
- privacy;
- data quality;
- governance;
- lineage;
- observability;
- recovery;
- maintainability;
- ownership;
- cost.

---

## Architecture Workflow

### 1. Understand the Business Context

Identify:

- business objectives;
- primary use cases;
- consumers;
- business outcomes;
- criticality;
- regulatory or organizational drivers.

Ask:

> What business capability is this architecture intended to enable?

Do not begin technology selection at this stage.

---

### 2. Characterize the Data

Determine where possible:

- source systems;
- interfaces;
- data formats;
- historical volume;
- daily or event volume;
- velocity;
- ingestion frequency;
- data variety;
- retention;
- growth expectations;
- change patterns.

Do not assume that large historical volume automatically requires
distributed processing.

Do not use average event throughput alone when peak throughput is
architecturally significant.

---

### 3. Identify Architecturally Significant Quality Attributes

Read:

`references/quality-attributes.md`

Evaluate relevant attributes such as:

- latency and freshness;
- performance;
- scalability;
- availability;
- reliability and recoverability;
- security;
- privacy;
- governance;
- lineage;
- data quality;
- observability;
- maintainability;
- interoperability;
- portability;
- cost efficiency;
- evolvability.

Do not treat every quality attribute as equally important.

Classify relevant attributes as:

- **Critical**
- **Important**
- **Desirable**
- **Not architecturally significant**

Translate vague requirements into architectural scenarios where possible.

For example, do not accept:

> Data must be real-time.

Prefer:

> Fraud-relevant events must be available for evaluation within 15 seconds
> because later detection reduces the ability to intervene.

---

### 4. Identify Constraints

Determine constraints such as:

- strategic cloud platform;
- on-premises dependencies;
- existing technology ecosystem;
- team skills;
- budget;
- licensing;
- regulation;
- data residency;
- legacy systems;
- delivery timeline;
- vendor restrictions;
- operational capacity.

Distinguish a genuine constraint from a technology preference.

---

### 5. Evaluate Candidate Architecture Patterns

Read:

`references/architecture-patterns.md`

Evaluate patterns based on the requirements already identified.

Possible patterns include:

- Data Warehouse;
- Data Lake;
- Lakehouse;
- Medallion Architecture;
- Batch Processing;
- CDC;
- Streaming;
- Event-Driven Architecture;
- Lambda Architecture;
- Kappa Architecture;
- Data Mesh;
- centralized architectures;
- federated architectures;
- hybrid approaches.

Do not select a pattern because it is fashionable or described as modern.

Multiple patterns may coexist when distinct requirements justify them.

For example, a platform may legitimately use:

```text
Batch processing for regulatory reporting
+
Streaming for fraud detection
```

without making the entire platform streaming-oriented.

---

### 6. Design the Logical Architecture

Define architectural capabilities before technologies.

Use capability names at this stage.

Examples:

```text
Operational Sources
        ↓
Batch / Incremental Ingestion
        ↓
Historical Analytical Storage
        ↓
Transformation & Data Quality
        ↓
Curated Analytical Models
        ↓
Semantic / Serving Layer
        ↓
BI & Analytics
```

A separate low-latency path might be:

```text
Event Sources
        ↓
Event Ingestion
        ↓
Stream Processing
        ↓
Fraud Evaluation
        ↓
Operational Action
```

Cross-cutting capabilities may include:

```text
Security
Privacy
Governance
Metadata
Lineage
Data Quality
Observability
```

Only include capabilities justified by the scenario.

At this stage, avoid product names.

---

### 7. Evaluate Alternatives and Trade-offs

Read:

`references/decision-framework.md`

For every significant architecture decision:

1. State the decision question.
2. Identify credible alternatives.
3. Define the criteria that matter.
4. Evaluate alternatives against those criteria.
5. Explain positive and negative consequences.
6. Select the simplest option that satisfies the requirements.
7. State the confidence level.
8. Define when the decision should be revisited.

Do not manufacture unrealistic alternatives merely to make the decision
appear rigorous.

---

## Complexity Test

Apply this test before technology mapping.

For each candidate component or pattern, ask:

### Streaming

What requirement cannot be satisfied with batch, incremental batch, or CDC?

### Distributed Processing

What workload characteristic requires distributed processing?

Consider:

- volume;
- peak throughput;
- processing window;
- transformation complexity;
- concurrency.

### Kubernetes

What deployment or operational requirement requires a general-purpose
container orchestration platform?

If a managed service satisfies the requirement with less operational
overhead, challenge Kubernetes.

### Multiple Processing Engines

What distinct workload requires each engine?

Do not add multiple engines simply because each is technically capable.

### Multiple Serving Engines

Why are multiple analytical serving technologies required?

If they overlap, determine whether one can be removed.

### Multiple Storage Platforms

What requirement requires each persistent storage technology?

Avoid unnecessary duplication.

### Data Mesh

What organizational scaling or ownership problem requires decentralized
domain-oriented data ownership?

Do not use Data Mesh as another term for a modern data platform.

### Additional Integration Layers

What capability is missing from the existing architecture?

Avoid adding messaging, orchestration, APIs, or integration services without
a concrete requirement.

If a component fails the Complexity Test, remove it.

---

### 8. Map Logical Capabilities to Technologies

Only now map capabilities to technologies.

Technology mapping must preserve the logical architecture.

Use a structure such as:

| Logical Capability | Recommended Technology | Alternatives | Requirement / Constraint | Trade-off |
|---|---|---|---|---|

For every major technology choice:

- identify the capability;
- identify the driver;
- compare credible alternatives;
- explain why the recommendation fits;
- identify operational implications;
- identify vendor dependency where relevant.

Do not add products simply because they belong to the selected cloud
ecosystem.

---

### 9. Identify Risks

Highlight architecture-specific risks such as:

- unnecessary complexity;
- scalability bottlenecks;
- peak-load uncertainty;
- poor data quality;
- governance gaps;
- lineage gaps;
- security exposure;
- excessive operational overhead;
- uncontrolled cloud cost;
- vendor lock-in;
- team skill gaps;
- migration complexity;
- unclear ownership.

Where possible, provide a mitigation.

---

### 10. Document Architecture Decisions

For significant decisions, create Architecture Decision Records.

Use the ADR structure in:

`references/decision-framework.md`

Significant decisions may include:

- Batch vs Streaming;
- Warehouse vs Lakehouse;
- Managed vs Self-Managed;
- Centralized vs Federated;
- Single vs Multiple Processing Engines;
- Storage format;
- Serving architecture;
- orchestration strategy.

Do not create ADRs for trivial implementation details.

---

### 11. Produce the Final Architecture Recommendation

Read and use:

`template/architecture-output.md`

For a full architecture design, the response should include, where relevant:

1. Executive Summary
2. Context and Business Drivers
3. Requirements and Assumptions
4. Architecturally Significant Quality Attributes
5. Recommended Architecture
6. Logical Architecture
7. Component Responsibilities
8. Technology Mapping
9. Data Flows
10. Security and Privacy
11. Governance, Metadata and Data Quality
12. Observability and Operations
13. Architectural Decisions
14. ADRs
15. Alternatives Considered
16. Trade-offs
17. Risks and Mitigations
18. Implementation Roadmap, when useful
19. Validation Checklist
20. Final Recommendation

Adapt the level of detail to the problem.

Do not omit architectural reasoning merely to make the answer shorter.

---

## Handling Missing Information

Do not silently fill important gaps.

If information is missing but an architecture can still be proposed:

1. state the assumption;
2. explain why it was necessary;
3. explain how changing it could affect the architecture.

If the missing information could fundamentally change the recommendation,
ask a focused question before making that decision.

Examples include:

- required latency is unknown when deciding batch vs streaming;
- peak event throughput is unknown when sizing an event platform;
- data residency is unknown for regulated workloads;
- recovery objectives are unknown for business-critical operational systems.

---

## Architecture Quality Gate

Before presenting the final recommendation, verify all of the following:

- [ ] Business objectives are explicit.
- [ ] Data characteristics are understood or assumptions are stated.
- [ ] Architecturally significant quality attributes are identified.
- [ ] Constraints are separated from preferences.
- [ ] Logical architecture was defined before technology mapping.
- [ ] Every major component has a requirement or constraint that justifies it.
- [ ] Credible alternatives were considered.
- [ ] Major trade-offs are explicit.
- [ ] The Complexity Test was applied.
- [ ] Overlapping technologies are justified or removed.
- [ ] Security and privacy are addressed where relevant.
- [ ] Governance, lineage and data quality are addressed where relevant.
- [ ] Operational concerns are considered.
- [ ] Technology choices are traceable to logical capabilities.
- [ ] Risks and assumptions are visible.
- [ ] Significant decisions are captured as ADRs.
- [ ] A simpler architecture could not satisfy the same requirements with
      acceptable trade-offs.

If the final check fails, simplify the architecture before presenting it.

---

## Anti-Patterns

Challenge these behaviors explicitly:

### Resume-Driven Architecture

Selecting technologies because they are interesting, fashionable, or useful
for a CV and then searching for requirements to justify them.

### Vendor Catalogue Architecture

Representing an architecture as a collection of services from one cloud
provider without demonstrating why each service exists.

### Diagram-Driven Complexity

Adding components because a more complex diagram appears more architectural.

### Real-Time by Default

Using streaming because low latency is technically possible rather than
because the business requires it.

### Distributed by Default

Using distributed processing because the dataset is described as large
without evaluating workload characteristics.

### Governance by Product

Assuming that deploying a catalog or governance tool creates ownership,
accountability, or data governance.

### Future-Proofing Without Evidence

Adding substantial complexity for hypothetical future requirements without
credible evidence that they are likely.

---

## Final Principle

A good architecture decision is not the one with the most sophisticated
technology.

It is the one that can clearly answer:

**Why is this necessary for this particular problem?**

Complexity is acceptable.

**Unjustified complexity is not.**