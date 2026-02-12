---
description: >-
  This handler (com.rierino.handler.TransformEventHandler) provides ability to
  transform incoming event payload using various transformation classes.
---

# Transform Event

## Handler Parameters

This handler requires no parameters.

## Actions

### Transform

Applies requested transformation on an event. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter | Definition                                    | Example                                              | Default |
| --------- | --------------------------------------------- | ---------------------------------------------------- | ------- |
| Class     | Fully qualified name of the transformer class | com.rierino.handler.transform.ReducePayloadTransform | -       |
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
            "class": {
              "type": "string",
              "description": "Fully qualified name of the transformer class",
              "examples": [
                "com.rierino.handler.transform.ReducePayloadTransform"
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

{% hint style="info" %}
All transformer classes available in saga flows can be used with this handler, making it possible to distribute resource consuming transformations to different runners.
{% endhint %}
