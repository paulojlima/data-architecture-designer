# Data Architecture Patterns

Use this reference to evaluate architectural patterns based on requirements,
constraints, quality attributes, and operational trade-offs.

Patterns are not products and should not be selected because they are
popular or associated with a specific vendor.

Multiple patterns may coexist in the same architecture when justified.

---

## 1. Data Warehouse

### Purpose

A Data Warehouse provides structured, curated, and integrated data optimized
primarily for analytics, reporting, and business intelligence.

### Consider when

- analytical workloads are predominantly structured;
- reporting and BI are primary use cases;
- governed business metrics are important;
- SQL is the dominant consumption interface;
- predictable analytical performance is required;
- dimensional or similar analytical modelling is appropriate.

### Advantages

- mature analytical pattern;
- strong SQL and BI ecosystem;
- clear modelling practices;
- relatively simple consumption model;
- strong support for governed reporting.

### Trade-offs

- less natural for unstructured or semi-structured data;
- raw data preservation may require an additional storage layer;
- advanced ML or data science workloads may require complementary services;
- transformations may become tightly coupled to the warehouse platform.

### Avoid or challenge when

- most workloads require raw or unstructured data;
- data science requires direct access to large heterogeneous datasets;
- the warehouse is being introduced without a clear analytical requirement.

---

## 2. Data Lake

### Purpose

A Data Lake provides scalable storage for raw, semi-structured,
unstructured, and structured data.

### Consider when

- raw data must be retained;
- data formats are heterogeneous;
- large-scale historical storage is required;
- multiple processing engines need access to the same data;
- data science or ML workloads require flexible data access;
- schema-on-read is useful.

### Advantages

- flexible storage;
- supports heterogeneous data;
- economical storage at large scale;
- preserves source fidelity;
- decouples storage from processing.

### Trade-offs

- governance can become difficult;
- discoverability may degrade without metadata management;
- uncontrolled ingestion can create a data swamp;
- business consumption often requires additional modelling layers;
- quality enforcement requires explicit design.

### Avoid or challenge when

- the use case is limited to straightforward structured reporting;
- the additional flexibility provides no business or technical value;
- the organization cannot support the required governance model.

---

## 3. Lakehouse

### Purpose

A Lakehouse combines characteristics of data lakes and analytical warehouses,
typically using open or platform-specific table formats over scalable object
storage.

### Consider when

- BI, data engineering, and data science should share a common data foundation;
- structured and semi-structured workloads coexist;
- scalable object storage is desirable;
- ACID-like table capabilities are required over lake storage;
- multiple analytical workloads need access to curated datasets.

### Advantages

- reduces separation between lake and warehouse workloads;
- supports multiple analytical personas;
- scalable storage and compute patterns;
- supports structured analytical tables over object storage;
- can reduce unnecessary data duplication.

### Trade-offs

- operational and conceptual complexity may exceed that of a simple warehouse;
- performance characteristics depend on engine and table design;
- governance remains essential;
- vendor implementations differ significantly;
- "one platform" does not automatically mean one execution engine or one cost model.

### Avoid or challenge when

- a conventional warehouse fully satisfies the requirements;
- the term "lakehouse" is being used primarily as a modernization buzzword;
- the team lacks the skills to operate the additional platform capabilities.

---

## 4. Medallion Architecture

### Purpose

Medallion Architecture organizes data into progressive quality layers,
commonly represented as Bronze, Silver, and Gold.

Typical interpretation:

- **Bronze** — raw or minimally processed data;
- **Silver** — validated, standardized, and integrated data;
- **Gold** — business-ready or consumption-oriented data.

### Consider when

- progressive refinement improves data quality and traceability;
- raw data retention is required;
- multiple downstream consumers need different levels of refinement;
- transformations benefit from clearly separated responsibilities.

### Advantages

- intuitive separation of processing stages;
- supports traceability;
- facilitates data quality controls;
- makes refinement stages explicit.

### Trade-offs

- may introduce unnecessary copies or transformations;
- layer definitions can become ambiguous;
- teams may mechanically create three layers even when fewer are sufficient;
- it does not define the complete architecture.

### Avoid or challenge when

- every dataset is forced through three layers without justification;
- layer boundaries add complexity without improving quality or maintainability;
- it is being treated as a complete enterprise architecture rather than a
  data refinement pattern.

---

## 5. Batch Processing

### Purpose

Batch processing handles data in bounded groups according to a schedule
or triggering condition.

