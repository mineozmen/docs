---
description: >-
  These actions provide ability to load and execute dynamic Java packages during
  runtime.
---

# Run Java Package

## Run Java Package Actions

### Process / ProcessPackage

Passes the requested event to a dynamic event handler for processing. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter       | Definition                             | Example            | Default |
| --------------- | -------------------------------------- | ------------------ | ------- |
| Dynamic Handler | Name of the handler to execute request | basket\_operations | -       |
| Dynamic Action  | Name of the action to call on handler  | PriceBasket        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Java Package Process/ProcessPackage action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "dynamicHandler": {
              "type": "string",
              "definition": "Name of the handler to execute request",
              "example": "basket_operations",
              "default": null
            },
            "dynamicAction": {
              "type": "string",
              "definition": "Name of the action to call on handler",
              "example": "PriceBasket",
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
All parameters applicable for the selected handler are also passed on by `JarLoadedEventHandler`.
{% endhint %}
