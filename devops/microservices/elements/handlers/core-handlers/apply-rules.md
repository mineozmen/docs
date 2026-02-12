---
description: >-
  This handler (com.rierino.handler.SimpleRuleEventHandler) provides ability to
  evaluate a list of rules and returns results based on matches.
---

# Apply Rules

## Handler Parameters

| Parameter   | Definition                                         | Example | Default |
| ----------- | -------------------------------------------------- | ------- | ------- |
| rules.state | Name of the state manager storing rule definitions | rules   | -       |

## Actions

### Process / ProcessSimpleRules

Evaluates list of rules for a domain and returns the result of matching rule. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                                                           | Example       | Default |
| -------------- | ---------------------------------------------------------------------------------------------------- | ------------- | ------- |
| Domain         | Set of rules to apply                                                                                | risk\_scoring | -       |
| Input Element  | Json path for the input in event payload for query variables                                         | parameters    | -       |
| Output Element | Json path for the output in response event payload where "result" or "results" element will be added | $.decision    | -       |
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
        "domain": {
          "type": "string",
          "description": "Set of rules to apply",
          "example": "risk_scoring"
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in event payload for query variables",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload where \"result\" or \"results\" element will be added",
          "example": "$.decision"
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
| Parameter     | Definition                                                                                          | Example                   | Default |
| ------------- | --------------------------------------------------------------------------------------------------- | ------------------------- | ------- |
| Input Pattern | JMESPath pattern to apply on data input                                                             | {id:id, group:data.group} | -       |
| Structure     | Whether the "result" of highest salience rule or "results" list of all matching rules should return | list                      | single  |
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
            "inputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data input",
              "example": "{id:id, group:data.group}"
            },
            "structure": {
              "type": "string",
              "description": "Whether the \"result\" of highest salience rule or \"results\" list of all matching rules should return",
              "enum": ["single", "list"],
              "default": "single",
              "example": "list"
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
