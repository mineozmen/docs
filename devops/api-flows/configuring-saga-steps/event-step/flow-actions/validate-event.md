---
description: >-
  These actions provide ability to check validity of event contents using
  different validator types.
---

# Validate Event

## Validate Event Actions

### Validate

Checks validity of the event input payload using the configured validator instance and returns results.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                             | Example    | Default |
| -------------- | -------------------------------------- | ---------- | ------- |
| Input Element  | Json path in payload to use            | parameters | -       |
| Output Element | Json path in payload to return results | result     | -       |
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
          "definition": "Json path in payload to use",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path in payload to return results",
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
| Parameter      | Definition                                        | Example             | Default |
| -------------- | ------------------------------------------------- | ------------------- | ------- |
| Output Pattern | JMESPath expression to convert results            | -                   | -       |
| \*             | Additional parameters used by the validator class | pattern=(value='a') | -       |
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
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath expression to convert results",
              "example": null,
              "default": null
            },
            "*": {
              "type": "string",
              "definition": "Additional parameters used by the validator class",
              "example": "pattern=(value='a')",
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
