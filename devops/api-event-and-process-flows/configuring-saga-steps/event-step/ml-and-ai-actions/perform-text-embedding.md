---
description: >-
  These actions provide ability to convert text into embeddings for advanced
  search functionality.
---

# Perform Text Embedding

## Perform Text Embedding Actions

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
          "examples": ["input"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
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
| Parameter     | Definition                                                                                        | Example                  | Default |
| ------------- | ------------------------------------------------------------------------------------------------- | ------------------------ | ------- |
| Input Pattern | JMESpath pattern to apply on input element for getting text and metadata fields                   | {text: "", metadata: ""} | -       |
| Extra Action  | Optional extra action after generating embedding, `add` to local index or `search` in local index | add                      | -       |
| Max Results   | Maximum results to return if `search` extra action is used                                        | 5                        | -       |
| Min Score     | Minimum similarity score required if `search` extra action is used                                | 0.5                      | -       |
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
              "examples": ["{text: \"\", metadata: \"\"}"],
              "default": null
            },
            "extraAction": {
              "type": "string",
              "description": "Optional extra action to perform after generating embedding",
              "examples": ["add"],
              "default": null
            },
            "maxResults": {
              "type": "integer",
              "description": "Maximum results to return if search extra action is used",
              "examples": [5],
              "default": null
            },
            "minScore": {
              "type": "number",
              "description": "Minimum similarity score required if search extra action is used",
              "examples": [0.5],
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

Input element can be a text value, or an object in `{chat, message}` format where `chat` includes the ID of an ongoing chat for assistant-style use cases with memory.
