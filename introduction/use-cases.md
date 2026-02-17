---
description: >-
  Rierino can boost development of any large scale backend application, with a
  state-of-the-art microservices architecture
icon: building-memo
---

# Use Cases

Rierino is a backend development platform for many modules and systems.

It is most useful when you need one or more of these traits:

* **Large scale operations.** Handle millions of requests with millisecond latency.
* **Dynamic business environment.** Ship frequent changes without heavy rewrites.
* **Automation needs.** Execute critical decisions and processes at high volume.
* **Data-heavy domains.** Turn data into real-time actions and insights.
* **Modernization.** Reduce monolith debt with incremental migration to services.
* **Many integrations.** Connect internal and external systems without fragile glue.

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
* [State Managers](../devops/microservices/elements/state-managers/), especially [MongoDB Collection](../devops/microservices/elements/state-managers/shared-states/mongodb-collection.md), for persistence. Use it as the operational store.
* [Runners](../devops/microservices/runners/), especially [CRUD Event Runner](../devops/microservices/runners/deploying-runners/spring-runners.md#crudeventrunner), for REST APIs. Keep reads and writes consistent.
* [API Flows](../devops/api-flows/) and [Queries](../configuration/queries/) for orchestration. Use them for enrichment and advanced reads.

### More advanced elements (frequently used)

These show up when you need derived views and performance layers:

* [Loading Strategies](../devops/microservices/elements/state-managers/loading-strategies.md) & [CDC Systems](../devops/microservices/elements/systems.md#cdc) for CQRS patterns. Use them for cache and read models.
* [Generate Text/Html](../devops/microservices/elements/handlers/core-handlers/generate-text-html.md) handler for server-side rendering. Use it for templates and rich outputs.

## Decision Automation

Rierino includes a flexible business rule engine and ML capabilities. It fits decision automation use cases like recommendations, risk scoring, and pricing.

These use cases combine deterministic rules and ML scoring. They also benefit from tight visibility into outcomes.

### Common scenarios

* Real-time eligibility and policy decisions.
* Next-best-action recommendations at request time.
* Continuous scoring from event streams.

### Typical elements to focus on

* [Apply Business Rules](../devops/microservices/elements/handlers/specialized-handlers/apply-advanced-rules.md) handler and [Business Rules](../configuration/business-rules/) configuration. Use them for rule-based decisions.
* [Score ML Models](../devops/microservices/elements/handlers/ml-and-ai-handlers/use-ml-models.md) handler and [ML Models](../data-science/ml-models/) configuration. Use them for AI-based decisions.
* [Data Visualizations](../data-science/data-visualizations.md) for insights. Use them for explainability and scenario checks.

### More advanced elements (frequently used)

These help when decisions are event-driven and time-sensitive:

* [Kafka Topic](../devops/microservices/elements/streams/kafka-topic.md) and [Samza Runners](../devops/microservices/runners/deploying-runners/samza-runners.md) for real-time streams. Use them for async decisions.
* [Calculate Real-time Metrics](../devops/microservices/elements/handlers/specialized-handlers/calculate-real-time-metrics.md) handler and [Complex Event Processing](../data-science/complex-event-processing/) configuration. Use them for windows and joins.
* [Run Python Procedure](../devops/microservices/elements/handlers/custom-code-handlers/run-python-procedure.md) handler for custom analytics. Use it for bespoke feature logic.

## Business Process Management

Rierino includes flexible authentication and task management. It fits [BPM](https://rierino.com/solutions/bpm) applications like HR, intranets, and ERP modules.

These use cases need secure access and traceable task execution. They often require human-in-the-loop steps.

### Common scenarios

* Approvals with audit trails and escalation.
* Case management with user tasks and SLAs.
* Data capture workflows with strict validation.

### Typical elements to focus on

* [Authenticate](../devops/microservices/elements/handlers/gateway-handlers/authenticate/) handlers and [Gateway Servers](../devops/gateway-and-security/gateway-servers/) for identity. Use them for role-based access.
* [Orchestrate User Task](../devops/microservices/elements/handlers/core-handlers/orchestrate-user-task.md) handler and [Dynamic Editor](../design/user-interface/uis/widgets/object-widgets.md#dynamic-editor) for work. Use them for task screens.
* [Condition](../devops/api-flows/configuring-saga-steps/condition-step/) & [Transform](../devops/api-flows/configuring-saga-steps/transform-step/) steps for control flow. Use them for branching decisions.

### More advanced elements (frequently used)

These are common once processes include files and strict validation:

* [File Systems](../devops/microservices/elements/systems.md#hdfs) and [Media Editors](../design/user-interface/uis/widgets/value-widgets.md#media-editor) for uploading, editing and displaying process documents and media.
* [Validate Event](../devops/microservices/elements/handlers/flow-handlers/validate-event.md) handler and [Data Schema](../design/data-schema/) for validation of user inputs against content structure.
* [Run Scripts](../devops/microservices/elements/handlers/custom-code-handlers/run-scripts.md) handler for implementing customized logic. Use it for edge cases.

## Integration & Orchestration

Rierino includes a microservices orchestration layer. It can incorporate external services and APIs into business flows. It can act as middleware for open APIs and enterprise integrations.

These use cases focus on connecting services reliably. They need transformations, batching, and controlled execution.

### Common scenarios

* Sync master data across systems with consistent mappings.
* Orchestrate multi-step API calls with retries and fan-out.
* Integrate file-based processes into service workflows.

### Typical elements to focus on

* [Call Rest API](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md) and [Call SOAP API](../devops/microservices/elements/handlers/specialized-handlers/call-soap-api.md) handlers for third-party calls. Use them for integrations.
* [Odata Service](../devops/microservices/elements/state-managers/specialized-states/odata-service.md) & [Odata Queries](../configuration/queries/query-platforms/odata-queries.md) for OData sources. Use them for data-backed integration.
* [API Flows](../devops/api-flows/) & [Orchestrate Saga](../devops/microservices/elements/handlers/flow-handlers/orchestrate-saga.md) handler for orchestration. Use them for multi-step flows.
* Handlers such as [Buffer Payloads](../devops/microservices/elements/handlers/flow-handlers/buffer-payloads.md), [Loop Each Entry](../devops/microservices/elements/handlers/flow-handlers/loop-each-entry.md) for control. Use them for batching and fan-out.

### More advanced elements (frequently used)

These help when integrations require extra dependencies or file orchestration:

* [Deployments](../devops/microservices/deployments/) for incorporating custom client libraries for integration.
* [Integrate with Camel](../devops/microservices/elements/handlers/specialized-handlers/integrate-with-camel.md) for integrations with less conventional services.
* [Perform File Operation](../devops/microservices/elements/handlers/core-handlers/perform-file-operation.md) for orchestrating files across different systems.
