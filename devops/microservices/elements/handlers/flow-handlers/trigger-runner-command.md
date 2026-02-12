---
description: >-
  This handler (com.rierino.handler.CommandEventHandler) triggers Commands on
  through Event streams.
---

# Trigger Runner Command

{% hint style="info" %}
Unlike commands, events are not broadcasted to all runners. Hence, triggering a command using this handler only causes a single runner to execute given command.

This handler is only used for simplifying development and testing processes, when a single partition runner is used and commands are not being sent through Kafka topics.
{% endhint %}

## Actions

### Command

Treats input payload as a command and executes on the runner:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                             | Example | Default |
| -------------- | -------------------------------------- | ------- | ------- |
| Input Element  | Json path in payload to use            | command | -       |
| Output Element | Json path in payload to return results | result  | -       |
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
        "inputElement": {
          "type": "string",
          "description": "Json path in payload to use",
          "examples": [
            "command"
          ],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path in payload to return results",
          "examples": [
            "result"
          ],
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
| Parameter     | Definition                            | Example | Default |
| ------------- | ------------------------------------- | ------- | ------- |
| Input Pattern | JMESPath expression to create command | -       | -       |
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
              "description": "JMESPath expression to create command",
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
