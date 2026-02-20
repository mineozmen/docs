---
description: >-
  Rierino is a low-code microservices and AI agent development platform with an
  architecture designed for scale and flexibility
icon: circle-info
---

# Overview

Rierino is a low-code backend platform for building microservices, orchestration flows, and AI agents. You assemble services from configuration-first building blocks, then deploy them across environments as APIs, async triggers, and workflows. It also includes an internal Admin UI builder plus plug-in adapters for data stores, streaming, and external integrations. The goal is fast iteration at scale, without losing governance.

## At a glance

* Build microservices with configuration-first building blocks.
* Orchestrate services into APIs, async triggers, and workflows.
* Use pluggable storage, query, and integration adapters.
* Add rules, ML scoring, and AI-assisted operations where needed.

The following table compares [Rierino Core](https://rierino.com/platform/core) against different categories of traditional low code development platforms, along with the core capabilities provided out of box with Rierino:

<figure><img src=".gitbook/assets/image (144).png" alt=""><figcaption><p>Rierino vs. Traditional Low Code Development Platforms</p></figcaption></figure>

{% hint style="info" %}
Fastest way to start testing out and developing on Rierino is using our 'Developer Lite' edition on AWS Marketplace, which is deployed as a multi-service VM within the region and instance type of your choosing with 100% control.

<mark style="background-color:yellow;">**Click**</mark> [<mark style="background-color:yellow;">**here**</mark>](https://aws.amazon.com/marketplace/pp/prodview-up2fcxku3k742) <mark style="background-color:yellow;">**to start with AWS now.**</mark>

You can also check all alternative ways to start using Rierino [here](https://rierino.com/start).
{% endhint %}

## Front-End UI

### Internal User Experience

Rierino provides a low-code UI builder for web forms and data lists. It is typically used by internal users and partners.

It fits product, content, and asset management scenarios. It is also used for workflow task interactions and triggering automation.

The UI includes embedded BI capabilities. This keeps decision data close to the operational screens.

It also supports AI-assisted operations, such as summarizing, translating, and rewording.

You can extend the UI with 3rd party webcomponents. You can also use Handlebars templates for deeper customization.

#### Where to go next

* [User Interface](design/user-interface/)
* [Data Visualization](data-science/data-visualizations.md)

## Services & Business Logic

### Microservices Development

Rierino lets you build microservices with a low-code, drag\&drop interface. Services can range from simple database operations to complex logic and ML steps.

You can build them without writing a single line of code.

You can deploy services to many environments. This includes public clouds, private clouds, on-prem, and bare metal.

#### Where to go next

* [Microservices](devops/microservices/)

### Microservices Orchestration

Rierino can orchestrate your microservices and 3rd party services. You can model them as real-time API flows, async trigger flows, and workflows.

This lets you extend the platform using any language. It also lets you bring existing services into a consistent execution model.

#### Where to go next

* [API Flows](devops/api-flows/)
* [Gateway & Security](devops/gateway-and-security/)

### Workflow Management

Rierino can be used to design workflows and assign tasks to users. Users can review and add process data from customizable UI screens.

You can escalate tasks when they are not completed on time.

You can incorporate business rules and ML into workflows. This supports decision automation and process automation.

#### Where to go next

* [Orchestrate User Task](devops/microservices/elements/handlers/core-handlers/orchestrate-user-task.md)

### Rule Management

Rierino includes a flexible and customizable business rule engine. Use it to configure real-time decisions for any domain. Common examples include pricing and fraud detection.

Depending on the modules deployed, rule domains may be preconfigured. You can then customize them for your business requirements.

#### Where to go next

* [Business Rules](configuration/business-rules/)

### ML Automation

Rierino provides real-time ML scoring and MLOps automation capabilities. You can automate training and deploy models for real-time scoring.

Once configured, ML models built in R or Python can be converted and uploaded. They can then be used as a step in any API flow.

#### Where to go next

* [ML Models](data-science/ml-models/)
* [Complex Event Processing](data-science/complex-event-processing/)

### AI Agents

Rierino lets you build AI agents as first-class, deployable backend capabilities. You can develop agents with the same low-code approach used across the platform.

Agents are usually composed from:

* **Models** governed centrally, so you can swap providers safely.
* **Tools** backed by existing microservices and flows.
* **Orchestration** that controls prompts, context, and multi-step execution.

Once configured, agents can be exposed as APIs. This makes them easy to embed into apps and workflows.

#### Where to go next

* [GenAI Models](data-science/genai-models/)
* [Service MCP Requests](devops/microservices/elements/handlers/ml-and-ai-handlers/service-mcp-requests.md)

## Systems & Data Integration

### Database Systems

Rierino has an open architecture for data sources. It integrates with SQL, NoSQL, search engines, and caches.

The platform also provides an abstraction layer for operations and queries. This helps you switch data systems without migrating all logic.

#### Where to go next

* [State Managers](devops/microservices/elements/state-managers/)
* [Query Managers](devops/microservices/elements/query-managers/)

### External APIs

Rierino can integrate with public and private 3rd party APIs. It supports different auth mechanisms and formats (e.g. JSON, XML, SOAP, OData, GraphQL).

#### Where to go next

* [Call Rest API](devops/microservices/elements/handlers/core-handlers/call-rest-api.md)
* [Call SOAP API](devops/microservices/elements/handlers/specialized-handlers/call-soap-api.md)
* [Integrate with Camel](devops/microservices/elements/handlers/specialized-handlers/integrate-with-camel.md)

### Streaming Events

Rierino can consume and produce real-time event streams (for example, Kafka). This enables patterns that are hard with request-only architectures. Examples include async tasks, CDC feeds, and some analytics use cases.

#### Where to go next

* [Streams](devops/microservices/elements/streams/)
