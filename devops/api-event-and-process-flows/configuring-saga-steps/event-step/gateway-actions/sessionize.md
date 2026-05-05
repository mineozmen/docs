---
description: >-
  These actions provide ability to create and track sessions for system users as
  part of gateway functionality.
---

# Sessionize

## Sessionize Actions

### Touch

Initializes or extends an existing session for the given origin and returns that session's id and expiration time.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Output Element | Json path for the output in response event payload | session | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Touch action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "session",
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
| Parameter | Definition               | Example | Default       |
| --------- | ------------------------ | ------- | ------------- |
| TTL       | TTL to apply for session | 900000  | Handler's ttl |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Touch action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "ttl": {
              "type": "string",
              "definition": "TTL to apply for the session",
              "example": "900000",
              "default": "Handler's ttl"
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

### Extend

Extends an existing session for the given origin and returns that session's new expiration time.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Output Element | Json path for the output in response event payload | session | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Extend action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "session",
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
| Parameter | Definition               | Example | Default       |
| --------- | ------------------------ | ------- | ------------- |
| TTL       | TTL to apply for session | 900000  | Handler's ttl |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Extend action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "ttl": {
              "type": "string",
              "definition": "TTL to apply for the session",
              "example": "900000",
              "default": "Handler's ttl"
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

### ExtendList

Extends a list of existing sessions for the given origin keys and returns their new expiration time.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                              | Example | Default |
| -------------- | ------------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the list of session keys in event payload | list    | -       |
| Output Element | Json path for the output in response event payload      | session | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize ExtendList action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the list of session keys in event payload",
          "example": "list",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "session",
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
| Parameter | Definition                | Example | Default       |
| --------- | ------------------------- | ------- | ------------- |
| TTL       | TTL to apply for sessions | 900000  | Handler's ttl |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize ExtendList action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "ttl": {
              "type": "string",
              "definition": "TTL to apply for the sessions",
              "example": "900000",
              "default": "Handler's ttl"
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

### Expire

Expires an existing session for the given origin.

### Stitch

Stitches an existing session, identified by `oldId` and `oldType` in origin parameters, with a new origin and returns the final session's id and expiration time.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Output Element | Json path for the output in response event payload | session | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Stitch action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "session",
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
| Parameter | Definition               | Example | Default       |
| --------- | ------------------------ | ------- | ------------- |
| TTL       | TTL to apply for session | 900000  | Handler's ttl |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Sessionize Stitch action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "ttl": {
              "type": "string",
              "definition": "TTL to apply for the session",
              "example": "900000",
              "default": "Handler's ttl"
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
