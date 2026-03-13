---
icon: flag-checkered
---

# Quick Start

## Easy Access to Rierino

Fastest way to start testing and developing on Rierino is using our free **Community Edition** on AWS Marketplace. It is deployed as a multi-service VM. You can choose the region and instance type. You also get full control of the environment.

<mark style="background-color:yellow;">**Click**</mark> [<mark style="background-color:yellow;">**here**</mark>](https://aws.amazon.com/marketplace/pp/prodview-up2fcxku3k742) <mark style="background-color:yellow;">**to start with AWS now.**</mark>

### What you get with Developer Lite

* A ready-to-use environment for exploring the platform.
* A good baseline for running training examples and quick experiments.
* A fast path to validate the architecture before a custom installation.

## Custom Installation

For installing Rierino yourself, follow the [Installation](https://app.gitbook.com/o/lZ6vN24S2tXuHH7DjdWa/s/pV7u8nn9fFM9XMp0tNic/) documentation. You can also check alternative ways to start using Rierino [here](https://rierino.com/start).

### Common installation options

* **Single VM (dev/test):** use the Docker Compose setup in [Sandbox Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/sandbox-vm-deployment).
* **Kubernetes (recommended for teams):** use the fully automated Helm Chart or Ansible options in [Kubernetes Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/kubernetes-deployment).

The standard configuration uses **MongoDB** as the main data store. This is a good default for most training and initial development.

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
* Perform [read](../devops/microservices/elements/handlers/core-handlers/read-data.md) and [write](../devops/microservices/elements/handlers/core-handlers/write-data.md) operations on existing runners.
* Perform [query](../devops/microservices/elements/handlers/core-handlers/query-data.md) operations and visually design [queries](../configuration/queries/).
* Integrate with 3rd party [rest systems](../devops/microservices/elements/systems/#rest) and [call their APIs](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md).
* Build new [UIs](../design/user-interface/uis/) and add them to [Apps](../design/user-interface/apps.md).

If you prefer a guided path, complete the [self-paced interactive training](https://rierino-open.github.io/training/intro-core-capabilities/8912351230123.html). It introduces the main concepts and terminology using hands-on steps.

## Examples

Once you are comfortable with the basics, use the examples below to build confidence. They are also a good way to validate that your deployment is working end to end.

* Reviewing [Training Runners](../examples/training-examples/microservice-examples.md)
* Reviewing [Training APIs](../examples/training-examples/api-flow-examples.md)
* Creating a [Hello World API](../examples/training-examples/exercise-hello-world-api.md)
* Creating a [new CRUD endpoint](../examples/training-examples/exercise-test-state.md)
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
