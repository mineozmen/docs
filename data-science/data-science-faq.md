---
description: >-
  This FAQ covers the core Data Science concepts, model governance approach, and
  when to use ML, GenAI, MCP, CEP, and visualizations
icon: comment-question
---

# Data Science FAQ

## Frequently asked questions

### What is the Data Science app responsible for?

<details>

<summary>Show answer</summary>

Data Science is where you configure and govern reusable AI, ML, and analytics assets.

It centralizes model settings, execution parameters, and related definitions so they can be invoked from APIs, batch jobs, or stream processing flows.

See [Overview](data-science-overview.md).

</details>

### What are the main building blocks in Data Science?

<details>

<summary>Show answer</summary>

Think in five main groups:

* **ML Models** for traditional model configuration and execution settings
* **GenAI Models** for LLMs, agents, prompts, tools, and governance
* **MCP Servers** for exposing existing platform capabilities over MCP
* **Complex Event Processing** for real-time stream logic and derived signals
* **Data Visualizations** for dashboards and reporting views

See [Overview](data-science-overview.md).

</details>

### How is Data Science different from Devops?

<details>

<summary>Show answer</summary>

Data Science defines governed AI, ML, and analytics assets.

Devops provides the runners, sagas, gateways, and deployments that invoke them.

A simple mental model is:

* **Data Science** defines the model or analytical asset
* **Devops** decides where and how that asset is executed

See [Devops FAQ](../devops/devops-faq.md).

</details>

### How is Data Science different from Configuration?

<details>

<summary>Show answer</summary>

Configuration stores reusable logic such as queries, rules, and dynamic handlers.

Data Science stores reusable analytical and AI assets such as models, agents, CEP flows, and dashboards.

They often work together, but they solve different layers of runtime behavior.

See [Configuration FAQ](../configuration/configuration-faq.md).

</details>

### What is an ML model in Rierino?

<details>

<summary>Show answer</summary>

An ML model definition stores the metadata and parameters needed to run or schedule traditional machine learning logic.

It can include versioning, assets, scheduling, parameters, and step-based pipelines.

This keeps inference and training settings consistent across environments.

See [ML Models](ml-models/).

</details>

### What is a GenAI model in Rierino?

<details>

<summary>Show answer</summary>

A GenAI model definition governs an LLM-backed capability.

It includes provider settings, prompts, memory, tool access, interfaces, and runner-level access control.

This is the main starting point for AI agents and governed LLM usage on the platform.

See [GenAI Models](genai-models/).

</details>

### When should I use ML Models versus GenAI Models?

<details>

<summary>Show answer</summary>

Use **ML Models** for predictive scoring, classification, regression, feature pipelines, and similar structured model execution.

Use **GenAI Models** for natural language interaction, content generation, tool-using agents, and LLM-driven reasoning.

If the output is primarily statistical prediction, start with ML. If it is conversational or prompt-driven, start with GenAI.

</details>

### What is an AI agent in this model?

<details>

<summary>Show answer</summary>

An AI agent is a governed GenAI model with access to approved tools and context.

Those tools can include sagas, states, systems, prompts, and interfaces.

That lets the agent do more than generate text. It can perform controlled business actions.

</details>

### How is tool access controlled for GenAI agents?

<details>

<summary>Show answer</summary>

Tool access is explicitly configured.

A model can be allowed to call selected sagas, read or write selected states, interact with selected systems, and use selected scripts or things.

Governance settings also limit repeated calls and excessive tool loops.

See [GenAI Models](genai-models/).

</details>

### What are AI guardrails in Rierino?

<details>

<summary>Show answer</summary>

AI guardrails are reusable safety and governance checks for GenAI agents.

They can inspect user prompts, model outputs, and tool responses inside the agent loop.

This gives you a central place to block, modify, or reprompt unsafe content.

See [AI Guardrails](genai-models/ai-guardrails.md).

</details>

### When should I add an input guardrail versus an output guardrail?

<details>

<summary>Show answer</summary>

Use **input guardrails** to stop unsafe prompts before the model or tools see them.

Use **output guardrails** to review or rewrite model responses before they reach the user.

Use **tool response guardrails** when retrieved or tool-generated data should be checked before it goes back into the model context.

</details>

### Can I use the same guardrail model across all stages?

<details>

<summary>Show answer</summary>

Yes.

Input, output, and tool-response guardrails use the same configuration model.

What usually changes is the stage-specific risk policy and how strict the thresholds are.

</details>

### When should I mask content instead of blocking it?

<details>

<summary>Show answer</summary>

Mask content when the request is still useful after redaction.

This is common for PII such as emails, phone numbers, or account identifiers.

Block content when the content itself is unsafe, such as prompt injection attempts or dangerous commands.

</details>

### What does `REPROMPT` do in AI guardrails?

<details>

<summary>Show answer</summary>

`REPROMPT` asks the model to generate a safer answer instead of failing immediately.

