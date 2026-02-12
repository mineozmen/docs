---
description: >-
  Rierino can boost development of any large scale backend application, with a
  state-of-the-art microservices architecture
icon: building-memo
---

# Use Cases

Rierino can be used as a backend development platform for all types of modules and systems, especially when the use case requires one or more of the following:

* Large scale operations - with a need to handle millions of requests in milliseconds latency
* Dynamic business environment - requiring new features or changes frequently
* Need for automation - with high volume of critical business decisions and processes
* Data heavy sector - requiring ability to make sense of data and use analytics in real-time
* Need for modernization - with high technical debt in monolithic architecture requiring revamp
* High level of integrations - with internal & external systems becoming difficult to manage

Rierino has been designed to address each of these requirements, providing tools and capabilities to organizations for focusing on their business domain logic, while taking care of technical complexity in delivering them with the best in class architecture and frameworks.

Typical applications that can be built using Rierino include:

## Information Management

Rierino's flexible data and UI model makes it the perfect platform for building information management applications such as [PIM](https://rierino.com/solutions/pim), [CMS](https://rierino.com/solutions/cms) and MDM.

Typical elements to start with when building these applications include:

* [Apps](../design/user-interface/apps.md) & [UIs](../design/user-interface/uis/) for building data listing and entry forms.
* [State Managers](../devops/microservices/elements/state-managers/), especially [MongoDB Collection](../devops/microservices/elements/state-managers/shared-states/mongodb-collection.md) for storing required data.&#x20;
* [Runners](../devops/microservices/runners/), especially [CRUD Event Runner](../devops/microservices/runners/deploying-runners/spring-runners.md#crudeventrunner) for creating RESTful endpoints to access data.
* [API Flows](../devops/api-flows/) and [Queries](../configuration/queries/) for going beyond basic read / write operations on data.

More advanced elements, which are also frequently used include:

* [Loading Strategies](../devops/microservices/elements/state-managers/loading-strategies.md) & [CDC Systems](../devops/microservices/elements/systems.md#cdc) for implementing CQRS pattern or a cache layer.
* [Generate Text/Html](../devops/microservices/elements/handlers/core-handlers/generate-text-html.md) handler for server side rendering of contents with rich data.

## Decision Automation

With a flexible business rule engine and ML capabilities, Rierino facilitates development of decision automation applications, such as product recommendations, risk scoring and dynamic pricing.

Typical elements to focus on when building these applications include:

* [Apply Business Rules](../devops/microservices/elements/handlers/specialized-handlers/apply-advanced-rules.md) handler and [Business Rules](../configuration/business-rules/) configuration for rule based decisions.
* [Score ML Models](../devops/microservices/elements/handlers/ml-and-ai-handlers/use-ml-models.md) handler and [ML Models](../data-science/ml-models/) configuration for AI based decisions.
* [Data Visualizations](../data-science/data-visualizations.md) for visualizing business insights for decisioning and scenario analysis.

More advanced elements, which are also frequently used include:

* [Kafka Topic](../devops/microservices/elements/streams/kafka-topic.md) and [Samza Runners](../devops/microservices/runners/deploying-runners/samza-runners.md) for consuming and producing event streams as real-time data flows.&#x20;
* [Calculate Real-time Metrics](../devops/microservices/elements/handlers/specialized-handlers/calculate-real-time-metrics.md) handler and [Complex Event Processing](../data-science/complex-event-processing/) configuration.
* [Run Python Procedure](../devops/microservices/elements/handlers/custom-code-handlers/run-python-procedure.md) handler for developing custom analytics modules in Python.

## Business Process Management

With a flexible authentication model and task management capabilities, Rierino offers an ideal platform for building [BPM](https://rierino.com/solutions/bpm) applications, such as HR solutions, intranet applications and ERP modules.

Typical elements to focus on when building these applications include:

* [Authenticate](../devops/microservices/elements/handlers/gateway-handlers/authenticate/) handlers and [Gateway Servers](../devops/gateway-and-security/gateway-servers/) for managing users and role based access.
* [Orchestrate User Task](../devops/microservices/elements/handlers/core-handlers/orchestrate-user-task.md) handler and [Dynamic Editor](../design/user-interface/uis/widgets/object-widgets.md#dynamic-editor) for building workflows and task screens.
* [Condition](../devops/api-flows/configuring-saga-steps/condition-step/) & [Transform](../devops/api-flows/configuring-saga-steps/transform-step/) steps for building customized flows based on user actions and decisions.

More advanced elements, which are also frequently used include:

* [File Systems](../devops/microservices/elements/systems.md#hdfs) and [Media Editors](../design/user-interface/uis/widgets/value-widgets.md#media-editor) for uploading, editing and displaying process documents and media.
* [Validate Event](../devops/microservices/elements/handlers/flow-handlers/validate-event.md) handler and [Data Schema](../design/data-schema/) for validation of user inputs against content structure.
* [Run Scripts](../devops/microservices/elements/handlers/custom-code-handlers/run-scripts.md) handler for implemeting highly customized business logic with coding flexibility.

## Integration & Orchestration

Rierino platform includes a microservices orchestration layer, with the ability to incorporate external services and APIs seamlessly into business flows, making it possible to use it as a solution for open API and enterprise services integration initiatives, acting as a middleware.

Typical elements to focus on when building these applications include:

* [Call Rest API](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md) and [Call SOAP API](../devops/microservices/elements/handlers/specialized-handlers/call-soap-api.md) handlers for 3rd party API integrations.
* [Odata Service](../devops/microservices/elements/state-managers/specialized-states/odata-service.md) & [Odata Queries](../configuration/queries/query-platforms/odata-queries.md) for using Odata APIs as data sources.
* [API Flows](../devops/api-flows/) & [Orchestrate Saga](../devops/microservices/elements/handlers/flow-handlers/orchestrate-saga.md) handler for building integration flows.
* Handlers such as [Buffer Payloads](../devops/microservices/elements/handlers/flow-handlers/buffer-payloads.md), [Loop Each Entry](../devops/microservices/elements/handlers/flow-handlers/loop-each-entry.md) for controlling flow logic.

More advanced elements, which are also frequently used include:

* [Deployments](../devops/microservices/deployments/) for incorporating custom client libraries for integration.
* [Integrate with Camel](../devops/microservices/elements/handlers/specialized-handlers/integrate-with-camel.md) for integrations with less conventional services.
* [Perform File Operation](../devops/microservices/elements/handlers/core-handlers/perform-file-operation.md) for orchestrating files across different systems.
