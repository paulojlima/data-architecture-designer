# Architecture Quality Attributes

Use this reference to identify the non-functional requirements that can
materially influence data architecture decisions.

Quality attributes should not be treated as a generic checklist.

For each architecture, identify which attributes are **architecturally
significant**, why they matter, and how they influence the design.

When requirements are vague, avoid inventing precise targets. State the
assumption or request clarification when the target could materially change
the architecture.

---

## 1. Performance

### Definition

The ability of the platform to process workloads and serve consumers within
acceptable response times.

### Evaluate

- query response expectations;
- transformation execution windows;
- ingestion throughput;
- concurrent workloads;
- compute-intensive operations;
- workload isolation requirements.

### Architecture implications

Performance requirements may influence:

- compute architecture;
- partitioning;
- indexing;
- caching;
- materialization;
- workload isolation;
- storage layout;
- serving technologies.

### Challenge

Do not optimize for performance without a measurable requirement.

---

## 2. Latency and Data Freshness

### Definition

How quickly data must become available after it is created or changed.

Distinguish between:

- ingestion latency;
- processing latency;
- end-to-end data freshness;
- query latency.

### Evaluate

Ask:

> How old can the data be before it negatively affects the business outcome?

Possible requirements may include:

- daily;
- hourly;
- near real-time;
- seconds;
- sub-second.

### Architecture implications

Latency requirements strongly influence the choice between:

- scheduled batch;
- micro-batch;
- Change Data Capture (CDC);
- event-driven processing;
- streaming.

### Challenge

"Real-time" is not a sufficient requirement.

Quantify the required freshness and identify the business consequence of
missing it.

---

## 3. Scalability

### Definition

The ability of the architecture to handle growth without unacceptable
degradation or redesign.

### Evaluate

Consider growth in:

- data volume;
- event throughput;
- users;
- queries;
- source systems;
- domains;
- workloads;
- data products.

### Architecture implications

Scalability may influence:

- distributed processing;
- storage architecture;
- partitioning;
- compute elasticity;
- workload separation;
- asynchronous processing.

### Challenge

Do not introduce distributed architecture solely because future growth is
possible.

Design for credible growth scenarios.

---

## 4. Availability

### Definition

The proportion of time the data platform or a specific capability must be
available to its consumers.

### Evaluate

Determine:

- required service hours;
- acceptable downtime;
- critical consumers;
- maintenance windows;
- dependency availability.

### Architecture implications

High availability may require:

- redundancy;
- multi-zone deployment;
- failover mechanisms;
- replicated services;
- resilient orchestration.

### Challenge

Not every analytical workload requires 24/7 high availability.

Availability should reflect business criticality.

---

## 5. Reliability and Recoverability

### Definition

The ability of the platform to process data correctly and recover from
failures without unacceptable data loss or corruption.

### Evaluate

Consider:

- retry behavior;
- idempotency;
- checkpointing;
- replay;
- backup and restore;
- disaster recovery;
- partial pipeline failures;
- duplicate processing.

Where relevant, identify:

- Recovery Time Objective (RTO);
- Recovery Point Objective (RPO).

### Architecture implications

Reliability requirements influence:

- pipeline design;
- orchestration;
- transactional behavior;
- event retention;
- backup strategies;
- failure isolation.

---

## 6. Data Quality

### Definition

The degree to which data is fit for its intended use.

### Evaluate

Relevant dimensions may include:

- completeness;
- accuracy;
- validity;
- consistency;
- uniqueness;
- timeliness.

### Architecture implications

Data quality may require:

- validation rules;
- data contracts;
- quality gates;
- quarantine mechanisms;
- reconciliation;
- observability;
- ownership and remediation workflows.

### Principle

Data quality is not only a transformation concern.

Critical data should have explicit ownership and remediation processes.

---

## 7. Security

### Definition

The protection of data, platform components, and access paths from
unauthorized use or modification.

### Evaluate

Consider:

- authentication;
- authorization;
- least privilege;
- encryption in transit;
- encryption at rest;
- secrets management;
- network isolation;
- audit logging;
- privileged access;
- service identities.

### Architecture implications

Security requirements may influence every architectural layer.

Security should not be added only after the data flow has been designed.

---

## 8. Privacy

### Definition

The appropriate handling of personal, sensitive, or regulated information.

### Evaluate

Consider:

- personally identifiable information;
- sensitive personal data;
- consent;
- retention;
- deletion;
- masking;
- anonymization or pseudonymization;
- data residency;
- purpose limitation.

### Architecture implications

Privacy requirements may influence:

- storage location;
- access controls;
- data lifecycle;
- transformation;
- environment separation;
- logging;
- data sharing.

---

## 9. Governance

### Definition

The mechanisms through which data ownership, policies, standards, and
accountability are established and enforced.

### Evaluate

Consider:

- data ownership;
- stewardship;
- classification;
- business glossary;
- policies;
- access governance;
- lifecycle management;
- data product ownership.

### Architecture implications

Governance may require:

- metadata management;
- catalog capabilities;
- policy enforcement;
- ownership workflows;
- access controls;
- data contracts.

