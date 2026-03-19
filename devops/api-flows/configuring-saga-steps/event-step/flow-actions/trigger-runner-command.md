---
description: These actions provide ability to trigger commands through event streams.
---

# Trigger Runner Command

## Trigger Runner Command Actions

{% hint style="info" %}
Unlike commands, events are not broadcast to all runners. Triggering a command with this handler causes only a single runner to execute the command.

This handler is mainly useful for development and testing when a single-partition runner is used and commands are not sent through Kafka topics.
{% endhint %}

### Command

Treats input payload as a command and executes it on the runner:

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
          "examples": ["command"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path in payload to return results",
          "examples": ["result"],
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
