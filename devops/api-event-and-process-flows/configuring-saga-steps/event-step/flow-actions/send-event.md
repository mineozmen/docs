---
description: These actions provide ability to send events to an output stream.
---

# Send Event

## Send Event Actions

### SendEvent

Sends the event received to another stream with the following event metadata parameters:

{% tabs %}
{% tab title="Table" %}
| Parameter | Definition              | Example        | Default                |
| --------- | ----------------------- | -------------- | ---------------------- |
| System    | System to send event to | kafka\_default | -                      |
| Stream    | Stream to send event to | tracking       | -                      |
| Key       | Key value to use        | 123            | -                      |
| Partition | Partition to use        | 1              | \[Calculated from key] |
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
            "system": {
              "type": "string",
              "definition": "System to send event to",
              "example": "kafka_default",
              "default": null
            },
            "stream": {
              "type": "string",
              "definition": "Stream to send event to",
              "example": "tracking",
              "default": null
            },
            "key": {
              "type": ["string", "integer"],
              "definition": "Key value to use",
              "example": 123,
              "default": null
            },
            "partition": {
              "type": ["integer", "string"],
              "definition": "Partition to use",
              "example": 1,
              "default": "[Calculated from key]"
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