### Challenge

A catalog alone is not a governance operating model.

---

## 10. Lineage and Traceability

### Definition

The ability to understand where data originated, how it was transformed,
and where it is consumed.

### Evaluate

Determine whether lineage is required for:

- regulatory compliance;
- audit;
- incident analysis;
- impact analysis;
- data quality investigation;
- change management.

### Architecture implications

Lineage requirements may influence:

- metadata architecture;
- orchestration;
- transformation tooling;
- catalog integration;
- naming and modelling standards.

---

## 11. Observability

### Definition

The ability to understand the operational state and behavior of the data
platform.

### Evaluate

Consider visibility into:

- pipeline execution;
- freshness;
- data volume;
- schema changes;
- data quality;
- failures;
- infrastructure;
- cost;
- downstream impact.

### Architecture implications

Observability may require:

- centralized logging;
- metrics;
- alerts;
- tracing;
- freshness monitoring;
- data quality monitoring;
- operational dashboards.

### Principle

A pipeline that runs successfully can still produce incorrect or stale data.

---

## 12. Maintainability

### Definition

The ease with which the platform can be understood, changed, tested, and
operated over time.

### Evaluate

Consider:

- code complexity;
- modularity;
- documentation;
- testing;
- deployment automation;
- dependency management;
- ownership;
- technology diversity.

### Architecture implications

Maintainability generally favors:

- clear boundaries;
- automation;
- reusable components;
- consistent standards;
- limited technology proliferation.

### Challenge

Flexibility obtained through additional technologies may reduce
maintainability.

---

## 13. Interoperability

### Definition

The ability of systems, platforms, and teams to exchange and use data
reliably.

### Evaluate

Consider:

- APIs;
- file formats;
- table formats;
- event schemas;
- metadata standards;
- data contracts;
- cross-platform access.

### Architecture implications

Interoperability may influence:

- open standards;
- interface design;
- storage formats;
- integration architecture;
- vendor dependency.

---

## 14. Portability and Vendor Lock-In

### Definition

The degree to which workloads, data, or capabilities depend on a specific
vendor or platform.

### Evaluate

Consider:

- proprietary APIs;
- proprietary storage formats;
- proprietary transformation logic;
- egress costs;
- migration complexity;
- specialist skills.

### Principle

Vendor lock-in is not automatically bad.

Managed proprietary capabilities can reduce delivery time and operational
complexity.

The architectural responsibility is to make the trade-off explicit.

---

## 15. Cost Efficiency

### Definition

The ability to meet requirements with an economically sustainable
architecture.

### Evaluate

Consider:

- compute;
- storage;
- data movement;
- network egress;
- licensing;
- idle resources;
- operational support;
- specialist skills;
- duplicated platforms.

Evaluate both:

**Technology cost**

and

**Total cost of ownership (TCO).**

### Challenge

The cheapest infrastructure is not necessarily the cheapest architecture.

Operational complexity has a cost.

---

## 16. Evolvability

### Definition

The ability of the architecture to adapt to credible changes in business,
data, scale, and technology.

### Evaluate

Consider likely changes in:

- source systems;
- consumers;
- data volume;
- regulatory requirements;
- analytical workloads;
- AI/ML workloads;
- organizational structure.

### Challenge

Do not build speculative flexibility for every possible future scenario.

Design for plausible evolution.

---

## 17. Sustainability

### Definition

The ability to use infrastructure and compute resources efficiently while
avoiding unnecessary processing and storage.

### Evaluate

Consider:

- unnecessary recomputation;
- duplicated datasets;
- idle compute;
- inefficient retention;
- over-provisioning;
- workload scheduling.

Sustainability and cost efficiency frequently reinforce each other.

---

## Quality Attribute Prioritization

Do not attempt to maximize every quality attribute simultaneously.

Quality attributes often conflict.

Examples:

| Priority | Potential trade-off |
|---|---|
| Very low latency | Higher cost and complexity |
| Maximum portability | Reduced use of managed proprietary capabilities |
| Maximum availability | Higher infrastructure cost |
| Strong isolation | Increased duplication |
| Maximum flexibility | Reduced simplicity and maintainability |

Identify the attributes that matter most for the specific architecture.

Where appropriate, classify them as:

- **Critical**
- **Important**
- **Desirable**
- **Not architecturally significant**

---

## Quality Attribute Scenario

For architecturally significant attributes, express the requirement as a
scenario whenever possible.

A useful structure is:

```text
Quality Attribute:
Latency

Context:
Customer transactions are generated continuously.

Requirement:
Validated transactions must be available to the fraud detection process
within 10 seconds.

Business impact:
Delays beyond the target reduce the ability to intervene before a
transaction completes.

Architecture influence:
Low-latency ingestion and processing must be evaluated against batch
alternatives.
```

This is more useful than simply stating:

> The platform must have high performance.

---

## Quality Attribute Principle

Do not design for abstract qualities such as:

> scalable, secure, real-time and highly available.

Translate them into requirements that can influence an architectural
decision.

**A quality attribute becomes architecturally useful when its required
behavior and business consequence are understood.**
