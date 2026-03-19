---
description: >-
  These actions provide ability to coordinate end-to-end API calls, locally
  executing or distributing microservice steps across the platform.
---

# Orchestrate Saga

## Orchestrate Saga Actions

### StartSaga

Initiates execution of a new saga, using `sagaPath` or `sagaId` parameter of the requesting event, or the request metadata path if not defined. The request is accepted only if there is an active saga matching this path.

When called from within another saga, following event metadata fields are applicable for this action:

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
| Parameter      | Definition                                                          | Example         | Default |
| -------------- | ------------------------------------------------------------------- | --------------- | ------- |
| Saga           | ID of the saga to call                                              | list\_customers | -       |
| Saga Path      | Path of the saga to call                                            | /ListCustomers  |         |
| Input Pattern  | JMESPath pattern to apply on data input                             | -               | -       |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | -               | -       |
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
            "sagaId": {
              "type": "string",
              "definition": "ID of the saga to call",
              "example": "list_customers",
              "default": null
            },
            "sagaPath": {
              "type": "string",
              "definition": "Path of the saga to call",
              "example": "/ListCustomers",
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
Nested sagas should only be used within the same runner as they do not replicate original event payload and metadata during execution for stateless distribution.
{% endhint %}

### StepSaga

Executes the next action after a saga step is completed by an event runner, using the saga id and step id in event saga metadata.

{% hint style="info" %}
Unlike the other event handlers, SagaEventHandler is rarely called from within saga flows, as it acts as a coordinator rather than a simple task executor.
{% endhint %}
