---
description: >-
  Overview of the Devops app for building API flows, microservices, gateways,
  deployments, and runtime operations in Rierino.
icon: circle-info
---

# Overview

The Devops app is where you design, deploy, and operate the platform’s core building blocks. It brings the runtime pieces (runners and deployments) together with orchestration (sagas) and the edge layer (gateway and security), so you can ship changes quickly and still keep control.

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption><p>Devops App</p></figcaption></figure>

Devops capabilities fall into four main areas:

* **Runtime:** Sagas, actions, runners and elements used for implementing [API flows](api-flows/) and building [microservices](microservices/)
* **Gateway:** Key capabilities for configuring [API gateways](gateway-and-security/), including authentication customizations
* **Rollout:** Key capabilities for creating & merging [branches](branching-and-migration/), migrating to test & production environments and deployments
* **Control & Admin:** Key capabilities for [controlling](administration/) deployed services and users

These building blocks are tightly connected. A typical request starts at the gateway, gets routed to one or more runners, and is often orchestrated through a saga. Supporting configuration, such as queries or model definitions, is usually stored in state managers so it can be updated without redeploying code.

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption><p>Relations between Blocks</p></figcaption></figure>

* **Gateway configuration** maps API channels (URL paths) to backend runners and services.
* **Deployments** package and install microservices. Each deployment contains one or more **runners**, and each runner contains **elements** that define what it can do and what it is allowed to access (handlers, systems, states, streams, and so on).
* **Sagas** orchestrate work across runners. A saga is a step graph that calls specific elements, potentially across multiple microservices, and returns a response or emits downstream events.
* **Configuration data** required by steps (for example queries, rules, or ML model definitions) is stored in **state managers** as records. This keeps behavior editable at runtime and easier to migrate between environments.
