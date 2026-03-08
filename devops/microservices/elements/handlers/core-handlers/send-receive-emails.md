---
description: >-
  This handler (com.rierino.handler.MailEventHandler) provides ability to send
  and receive emails using existing email servers.
---

# Send/Receive Emails

## Handler Parameters

| Parameter | Definition                                                         | Example | Default |
| --------- | ------------------------------------------------------------------ | ------- | ------- |
| system    | Name of the system describing email server access details          | gmail   | -       |
| sendOnly  | Whether the handler should only send emails without receiving them | true    | false   |

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'mail', version:"${rierinoVersion}")
```

## Actions

### SendEmail

Sends an email using the configured email server. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                                       | Example    | Default |
| ------------- | ------------------------------------------------ | ---------- | ------- |
| Input Element | Json path for the input in request event payload | parameters | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SendEmail action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters"
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
| Parameter     | Definition                              | Example                                                                                                                    | Default |
| ------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------- |
| Input Pattern | JMESPath pattern to apply on data input | {"from": from, "replyTo": replyTo, "to": to, "cc": cc, "subject": subject, "charset": charset, "text": text, "html": html} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SendEmail action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on data input",
              "example": "{\"from\": from, \"replyTo\": replyTo, \"to\": to, \"cc\": cc, \"subject\": subject, \"charset\": charset, \"text\": text, \"html\": html}"
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

### ReceiveEmail

Receives details of a specific email with given UID using the configured email server. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | result     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ReceiveEmail action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "result"
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
| Parameter      | Definition                                                          | Example | Default |
| -------------- | ------------------------------------------------------------------- | ------- | ------- |
| ID Path        | Json path for the id field in input element                         | mail.id | id      |
| No Content     | Whether to discard email content details                            | true    | false   |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ReceiveEmail action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "idPath": {
              "type": "string",
              "definition": "Json path for the id field in input element",
              "default": "id",
              "example": "mail.id"
            },
            "noContent": {
              "type": "boolean",
              "definition": "Whether to discard email content details",
              "default": false,
              "example": true
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response"
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
