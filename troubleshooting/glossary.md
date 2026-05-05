---
description: >-
  Definitions for the main Rierino terms used across the documentation,
  including runtime, UI, configuration, integration, and AI concepts.
icon: book
---

# Glossary

{% hint style="info" icon="magnifying-glass" %}
**In brief:** This glossary defines the core terms used across Rierino documentation. Use it when you want a quick meaning, then follow the linked pages for full setup and reference details.
{% endhint %}

## Core platform terms

### Admin UI

The built-in web interface for internal users. Use it to manage data, workflows, operations, and configuration.

See [User Interface](../design/user-interface/).

### App

A top-level container in the Admin UI. An app groups related screens, menus, and navigation for a business domain or team.

See [Apps](../design/user-interface/apps.md).

### Branch

An isolated workspace for making changes before merging or migrating them to another environment.

See [Managing Branches](../devops/branching-and-migration/managing-branches.md).

### Deployment

A packaged runtime unit that installs one or more runners onto a target environment.

See [Deployments](../devops/microservices/deployment-packages/).

### Element

A building block inside a runner. Elements define what a runner can access and what it can do.

See [Elements](../devops/microservices/building-blocks/).

### Gateway

The entry layer for external and internal API access. It handles routing, authentication, sessions, and edge security.

See [Gateway & Security](../devops/api-gateway-and-security/).

### Microservice

A deployable backend service that handles part of the platform’s business logic, integration, or data processing.

See [Microservices](../devops/microservices/).

### Rierino

A low-code backend platform for microservices, orchestration, integrations, internal UI, and AI-enabled operations.

See [Rierino Overview](../).

### Runner

The main runtime container for microservice capabilities. A runner holds elements such as handlers, systems, states, and streams.

See [Runners](../devops/microservices/service-runners/).

### Saga

A flow definition used for APIs, events, and process orchestration. A saga coordinates steps across one or more runners.

See [API Flows](../devops/api-event-and-process-flows/).

### Screen

A page inside an Admin UI app. Most screens combine a lister and an editor around one business task or record type.

See [Layout & Navigation](../quick-start/layout-and-navigation.md).

### System

A shared configuration object for connecting to a data store, external API, message stream, or other integration target.

See [Systems](../devops/microservices/building-blocks/systems-integrations/).

## Runtime and orchestration

### Action

A reusable function call, usually backed by a handler action with predefined parameters.

See [Actions](../devops/microservices/building-blocks/additional-elements/actions.md).

### Event

A payload passed between steps, handlers, streams, or services. Events are the main unit of processing in many flows.

See [Event Step](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/).

### Event step

A saga step that sends an event payload to an event handler for execution.

See [Event Step](../devops/api-event-and-process-flows/configuring-saga-steps/event-step/).

### Handler

A runtime component that executes logic when a runner receives an event or request.

See [Handlers](../devops/microservices/building-blocks/execution-handlers/).

### Listener

A runner element that watches for changes in state managers and reacts to them.

See [Listeners](../devops/microservices/building-blocks/additional-elements/listeners.md).

### Query manager

A runner element used to execute queries against a backing data system.

See [Query Managers](../devops/microservices/building-blocks/query-and-search-sources/).

### State manager

A runner element used to store and retrieve aggregate data. It provides a consistent model across different data stores.

See [State Managers](../devops/microservices/building-blocks/data-sources/).

### Step link

A connection between saga steps. Step links define how a flow moves from one step to the next.

See [Step Link](../devops/api-event-and-process-flows/configuring-saga-steps/step-link.md).

### Stream

A runner element for input or output data flow, such as events, journals, or CDC feeds.

See [Streams](../devops/microservices/building-blocks/data-and-event-streams/).

### Transform step

A saga step that changes an incoming event before passing it onward.

See [Transform Step](../devops/api-event-and-process-flows/configuring-saga-steps/transform-step/).

### Condition step

A saga step that checks a condition and routes flow based on the result.

See [Condition Step](../devops/api-event-and-process-flows/configuring-saga-steps/condition-step/).

## Gateway and API terms

### API flow

A saga used as a request-response API or service orchestration layer.

See [API Flows](../devops/api-event-and-process-flows/).

### Gateway channel

