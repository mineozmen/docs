---
description: >-
  These actions provide ability to review contents of events received by a
  runner from logs.
---

# Log Event

## Log Event Actions

### Log

Logs contents of the event received with the following event metadata parameters:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition                                     | Example | Default           |
| --------- | ---------------------------------------------- | ------- | ----------------- |
| Level     | Logging level to use                           | ERROR   | DEBUG             |
| Type      | Whether full event or payload should be logged | payload | event             |
| Message   | Message format to use for logging              | Success | Event details: {} |
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
            "level": {
              "type": "string",
              "definition": "Logging level to use",
              "example": "ERROR",
              "default": "DEBUG"
            },
            "type": {
              "type": "string",
              "definition": "Whether full event or payload should be logged",
              "example": "payload",
              "default": "event"
            },
            "message": {
              "type": "string",
              "definition": "Message format to use for logging",
              "example": "Success",
              "default": "Event details: {}"
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
If the runner log level is above the selected level, event details will not be displayed.
{% endhint %}
