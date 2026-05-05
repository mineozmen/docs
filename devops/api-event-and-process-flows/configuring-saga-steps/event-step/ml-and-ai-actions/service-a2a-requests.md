---
description: >-
  These actions provide ability to expose existing microservice capabilities
  over A2A.
---

# Service A2A Requests

## Service A2A Requests Actions

### GetA2A

Responds with the agent card configuration for `.well-known/agent.json` calls.

### CallA2A

Responds to a request using A2A protocol. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                                              | Example                 | Default |
| -------------- | ----------------------------------------------------------------------- | ----------------------- | ------- |
| Domain         | ID of the A2A server to interact with                                   | procurement\_specialist | -       |
| Input Element  | Json path for the input in request event payload in A2A JSON-RPC format | message                 | -       |
| Output Element | Json path for the output in response event payload                      | output                  | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "description": "ID of the A2A server to interact with",
          "examples": ["procurement_specialist"],
          "default": null
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload in A2A JSON-RPC format",
          "examples": ["message"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
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
{% tab title="Parameters (table)" %}
| Parameter     | Definition                                                   | Example | Default |
| ------------- | ------------------------------------------------------------ | ------- | ------- |
| Input Pattern | Jmespath expression for converting input payload to A2A call | -       | -       |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
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
            "inputPattern": {
              "type": "string",
              "description": "Jmespath expression for converting input payload to A2A call",
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
