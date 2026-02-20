---
description: >-
  Rierino provides low/no-code capabilities for customizing and managing data
  science models.
icon: circle-info
---

# Overview

The Data Science app is where you configure and govern ML and GenAI capabilities in a reusable way. You define model parameters, execution settings, and supporting assets so they can be invoked from real-time APIs (runners + sagas) or from batch jobs (Python processes, schedulers, and offline pipelines).

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption><p>Data Science App</p></figcaption></figure>

Data Science capabilities are organized into these areas:

* **ML Models:** Configure traditional machine learning models, their inputs, and runtime settings in [ML Models](ml-models/). Use this when you want consistent inference behavior across environments.
* **GenAI Models:** Manage LLM providers, credentials, and agent-facing configuration in [GenAI Models](genai-models/). This is the starting point for agent APIs and GenAI troubleshooting.
* **MCP Servers:** Expose existing platform capabilities over MCP by configuring [MCP Servers](mcp-servers.md). Use this when you want tools and microservices to be consumable by MCP clients.
* **Complex Event Processing:** Define real-time stream logic and windowed aggregations in [Complex Event Processing](complex-event-processing/). This is typically used for CEP and near-real-time enrichment pipelines.
* **Data Visualizations:** Publish embedded dashboards and reporting views through [Data Visualizations](data-visualizations.md). Use this for observability of outcomes and business-facing reporting.

In practice, you configure assets here and invoke them elsewhere. A saga step might call an ML/GenAI handler for inference, a batch task might train or backfill features, and CEP flows might continuously compute derived signals. Keeping those definitions centralized makes changes traceable and easier to roll out safely.
