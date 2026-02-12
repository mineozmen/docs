---
icon: flag-checkered
---

# Quick Start

## Easy Access to Rierino

Fastest way to start testing out and developing on Rierino is using our 'Developer Lite' edition on AWS Marketplace, which is deployed as a multi-service VM within the region and instance type of your choosing with 100% control.&#x20;

<mark style="background-color:yellow;">**Click**</mark> [<mark style="background-color:yellow;">**here**</mark>](https://aws.amazon.com/marketplace/pp/prodview-up2fcxku3k742) <mark style="background-color:yellow;">**to start with AWS now.**</mark>

## Custom Installation

For installing Rierino yourself, you can follow instructions provided in [Installation](https://app.gitbook.com/o/lZ6vN24S2tXuHH7DjdWa/s/pV7u8nn9fFM9XMp0tNic/) documentation or check all alternative ways to start using Rierino [here](htps://rierino.com/start).

* A single VM deployment for development & testing can be achieved by using the docker compose details provided in [Sandbox Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/sandbox-vm-deployment) section.&#x20;
* Installation on a K8s cluster can be achieved by following the command provided in Fully Automated Helm Chart or Fully Automated Ansible alternative in [Kubernetes Deployment](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/deployment-alternatives/kubernetes-deployment) section.&#x20;

The standard configuration uses MongoDB system as the main data store.

## Initial Reading

To start using Rierino platform right away, you can utilize training microservices deployed with the standard installation. To do so, we suggest you familiarize yourself with some of the key concepts and basic yet frequently used features:

* How you can start using existing training microservices with [saga flows](../devops/api-flows/)
* How you can perform [read](../devops/microservices/elements/handlers/core-handlers/read-data.md), [write](../devops/microservices/elements/handlers/core-handlers/write-data.md) operations on existing runners
* How you can perform [query](../devops/microservices/elements/handlers/core-handlers/query-data.md) operations and visually design [queries](../configuration/queries/)
* How you can integrate with 3rd party [rest systems](../devops/microservices/elements/systems.md#rest) and [call their APIs](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md)
* How you can build new [UIs](../design/user-interface/uis/) and add them to [Apps](../design/user-interface/apps.md)

In addition to documentation, an alternative way to start learning more about Rierino is to complete our [self-paced interactive training](https://rierino-open.github.io/training/intro-core-capabilities/8912351230123.html) content, which provides an introduction to main Rierino concepts and terminology.

## Examples

To experience and start building your own applications, you can refer to our list of examples, such as:

* Reviewing [Training Runners](../examples/training-examples/microservice-examples.md)
* Reviewing [Training APIs](../examples/training-examples/api-flow-examples.md)
* Creating a [Hello World API](../examples/training-examples/exercise-hello-world-api.md)
* Creating a [new CRUD endpoint](../examples/training-examples/exercise-test-state.md)
* Building a complete [To-do List App](../examples/exercise-to-do-list/)

You can also visit our [Youtube channel](https://www.youtube.com/@rierino) featuring Rierino QuickBuild videos.

{% embed url="https://www.youtube.com/watch?ab_channel=Rierino&v=iW_klLfvJuk" %}

## Troubleshooting

Rierino provides various means for auditing and troubleshooting purposes, with the options for starting out as:

* Checking meaning or [Error Codes](../troubleshooting/error-codes.md) returned
* Performing [checks for common errors](../troubleshooting/useful-checks.md)
* Checking feature availability by versions in [release notes](../troubleshooting/release-notes.md)
