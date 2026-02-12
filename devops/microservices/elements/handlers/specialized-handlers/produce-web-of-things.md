---
description: >-
  This handler (com.rierino.handler.wot.WOTProducerEventHandler) provides
  ability to consume WoT data and invoke actions on IoT and similar endpoints
---

# Produce Web of Things

## Handler Parameters

| Parameter  | Definition                                      | Example | Default |
| ---------- | ----------------------------------------------- | ------- | ------- |
| saga.state | Name of the state manager with saga definitions | -       | saga    |

## Actions

### ReadThing

Creates and returns a "Thing" description with a list of actions to invoke and properties to read from allowed sagas. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example       | Default |
| -------------- | -------------------------------------------------- | ------------- | ------- |
| Domain         | State to read "Thing" from                         | custom\_thing | thing   |
| Input Element  | Json path for the id field in event payload        | input         | -       |
| Output Element | Json path for the output in response event payload | output        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Produce Web of Things ReadThing action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "State to read \"Thing\" from",
          "example": "custom_thing",
          "default": "thing"
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the id field in event payload",
          "example": "input",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "output",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Thing definitions are stored in state managers with the following data elements:

* title
* description
* baseURL
* securityScheme
* contentType
* actionsSagas - list of saga ids that will be used to invoke actions
* propertiesSagas - list of saga ids that will be used to read properties
