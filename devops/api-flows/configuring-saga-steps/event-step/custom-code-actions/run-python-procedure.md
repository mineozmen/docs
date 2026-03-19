---
description: These actions provide ability to handle events using Python libraries.
---

# Run Python Procedure

## Run Python Procedure Actions

### Process / ProcessPython

Passes the requested event to a Python interface for processing. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                             | Example    | Default |
| -------------- | -------------------------------------- | ---------- | ------- |
| Input Element  | Json path of payload input to pass     | parameters | -       |
| Output Element | Json path for output to add on payload | $          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure Process/ProcessPython action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path of payload input to pass",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for output to add on payload",
          "example": "$",
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
| Parameter      | Definition                                 | Example         | Default |
| -------------- | ------------------------------------------ | --------------- | ------- |
| Input Pattern  | JMESPath pattern to apply on input element | {data:contents} | -       |
| Python Package | Name of the Python package to use          | rierino\_runner | -       |
| Python Module  | Name of the Python module to use           | CustomHandler   | -       |
| Python Action  | Name of the Python function to call        | Calculate       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure Process/ProcessPython action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on input element",
              "example": "{data:contents}",
              "default": null
            },
            "pyPackage": {
              "type": "string",
              "definition": "Name of the Python package to use",
              "example": "rierino_runner",
              "default": null
            },
            "pyModule": {
              "type": "string",
              "definition": "Name of the Python module to use",
              "example": "CustomHandler",
              "default": null
            },
            "pyAction": {
              "type": "string",
              "definition": "Name of the Python function to call",
              "example": "Calculate",
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
The Python function receives the Java handler, the processed input as a dict, and the event metadata parameters.
{% endhint %}
