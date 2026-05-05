---
description: >-
  Rierino Java SDK provides ability to build highly customized elements and use
  them as part of the low-code development environment.
icon: display-code
---

# Pro-Code

Rierino core library, which is accessible for our clients through our Maven repository provide all utility functions, abstract classes and interfaces for creating and incorporating new elements into backend microservices.

## Creating a New Element &#x20;

New microservice elements can be implemented using Rierino Java SDK, for use cases such as:

* Implementing a new [custom handler](custom-handlers.md), which wraps a 3rd party Java library for Rierino, or executes a highly customized logic
* Implementing a new [custom state manager](custom-state-managers.md), for a data source type which is not currently supported out of box by Rierino

New elements can be built and stored in a Maven repository that is accessible by Rierino deployment environment.

## Deploying in Microservices

All custom elements can be included in required microservices as dependencies within their [deployment configurations](../microservices/deployment-packages/defining-a-deployment.md).

## Using in Microservices

Once the microservices are configured to use new element dependencies, these elements can be configured on the [elements](../microservices/building-blocks/) screen of devops application and added to the runners which will be utilizing them.

{% hint style="warning" %}
Rierino provides a wide range of options for creating flexible microservices, APIs and event processing capabilities, including custom scripting and dynamic code processing features.&#x20;

Unless it is an edge case or implementation of an adapter for an existing 3rd party library, it is recommended to utilize built-in functionalities, which are continuously maintained and improved by Rierino team.
{% endhint %}

{% hint style="info" %}
In addition to provided list of elements, it is also possible to create custom runners, transformation and condition classes, listeners, aggregate manipulators, locking mechanisms using available base classes, if required.
{% endhint %}
