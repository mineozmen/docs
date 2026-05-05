---
description: >-
  Microservices sit at the heart of Rierino platform, for building flexible and
  scalable backend applications.
icon: microchip
---

# Microservices

Typical microservice development with Rierino follows 3 main steps, building and using reusable components in each step:

1. Configuration of micro-composable [elements](building-blocks/), which define data stores and streams to use, key functions to execute and systems to access. These elements are configured once and utilized by multiple runners, minimizing workload and user errors, while making it possible to create infinite number of runners with plug and play capabilities.
2. Assembly of [runners](service-runners/), which is basically defining list of micro-composable elements to include inside a runner, and defining relations between them (such as which handler to respond to which type of requests coming from a stream). A runner itself does not mandate how it is deployed (e.g. which cloud provider, which containerization), isolating deployment from design.
3. Structuring of [deployments](deployment-packages/), which describes how and where to deploy each runner, including required CD settings. Typically, a deployment is executed by triggering a specific Jenkins or similar automation task with specific parameters. A deployment can include one or more runners, which allows for shrinkage or expansion of scale.

{% embed url="https://www.youtube.com/watch?v=qIJNBnN6v8U" %}