### Consider when

- seconds-level latency is unnecessary;
- hourly, daily, or periodic processing satisfies the business requirement;
- source systems naturally produce extracts;
- simpler operations are preferred;
- workloads can be efficiently grouped.

### Advantages

- comparatively simple;
- easier recovery and replay;
- predictable operational model;
- often lower cost than continuous processing;
- broad tooling support.

### Trade-offs

- data freshness is limited by processing frequency;
- large batches can create processing peaks;
- downstream consumers wait for batch completion.

### Decision principle

**Prefer batch until a business requirement demonstrates that lower latency
creates meaningful value.**

---

## 6. Event-Driven / Streaming Architecture

### Purpose

Streaming architectures continuously process events or data changes with
low latency.

### Consider when

- decisions depend on fresh data within seconds or minutes;
- event-driven business processes are required;
- continuous monitoring is necessary;
- high-frequency telemetry or events must be processed;
- change data capture must propagate rapidly;
- real-time operational analytics has measurable value.

### Advantages

- low data latency;
- supports reactive systems;
- enables continuous processing;
- can decouple producers and consumers.

### Trade-offs

- higher operational complexity;
- event ordering and duplication must be handled;
- replay and recovery require careful design;
- observability becomes more important;
- cost can increase;
- debugging distributed event flows can be difficult.

### Avoid or challenge when

- "real-time" has not been quantified;
- hourly or daily processing satisfies the business requirement;
- streaming is selected only because the technology is available;
- the team cannot operate the additional infrastructure reliably.

### Key question

> What business outcome becomes materially worse if this data arrives
> 30 minutes later?

If there is no convincing answer, challenge the need for streaming.

---

## 7. Lambda Architecture

### Purpose

Lambda Architecture combines separate batch and speed processing paths
to support both comprehensive historical processing and low-latency results.

### Consider when

- both low-latency and batch recomputation are genuine requirements;
- historical recomputation is essential;
- the organization can justify operating separate processing paths.

### Advantages

- supports low-latency and comprehensive batch views;
- historical data can be recomputed;
- provides resilience through separate processing approaches.

### Trade-offs

- duplicated processing logic;
- significant operational complexity;
- results from batch and speed layers may diverge;
- increased maintenance burden.

### Avoid or challenge when

- one processing model can satisfy both requirements;
- modern streaming/reprocessing capabilities remove the need for separate paths;
- the organization cannot justify maintaining duplicate logic.

---

## 8. Kappa Architecture

### Purpose

Kappa Architecture treats the event stream as the primary processing model
and avoids maintaining separate batch and streaming pipelines.

### Consider when

- events are the natural source of truth;
- streaming is a core architectural requirement;
- historical processing can be achieved through event replay;
- the platform has mature streaming capabilities.

### Advantages

- avoids separate batch and speed implementations;
- consistent event-processing model;
- historical reprocessing can use the same logic.

### Trade-offs

- depends heavily on reliable event retention and replay;
- streaming expertise is required;
- some batch-oriented workloads may become unnecessarily complicated;
- debugging and operations remain complex.

### Avoid or challenge when

- most workloads are naturally batch-oriented;
- low latency is not a genuine requirement;
- event replay cannot reliably reconstruct required state.

---

## 9. Event-Driven Architecture

### Purpose

Event-Driven Architecture structures interactions around the production,
distribution, and consumption of business or technical events.

It is broader than stream analytics: events may trigger workflows,
integration, state changes, or downstream processing.

### Consider when

- systems need loose temporal coupling;
- multiple consumers react independently to events;
- business processes naturally generate meaningful events;
- asynchronous integration improves resilience or scalability.

### Advantages

- decouples producers and consumers;
- supports extensibility;
- enables asynchronous workflows;
- new consumers can often be added without changing producers.

### Trade-offs

- eventual consistency may be introduced;
- end-to-end tracing becomes harder;
- event contracts require governance;
- duplicate or out-of-order events must be considered;
- distributed workflows complicate troubleshooting.

### Avoid or challenge when

- synchronous request/response is simpler and sufficient;
- events do not represent meaningful domain occurrences;
- the architecture introduces a broker merely to connect two simple systems.

---

## 10. Data Mesh

### Purpose

Data Mesh is a socio-technical approach that decentralizes data ownership
toward business domains while maintaining federated governance and treating
data as a product.

Core principles commonly include:

