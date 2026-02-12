---
description: >-
  This handler (com.rierino.handler.langchain4j.EmbeddingEventHandler) provides
  ability to convert text into embedding for advanced search functionality
---

# Perform Text Embedding

## Handler Parameters

For Onnx models available on a target system:

| Parameter          | Definition                                        | Example         | Default                    |
| ------------------ | ------------------------------------------------- | --------------- | -------------------------- |
| model.store.system | Name of the system to get Onnx model details from | model\_fs       | A default huggingface repo |
| model.path         | File path for the model to use                    | /model.onnx     | A default ONNX model       |
| tokenizer.path     | File path for the tokenizer to use                | /tokenizer.json | A default tokenizer        |
| model.local.dir    | Local directory to store downloaded model files   | /app/models     | System temp dir            |

For non-Onnx models serviced through custom embedding classes:

| Parameter        | Definition                                                      | Example                                           | Default |
| ---------------- | --------------------------------------------------------------- | ------------------------------------------------- | ------- |
| embedding.class  | Java class for using a custom embedding provider                | dev.langchain4j.model.openai.OpenAiEmbeddingModel | -       |
| embeddingMethods | Json object configuration of methods to call on embedding class | {"apiKey": "xxx"}                                 | -       |

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md) as well as any custom embedding class dependencies:

```gradle
implementation (group:'com.rierino.custom', name: 'langchain4j', version:"${rierinoVersion}")
```

## Actions

### EmbedText

Transforms text into an embedding. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | input   | -       |
| Output Element | Json path for the output in response event payload | output  | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "examples": [
            "input"
          ],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": [
            "output"
          ],
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter     | Definition                                                                                                  | Example                  | Default |
| ------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------ | ------- |
| Input Pattern | JMESpath pattern to apply on input element for getting text and metadata fields                             | {text: "", metadata: ""} | -       |
| Extra Action  | Optional extra action to perform after generating embedding ("add" to local index, "search" in local index) | add                      | -       |
| Max Results   | Maximum results to return if "search" extra action is used                                                  | 5                        | -       |
| Min Score     | Minimum similarity score required if "search" extra action is used                                          | 0.5                      | -       |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "inputPattern": {
              "type": "string",
              "description": "JMESpath pattern to apply on input element for getting text and metadata fields",
              "examples": [
                "{text: \"\", metadata: \"\"}"
              ],
              "default": null
            },
            "extraAction": {
              "type": "string",
              "description": "Optional extra action to perform after generating embedding (\"add\" to local index, \"search\" in local index)",
              "examples": [
                "add"
              ],
              "default": null
            },
            "maxResults": {
              "type": "integer",
              "description": "Maximum results to return if \"search\" extra action is used",
              "examples": [
                5
              ],
              "default": null
            },
            "minScore": {
              "type": "number",
              "description": "Minimum similarity score required if \"search\" extra action is used",
              "examples": [
                0.5
              ],
              "default": null
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Input element can be a text value, or an object in {chat, message} format, where "chat" field includes ID of an ongoing chat for an assistant type use case with memory.
