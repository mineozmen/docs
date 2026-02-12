---
description: >-
  This handler (com.rierino.handler.camel.CamelEventHandler) provides ability to
  use Apache Camel producers for 3rd party integrations.
---

# Integrate with Camel

## Handler Parameters

This handler has no parameters, it uses all systems configured for the runner as potential producers.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.runner', name: 'camel', version:"${rierinoVersion}")
```

## Actions

### SendMessage

Sends a message to a Camel producer and return response (if target system has output). Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Domain         | System name to use as target                       | mq         | -       |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | result     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Integrate with Camel SendMessage action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "System name to use as target",
          "example": "mq",
          "default": null
        },
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

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter    | Definition                          | Example | Default |
| ------------ | ----------------------------------- | ------- | ------- |
| Exchange     | Camel message exchange pattern      | InOut   | -       |
| Camel Header | Header parameters to send to target | ...     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Integrate with Camel SendMessage action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "exchange": {
              "type": "string",
              "definition": "Camel message exchange pattern",
              "example": "InOut",
              "default": null
            },
            "camelHeader": {
              "type": "object",
              "definition": "Header parameters to send to target (represents camelHeader.*)",
              "example": {
                "myHeader": "value"
              },
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
