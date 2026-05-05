---
description: >-
  Rierino can boost development of any large scale backend heavy application,
  such as information management, decision automation, BPM, agentic operations &
  orchestration
icon: building-memo
---

# Rierino Use Cases

{% hint style="info" icon="magnifying-glass" %}
**In brief:** Rierino is a backend heavy development platform for many modules and systems. It is most useful when you need one or more of these traits:

* **Large scale operations.** Handle millions of requests with millisecond latency.
* **Dynamic business environment.** Ship frequent changes without heavy rewrites.
* **Automation needs.** Execute critical decisions and processes at high volume.
* **Data-heavy domains.** Turn data into real-time actions and insights.
* **Modernization.** Reduce monolith debt with incremental migration to services.
* **Many integrations.** Connect internal and external systems without fragile glue.
{% endhint %}

Rierino keeps teams focused on domain logic. It provides consistent building blocks for services and orchestration. It reduces the operational cost of scaling and integrating.

Typical applications that can be built using Rierino include:

## Information Management

Rierino's flexible data and UI model fits information management well. It works for [PIM](https://rierino.com/solutions/pim), [CMS](https://rierino.com/solutions/cms) and MDM.

These use cases usually center on data entry, enrichment, and publishing. They also need strong validation and fast retrieval at scale.

### Common scenarios

* Manage large catalogs with frequent edits and approvals.
* Enrich records using internal rules and external signals.
* Publish structured content to multiple channels and systems.

### Typical elements to start with

Start with a clean CRUD backbone and an admin UX:

