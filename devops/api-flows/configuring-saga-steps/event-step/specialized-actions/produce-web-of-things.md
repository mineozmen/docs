---
description: >-
  These actions provide ability to produce WoT descriptions and endpoints from
  allowed sagas.
---

# Produce Web of Things

## Produce Web of Things Actions

### ReadThing

Creates and returns a Thing description with a list of actions to invoke and properties to read from allowed sagas. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example       | Default |
| -------------- | -------------------------------------------------- | ------------- | ------- |
| Domain         | State to read Thing from                           | custom\_thing | thing   |
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

* `title`
* `description`
* `baseURL`
* `securityScheme`
* `contentType`
* `actionsSagas` - list of saga ids used to invoke actions
* `propertiesSagas` - list of saga ids used to read properties
