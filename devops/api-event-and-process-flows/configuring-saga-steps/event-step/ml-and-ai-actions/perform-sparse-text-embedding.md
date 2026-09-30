---
description: >-
  These actions convert text into sparse (lexical) embeddings for keyword-aware
  and hybrid search.
---

# Perform Sparse Text Embedding

## Perform Sparse Text Embedding Actions

### **SparseEmbedText**

Transforms text into a sparse embedding. Event metadata fields applicable for this action are as follows:

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
| Parameter     | Definition                                                                                          | Example                  | Default |
| ------------- | --------------------------------------------------------------------------------------------------- | ------------------------ | ------- |
| Input Pattern | JMESpath pattern to apply on input element for getting text and metadata fields                     | {text: "", metadata: ""} | -       |
| Output Format | Format of the sparse vector, `indices` for index/value arrays or `tokens` for a token to weight map | tokens                   | indices |
| Extra Action  | Optional extra action after generating embedding, `add` to local index or `search` in local index   | search                   | -       |
| Max Results   | Maximum results to return if `search` extra action is used                                          | 5                        | -       |
| Min Score     | Minimum dot-product score required if `search` extra action is used                                 | 2.5                      | -       |
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
            "outputFormat": {
              "type": "string",
              "description": "Format of the sparse vector in the output",
              "enum": ["indices", "tokens"],
              "examples": ["tokens"],
              "default": "indices"
            },
            "extraAction": {
              "type": "string",
              "description": "Optional extra action to perform after generating embedding",
              "enum": ["add", "search"],
              "examples": ["search"],
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
              "description": "Minimum dot-product score required if search extra action is used",
              "examples": [2.5],
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

The input element (after the input pattern is applied) must be an object with a `text` field and an optional `metadata` object. The metadata is stored with the embedding when `add` is used, and returned with search results.

The sparse vector is written to `embedding` in the output element:

* `indices` format: `{"indices": [1012, 2054, ...], "values": [0.83, 0.41, ...]}`
* `tokens` format: `{"refund": 1.42, "payment": 0.87, ...}`, sorted with the highest weight first. This format fits fields such as Elastic `sparse_vector`.

With `add`, the ID of the stored embedding is written to `embeddingId`. With `search`, matches are written to `results` as `{score, embeddingId, text, metadata}` entries.

{% hint style="info" %}
Sparse scores are dot products, so they are non-negative and have no upper bound, unlike the cosine similarity scores of dense embeddings. Set `Min Score` for your model and data rather than on a 0–1 scale.
{% endhint %}