* [Apps](../design/user-interface/apps.md) & [UIs](../design/user-interface/uis/) for admin screens. Use them for listing and editing records.
* [State Managers](../devops/microservices/building-blocks/data-sources/), especially [MongoDB Collection](../devops/microservices/building-blocks/data-sources/shared-states/mongodb-collection.md), for persistence. Use it as the operational store.
* [Runners](../devops/microservices/service-runners/), especially [CRUD Event Runner](../devops/microservices/service-runners/deploying-runners/spring-runners.md#crudeventrunner), for REST APIs. Keep reads and writes consistent.
* [API Flows](../devops/api-event-and-process-flows/) and [Queries](../configuration/queries/) for orchestration. Use them for enrichment and advanced reads.

### More advanced elements (frequently used)

These show up when you need derived views and performance layers:

* [Loading Strategies](../devops/microservices/building-blocks/data-sources/loading-strategies.md) & [CDC Systems](../devops/microservices/building-blocks/systems-integrations/#cdc) for CQRS patterns. Use them for cache and read models.
* [Generate Text/Html](/broken/pages/4agpq23HZi7h24EmlS5j) handler for server-side rendering. Use it for templates and rich outputs.

## Decision Automation

Rierino includes a flexible business rule engine and ML capabilities. It fits decision automation use cases like recommendations, risk scoring, and pricing.

These use cases combine deterministic rules and ML scoring. They also benefit from tight visibility into outcomes.

### Common scenarios

* Real-time eligibility and policy decisions.
* Next-best-action recommendations at request time.
* Continuous scoring from event streams.

### Typical elements to focus on

* [Apply Business Rules](/broken/pages/PAl3ujZHgdLsoCDNTkcP) handler and [Business Rules](../configuration/business-rules/) configuration. Use them for rule-based decisions.
* [Score ML Models](/broken/pages/cNRp2sZlzMyLGcNuHh6y) handler and [ML Models](../data-science/ml-models/) configuration. Use them for AI-based decisions.
* [Data Visualizations](../data-science/data-visualizations.md) for insights. Use them for explainability and scenario checks.

### More advanced elements (frequently used)

These help when decisions are event-driven and time-sensitive:

* [Kafka Topic](../devops/microservices/building-blocks/data-and-event-streams/kafka-topic.md) and [Samza Runners](../devops/microservices/service-runners/deploying-runners/samza-runners.md) for real-time streams. Use them for async decisions.
* [Calculate Real-time Metrics](/broken/pages/DkSrF2iIprUTYDcbgTeJ) handler and [Complex Event Processing](../data-science/complex-event-processing/) configuration. Use them for windows and joins.
* [Run Python Procedure](/broken/pages/PpEVmUyz1AQjjKom7mlO) handler for custom analytics. Use it for bespoke feature logic.

## Business Process Management

Rierino includes flexible authentication and task management. It fits [BPM](https://rierino.com/solutions/bpm) applications like HR, intranets, and ERP modules.

These use cases need secure access and traceable task execution. They often require human-in-the-loop steps.

### Common scenarios

* Approvals with audit trails and escalation.
* Case management with user tasks and SLAs.
* Data capture workflows with strict validation.

### Typical elements to focus on

* [Authenticate](/broken/pages/aLGgdzi8fy5sQg5x9cJZ) handlers and [Gateway Servers](../devops/api-gateway-and-security/gateway-servers/) for identity. Use them for role-based access.
* [Orchestrate User Task](/broken/pages/xzL5715XvDnlRM8mwJ7m) handler and [Dynamic Editor](../design/user-interface/uis/widgets/object-widgets.md#dynamic-editor) for work. Use them for task screens.
* [Condition](../devops/api-event-and-process-flows/configuring-saga-steps/condition-step/) & [Transform](../devops/api-event-and-process-flows/configuring-saga-steps/transform-step/) steps for control flow. Use them for branching decisions.

### More advanced elements (frequently used)

These are common once processes include files and strict validation:

* [File Systems](../devops/microservices/building-blocks/systems-integrations/#hdfs) and [Media Editors](../design/user-interface/uis/widgets/value-widgets.md#media-editor) for uploading, editing and displaying process documents and media.
* [Validate Event](/broken/pages/kJ3kXYeQsHNpcrHrgp2k) handler and [Data Schema](../design/data-schema/) for validation of user inputs against content structure.
* [Run Scripts](/broken/pages/nu3R4vUYzNbUuyXTQKuc) handler for implementing customized logic. Use it for edge cases.

## Integration & Orchestration

Rierino includes a microservices orchestration layer. It can incorporate external services and APIs into business flows. It can act as middleware for open APIs and enterprise integrations.

These use cases focus on connecting services reliably. They need transformations, batching, and controlled execution.

### Common scenarios

* Sync master data across systems with consistent mappings.
* Orchestrate multi-step API calls with retries and fan-out.
* Integrate file-based processes into service workflows.

### Typical elements to focus on

* [Call Rest API](/broken/pages/UhdC9EjYpWaLh8PhyvdB) and [Call SOAP API](/broken/pages/oMG74wG5QWnemFi8qMQ5) handlers for third-party calls. Use them for integrations.
* [Odata Service](../devops/microservices/building-blocks/data-sources/specialized-states/odata-service.md) & [Odata Queries](../configuration/queries/query-platforms/odata-queries.md) for OData sources. Use them for data-backed integration.
* [API Flows](../devops/api-event-and-process-flows/) & [Orchestrate Saga](/broken/pages/oHzAjO7pithj1w5QvAb8) handler for orchestration. Use them for multi-step flows.
* Handlers such as [Buffer Payloads](/broken/pages/Ie4Y5ViXoXGjLjopJZhw), [Loop Each Entry](/broken/pages/69VDPwaKpR3esVnQp8UM) for control. Use them for batching and fan-out.

### More advanced elements (frequently used)

These help when integrations require extra dependencies or file orchestration:

* [Deployments](../devops/microservices/deployment-packages/) for incorporating custom client libraries for integration.
* [Integrate with Camel](/broken/pages/KnQmumvddzfbZ4c0o80B) for integrations with less conventional services.
* [Perform File Operation](/broken/pages/HC0rqt1ZVaVnPiMcwX4P) for orchestrating files across different systems.