A mapping from an external path segment to a backend system or service route.

See [Gateway Channels](../devops/api-gateway-and-security/gateway-servers/gateway-channels.md).

### Gateway server

The service that accepts API requests at the edge and routes them into runners, flows, or gateway-native features.

See [Gateway Servers](../devops/api-gateway-and-security/gateway-servers/).

### Gateway service

A gateway-side integration for capabilities handled directly by the gateway, such as logging or file operations.

See [Gateway Services](../devops/api-gateway-and-security/gateway-servers/gateway-services.md).

### Gateway system

A gateway configuration that defines how the gateway reaches backend runners or services.

See [Gateway Systems](../devops/api-gateway-and-security/gateway-servers/gateway-systems.md).

### Gateway token

A token definition for authentication details, claims, and user-type-specific access behavior.

See [Gateway Tokens](../devops/api-gateway-and-security/gateway-servers/gateway-tokens.md).

### OpenAPI specification

A machine-readable API description generated from runner and saga configuration.

See [OpenAPI Specification](../devops/api-gateway-and-security/apis/openapi-specification.md).

## UI and design terms

### Editor

The part of a screen used to view, create, and update one selected record.

See [Layout & Navigation](../quick-start/layout-and-navigation.md).

### Lister

The part of a screen used to search, filter, and select records.

See [Listers](../design/user-interface/uis/listers.md).

### Menu action

A user-triggered action exposed through lister, editor, selection, or widget menus.

See [Menus](../design/user-interface/uis/menus/).

### Option

A configurable list of values used in UI fields, selectors, and related display logic.

See [Options](../design/user-interface/options.md).

### Source

An API mapping that connects an Admin UI screen to listing, reading, or writing endpoints.

See [API Mapping](../design/api-mapping/).

### Translation

A configurable label or text resource used to localize the UI.

See [Translations](../design/user-interface/translations.md).

### UI

A screen definition in the Design app. It controls layout, widgets, menus, and screen behavior.

See [UIs](../design/user-interface/uis/).

### Widget

A UI component used to display or edit a field, object, array, or related data.

See [Widgets](../design/user-interface/uis/widgets/).

## Configuration and logic terms

### Business rule

A configurable decision rule stored as data instead of hard-coded logic.

See [Business Rules](../configuration/business-rules/).

### Dynamic handler

A runtime-editable handler definition used to customize logic without Devops changes.

See [Dynamic Handlers](../configuration/dynamic-handlers.md).

### Query

A reusable query definition stored as configuration data and executed by a query manager or flow.

See [Queries](../configuration/queries/).

### Schema

A structural definition of data fields, types, and validation rules used across the platform.

See [Data Schema](../design/data-schema/).

## AI and data science terms

### A2A

Agent-to-Agent protocol support. It lets other agents delegate tasks to Rierino-exposed agent capabilities.

See [Built with ML & AI](../introduction/built-with-ml-and-ai.md).

### AI agent

A configured capability that combines models, tools, and orchestration to answer, decide, and act.

See [GenAI Models](../data-science/genai-models/).

### CEP

Complex Event Processing. It evaluates streams over time for windows, patterns, and real-time analytics.

See [Complex Event Processing](../data-science/complex-event-processing/).

### GenAI model

A governed large language model or similar generative model configured for prompts, agents, or automation.

See [GenAI Models](../data-science/genai-models/).

### MCP

Model Context Protocol support. It exposes selected Rierino capabilities as tools and resources for LLM clients.

See [MCP Servers](../data-science/mcp-servers.md).

### ML model

A machine learning model used for scoring, prediction, classification, or other inference tasks.

See [ML Models](../data-science/ml-models/).

### RAI

Rierino’s built-in AI assistant for guided productivity inside the platform.

See [Built with ML & AI](../introduction/built-with-ml-and-ai.md).

## Related pages

* [Rierino Overview](../)
* [Use Cases](../introduction/rierino-use-cases.md)
* [Platform Architecture](../introduction/platform-architecture.md)
* [Devops Overview](../devops/devops-overview.md)
* [Design Overview](../design/design-overview.md)
* [Configuration Overview](../configuration/configuration-overview.md)
* [Data Science Overview](../data-science/data-science-overview.md)
