---
description: >-
  Rierino natively supports embedding GenAI as well as traditional ML models and
  features into any UI, workflow or API through low-code configurations
icon: brain-circuit
---

# Built with ML & AI

{% hint style="info" icon="magnifying-glass" %}
**In brief:** Rierino supports extensive ML and AI capabilities out of the box. These capabilities are designed to work together. They cover productivity, agent development, protocol-based exposure, and real-time inference. You can use them independently. You can also combine them into end-to-end flows.
{% endhint %}

## 'Empowered' AI Agent Development

<figure><img src="../.gitbook/assets/A.5. Agent Response.png" alt=""><figcaption></figcaption></figure>

[Rierino AI Agent Builder](https://rierino.com/platform/agent) lets you rapidly create AI agents. Agents can access internal data sources and call enterprise systems and APIs. They can also trigger automated actions when tasks require execution, not just answers.

This enables assistants that are more than Q\&A bots. Agents can retrieve, decide, and act. They can also coordinate multiple tools in a single interaction.

### Typical agent capabilities

* Retrieve domain data (products, customers, tasks, documents).
* Enrich or validate information before taking action.
* Execute actions through APIs and flows (create orders, open tickets, send emails).
* Keep context and guide users across multi-step processes.

### Governance by default

Agent tools follow existing security policies and user access rights. This keeps assistants structured and safe for different user types. It also supports using the same agent across teams with different permissions.

### Multiple Agent Patterns

In addition to building and running individual AI agents, Rierino also allows buildings ecosystems of agents, in different formats, such as:

* **Agent Panels:** Multiple agents voting or returning responses which are processed & judged by a 'panelist' agent
* **Agent Teams:** Multiple agents working together with complementing skills, with oversight by a 'manager' agent

### When to use agents vs. flows

Use agents when users need a conversational interface. Use agents when the “next step” depends on context. Use flows alone when you want strict, fixed-step execution.

{% embed url="https://www.youtube.com/watch?ab_channel=Rierino&v=jgKj4_019Ps" %}

For more details [click here](../data-science/genai-models/), for details on training AI agent example [click here](../examples/training-examples/ai-agent-example.md).

## MCP Server & Middleware

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption><p>MCP Server</p></figcaption></figure>

Rierino can service its functionality over Model Context Protocol (MCP). This is done through configuration. You select which flows become MCP tools. You select which states become MCP resources.

Security policies and user roles stay in place. This is important for enterprise use cases. It prevents tools from bypassing platform governance.

Any API, event processing, or CDC flow can be exposed as an MCP tool. Tools can still be triggered through protocols Rierino already supports. Examples include REST, WebSocket, and event streaming. This goes beyond basic MCP implementations that only wrap HTTP calls.

### Common patterns

* Build MCP servers from scratch using database access, rule evaluation, and ML prediction.
* Wrap external integrations using existing handlers and flows.
* Act as middleware by routing requests to 3rd party MCP servers when needed.

This keeps the “tool layer” consistent across internal and external capabilities.

### When MCP is a good fit

* You want to expose tools and resources to an LLM client with a standard protocol.
* You want to reuse existing API, event, or CDC flows without rebuilding tool wrappers.
* You need enterprise governance to apply to the tool layer by default.

For more details [click here](/broken/pages/nROiQweGSG7a5Q2JITkp).

## A2A Server & Middleware

Similar to MCP, Rierino can service functionality over Agent2Agent (A2A) protocol. This allows other agents to delegate tasks to Rierino.

You configure which LLM flows are exposed for agent-to-agent task execution. This makes it easier to build agent ecosystems. It also supports splitting responsibilities across agents and teams.

### Typical A2A use cases

* Centralize “enterprise tools” in Rierino, used by multiple agents.
* Route specialized tasks to dedicated agents (support, ops, data, commerce).
* Keep agent coordination decoupled from UI channels and client apps.

For more details [click here](/broken/pages/OJZXriH2cJwka5MR5bKI).

## AI-Powered Productivity with RAI

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

At the heart of Rierino’s AI-supported development experience is the AI assistant, [RAI](https://rierino.com/platform/rai). RAI is embedded directly into the Rierino UI. This keeps AI assistance close to where users configure apps, services, and data.

RAI helps both technical and non-technical users move faster. It provides a conversational interface to accelerate common tasks. It also reduces handoffs between teams by turning intent into structured changes.

### What RAI is used for

* Generate and improve content (summaries, translations, rewrites).
* Assist with creating and updating UI screens and menus.
* Help configure workflows and automation steps.
* Analyze data and suggest next actions based on context.

### Why it matters

RAI is not a separate “chat tool”. It is designed to operate inside the platform. That means it can be guided by the same roles and security controls as the UI.

### Typical teams that benefit

* Business users who need help producing and reviewing content at scale.
* Product and operations teams who need guided automation.
* Developers who want faster iteration on UI and workflow configuration.

For more details [click here](../design/user-interface/uis/menus/rai-menu-actions.md).

## Real-Time Inference with Specialized Handlers

Rierino enables real-time machine learning with specialized handlers. These handlers are designed to work with a variety of ML model types. They let you score models inside APIs and workflows with millisecond-level latency.

This is useful when decisions must happen “in the transaction”. It also helps when you want consistent scoring behavior across many services.

### Typical use cases

* Dynamic pricing decisions during checkout.
* Personalized recommendations on search and browse pages.
* Fraud and risk scoring on payments and account actions.
* Real-time classification or routing of incoming requests.

### What you get

* A consistent interface for inference across models.
* Low operational overhead for integrating scoring into flows.
* A path to scale scoring as part of your backend architecture.

### Design notes

Keep feature extraction close to the scoring call. Prefer deterministic inputs and explicit versions. This makes results easier to debug and audit.

For more details [click here](/broken/pages/cNRp2sZlzMyLGcNuHh6y).

## Seamless Integration with REST API-Based AI Services

Rierino integrates with REST API-based AI services, including OpenAI and others. This lets you access generative AI and predictive capabilities without heavy middleware.

It is useful when you want to adopt new models quickly. It is also useful when you need to call AI providers from controlled backend flows.

### Typical use cases

* Text generation, summarization, and rewriting in business workflows.
* Classification, extraction, and sentiment analysis at request time.
* Hybrid patterns that combine external LLM calls with internal data and rules.

### Why this approach scales

Calls to external AI services can be modeled like any other integration step. This keeps authentication, retries, transformation, and observability consistent. It also makes it easier to swap providers and evolve prompts over time.

### Operational considerations

External model calls have cost and latency implications. Treat prompts and model parameters as versioned configuration. Monitor both output quality and throughput over time.

For more details [click here](/broken/pages/UhdC9EjYpWaLh8PhyvdB).

## Custom Python ML Model Training and Scoring

You can also build custom ML capabilities in Python. This includes training, evaluation, and scoring. It is a good fit when you need domain-specific models or custom feature logic.

Python integration supports both experimentation and operational execution. It also makes it easier to reuse existing data science assets and libraries.

### Typical use cases

* Predictive analytics and forecasting.
* Customer segmentation and clustering.
* Custom scoring logic for operational optimization.
* Feature engineering pipelines used by multiple services.

### How it fits into the platform

Python procedures can be orchestrated alongside other steps. This includes calls to databases, rules, and external APIs. It keeps ML work connected to the same deployment and governance model.

### What this enables

* A clear path from notebooks to scheduled jobs.
* Shared scoring logic between batch and real-time flows.
* Custom libraries and domain-specific model pipelines.

For more details [click here](/broken/pages/PpEVmUyz1AQjjKom7mlO).
