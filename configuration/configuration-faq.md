---
description: >-
  This FAQ covers the core Configuration concepts, reusable logic model, and
  when to use queries, rules, and dynamic handlers
icon: comment-question
---

# Configuration FAQ

## Frequently asked questions

### What is the Configuration app responsible for?

<details>

<summary>Show answer</summary>

Configuration is where you define reusable logic as managed records.

Instead of hardcoding behavior into services, you store queries, rules, and dynamic handlers so runners and sagas can load them at runtime.

See [Overview](configuration-overview.md).

</details>

### What kinds of logic belong in Configuration?

<details>

<summary>Show answer</summary>

Configuration is best for logic that should stay editable, reusable, and portable.

The main categories are:

* **Queries** for data retrieval and filtering
* **Business Rules** for decision logic and policy evaluation
* **Dynamic Handlers** for specialized runtime logic that extends built-in handlers

See [Overview](configuration-overview.md).

</details>

### How is Configuration different from Devops?

<details>

<summary>Show answer</summary>

Configuration stores reusable runtime logic.

Devops defines the services, flows, runners, and deployments that execute that logic.

A simple mental model is:

* **Configuration** defines what reusable logic exists
* **Devops** decides where and how that logic runs

See [Devops FAQ](../devops/devops-faq.md).

</details>

### Why keep logic in Configuration instead of hardcoding it?

<details>

<summary>Show answer</summary>

Because it makes behavior easier to change, review, and migrate.

It also reduces redeploys for common logic changes and keeps reusable definitions in one place instead of spreading them across services.

</details>

### What is a query in Rierino?

<details>

<summary>Show answer</summary>

A query is a stored, parameterized definition for reading data from a target platform.

It is usually executed by a query manager or invoked from a handler or saga step.

Queries are useful when you want reusable listing, filtering, search, or reporting logic without embedding platform-specific syntax everywhere.

See [Queries](queries/).

</details>

### When should I use a query instead of a state manager read?

<details>

<summary>Show answer</summary>

Use a state manager read for direct record access.

Use a query when you need custom filtering, search, pagination, joins, aggregations, or platform-specific read behavior.

Queries are especially useful for list endpoints and read-heavy screens.

</details>

### Are queries tied to one database technology?

<details>

<summary>Show answer</summary>

No.

Queries are designed as standardized definitions that target a specific platform at execution time.

That makes them easier to manage and migrate than scattering raw query syntax across services.

See [Query Platforms](queries/query-platforms/).

</details>

### What are business rules in Configuration?

<details>

<summary>Show answer</summary>

Business rules are stored decision definitions.

They let you manage conditions and actions as data, so rule behavior can evolve without rewriting service code for every policy change.

Rules are usually grouped into domains and executed together.

See [Business Rules](business-rules/).

</details>

### What is a rule domain?

<details>

<summary>Show answer</summary>

A rule domain is a grouped ruleset.

It defines a logical area where related rules, shared functions, and execution settings live together.

Examples could include pricing rules, validation policies, or promotion logic.

</details>

### When should I use business rules instead of a saga?

<details>

<summary>Show answer</summary>

Use a saga for orchestration.

Use business rules for decision evaluation.

If you need step flow, routing, retries, or cross-service coordination, use a saga. If you need policy logic such as qualification, scoring, matching, or action selection, rules are often the better fit.

</details>

### What is a dynamic handler?

<details>

<summary>Show answer</summary>

A dynamic handler is runtime-loaded custom logic.

It lets you add or customize handler behavior without going through the normal service build path for every change.

This is useful when built-in handlers are close, but not enough for the use case.

See [Dynamic Handlers](dynamic-handlers.md).

</details>

### What is the difference between handler codes and handler packages?

<details>

<summary>Show answer</summary>

**Handler codes** are runtime-loaded code blocks.

**Handler packages** are runtime-loaded JAR packages containing handler implementations.

Use codes for smaller or more direct extensions. Use packages when you need bundled classes, dependencies, or a more structured custom extension.

</details>

### When should I use a dynamic handler instead of a built-in handler?

<details>

<summary>Show answer</summary>

Use a built-in handler first whenever it fits.

Use a dynamic handler when the use case is too specialized for the standard handler set, but you still want the logic managed as a runtime asset instead of packaging it into a normal service release.

</details>

### What is the relationship between Configuration and runtime safety?

<details>

<summary>Show answer</summary>

Configuration defines the reusable logic.

Runners and handlers still control what systems, states, and operations are allowed at execution time.

That separation helps teams iterate faster without giving every configuration record unlimited runtime access.

</details>

### Can Configuration changes be reused across multiple services?

<details>

<summary>Show answer</summary>

Yes.

That is one of the main reasons to keep logic here.

A stored query, ruleset, or dynamic handler can be used by multiple runners, flows, or screens instead of being duplicated in each service.

</details>

### How does Configuration relate to Design?

<details>

<summary>Show answer</summary>

Configuration often provides the reusable logic behind screens.

For example, a Design source may call an endpoint that runs a stored query, or a UI workflow may depend on rule evaluation defined here.

Design handles the screen. Configuration helps supply the runtime logic behind it.

See [Design FAQ](../design/design-faq.md).

</details>

### What should I understand first if I am new to Configuration?

<details>

<summary>Show answer</summary>

Start with these concepts:

* **Query** for reusable data retrieval
* **Rule domain** for grouped decision logic
* **Dynamic handler** for specialized runtime extension
* **Runtime reuse** as the main reason this app exists

Once these are clear, the rest of the section becomes much easier to navigate.

</details>

### Where should I go next for deeper Configuration FAQs?

<details>

<summary>Show answer</summary>

Start with the subsection that matches your goal:

* Need data retrieval logic → [Queries](queries/)
* Need policy or decision logic → [Business Rules](business-rules/)
* Need runtime custom extensions → [Dynamic Handlers](dynamic-handlers.md)

Subsection-specific FAQs can then go deeper into query types, platforms, rule patterns, and custom handler strategies.

</details>