This is most useful on output checks where the response can be recovered.

Use `BLOCK` when the request should stop instead of retrying.

</details>

### How does the risk policy affect blocking?

<details>

<summary>Show answer</summary>

Each finding contributes a risk level such as `LOW`, `MEDIUM`, `HIGH`, or `CRITICAL`.

The risk policy then combines those findings using weighted scores, thresholds, critical overrides, or count-based rules.

This lets you tune strictness without rewriting every guardrail.

</details>

### Should I start with presets or custom guardrail patterns?

<details>

<summary>Show answer</summary>

Start with presets for common cases such as PII redaction or jailbreak protection.

Use custom patterns when your policy is domain-specific or more precise than the built-in presets.

You can also combine presets and custom rules in the same stage.

</details>

### Do AI guardrails replace custom governance in AI flows?

<details>

<summary>Show answer</summary>

No.

AI guardrails provide a reusable built-in governance layer for common safety controls.

If you need fully custom behavior, you can still embed checks directly in your AI flow logic.

</details>

### Do GenAI capabilities have special runtime requirements?

<details>

<summary>Show answer</summary>

Yes.

GenAI-enabled runners usually need more memory and are best kept separate from lightweight general-purpose runners.

This helps isolate resource-heavy agent workloads and keeps standard services lean.

</details>

### What is an MCP server in Rierino?

<details>

<summary>Show answer</summary>

An MCP server maps existing platform capabilities into the Model Context Protocol.

In practice, it exposes selected sagas, prompts, and resources so MCP clients can consume them as tools or context.

See [MCP Servers](mcp-servers.md).

</details>

### When should I use an MCP server instead of a GenAI model?

<details>

<summary>Show answer</summary>

Use a **GenAI model** when you need an LLM-backed agent or prompt-driven capability.

Use an **MCP server** when you want to expose existing tools and resources in a standard protocol for external MCP clients.

They complement each other. One governs model behavior. The other exposes capabilities.

</details>

### What is Complex Event Processing used for?

<details>

<summary>Show answer</summary>

Complex Event Processing is for real-time stream analysis.

Use it when you need windowed aggregations, event correlation, continuous enrichment, or derived signals based on incoming data streams.

It is a better fit than request-response APIs when logic depends on continuous event flow.

See [Complex Event Processing](complex-event-processing/).

</details>

### How is CEP different from a normal saga?

<details>

<summary>Show answer</summary>

A saga is request or process orchestration.

CEP is continuous stream processing.

Use a saga for step-based business flows. Use CEP when data keeps arriving and the system must evaluate patterns or aggregates over time.

</details>

### What are data visualizations for?

<details>

<summary>Show answer</summary>

Data visualizations are embedded dashboards and reporting views.

They let teams present metrics, trends, charts, and tables using configured layouts and linked data sources.

This is useful for operational visibility and business-facing reporting inside the platform.

See [Data Visualizations](data-visualizations.md).

</details>

### Are Data Science assets only for batch workloads?

<details>

<summary>Show answer</summary>

No.

They can be used in real-time APIs, in background processes, or in stream-oriented pipelines.

A saga can call an ML or GenAI handler directly. A batch process can train or backfill. A CEP flow can compute live signals.

See [Overview](data-science-overview.md).

</details>

### Why keep models and AI settings in one central app?

<details>

<summary>Show answer</summary>

It improves governance, reuse, and rollout safety.

Teams can version assets, review changes, control access, and update execution settings without scattering model logic across many services.

</details>

### How does Data Science relate to Design?

<details>

<summary>Show answer</summary>

Design builds the admin UI and screens.

Data Science provides model-driven capabilities those screens may call or display.

For example, a UI can trigger an agent interaction, render an analytics view, or display outputs from an ML-backed endpoint.

See [Design FAQ](../design/design-faq.md).

</details>

### What should I understand first if I am new to Data Science?

<details>

<summary>Show answer</summary>

Start with these concepts:

* **ML model** for traditional predictive logic
* **GenAI model** for LLM and agent behavior
* **MCP server** for protocol-based capability exposure
* **CEP flow** for continuous event analysis
* **Visualization** for reporting and dashboards

Once these are clear, the rest of the section becomes much easier to navigate.

</details>

### Where should I go next for deeper Data Science FAQs?

<details>

<summary>Show answer</summary>

Start with the subsection that matches your goal:

* Need governed predictive models → [ML Models](ml-models/)
* Need agents, prompts, or LLM governance → [GenAI Models](genai-models/)
* Need MCP exposure for tools and resources → [MCP Servers](mcp-servers.md)
* Need streaming analytics → [Complex Event Processing](complex-event-processing/)
* Need embedded reporting → [Data Visualizations](data-visualizations.md)

Subsection-specific FAQs can then go deeper into model lifecycle, agent setup, prompts, tool governance, CEP patterns, and dashboard design.

</details>
