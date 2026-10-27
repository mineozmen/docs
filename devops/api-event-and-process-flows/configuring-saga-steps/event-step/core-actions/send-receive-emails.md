---
description: >-
  These actions provide ability to send and receive emails using existing email
  servers.
---

# Send/Receive Emails

## Send/Receive Emails Actions

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
| Parameter     | Definition                                                                      | Example                                                                                                                    | Default |
| ------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------- |
| Input Pattern | JMESPath pattern to apply on data input                                         | {"from": from, "replyTo": replyTo, "to": to, "cc": cc, "subject": subject, "charset": charset, "text": text, "html": html} | -       |
| To            | Default comma-separated recipients when the input does not provide them         | user@example.com                                                                                                           | -       |
| CC            | Default comma-separated CC recipients when the input does not provide them      | team@example.com                                                                                                           | -       |
| From          | Default sender address when the input does not provide one                      | no-reply@example.com                                                                                                       | -       |
| Reply To      | Default comma-separated reply-to addresses when the input does not provide them | support@example.com                                                                                                        | -       |
| Subject       | Default email subject when the input does not provide one                       | Update                                                                                                                     | -       |
| Charset       | Default character set of the email content when the input does not provide one  | UTF-8                                                                                                                      | -       |
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
            },
            "to": {"type": "string", "definition": "Default comma-separated recipients when the input does not provide them", "example": "user@example.com"},
            "cc": {"type": "string", "definition": "Default comma-separated CC recipients when the input does not provide them", "example": "team@example.com"},
            "from": {"type": "string", "definition": "Default sender address when the input does not provide one", "example": "no-reply@example.com"},
            "replyTo": {"type": "string", "definition": "Default comma-separated reply-to addresses when the input does not provide them", "example": "support@example.com"},
            "subject": {"type": "string", "definition": "Default email subject when the input does not provide one", "example": "Update"},
            "charset": {"type": "string", "definition": "Default character set of the email content when the input does not provide one", "example": "UTF-8"}
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
| Parameter       | Definition                                                          | Example          | Default |
| --------------- | ------------------------------------------------------------------- | ---------------- | ------- |
| ID Path         | Json path for the id field in input element                         | mail.id          | id      |
| No Content      | Whether to discard email content details                            | true             | false   |
| Output Pattern  | JMESPath pattern to apply on data output, before returning response | -                | -       |
| Attachment Mode | How attachments are returned                                        | file             | ignore  |
| Attachment Root | Folder to save attachments to when the mode is `file`               | /tmp/attachments | -       |
| Group By Date   | Save attachments in date-based folders                              | true             | false   |
| No Headers      | Exclude email headers from the response                             | true             | false   |
| No Extensions   | Exclude extension headers from the response                         | true             | false   |
| ID Value        | Static ID used instead of reading it from input                     | 1234             | -       |
| ID Parameter    | Name of the request field to read the ID from                       | id               | -       |
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
            },
            "attachmentMode": {"type": "string", "definition": "How attachments are returned", "default": "ignore", "example": "file"},
            "attachmentRoot": {"type": "string", "definition": "Folder to save attachments to when the mode is file", "example": "/tmp/attachments"},
            "groupByDate": {"type": "boolean", "definition": "Save attachments in date-based folders", "default": false, "example": true},
            "noHeaders": {"type": "boolean", "definition": "Exclude email headers from the response", "default": false, "example": true},
            "noExtensions": {"type": "boolean", "definition": "Exclude extension headers from the response", "default": false, "example": true},
            "idValue": {"type": ["string", "integer"], "definition": "Static ID used instead of reading it from input", "example": 1234},
            "idParameter": {"type": "string", "definition": "Name of the request field to read the ID from", "example": "id"}
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
