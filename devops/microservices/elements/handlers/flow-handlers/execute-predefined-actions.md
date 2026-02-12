---
description: >-
  This handler (com.rierino.handler.ActionEventHandler) provides ability to
  execute predefined reusable actions as saga steps.
---

# Execute Predefined Actions

## Handler Parameters

| Parameter    | Definition                              | Example        | Default |
| ------------ | --------------------------------------- | -------------- | ------- |
| action.state | State manager that keeps action records | custom\_action | action  |

## Actions

### TakeAction

Calls a predefined action using given parameters and event payload. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | result     | -       |
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
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "result",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With the following event metadata parameters:

{% tabs %}
{% tab title="Table" %}
| Parameter      | Definition                                                                          | Example         | Default |
| -------------- | ----------------------------------------------------------------------------------- | --------------- | ------- |
| Action         | ID of the action to call                                                            | list\_customers | -       |
| Input Pattern  | JMESPath pattern to apply on data input                                             | -               | -       |
| Output Pattern | JMESPath pattern to apply on data output, before returning response                 | -               | -       |
| \*             | Any parameter to replace in action configuration mapping to %%param\_\*%% variables | domain=test     | -       |
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
            "action": {
              "type": "string",
              "definition": "ID of the action to call",
              "example": "list_customers",
              "default": null
            },
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data input",
              "example": null,
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": null,
              "default": null
            },
            "*": {
              "type": "string",
              "definition": "Any parameter to replace in action configuration mapping to %%param_*%% variables",
              "example": "domain=test",
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
