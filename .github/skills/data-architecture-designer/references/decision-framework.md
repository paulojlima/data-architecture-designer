# Architecture Decision Framework

Use this framework to make significant architectural decisions explicit,
traceable, and defensible.

The goal is not to identify the most modern technology or pattern.
The goal is to identify the simplest architecture that satisfies the
requirements and constraints with acceptable trade-offs.

---

## 1. Decision Inputs

Before making an architectural decision, identify the evidence that
should influence it.

### Business Drivers

Consider:

- business objectives;
- critical use cases;
- expected business value;
- time-to-market;
- regulatory obligations;
- organizational priorities.

### Data Characteristics

Consider:

- data volume;
- data velocity;
- data variety;
- data formats;
- expected growth;
- retention;
- historical requirements;
- data quality characteristics.

### Quality Attributes

Consider:

- performance;
- latency;
- scalability;
- availability;
- reliability;
- security;
- privacy;
- governance;
- lineage;
- observability;
- maintainability;
- interoperability;
- cost efficiency.

### Constraints

Consider:

- existing platforms;
- cloud provider;
- on-premises dependencies;
- team skills;
- budget;
- licensing;
- delivery timeline;
- regulatory constraints;
- vendor restrictions;
- existing contracts.

---

## 2. Classify the Decision

Determine the architectural scope of the decision.

Examples:

- architecture style;
- ingestion strategy;
- storage strategy;
- processing strategy;
- transformation strategy;
- serving strategy;
- orchestration;
- governance;
- security;
- observability;
- technology selection.

This prevents unrelated concerns from being combined into a single
architectural decision.

---

## 3. Define the Decision Question

Express the decision as a question.

Examples:

> Should this workload use batch ingestion or event-driven ingestion?

> Should analytical data be stored in a warehouse, lake, or lakehouse?

> Should transformations execute inside the warehouse or using a
> distributed processing engine?

> Does this use case justify a streaming architecture?

A good decision question should allow multiple viable answers.

---

## 4. Establish Decision Criteria

Identify the criteria that distinguish the alternatives.

Do not apply every criterion to every decision.

Typical criteria include:

| Criterion | Example concern |
|---|---|
| Latency | How quickly must new data become available? |
| Scale | What volume and throughput must be supported? |
| Complexity | How difficult is the solution to build and operate? |
| Reliability | What happens when components fail? |
| Cost | What are the expected infrastructure and operational costs? |
| Skills | Can the existing team operate the solution? |
| Governance | Can data ownership, lineage and policies be enforced? |
| Security | Does the option satisfy access and privacy requirements? |
| Portability | How dependent is the architecture on one vendor? |
| Maintainability | How easily can the solution evolve? |
| Time-to-market | How quickly can the architecture be delivered? |

Prioritize the criteria according to the specific scenario.

---

## 5. Generate Viable Alternatives

Consider at least two realistic alternatives for significant decisions.

Do not create artificial alternatives simply to increase the number
of options.

For each alternative identify:

- description;
- advantages;
- disadvantages;
- operational implications;
- cost implications where relevant;
- constraints or prerequisites.

Include "keep the existing approach" or "do nothing" when it is a
legitimate option.

---

## 6. Evaluate Trade-offs

Compare alternatives against the decision criteria.

Prefer qualitative reasoning when reliable quantitative information
is unavailable.

Example:

| Criterion | Batch | Streaming |
|---|---|---|
| Latency | Minutes/hours | Seconds/sub-seconds |
| Complexity | Low | High |
| Operational overhead | Low | Medium/High |
| Cost | Usually lower | Usually higher |
| Replay/recovery | Simpler | Requires careful design |

Do not convert uncertain assumptions into precise numerical scores.

A scoring model may be used only when:

- criteria are explicitly defined;
- weighting is justified;
- the scores are supported by evidence.

---

## 7. Apply the Complexity Test

Before accepting the preferred alternative, ask:

> Does the requirement genuinely justify this complexity?

Examples:

Do not introduce streaming when hourly batch processing satisfies
the business requirement.

Do not introduce multiple storage engines when one platform can
satisfy the workloads.

Do not introduce Kubernetes solely because it provides flexibility
that the project does not require.

Do not introduce Data Mesh solely because multiple business domains
exist.

Do not introduce distributed processing solely because the dataset
is described as "large".

Complexity must have a requirement behind it.

---

## 8. Make the Decision

Document the recommendation using the following structure.

### Decision

State the recommended approach.

### Drivers

Identify the requirements, quality attributes, and constraints that
led to the decision.

### Alternatives Considered

List realistic alternatives.

### Rationale

Explain why the selected option better satisfies the decision criteria.

### Trade-offs

Explicitly state what is gained and what is sacrificed.

### Consequences

Describe implications for:

- implementation;
- operations;
- skills;
- cost;
- security;
- governance;
- future evolution.

### Confidence

Classify confidence as:

- **High** — requirements and constraints are sufficiently known;
- **Medium** — some assumptions could affect the decision;
- **Low** — important information is missing.

For Medium or Low confidence, identify what additional information
would increase confidence.

---

## 9. Detect Architecture Smells

Challenge the proposed architecture if any of the following appear.

### Technology-first design

A product was selected before the requirements were understood.

### Resume-driven architecture

Technologies appear to have been selected because they are fashionable
or desirable skills rather than because they solve a requirement.

### Premature distribution

Distributed processing or microservices are introduced before scale
requires them.

### Unnecessary real-time

Streaming is selected despite the absence of a meaningful low-latency
business requirement.

### Tool duplication

Multiple technologies perform substantially the same capability
without a clear reason.

### Governance as an afterthought

Catalog, lineage, ownership, security, or data quality are postponed
until after the platform design.

### Missing operational model

The architecture explains how data flows but not how the platform
will be monitored, supported, recovered, and evolved.

### Vendor lock-in without acknowledgement

Vendor-specific capabilities are selected without considering their
long-term consequences.

---

## 10. Architecture Decision Record

For important decisions, produce an Architecture Decision Record (ADR).

Use the following structure:

```text
ADR-[number]: [Decision title]

Status:
Proposed | Accepted | Superseded

Context:
What problem requires a decision?

Decision:
What has been decided?

Drivers:
Which requirements and constraints influenced the decision?

Alternatives considered:
What other realistic approaches were evaluated?

Rationale:
Why was this option selected?

Consequences:
What positive and negative consequences follow from the decision?

Risks:
What could make this decision problematic?

Confidence:
High | Medium | Low

Revisit when:
Which changes in requirements, scale, cost, regulation, or technology
would justify reconsidering this decision?
```

---

## Decision Principle

A good architecture decision is not the one with the most sophisticated
technology.

It is the one that can clearly answer:

**Why is this necessary for this particular problem?**
