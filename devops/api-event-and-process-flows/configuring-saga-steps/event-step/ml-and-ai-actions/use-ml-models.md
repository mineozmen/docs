---
description: >-
  These actions provide ability to run real-time inference using various machine
  learning libraries.
---

# Use ML Models

## Use ML Models Actions

### Score / ScoreML

Scores input data using a pretrained model managed by this handler. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | $.score    | -       |
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
          "examples": ["parameters"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["$.score"],
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
| Parameter      | Definition                                                          | Example                        | Default |
| -------------- | ------------------------------------------------------------------- | ------------------------------ | ------- |
| Input Pattern  | JMESPath pattern to apply on data input                             | \[ \[text] ]                   | -       |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | {totalPrice:basket.totalPrice} | -       |
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
              "description": "JMESPath pattern to apply on data input",
              "examples": ["[ [text] ]"],
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data output, before returning response",
              "examples": ["{totalPrice:basket.totalPrice}"],
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
