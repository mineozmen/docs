---
description: >-
  This handler (com.rierino.handler.ShellEventHandler) provides ability to
  execute shell commands.
---

# Run Shell Command

## Handler Parameters

| Parameter    | Definition                                                | Example | Default |
| ------------ | --------------------------------------------------------- | ------- | ------- |
| processState | Name of the state manager to record shell process results | process | -       |

## Actions

### CallShell

Executes given shell command. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter  | Definition                                                        | Example | Default |
| ---------- | ----------------------------------------------------------------- | ------- | ------- |
| Command    | Main command to execute                                           | ls      | -       |
| Track      | Whether status updates should be recorded in process state or not | true    | false   |
| Parameters | Arguments to add                                                  | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CallShell action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "command": {
              "type": "string",
              "definition": "Main command to execute",
              "example": "ls"
            },
            "track": {
              "type": "boolean",
              "definition": "Whether status updates should be recorded in process state or not",
              "default": false,
              "example": true
            }
          },
          "patternProperties": {
            "^param_\\\\[\\\\d+\\\\]_arg$": {
              "type": "string",
              "definition": "Name of the ith argument to add",
              "example": "--dir"
            },
            "^param_\\\\[\\\\d+\\\\]_value$": {
              "type": "string",
              "definition": "Value of the ith argument to add",
              "example": "/tmp"
            },
            "^param_\\\\[\\\\d+\\\\]_path$": {
              "type": "string",
              "definition": "Path from input payload for ith argument",
              "example": "parameters.dir"
            },
            "^param_\\\\[\\\\d+\\\\]_asText$": {
              "type": "boolean",
              "definition": "Whether payload value should be added as text",
              "default": false,
              "example": true
            },
            "^param_\\\\[\\\\d+\\\\]_repeat$": {
              "type": "boolean",
              "definition": "Whether array or object payload values should repeat argument",
              "default": false,
              "example": true
            },
            "^param_\\\\[\\\\d+\\\\]_delimiter$": {
              "type": "string",
              "definition": "Delimiter to put between array or object payload values if argument won't be repeated",
              "example": ","
            },
            "^param_\\\\[\\\\d+\\\\]_separator$": {
              "type": "string",
              "definition": "Separator to put between argument and object values if argument will be repeated",
              "example": "="
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
