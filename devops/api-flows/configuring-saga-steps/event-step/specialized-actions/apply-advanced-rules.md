---
description: These actions provide ability to execute business rules using Drools BRMS.
---

# Apply Advanced Rules

## Apply Advanced Rules Actions

### Process / ProcessRules

Passes structured request data to Drools and returns rule evaluation results in the output. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | basket  | -       |
| Output Element | Json path for the output in response event payload | basket  | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Apply Advanced Rules action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "basket",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "basket",
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
{% tab title="Table" %}
| Parameter      | Definition                                                                                 | Example                        | Default |
| -------------- | ------------------------------------------------------------------------------------------ | ------------------------------ | ------- |
| Input Pattern  | JMESPath pattern to apply on data input                                                    | {items:items.productid}        | -       |
| Output Pattern | JMESPath pattern to apply on data output, before returning response                        | {totalPrice:basket.totalPrice} | -       |
| For Each       | Field for which a separate record should be passed on to Drools                            | items                          | -       |
| For Each Map   | Json pattern for mapping each record passed to Drools, allowing replication of common data | -                              | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Apply Advanced Rules action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on data input",
              "example": "{items:items.productid}",
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{totalPrice:basket.totalPrice}",
              "default": null
            },
            "forEach": {
              "type": "string",
              "definition": "Field for each a separate record should be passed on to Drools",
              "example": "items",
              "default": null
            },
            "forEachMap": {
              "type": "string",
              "definition": "Json pattern for mapping each record that should be passed on to Drools, allowing replication of common data",
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
