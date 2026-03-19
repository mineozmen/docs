---
description: >-
  ML & AI handlers allow incorporation of AI agent and machine learning
  capabilities
---

# ML & AI Handlers

ML & AI handlers add model inference, LLM orchestration, embeddings, and AI protocol support.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Built-in variants

Use [ML & AI Actions](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/) for action-level saga step fields and parameters.

### Use ML Models

Classes:

* `com.rierino.handler.tensor.TensorScoreEventHandler`
* `com.rierino.handler.onnx.OnnxScoreEventHandler`
* `com.rierino.handler.pmml.PMMLScoreEventHandler`
* `com.rierino.handler.opennlp.OpenNLPScoreEventHandler`

Runs real-time inference using pre-trained ML models.

#### Handler parameters

| Parameter            | Definition                               | Example          | Default          |
| -------------------- | ---------------------------------------- | ---------------- | ---------------- |
| `model.state`        | State manager with model configurations  | `model`          | -                |
| `model.id`           | Model id to run on this handler instance | `recommendation` | -                |
| `model.store.system` | System used to store model assets        | `hdfs_default`   | -                |
| `model.local.dir`    | Local directory used for model assets    | `/tmp`           | `java.io.tmpdir` |

This handler supports caching.

#### Runtime dependencies

TensorFlow:

```gradle
implementation (group:'com.rierino.custom', name: 'tensor', version:"${rierinoVersion}")
```

ONNX:

```gradle
implementation (group:'com.rierino.custom', name: 'onnx', version:"${rierinoVersion}")
```

PMML:

```gradle
implementation (group:'com.rierino.custom', name: 'pmml', version:"${rierinoVersion}")
```

OpenNLP:

```gradle
implementation (group:'com.rierino.custom', name: 'opennlp', version:"${rierinoVersion}")
```

Action details: [Use ML Models](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/use-ml-models.md)

### Use GenAI Models

Class: `com.rierino.handler.langchain4j.LangChainModelEventHandler`

Runs LLM chat and image generation flows through LangChain model providers.

#### Handler parameters

| Parameter      | Definition                                        | Example       | Default |
| -------------- | ------------------------------------------------- | ------------- | ------- |
| `model.state`  | State manager with model configurations           | `genai_model` | -       |
| `model.domain` | Model domain to include                           | `chat`        | -       |
| `saga.handler` | Saga handler used for tool sagas                  | -             | `saga`  |
| `saga.state`   | State manager with saga definitions used as tools | -             | `saga`  |

Action details: [Use GenAI Models](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/use-genai-models.md)

### Perform Text Embedding

Class: `com.rierino.handler.langchain4j.EmbeddingEventHandler`

Creates embeddings for search and retrieval use cases.

#### Handler parameters

For ONNX-based models:

| Parameter            | Definition                                 | Example           | Default                   |
| -------------------- | ------------------------------------------ | ----------------- | ------------------------- |
| `model.store.system` | System to fetch ONNX model files from      | `model_fs`        | default Hugging Face repo |
| `model.path`         | Model file path                            | `/model.onnx`     | default ONNX model        |
| `tokenizer.path`     | Tokenizer file path                        | `/tokenizer.json` | default tokenizer         |
| `model.local.dir`    | Local directory for downloaded model files | `/app/models`     | system temp dir           |

For custom embedding providers:

| Parameter          | Definition                                         | Example                                             | Default |
| ------------------ | -------------------------------------------------- | --------------------------------------------------- | ------- |
| `embedding.class`  | Java class implementing the embedding provider     | `dev.langchain4j.model.openai.OpenAiEmbeddingModel` | -       |
| `embeddingMethods` | Method configuration passed to the embedding class | `{"apiKey":"xxx"}`                                  | -       |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'langchain4j', version:"${rierinoVersion}")
```

Action details: [Perform Text Embedding](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/perform-text-embedding.md)

### Service MCP Requests

Class: `com.rierino.handler.mcp.McpServerEventHandler`

Exposes existing microservice capabilities over MCP.

#### Handler parameters

| Parameter       | Definition                                        | Example       | Default       |
| --------------- | ------------------------------------------------- | ------------- | ------------- |
| `server.state`  | State manager with MCP server configurations      | `mcp_server`  | `genai_model` |
| `server.domain` | Server domain to include                          | `procurement` | -             |
| `saga.handler`  | Saga handler used for tool sagas                  | `ai_saga`     | `saga`        |
| `saga.state`    | State manager with saga definitions used as tools | `ai_saga`     | `saga`        |

Action details: [Service MCP Requests](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/service-mcp-requests.md)

### Service A2A Requests

Class: `com.rierino.handler.a2a.A2AServerEventHandler`

Exposes existing microservice capabilities over A2A.

#### Handler parameters

| Parameter       | Definition                                   | Example       | Default       |
| --------------- | -------------------------------------------- | ------------- | ------------- |
| `server.state`  | State manager with A2A server configurations | `mcp_server`  | `genai_model` |
| `server.domain` | Server domain to include                     | `procurement` | -             |
| `saga.handler`  | Saga handler used for tool sagas             | `ai_saga`     | `saga`        |

Action details: [Service A2A Requests](../../../api-flows/configuring-saga-steps/event-step/ml-and-ai-actions/service-a2a-requests.md)
