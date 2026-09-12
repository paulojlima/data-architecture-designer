# Example — Batch Analytics Platform

This example demonstrates how the Data Architecture Designer should approach
a relatively simple analytics requirement.

The objective is to validate that architectural complexity remains
proportional to the actual requirements.

---

## Scenario

A mid-sized retail company wants to modernize its reporting platform.

The organization currently has:

- one ERP system containing orders, products, inventory, and suppliers;
- one CRM containing customer and sales activity;
- several CSV files maintained by business teams;
- approximately 500 GB of historical data;
- around 2 GB of new or changed data per day;
- 50 business intelligence users;
- 5 data analysts;
- no dedicated data science team.

The existing reporting process relies heavily on spreadsheets and manual
data extracts.

---

## Business Objectives

The company wants to:

- centralize analytical data;
- reduce manual spreadsheet preparation;
- create consistent business metrics;
- improve data quality;
- provide self-service BI capabilities;
- retain historical data for trend analysis.

---

## Requirements

### Data Freshness

Business dashboards must be refreshed every morning before 08:00.

There is currently no requirement for real-time or near-real-time analytics.

### Availability

The analytical platform is primarily required during business hours.

Temporary unavailability outside business hours has limited business impact.

### Data Volume

Current historical data:

```text
~500 GB
```

Expected daily change:

```text
~2 GB/day
```

Expected growth is moderate.

### Consumers

Primary consumers:

- BI dashboards;
- business analysts;
- finance;
- sales;
- operations.

### Data Types

Most data is structured.

Sources include:

- relational ERP database;
- CRM API;
- CSV files.

### Governance

The organization requires:

- documented business metrics;
- clear ownership of critical datasets;
- basic lineage from source to reporting;
- role-based access;
- identification of sensitive customer information.

### Team

The data team consists of:

- 2 data engineers;
- 1 BI developer;
- 5 data analysts.

The team has strong SQL skills but limited experience operating distributed
streaming infrastructure.

### Budget

The organization prefers managed cloud services and wants to minimize
operational overhead.

---

## Constraints

- cloud deployment is acceptable;
- managed services are preferred;
- the team is relatively small;
- SQL should remain the primary transformation language where practical;
- implementation should deliver usable business value within three months.

---

## Architecture Questions

Use the Data Architecture Designer Skill to answer:

1. What architecture pattern best fits this scenario?
2. Is a Data Warehouse, Data Lake, or Lakehouse justified?
3. Should ingestion use batch, CDC, or streaming?
4. Is distributed processing necessary?
5. Which capabilities are required for governance and data quality?
6. Which architectural components would represent unnecessary complexity?
7. What technology characteristics should guide product selection?
8. Which decisions should be captured as ADRs?

---

## Expected Reasoning

The Skill should recognize that:

- daily dashboard refresh does not justify streaming;
- current scale does not automatically justify distributed processing;
- predominantly structured BI workloads may be satisfied by a relatively
  simple analytical architecture;
- the small team makes operational simplicity important;
- governance is required, but this does not automatically justify a large
  enterprise governance platform;
- technology should be selected only after the logical architecture is clear.

The Skill should evaluate both:

- a managed analytical Data Warehouse;
- a simple Lakehouse architecture;

and explain whether the additional flexibility of a Lakehouse provides
meaningful value for this scenario.

---

## Architecture Smells to Avoid

The proposed solution should challenge unnecessary introduction of:

- Kafka or equivalent event-streaming infrastructure;
- Kubernetes;
- multiple transformation engines;
- separate batch and streaming pipelines;
- Data Mesh;
- complex distributed processing;
- multiple storage platforms without a clear requirement.

The absence of these technologies is not a limitation if the requirements
do not justify them.

---

## Expected Architectural Direction

A reasonable recommendation is likely to favor:

```text
ERP / CRM / CSV
       |
       v
Managed Batch Ingestion
       |
       v
Central Analytical Storage
       |
       v
SQL Transformation & Data Quality
       |
       v
Curated Business Models
       |
       v
Semantic Layer
       |
       v
BI / Analytics
```

Cross-cutting capabilities should include:

```text
Security
Governance & Metadata
Lineage
Data Quality
Observability
```

This is a likely direction, not a mandatory answer.

The Skill must still evaluate alternatives and justify the recommendation
from the requirements.

---

## Expected Decisions

The architecture should explicitly evaluate decisions such as:

### ADR-001 — Batch vs Streaming Ingestion

Expected direction:

**Batch**

Primary driver:

Daily data freshness requirement.

---

### ADR-002 — Warehouse vs Lakehouse

Expected direction:

**Context-dependent**

The Skill should compare the simplicity of a managed Data Warehouse against
the additional flexibility of a Lakehouse.

It should not assume that Lakehouse is automatically the more modern or
appropriate choice.

---

### ADR-003 — Managed vs Self-Managed Platform Services

Expected direction:

**Managed services**

Primary drivers:

- small engineering team;
- limited operational capacity;
- short delivery timeline.

---

### ADR-004 — Distributed Processing

Expected direction:

**Not initially required**

Primary drivers:

- moderate data volume;
- SQL-oriented workloads;
- preference for operational simplicity.

The architecture should allow this decision to be revisited if workload
characteristics materially change.

---

## Success Criteria

The Skill passes this example if the resulting architecture:

- satisfies the business requirements;
- remains relatively simple;
- explains why streaming is unnecessary;
- does not introduce technologies without justification;
- evaluates Warehouse vs Lakehouse rather than assuming one;
- addresses governance, security, quality, and observability;
- identifies assumptions and open questions;
- documents significant trade-offs;
- produces traceable architectural decisions.

The Skill fails this example if it produces a complex "modern data stack"
primarily because those technologies are fashionable.

---

## Key Lesson

A modern data architecture does not need to be complex.

**Architectural maturity is demonstrated by justified decisions, not by the
number of technologies in the diagram.**
