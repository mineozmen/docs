---
description: >-
  Devops, Configurtion, Design and Data Science apps allow users develop
  sophisticated enterprise applications rapidly through low-code interfaces
icon: display-code
---

# Development

## Rierino Development Apps

Most development work in Rierino happens across four apps. Each app “owns” a slice of the platform, so you can iterate without mixing concerns.

### Devops

Use **Devops** to build and operate backend execution. This is where you define and deploy **runners** (microservices), design **sagas** (API flows), configure **gateway routing/security**, and manage deployments.

Start here when you are building APIs, orchestrations, or anything that needs to run in production. See [Devops Overview](../devops/overview.md).

### Configuration

Use **Configuration** to store “logic as data”. This is where you define reusable **queries**, **business rules**, and **dynamic handlers**, which can then be executed by runners and sagas at runtime.

Start here when you want behavior to be editable without redeploying services. See [Configuration Overview](../configuration/overview.md).

### Design (Admin UI)

Use **Design** to build the admin UI. This is where you define **apps**, **UIs**, **listers/widgets**, and the **Source** mappings that connect screens to backend APIs. You also manage UI resources like options, translations, icons, and styles.

Start here when you want a screen for listing and editing data or operating flows. See [Design Overview](../design/overview.md).

### Data Science

Use **Data Science** to configure ML and GenAI assets that can be invoked from real-time APIs or batch processes. This includes ML models, GenAI models, MCP servers, CEP flows, visualizations, and customizations.

Start here when your use case needs inference, agents, personalization, or stream processing. See [Data Science Overview](../data-science/overview.md).

### How they fit together (typical workflow)

Most teams iterate in a loop: define runtime services in **Devops**, keep reusable logic in **Configuration**, expose operations through **Design** screens, and add intelligence through **Data Science** assets when needed. The boundaries are intentional, but the components are designed to compose cleanly.

## Initial Reading

To start using Rierino right away, use the training microservices shipped with the standard installation. They let you explore the platform without building anything from scratch.

Focus on the following concepts first:

* Start using existing training microservices with [saga flows](../devops/api-flows/).
* Perform [read](/broken/pages/rd5eitG70c99RohCh3AX) and [write](/broken/pages/0a3jLM4TdCuK7kEQqgi2) operations on existing runners.
* Perform [query](/broken/pages/oAal4A8dPg9i6GOLrJcV) operations and visually design [queries](../configuration/queries/).
* Integrate with 3rd party [rest systems](../devops/microservices/elements/systems/#rest) and [call their APIs](/broken/pages/UhdC9EjYpWaLh8PhyvdB).
* Build new [UIs](../design/user-interface/uis/) and add them to [Apps](../design/user-interface/apps.md).

If you prefer a guided path, complete the [self-paced interactive training](https://rierino-open.github.io/training/intro-core-capabilities/8912351230123.html). It introduces the main concepts and terminology using hands-on steps.

## Examples

Once you are comfortable with the basics, use the examples below to build confidence. They are also a good way to validate that your deployment is working end to end.

* Reviewing [Training APIs](../examples/training-examples/api-flow-examples.md) & creating an [API endpoint](../examples/training-examples/exercise-create-an-api-endpoint.md)
* Reviewing [Training Microservices](../examples/training-examples/microservice-examples.md) & creating a [CRUD service](../examples/training-examples/exercise-create-a-crud-service.md)
* Reviewing [Training UI](../examples/training-examples/ui-example.md) & creating a [UI screen](../examples/training-examples/exercise-create-a-ui-screen.md)
* Reviewing [Training AI Agent](../examples/training-examples/ai-agent-example.md)
* Building a complete [To-do List App](../examples/in-depth-exercise/)

### Videos

You can also visit our [YouTube channel](https://www.youtube.com/@rierino) for QuickBuild videos. These are useful when you want to see a full workflow, start to finish.

{% embed url="https://www.youtube.com/watch?ab_channel=Rierino&v=iW_klLfvJuk" %}

## Troubleshooting

Rierino provides multiple ways to audit and troubleshoot issues. Start with these quick checks:

* Checking meaning or [Error Codes](../troubleshooting/error-codes.md) returned
* Performing [checks for common errors](../troubleshooting/useful-checks.md)
* Checking feature availability by versions in [release notes](../troubleshooting/release-notes.md)

If you are stuck early, validate that:

* Your gateway is reachable.
* Your training microservices are deployed.
* Your data store is accessible (MongoDB by default).
