---
description: >-
  This handler (com.rierino.handler.ScriptLoadedEventHandler) provides ability
  to execute stored scripts using Javax engines.
---

# Run Scripts

## Handler Parameters

| Parameter   | Definition                                                                       | Example             | Default       |
| ----------- | -------------------------------------------------------------------------------- | ------------------- | ------------- |
| code.state  | Name of the state manager storing code definitions                               | customJSCodes       | handler\_code |
| code.domain | Category of codes within the code state manager that can be used by this handler | custom\_calculation | -             |

{% file src="../../../../../.gitbook/assets/handler-script-0001.json" %}
Example Script Handler Definition (Can be Imported on Element Screen)
{% endfile %}

## Actions

### Process / ProcessScript

Passes requested event to dynamic event handler for processing. Event metadata parameters applicable for this action are as follows:

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
All parameters applicable for the selected code handler are also passed on by the ScriptLoadedEventHandler.

Different scripting languages such as Groovy, JavaScript, etc. can be supported using Javax classes. Engine and code details are stored in code.state and automatically recompiled / updated on change.
{% endhint %}

{% file src="../../../../../.gitbook/assets/transform_product_shopify.json" %}
Example Code Definition (Can be Imported on Handler Code Screen)
{% endfile %}
