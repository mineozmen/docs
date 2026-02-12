---
description: >-
  This handler (com.rierino.handler.ForEachEventHandler) provides ability to
  repeat an action for each entry in a specific payload element.
---

# Loop Each Entry

## Handler Parameters

This handler requires no parameters.

## Actions

### RunEach

Repeats an action multiple times, each with an assigned part of the payload. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter    | Definition                                                                         | Example                                              | Default |
| ------------ | ---------------------------------------------------------------------------------- | ---------------------------------------------------- | ------- |
| For Each     | Json path in payload to array entries for repetition                               | parameters.productids                                | list    |
| Parallel     | Whether actions should run in parallel or not                                      | true                                                 | false   |
| As           | Field name to set results from each action                                         | output                                               | \_      |
| Inner Fields | Event metadata fields for each action execution (e.g. action, handler, parameters) | <p>action=CallRest</p><p>parameters.url=/product</p> | -       |
{% endtab %}

{% tab title="JSON Schema" %}
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
            "forEach": {
              "type": "string",
              "definition": "Json path in payload to array entries for repetition",
              "example": "parameters.productids",
              "default": "list"
            },
            "parallel": {
              "type": "boolean",
              "definition": "Whether actions should run in parallel or not",
              "example": true,
              "default": false
            },
            "as": {
              "type": "string",
              "definition": "Field name to set results from each action",
              "example": "output",
              "default": "_"
            },
            "inner_[field]": {
              "type": "string",
              "definition": "Event metadata fields for each action execution (e.g. action, handler, parameters)",
              "example": "action=CallRest\\nparameters.url=/product",
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

<details>

<summary>Example</summary>

Input

```json
{
    "translations": ['enUS', 'deDE', 'frFR'],
    "text": "Hello World"
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (131).png>)

</details>
