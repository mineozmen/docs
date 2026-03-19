---
description: >-
  This FAQ helps new teams get started fast, it covers setup, navigation, which
  app to use, and where to begin for common build paths
icon: comment-question
---

# FAQ

## Frequently asked questions

### What is the fastest way to try Rierino?

<details>

<summary>Show answer</summary>

The fastest path is the free Community Edition on AWS Marketplace.

It gives you a ready-to-use environment for exploring the platform and validating the architecture quickly.

See [Installation](installation.md).

</details>

### Do I need to install everything myself first?

<details>

<summary>Show answer</summary>

No.

You can start with the hosted marketplace option first. Use custom installation when you need a licensed, self-managed setup for your own environment.

Common self-managed options are a single VM with Docker Compose or Kubernetes with Helm.

See [Installation](installation.md).

</details>

### Which data store should I expect by default?

<details>

<summary>Show answer</summary>

Most standard setups use MongoDB as the main data store.

That is a good default for training, initial development, and many early projects.

</details>

### Where do I define secrets and connection settings?

<details>

<summary>Show answer</summary>

Keep secrets and connection values outside the UI when possible.

On a single VM, they usually live in `globalconfig.properties` and `globalsecrets.properties`. On Kubernetes, they are commonly stored in ConfigMaps and Secrets.

See [Installation](installation.md).

</details>

### What is the main entry point in the Admin UI?

<details>

<summary>Show answer</summary>

The global home page is the main landing page.

It shows accessible screens, AI agents, quick links, favorites, and recent pages.

See [Layout & Navigation](layout-and-navigation.md).

</details>

### What is an app home page?

<details>

<summary>Show answer</summary>

Each app has its own landing page.

It usually includes modules, profile details, and app-specific FAQs. It acts as the local starting point once you enter an app.

See [Layout & Navigation](layout-and-navigation.md).

</details>

### How do users usually navigate in Rierino?

<details>

<summary>Show answer</summary>

Most users follow a simple flow:

1. Open the global home page.
2. Switch to the right app.
3. Open a screen.
4. Find a record in the lister.
5. Open or edit it in the editor.

See [Layout & Navigation](layout-and-navigation.md).

</details>

### What is the difference between a lister and an editor?

<details>

<summary>Show answer</summary>

The lister is for finding, filtering, sorting, and selecting records.

The editor is for viewing, creating, and updating a selected record.

Most screens use both together.

See [Layout & Navigation](layout-and-navigation.md).

</details>

### Which app should I use to build backend services?

<details>

<summary>Show answer</summary>

Use **Devops**.

That is where you define runners, sagas, gateway routing, security, and deployments.

See [Development](development.md).

</details>

### Which app should I use for reusable logic like queries and rules?

<details>

<summary>Show answer</summary>

Use **Configuration**.

That is where you define reusable queries, business rules, and dynamic handlers that can run at runtime without redeploying services.

See [Development](development.md).

</details>

### Which app should I use to build the admin UI?

<details>

<summary>Show answer</summary>

Use **Design**.

That is where you define apps, UIs, widgets, listers, and source mappings that connect the UI to backend APIs.

See [Development](development.md).

</details>

### Which app should I use for AI and ML features?

<details>

<summary>Show answer</summary>

Use **Data Science**.

That is where you manage ML models, GenAI models, MCP servers, visualizations, and related assets.

See [Development](development.md).

</details>

### What should I read first if I am brand new?

<details>

<summary>Show answer</summary>

Start with [Development](development.md).

Then review the training examples and the guided starting paths. That gives you the quickest mental model of how the platform fits together.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### I want a standard CRUD API. Where should I start?

<details>

<summary>Show answer</summary>

Start with the CRUD service path.

That is the fastest route when you need standard create, read, update, and delete behavior on a collection or table.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### I want a custom API endpoint. Where should I start?

<details>

<summary>Show answer</summary>

Start with an API endpoint built as a saga.

This is the right path when you need custom validation, transformation, branching, or aggregation logic.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### I want to integrate multiple systems. Where should I start?

<details>

<summary>Show answer</summary>

Start with the integration or orchestration path.

Use this when you need to call third-party APIs, combine internal services, or coordinate retries, batching, and transformations.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### I want to build an internal app. Where should I start?

<details>

<summary>Show answer</summary>

Start with the internal application path.

Use this when you need forms, lists, editors, and back-office workflows without building a custom front-end first.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### I want to build an AI agent. Where should I start?

<details>

<summary>Show answer</summary>

Start with the AI agent path.

Rierino agents usually combine a governed GenAI model with tool sagas or tool states, plus optional interfaces.

See [I would like to start with...](i-would-like-to-start-with....md).

</details>

### What if I am not sure which path fits?

<details>

<summary>Show answer</summary>

Use this quick rule:

* Need standard data APIs fast → start with CRUD service
* Need custom business endpoints → start with API endpoint
* Need multi-system coordination → start with integration or orchestration
* Need an internal screen → start with internal application
* Need conversational automation → start with AI agent

If you want one guided path that touches most core concepts, start with the [In-depth Exercise](../examples/in-depth-exercise/).

</details>

### What should I check first if something does not work?

<details>

<summary>Show answer</summary>

Start with the basics:

* Confirm the gateway is reachable
* Confirm the training microservices are deployed
* Confirm the main data store is accessible

Then review error codes, common checks, and release notes if needed.

See [Development](development.md).

</details>
