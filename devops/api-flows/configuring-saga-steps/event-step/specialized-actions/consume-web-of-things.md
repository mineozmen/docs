---
description: >-
  These actions provide ability to consume WoT data and invoke actions on IoT
  and similar endpoints.
---

# Consume Web of Things

## Consume Web of Things Actions

### GetWOTEntry

Returns details of a Thing using its discovery endpoint. Uses `domain` event metadata to refer to the Thing ID.

### GetWOTProperty

Reads a specific property from a Thing. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example         | Default |
| -------------- | -------------------------------------------------- | --------------- | ------- |
| Domain         | ID of the Thing to read from                       | coffee\_machine | -       |
| Output Element | Json path for the output in response event payload | output          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things GetWOTProperty action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "ID of the \"Thing\" to read from",
          "example": "coffee_machine",
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

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                   | Example | Default |
| --------- | ---------------------------- | ------- | ------- |
| Property  | Name of the property to read | level   | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things GetWOTProperty action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "property": {
              "type": "string",
              "definition": "Name of the property to read",
              "example": "level",
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

### WriteWOTProperty

Writes a specific property to a Thing. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                             | Example         | Default |
| -------------- | ------------------------------------------------------ | --------------- | ------- |
| Domain         | ID of the Thing to write to                            | coffee\_machine | -       |
| Input Element  | Json path for the input in event payload to write from | input           | -       |
| Output Element | Json path for the output in response event payload     | output          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things WriteWOTProperty action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "ID of the \"Thing\" to write to",
          "example": "coffee_machine",
          "default": null
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload to write from",
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

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                    | Example | Default |
| --------- | ----------------------------- | ------- | ------- |
| Property  | Name of the property to write | level   | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things WriteWOTProperty action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "property": {
              "type": "string",
              "definition": "Name of the property to write",
              "example": "level",
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

### InvokeWOTAction

Invokes a specific action on a Thing. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                           | Example         | Default |
| -------------- | -------------------------------------------------------------------- | --------------- | ------- |
| Domain         | ID of the Thing to invoke action on                                  | coffee\_machine | -       |
| Input Element  | Json path for the input in event payload to use as action parameters | input           | -       |
| Output Element | Json path for the output in response event payload                   | output          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things InvokeWOTAction action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "ID of the \"Thing\" to invoke action on",
          "example": "coffee_machine",
          "default": null
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload to use as action parameters",
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

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                 | Example | Default |
| --------- | -------------------------- | ------- | ------- |
| Action    | Name of the action to call | cleanup | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Consume Web of Things InvokeWOTAction action eventMeta.parameters",
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
              "definition": "Name of the action to call",
              "example": "cleanup",
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
