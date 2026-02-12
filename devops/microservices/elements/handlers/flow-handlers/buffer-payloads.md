---
description: >-
  This handler (com.rierino.handler.BufferEventHandler) provides ability to
  buffer multiple event payloads into a single batch event.
---

# Buffer Payloads

## Handler Parameters

This handler requires no parameters.

## Actions

### Buffer

Buffers payload of the events received and releases them as a single combined event with array of input payloads in either of the following cases:

* When the runner performs a commit operation (based on its settings or a commit command received)
* When the number of buffered events reach buffer size provided in event parameters

Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                            | Example    | Default |
| -------------- | ----------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for payload element to buffer               | parameters | -       |
| Output Element | Json path for payload element to output buffered data | batch      | -       |
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
          "definition": "Json path for payload element to buffer",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for payload element to output buffered data",
          "example": "batch",
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
| Parameter      | Definition                                                   | Example           | Default |
| -------------- | ------------------------------------------------------------ | ----------------- | ------- |
| Input Pattern  | JMES path for transforming input element                     | {"id": productid} | -       |
| Output Pattern | JMES path for transforming buffered data (which is an array) | @\[?id>0]         | -       |
| Buffer ID      | Unique id for buffering similar payloads together            | product\_ids      | -       |
| Buffer Size    | Max number of records to buffer                              | 10                | -       |
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
              "definition": "JMES path for transforming input element",
              "example": "{\"id\": productid}",
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMES path for transforming buffered data (which is an array)",
              "example": "@[?id>0]",
              "default": null
            },
            "bufferId": {
              "type": "string",
              "definition": "Unique id for buffering similar payloads together",
              "example": "product_ids",
              "default": null
            },
            "bufferSize": {
              "type": "integer",
              "definition": "Max number of records to buffer",
              "example": 10,
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

{% hint style="info" %}
Note that bufferSize parameter is defined at action level instead of the handler, allowing real-time changing of buffer size for different buffer types (e.g. decreasing or increasing buffer size when required without rebuilding the handler).
{% endhint %}

This handler can facilitate performance improvements for event handlers which can handle batched requests (e.g. sending update requests as a batch to write handlers or calculations to a rule event handler).
