---
description: >-
  This page lists frequently asked questions about Rierino platform positioning
  and architecture
icon: comment-question
---

# FAQ

## Frequently asked questions

### What is Rierino?

<details>

<summary>Show answer</summary>

Rierino is a low-code backend platform for building microservices, APIs, workflows, and AI agents.

It combines service development, orchestration, integrations, and an internal Admin UI in one platform.

</details>

### Is Rierino only for low-code teams?

<details>

<summary>Show answer</summary>

No.

It works for business and technical users together. Teams can move fast with configuration-first building blocks. Developers can still extend the platform when needed.

See [Overview](../) and [Pro-Code](../devops/pro-code/).

</details>

### What kinds of applications can I build?

<details>

<summary>Show answer</summary>

Rierino fits backend-heavy applications with many workflows, integrations, or data operations.

Common examples include:

* Information management systems such as PIM, CMS, and MDM
* Decision automation such as pricing, risk, and recommendations
* BPM and task-driven internal applications
* Integration and orchestration layers across many systems
* AI-enabled services and agents

See [Use Cases](use-cases.md).

</details>

### Does Rierino include a user interface?

<details>

<summary>Show answer</summary>

Yes.

Rierino includes a built-in Admin UI for internal users. You can use it for forms, data lists, workflows, and operational screens.

It is best suited for internal tools, partner portals, and back-office processes.

See [User Interface](../design/user-interface/).

</details>

### Can I use my own front-end?

<details>

<summary>Show answer</summary>

Yes.

Rierino is headless. You can connect any customer-facing web, mobile, or external system through APIs.

The built-in Admin UI is optional. It is simply the fastest path for internal tooling.

See [Platform Architecture](platform-architecture.md).

</details>

### What does the platform architecture look like?

<details>

<summary>Show answer</summary>

Rierino has three main layers:

* A front-end layer for internal UIs and external channels
* A back-end layer with gateways, service runners, and orchestration
* A storage layer with pluggable databases, caches, search, and analytics stores

This structure keeps the platform flexible and scalable.

See [Platform Architecture](platform-architecture.md).

</details>

### Can I build microservices without writing code?

<details>

<summary>Show answer</summary>

Yes.

You can assemble services from runners, handlers, states, queries, and flows through configuration. This covers common CRUD, orchestration, integration, and automation use cases.

You can also add custom code when you need deeper specialization.

See [Microservices](../devops/microservices/) and [API Flows](../devops/api-flows/).

</details>

### Can Rierino orchestrate existing services?

<details>

<summary>Show answer</summary>

Yes.

Rierino can orchestrate its own services and external services in the same flow. That includes REST APIs, SOAP services, event streams, and file-based processes.

This makes it useful as both an application platform and an integration layer.

</details>

### Which data systems does Rierino support?

<details>

<summary>Show answer</summary>

Rierino supports multiple storage types through adapters.

That includes SQL databases, NoSQL databases, caches, search engines, file systems, and analytics-oriented stores.

This helps teams switch or combine data systems without rewriting all service logic.

See [State Managers](../devops/microservices/elements/state-managers/) and [Query Managers](../devops/microservices/elements/query-managers/).

</details>

### Does Rierino support event-driven and async patterns?

<details>

<summary>Show answer</summary>

Yes.

Rierino can consume and produce streams such as Kafka. It also supports async triggers, CDC-based flows, and real-time processing patterns.

This is useful for high-volume automation and loosely coupled architectures.

See [Streams](../devops/microservices/elements/streams/).

</details>

### What AI and ML capabilities are included?

<details>

<summary>Show answer</summary>

Rierino supports both traditional ML and generative AI.

You can use ML for real-time scoring. You can use GenAI for assistants, content generation, and AI agents. You can also expose capabilities through MCP and A2A protocols.

See [Built with ML & AI](built-with-ml-and-ai.md).

</details>

### Can I build AI agents on Rierino?

<details>

<summary>Show answer</summary>

Yes.

Rierino lets you build agents that use governed models, call internal tools, access enterprise data, and trigger actions.

This makes the agents useful for real business tasks, not only Q\&A.

See [GenAI Models](../data-science/genai-models/).

</details>

### Can Rierino use external AI services?

<details>

<summary>Show answer</summary>

Yes.

You can call external AI APIs from backend flows just like any other integration. This helps with summarization, extraction, classification, and prompt-driven automation.

It also keeps authentication, retries, and observability consistent.

</details>

### Where can Rierino be deployed?

<details>

<summary>Show answer</summary>

Rierino can be deployed in public cloud, private cloud, on-prem, or bare metal environments.

This gives teams full control over security, scale, and operating model.

</details>

### Who is Rierino a good fit for?

<details>

<summary>Show answer</summary>

Rierino is strongest when you need one or more of these:

* Large-scale operations
* Frequent business change
* Many integrations
* Data-heavy workflows
* Decision automation
* Incremental modernization from monoliths

See [Use Cases](use-cases.md).

</details>

### What is the fastest way to get started?

<details>

<summary>Show answer</summary>

Start with the [Overview](../), then move into [Installation](../quick-start/installation.md) and [Quick Start](/broken/spaces/cnDk3J1AzTgg2NFrGPlh/pages/13CTa2zTaEUFEIKvmPib).

If you already know what you want to build, use [I would like to start with...](../quick-start/i-would-like-to-start-with....md).

</details>
