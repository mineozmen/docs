---
description: Use this page when you know what you want to build, but not where to start.
icon: user-helmet-safety
---

# I would like to start with...

{% hint style="info" %}
If you are brand new to the platform, first skim [Development](development.md) and [Training Examples](../examples/training-examples/). They give you the basic mental model fast.
{% endhint %}

## A CRUD service

**Typical use cases**

* You need a backend for a front-end or admin tool.
* You want standard create, read, update, and delete endpoints.
* You want to expose a collection or table quickly.

This is the fastest path in Rierino. In most cases, you only need to drag & drop an element to an existing runner and click on save.

**Start here**

* [Exercise: Create a CRUD Service](../examples/training-examples/exercise-create-a-crud-service.md)
* [Video: Build full CRUD under 60 seconds](https://www.youtube.com/watch?v=iW_klLfvJuk)

**Learn next**

* [State Managers](../devops/microservices/elements/state-managers/) for storage patterns
* [Runners](../devops/microservices/runners/) for selecting microservices for your services
* [Gateway Channels](../devops/gateway-and-security/gateway-servers/gateway-channels.md) for granular control on RBAC

## An API endpoint

**Typical use cases**

* You need more than standard CRUD behavior.
* You want a custom request/response contract.
* You need validation, transformation, aggregation, or branching.
* You want an API-first architecture.

In Rierino, custom APIs usually start as **Sagas**. A saga is a flow. It can call handlers, states, queries, and other services.

**Start here**

* [Exercise: Create an API Endpoint](../examples/training-examples/exercise-create-an-api-endpoint.md)

**Most common flow steps**

* [Read Data](../devops/microservices/elements/handlers/core-handlers/read-data.md)
* [Write Data](../devops/microservices/elements/handlers/core-handlers/write-data.md)
* [Query Data](../devops/microservices/elements/handlers/core-handlers/query-data.md)
* [Transform Step](../devops/api-flows/configuring-saga-steps/transform-step/)
* [Condition Step](../devops/api-flows/configuring-saga-steps/condition-step/)

**Learn next**

* [API Flows](../devops/api-flows/) for details on how to build Sagas
* [Handlers](../devops/microservices/elements/handlers/) for rich set of actions available in steps

## Multi-systems integration or orchestration

**Typical use cases**

* You need to call third-party APIs.
* You need to combine multiple internal services.
* You need fan-out, batching, retries, or transformations.
* You want middleware or ESB-like behavior.

This path is also saga-first. The difference is that the saga coordinates **multiple systems**, not just one endpoint.

**Start here**

* [Call Rest API](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md)
* [Video: Integrate External APIs in 2 Minutes](https://www.youtube.com/watch?v=4KXbjt1qylk)

**Learn next**

* [API Based Systems](../devops/microservices/elements/systems/api-based-systems.md)
* [Call SOAP API](../devops/microservices/elements/handlers/specialized-handlers/call-soap-api.md)

## An internal application

**Typical use cases**

* You need an admin tool or back-office UI.
* You want forms, lists, editors, and actions without custom front-end code.
* You want internal users to manage data or trigger flows.

**Start here**

* [Exercise: Create a UI Screen](../examples/training-examples/exercise-create-a-ui-screen.md)
* [Video: Build a Functional UI in 2 Minutes](https://www.youtube.com/watch?v=qWVzLsIXTmA\&t=26s)

**Learn next**

* [Apps](../design/user-interface/apps.md) for grouping UIs into applications
* [UIs](../design/user-interface/uis/) for creating data entry and workflow forms
* [API Mapping](../design/api-mapping/) for mapping backend to frontend
* [Data Schema](../design/data-schema/) for structuring your data model
* [Orchestrate User Task](../devops/microservices/elements/handlers/core-handlers/orchestrate-user-task.md) for business process flows

## An AI agent

**Typical use cases**

* You want a conversational interface on top of your services.
* You want an agent that can call tools, not just chat.
* You want to expose capabilities as agent APIs, MCP, or A2A.

Rierino agents are usually built from:

* a governed **GenAI model**
* one or more **tool sagas** or **tool states**
* optional **interfaces** for richer responses

**Start here**

* [AI Agent Example](../examples/training-examples/ai-agent-example.md)
* [Video: Build Actionable AI Agents in 2 Minutes](https://www.youtube.com/watch?v=jgKj4_019Ps)
* [GenAI Models](../data-science/genai-models/)

**Learn next**

* [AI Agent APIs](../data-science/genai-models/ai-agent-apis.md)
* [Service MCP Requests](../devops/microservices/elements/handlers/ml-and-ai-handlers/service-mcp-requests.md)
* [Service A2A Requests](../devops/microservices/elements/handlers/ml-and-ai-handlers/service-a2a-requests.md)

## If you are not sure which path fits

Use this quick rule:

* Need standard data APIs fast → start with **CRUD service**
* Need custom business endpoints → start with **API endpoint**
* Need multi-system coordination → start with **integration or orchestration flow**
* Need an internal screen → start with **internal application**
* Need conversational automation → start with **AI agent**

If you want one guided path that touches most core concepts, start with the [In-depth Exercise](../examples/in-depth-exercise/).
