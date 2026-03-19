---
description: These actions provide ability to execute stored scripts using Javax engines.
---

# Run Scripts

## Run Scripts Actions

### Process / ProcessScript

Passes the requested event to a dynamic event handler for processing. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter       | Definition                                  | Example         | Default |
| --------------- | ------------------------------------------- | --------------- | ------- |
| Dynamic Handler | Name of the code handler to execute request | custom\_handler | -       |
| Dynamic Action  | Name of the action to call on code handler  | CustomCalculate | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Scripts Process/ProcessScript action eventMeta.parameters",
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
              "definition": "Name of the code handler to execute request",
              "example": "custom_handler",
              "default": null
            },
            "dynamicAction": {
              "type": "string",
              "definition": "Name of the action to call on code handler",
              "example": "CustomCalculate",
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
    "product": {
        "id": "product-1",
        "data": {
            "name": "Test Product"
        }
    }
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (130).png>)

</details>

{% hint style="info" %}
All parameters applicable for the selected code handler are also passed on by `ScriptLoadedEventHandler`.
{% endhint %}
