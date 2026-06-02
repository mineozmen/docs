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
* [Runners](../devops/microservices/service-runners/), especially [Spring Runners](../devops/microservices/service-runners/deploying-runners/spring-runners.md), for REST APIs. Keep reads and writes consistent.
* [API Flows](../devops/api-event-and-process-flows/) and [Queries](../configuration/queries/) for orchestration. Use them for enrichment and advanced reads.

### More advanced elements (frequently used)

These show up when you need derived views and performance layers:

* [Loading Strategies](../devops/microservices/building-blocks/data-sources/loading-strategies.md) and [CDC Feed](../devops/microservices/building-blocks/data-and-event-streams/cdc-feed.md) for CQRS patterns. Use them for cache and read models.
* [Generate Text/Html](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/generate-text-html.md) actions for server-side rendering. Use it for templates and rich outputs.

## Decision Automation

Rierino includes a flexible business rule engine and ML capabilities. It fits decision automation use cases like recommendations, risk scoring, and pricing.

These use cases combine deterministic rules and ML scoring. They also benefit from tight visibility into outcomes.

### Common scenarios

* Real-time eligibility and policy decisions.
* Next-best-action recommendations at request time.
* Continuous scoring from event streams.

### Typical elements to focus on

* [Apply Advanced Rules](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/apply-advanced-rules.md) action and [Business Rules](../configuration/business-rules/) configuration. Use them for rule-based decisions.
* [Use ML Models](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/ml-and-ai-actions/use-ml-models.md) action and [ML Models](../data-science/ml-models/) configuration. Use them for AI-based decisions.
* [Data Visualizations](../data-science/data-visualizations.md) for insights. Use them for explainability and scenario checks.

### More advanced elements (frequently used)

These help when decisions are event-driven and time-sensitive:

* [Kafka Topic](../devops/microservices/building-blocks/data-and-event-streams/kafka-topic.md) and [Samza Runners](../devops/microservices/service-runners/deploying-runners/samza-runners.md) for real-time streams. Use them for async decisions.
* [Calculate Real-time Metrics](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/calculate-real-time-metrics.md) action and [Complex Event Processing](../data-science/complex-event-processing/) configuration. Use them for windows and joins.
* [Run Python Procedure](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-python-procedure.md) action for custom analytics. Use it for bespoke feature logic.

## Business Process Management

Rierino includes flexible authentication and task management. It fits [BPM](https://rierino.com/solutions/bpm) applications like HR, intranets, and ERP modules.

These use cases need secure access and traceable task execution. They often require human-in-the-loop steps.

### Common scenarios

* Approvals with audit trails and escalation.
* Case management with user tasks and SLAs.
* Data capture workflows with strict validation.

### Typical elements to focus on

* [Authenticate](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/) action and [Gateway Servers](../devops/api-gateway-and-security/gateway-servers/) for identity. Use them for role-based access.
* [Orchestrate User Task](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/orchestrate-user-task.md) action and [Object Widgets](../design/user-interface/uis/widgets/object-widgets.md) for work. Use them for task screens.
* [Condition](../devops/api-event-and-process-flows/configuring-saga-steps/condition-step/) & [Transform](../devops/api-event-and-process-flows/configuring-saga-steps/transform-step/) steps for control flow. Use them for branching decisions.

### More advanced elements (frequently used)

These are common once processes include files and strict validation:

* [Perform File Operation](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/perform-file-operation.md) and [Value Widgets](../design/user-interface/uis/widgets/value-widgets.md) for uploading, editing and displaying process documents and media.
* [Validate Event](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/validate-event.md) action and [Data Schema](../design/data-schema/) for validation of user inputs against content structure.
* [Run Scripts](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-scripts.md) action for implementing customized logic. Use it for edge cases.

## Integration & Orchestration

Rierino includes a microservices orchestration layer. It can incorporate external services and APIs into business flows. It can act as middleware for open APIs and enterprise integrations.

These use cases focus on connecting services reliably. They need transformations, batching, and controlled execution.

### Common scenarios

* Sync master data across systems with consistent mappings.
* Orchestrate multi-step API calls with retries and fan-out.
* Integrate file-based processes into service workflows.

### Typical elements to focus on

* [Call Rest API](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/call-rest-api.md) and [Call SOAP API](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/call-soap-api.md) actions for third-party calls. Use them for integrations.
* [Odata Service](../devops/microservices/building-blocks/data-sources/specialized-states/odata-service.md) & [Odata Queries](../configuration/queries/query-platforms/odata-queries.md) for OData sources. Use them for data-backed integration.
* [API Flows](../devops/api-event-and-process-flows/) & [Orchestrate Saga](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/orchestrate-saga.md) action for orchestration. Use them for multi-step flows.
* Actions such as [Buffer Payloads](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/buffer-payloads.md), [Loop Each Entry](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/loop-each-entry.md) for control. Use them for batching and fan-out.

### More advanced elements (frequently used)

These help when integrations require extra dependencies or file orchestration:

* [Deployments](../devops/microservices/deployment-packages/) for incorporating custom client libraries for integration.
* [Integrate with Camel](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/integrate-with-camel.md) for integrations with less conventional services.
* [Perform File Operation](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/perform-file-operation.md) for orchestrating files across different systems.
