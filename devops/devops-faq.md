---
description: >-
  This FAQ covers the core Devops concepts, runtime components, rollout model,
  and operational boundaries across the platform
icon: comment-question
---

# Devops FAQ

## Frequently asked questions

### What is the Devops app responsible for?

<details>

<summary>Show answer</summary>

Devops is the backend runtime and delivery layer of Rierino.

It is where you define API flows, assemble microservices, expose them through gateways, deploy them, and operate them.

See [Overview](devops-overview.md).

</details>

### What are the main building blocks in Devops?

<details>

<summary>Show answer</summary>

Think in six main groups:

* **API Flows** for orchestration and endpoint logic
* **Microservices** for runtime services and reusable backend components
* **Gateway & Security** for API exposure, auth, and edge control
* **Branching & Migration** for promoting changes across environments
* **Administration** for controlling deployed services
* **Batch Tasks** and **Pro-Code** for specialized jobs and extensions

Most teams spend the most time in [API Flows](api-flows/), [Microservices](microservices/), and [Gateway & Security](gateway-and-security/).

</details>

### How do sagas, runners, and deployments fit together?

<details>

<summary>Show answer</summary>

A **saga** defines the flow.

A **runner** executes backend work.

A **deployment** decides where one or more runners run.

In practice, a request often enters through the gateway, triggers a saga, calls handlers inside one or more runners, and runs inside a specific deployment target.

See [API Flows](api-flows/) and [Microservices](microservices/).

</details>

### What is the difference between a saga and a runner?

<details>

<summary>Show answer</summary>

A saga is orchestration.

It defines the sequence, branching, and routing of work.

A runner is execution infrastructure.

It hosts the handlers, states, queries, streams, and other elements that actually perform the work.

Use a saga when you need flow control. Use a runner when you need reusable service capability.

</details>

### What is an element in Devops?

<details>

<summary>Show answer</summary>

An element is a reusable backend building block inside a runner.

Examples include handlers, state managers, query managers, systems, streams, actions, and roles.

Elements let you configure a capability once and reuse it across many services.

See [Elements](microservices/elements/).

</details>

### When should I use a saga instead of only exposing a CRUD service?

<details>

<summary>Show answer</summary>

Use a plain CRUD-style service when the endpoint maps cleanly to create, read, update, or delete on one domain.

Use a saga when you need orchestration across steps or systems.

Typical saga cases include validation, branching, retries, transformations, enrichment, async triggers, and combining multiple services into one API.

</details>

### What are the most critical runtime components to understand first?

<details>

<summary>Show answer</summary>

Start with these:

* **Sagas** for API and process flow
* **Runners** for service execution
* **Handlers** for business actions
* **State managers** for aggregate storage
* **Query managers** for read models and searches
* **Streams** for async and event-driven communication
* **Deployments** for rollout and scaling

Once these click, most of Devops becomes much easier to navigate.

</details>

### How are APIs exposed to consumers?

<details>

<summary>Show answer</summary>

APIs are exposed through the gateway layer.

The gateway maps paths to backend services, applies security rules, and forwards requests to the right runners or channels.

This is also where token handling, session behavior, and edge-level controls live.

See [Gateway & Security](gateway-and-security/).

</details>

### Where do authentication and authorization fit?

<details>

<summary>Show answer</summary>

They start at the gateway edge.

Gateway configuration controls how requests are authenticated and how sessions or tokens are handled before business logic runs.

Some authorization decisions can also be enforced deeper in flows and handlers, depending on the use case.

</details>

### What can usually change in real time, and what usually needs deployment?

<details>

<summary>Show answer</summary>

Many configuration-level changes can be applied quickly.

That often includes saga updates, routing logic, element configuration, and other metadata-driven behavior.

Deployment is usually required when runtime packaging, infrastructure placement, binaries, or environment-specific rollout settings change.

The safest mental model is:

* **Flow and config changes** are often immediate
* **Runtime packaging and environment rollout** are deployment concerns

</details>

### What is a deployment in Rierino?

<details>

<summary>Show answer</summary>

A deployment is a grouped release unit for runners.

It defines how and where runners are installed or started for a target environment.

One deployment can contain one or many runners, depending on how you want to package scale, isolation, and operational ownership.

See [Deployments](microservices/deployments/).

</details>

### How should I think about state managers, query managers, and streams?

<details>

<summary>Show answer</summary>

They solve different backend access patterns:

* **State managers** are for entity storage and direct record access
* **Query managers** are for search, filtering, reporting, and read-heavy access
* **Streams** are for event-driven input and output

Together, they let one platform handle transactional APIs, search-oriented reads, and async processing.

See [State Managers](microservices/elements/state-managers/), [Query Managers](microservices/elements/query-managers/), and [Streams](microservices/elements/streams/).

</details>

### Can Devops orchestrate both internal and external systems?

<details>

<summary>Show answer</summary>

Yes.

That is one of its main strengths.

Flows can combine internal runners with external APIs, databases, streams, file systems, and specialized connectors in one controlled process.

</details>

### How does Devops relate to Configuration, Design, and Data Science?

<details>

<summary>Show answer</summary>

Devops runs the backend services.

**Configuration** defines reusable runtime assets such as queries, rules, and dynamic handlers.

**Design** builds the admin UI that consumes Devops APIs.

**Data Science** adds ML, GenAI, MCP, and related model-driven capabilities that Devops flows can call.

Devops is often the execution center that ties the other apps together.

</details>

### What is the typical path for building a new backend capability?

<details>

<summary>Show answer</summary>

A common path looks like this:

1. Define the needed data access or integration elements.
2. Assemble or update a runner with those elements.
3. Create a saga if the capability needs orchestration.
4. Expose it through the gateway if it needs an API endpoint.
5. Add it to a deployment and promote it across environments.

This keeps service design, exposure, and rollout separate but connected.

</details>

### When should I care about branching and migration?

<details>

<summary>Show answer</summary>

As soon as multiple people or environments are involved.

Branching helps teams isolate work. Migration helps teams promote those changes safely to test and production.

This matters even more when configuration is edited live and multiple services depend on shared assets.

See [Branching & Migration](branching-and-migration/).

</details>

### Where should I go next for deeper Devops FAQs?

<details>

<summary>Show answer</summary>

Start with the subsection that matches your immediate goal:

* Need endpoint orchestration or process logic → [API Flows](api-flows/)
* Need service structure or deployment model → [Microservices](microservices/)
* Need API exposure or auth setup → [Gateway & Security](gateway-and-security/)
* Need promotion and release flow → [Branching & Migration](branching-and-migration/)
* Need runtime control after deployment → [Administration](administration/)

Subsection-specific FAQs can then go deeper without repeating the core mental model.

</details>
