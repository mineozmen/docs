---
description: >-
  These actions reorder candidate documents by their relevance to a query, to
  improve search and RAG retrieval quality.
---

# Rerank Documents

## Rerank Documents Actions

### **Rerank**

Scores candidate documents against a query and returns them ordered by relevance. Event metadata fields applicable for this action are as follows:

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
| Parameter     | Definition                                                                 | Example                     | Default |
| ------------- | -------------------------------------------------------------------------- | --------------------------- | ------- |
| Input Pattern | JMESpath pattern to apply on input element for getting query and documents | {query: q, documents: hits} | -       |
| Max Results   | Maximum number of reranked documents to return                             | 5                           | -       |
| Min Score     | Minimum relevance score required for a document to be returned             | 0.3                         | -       |
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
              "description": "JMESpath pattern to apply on input element for getting query and documents",
              "examples": ["{query: q, documents: hits}"],
              "default": null
            },
            "maxResults": {
              "type": "integer",
              "description": "Maximum number of reranked documents to return",
              "examples": [5],
              "default": null
            },
            "minScore": {
              "type": "number",
              "description": "Minimum relevance score required for a document to be returned",
              "examples": [0.3],
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

The input element (after the input pattern is applied) must be in `{query, documents}` format. `query` is the search text. `documents` is a non-empty array whose items are plain strings or objects in `{text, metadata}` format:

```json
{
  "query": "how do I get a refund?",
  "documents": [
    "Shipping usually takes 3-5 business days.",
    {"text": "Refunds are issued within 14 days of return.", "metadata": {"id": "faq-12"}}
  ]
}
```

Reranked documents are written to `results` in the output element, highest relevance first. Each entry is in `{score, index, text, metadata}` format:

* `index` is the document's position in the input array.
* `metadata` is passed through unchanged from object documents.
