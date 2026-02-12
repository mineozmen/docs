---
description: >-
  This handler (com.rierino.handler.ValidationEventHandler) provides ability to
  check validity of event contents using different validator types.
---

# Validate Event

## Handler Parameters

| Parameter    | Definition                                          | Example                                                | Default |
| ------------ | --------------------------------------------------- | ------------------------------------------------------ | ------- |
| validator    | Fully qualified class name for the validator to use | com.rierino.handler.util.validator.JsonSchemaValidator | -       |
| validator.\* | Additional parameters used by the validator class   | state=schema                                           | -       |

Following validators are available (which can be also used as validators within state managers):

### JMESValidator

Validates input against a given "pattern". Pattern information can be provided as a handler or action event metadata parameter. Returns a "valid" flag in results.

### JsonSchemaValidator

Validates input against a Json schema stored in a state manager. Uses the following handler parameters:

| Parameter         | Definition                                 | Example | Default |
| ----------------- | ------------------------------------------ | ------- | ------- |
| validator.state   | State manager with schema records.         | -       | schema  |
| validator.version | Json schema version to use for validation. | -       | V201909 |

Also uses the following action event metadata parameter:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                                                   | Example | Default |
| --------- | ------------------------------------------------------------ | ------- | ------- |
| schema    | Id of the schema record to use for current event validation. | product | -       |
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
            "schema": {
              "type": "string",
              "definition": "Id of the schema record to use for current event validation.",
              "example": "product",
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

Returns a "valid" flag as well as "validations" list of findings.

## Actions

### Validate

Checks validity of the event input payload using configured validator instance and returns results.

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
| \*             | Additional parameters used by the validator class | pattern=(value='a') |         |
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
