---
description: >-
  This handler (com.rierino.handler.OpenHFTEventHandler) provides ability to
  load and execute dynamic Java event handler codes during runtime.
---

# Run Java Code

## Handler Parameters

| Parameter      | Definition                                                                       | Example            | Default        |
| -------------- | -------------------------------------------------------------------------------- | ------------------ | -------------- |
| code.state     | Name of the state manager storing code definitions                               | customCodes        | handler\_code  |
| code.domain    | Category of codes within the code state manager that can be used by this handler | basket\_operations | -              |
| code.local.dir | Local directory to store dynamic codes                                           | /tmp               | java.io.tmpdir |

## Actions

### Process / ProcessCode

Passes requested event to dynamic event handler for processing. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter       | Definition                                  | Example            | Default |
| --------------- | ------------------------------------------- | ------------------ | ------- |
| Dynamic Handler | Name of the code handler to execute request | basket\_operations | -       |
| Dynamic Action  | Name of the action to call on code handler  | PriceBasket        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Java Code Process/ProcessCode action eventMeta.parameters",
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
              "example": "basket_operations",
              "default": null
            },
            "dynamicAction": {
              "type": "string",
              "definition": "Name of the action to call on code handler",
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
All parameters applicable for the selected code handler are also passed on by the OpenHFTEventHandler.
{% endhint %}

## Codes

Handler codes provide ultimate flexibility in deploying new codes during runtime. These codes are typically deployed using an OpenHFTEventHandler and can be automatically recompiled and put to use in production with a code pulse or journal.

It is possible to extend the languages that are supported for dynamic handler codes with new event handlers. Rierino is shipped with dynamic code deployment for Java with the following properties:

* **Name**: Descriptive name of the handler code
* **Status**: Whether the handler code should still execute or not
* **Description**: Detailed explanation of the code contents
* **Domain**: Business domain for the handler code
* **Class**: Fully classified classname for the handler code (e.g. com.rierino.handler.dynamic.ExampleEventHandler)
* **Dependencies**: List of Jar files and locations of dependencies of this code, which are also loaded during runtime
* **Code**: Complete code for the handler implementation

{% hint style="info" %}
While using a pre-compiled deployment is a better practice, dynamic handler coding allows for faster deployment of mission critical changes or simpler CI/CD processes during development.
{% endhint %}