- domain-oriented ownership;
- data as a product;
- self-service data platform capabilities;
- federated computational governance.

### Consider when

- the organization contains genuinely autonomous business domains;
- centralized data teams have become a scaling bottleneck;
- domains can own data products throughout their lifecycle;
- organizational governance can support federated ownership;
- platform capabilities enable domain teams without duplicating infrastructure.

### Advantages

- aligns data ownership with domain knowledge;
- can reduce central-team bottlenecks;
- encourages explicit data product ownership;
- can scale organizational responsibility.

### Trade-offs

- significant organizational change;
- duplicated capabilities may emerge;
- governance becomes more complex;
- domain maturity varies;
- requires strong platform engineering;
- ownership without accountability can worsen data quality.

### Avoid or challenge when

- the organization is small;
- domains cannot realistically own and operate data products;
- the primary problem is simply poor tooling;
- "Data Mesh" is being used as another name for decentralized storage;
- organizational change is not supported.

### Key principle

**Data Mesh is primarily an organizational and operating-model decision,
not a technology product.**

---

## 11. Centralized Data Platform

### Purpose

A centralized data platform consolidates core data capabilities and
responsibility within a common platform and often a central data team.

### Consider when

- organizational scale is manageable;
- governance consistency is a priority;
- shared capabilities provide meaningful economies of scale;
- domain teams lack specialist data engineering capabilities;
- centralized ownership does not create an unacceptable delivery bottleneck.

### Advantages

- consistent standards;
- simpler governance;
- centralized expertise;
- reduced duplication;
- potentially simpler operations.

### Trade-offs

- central teams can become bottlenecks;
- domain context may be lost;
- prioritization across business areas can become difficult;
- ownership may become disconnected from data producers.

---

## 12. Hybrid / Federated Architecture

### Purpose

A hybrid or federated architecture combines centralized platform capabilities
with decentralized ownership or specialized workloads.

### Consider when

- different domains have materially different requirements;
- common governance and platform capabilities should remain centralized;
- some workloads require specialized technology;
- organizational boundaries prevent complete centralization.

### Advantages

- balances standardization and autonomy;
- allows specialized workloads;
- supports gradual organizational evolution.

### Trade-offs

- boundaries must be clearly defined;
- governance can become ambiguous;
- duplicated capabilities may appear;
- interoperability standards become important.

### Decision principle

Do not use "hybrid" as a justification for avoiding architectural decisions.
Clearly identify which capabilities are centralized, which are decentralized,
and why.

---

## 13. Modern Data Warehouse vs Lakehouse

Do not assume one is inherently more modern or superior.

### Favor a Data Warehouse when

- workloads are predominantly structured analytics;
- BI and SQL are primary consumption patterns;
- simplicity and governed reporting dominate;
- advanced heterogeneous workloads are limited.

### Favor a Lakehouse when

- heterogeneous data types matter;
- data science and engineering share the platform;
- raw and curated data need a common foundation;
- scalable object storage is strategically important;
- multiple processing patterns are justified.

### Consider both when

distinct workloads genuinely benefit from different serving or execution
characteristics.

Avoid duplicating data platforms simply to satisfy terminology or vendor
positioning.

---

## 14. Pattern Composition

Real-world architectures frequently combine patterns.

Example:

```text
Operational Systems
        |
        v
Batch + CDC Ingestion
        |
        v
Lakehouse
  |     |      |
 Raw  Curated  Serving
        |
        +----> BI / Analytics
        |
        +----> Data Science / AI

Event Sources
        |
        v
Event Streaming
        |
        +----> Operational Consumers
        |
        +----> Lakehouse
```

This does not mean every architecture should contain batch, CDC,
streaming, a lakehouse, BI, and AI.

Each component must still pass the complexity test.

---

## 15. Pattern Selection Checklist

Before recommending a pattern, ask:

- Which business requirement does this pattern satisfy?
- Which quality attribute does it improve?
- What complexity does it introduce?
- Can a simpler pattern satisfy the same requirement?
- Does the organization have the skills to operate it?
- How does it affect governance and security?
- What are the cost implications?
- Does it introduce significant vendor dependency?
- What happens when the system fails?
- How will data be replayed or recovered?
- How will the architecture evolve if scale changes?

---

## Pattern Principle

Do not ask:

> Which architecture pattern is the most modern?

Ask:

> Which is the simplest combination of patterns that satisfies the
> requirements, constraints, and quality attributes of this system?
