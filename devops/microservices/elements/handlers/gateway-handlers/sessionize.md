---
description: >-
  This handler (com.rierino.handler.SessionEventHandler) provides ability to
  create and track sessions for system users, as part of Gateway functionality.
---

# Sessionize

## Handler Parameters

| Parameter          | Definition                                                                             | Example       | Default  |
| ------------------ | -------------------------------------------------------------------------------------- | ------------- | -------- |
| store.state        | Name of state manager to store session details                                         | session       | -        |
| init.stream        | Name of stream to forward session initializations as triggers (optional)               | session\_init | -        |
| stitchPriority[^1] | Strategy for selecting session id when stitching sessions (self, old, new or existing) | old           | existing |
| ttl                | Milliseconds of inactivity to expire a session                                         | 900000        | 60000    |

## Actions

### Touch

Initializes or extends an existing session for the given origin and returns that session's id and expiration time:

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
| Parameter | Definition                                                          | Example                                               | Default       |
| --------- | ------------------------------------------------------------------- | ----------------------------------------------------- | ------------- |
| TTL       | JMESPath pattern to apply on data output, before returning response | {id:id, name:data.name, description:data.description} | Handler's ttl |
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
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}",
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

Extends an existing session for the given origin and returns that session's new expiration time:

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
| Parameter | Definition                                                          | Example                                               | Default       |
| --------- | ------------------------------------------------------------------- | ----------------------------------------------------- | ------------- |
| TTL       | JMESPath pattern to apply on data output, before returning response | {id:id, name:data.name, description:data.description} | Handler's ttl |
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
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}",
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

Extends a list of existing sessions for the given origin keys and returns their new expiration time:

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
| Parameter | Definition                                                          | Example                                               | Default       |
| --------- | ------------------------------------------------------------------- | ----------------------------------------------------- | ------------- |
| TTL       | JMESPath pattern to apply on data output, before returning response | {id:id, name:data.name, description:data.description} | Handler's ttl |
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
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}",
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

Stitches an existing session (identified by "oldId" and "oldType" in origin parameters) with a new origin (using stitchPriority strategy) and returns the final session's id and expiration time:

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
| Parameter | Definition                                                          | Example                                               | Default       |
| --------- | ------------------------------------------------------------------- | ----------------------------------------------------- | ------------- |
| TTL       | JMESPath pattern to apply on data output, before returning response | {id:id, name:data.name, description:data.description} | Handler's ttl |
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
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}",
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

[^1]: self: stitch to given key's session

    old: stitch to oldest session

    new: stitch to newest session

    existing: stitch to previous key's session
