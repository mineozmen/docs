---
description: >-
  This handler (com.rierino.handler.LockEventHandler) provides ability to create
  locks & unlock them for a given id and a domain.
---

# Lock & Unlock

## Handler Parameters

| Parameter | Definition                                                        | Example                                 | Default |
| --------- | ----------------------------------------------------------------- | --------------------------------------- | ------- |
| locker    | StateLocker class to use for issuing and releasing locks          | com.rierino.state.lock.RedisStateLocker | -       |
| timeout   | Default milliseconds to wait for acquiring a lock                 | 1000                                    | -       |
| expiry    | Default TTL of a lock in milliseconds (-1 for non-expiring locks) | 60000                                   | -1      |

## Actions

### TryLock

Tries creating a lock and returns whether it was successful:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                      | Example    | Default |
| -------------- | ----------------------------------------------- | ---------- | ------- |
| Input Element  | Json path in payload that has id parameter      | parameters | -       |
| Output Element | Json path in payload to return "locked" results | result     | -       |
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
          "description": "Json path in payload that has id parameter",
          "examples": [
            "parameters"
          ],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path in payload to return \"locked\" results",
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

With the following event metadata parameters:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter     | Definition                                                                                                 | Example | Default |
| ------------- | ---------------------------------------------------------------------------------------------------------- | ------- | ------- |
| Master        | Whether master lock should be used instead of a specific id                                                | true    | false   |
| Timeout       | Milliseconds to wait for acquiring a lock (overrides default)                                              | 2000    | -       |
| Expiry        | TTL in milliseconds for lock expiry (overrides default)                                                    | 10000   | -       |
| Allow No Lock | Whether failure to hold a lock should return a successful result (returns "locked" value instead of error) | true    | false   |
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
            "master": {
              "type": "boolean",
              "description": "Whether master lock should be used instead of a specific id",
              "examples": [
                true
              ],
              "default": false
            },
            "timeout": {
              "type": "integer",
              "description": "Milliseconds to wait for acquiring a lock (overrides default)",
              "examples": [
                2000
              ],
              "default": null
            },
            "expiry": {
              "type": "integer",
              "description": "TTL in milliseconds for lock expiry (overrides default)",
              "examples": [
                10000
              ],
              "default": null
            },
            "allowNoLock": {
              "type": "boolean",
              "description": "Whether failure to hold a lock should return a successful result (returns \"locked\" value instead of error)",
              "examples": [
                true
              ],
              "default": false
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

### Unlock

Unlocks an existing lock:

{% tabs %}
{% tab title="Fields (table)" %}
| Field         | Definition                                 | Example    | Default |
| ------------- | ------------------------------------------ | ---------- | ------- |
| Input Element | Json path in payload that has id parameter | parameters | -       |
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
          "description": "Json path in payload that has id parameter",
          "examples": [
            "parameters"
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

With the following event metadata parameters:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter | Definition                                                  | Example | Default |
| --------- | ----------------------------------------------------------- | ------- | ------- |
| Master    | Whether master lock should be used instead of a specific id | true    | false   |
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
            "master": {
              "type": "boolean",
              "description": "Whether master lock should be used instead of a specific id",
              "examples": [
                true
              ],
              "default": false
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

### CheckLock

Returns the current status for a lock:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                      | Example    | Default |
| -------------- | ----------------------------------------------- | ---------- | ------- |
| Input Element  | Json path in payload that has id parameter      | parameters | -       |
| Output Element | Json path in payload to return "locked" results | result     | -       |
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
          "description": "Json path in payload that has id parameter",
          "examples": [
            "parameters"
          ],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path in payload to return \"locked\" results",
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

With the following event metadata parameters:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter | Definition                                                  | Example | Default |
| --------- | ----------------------------------------------------------- | ------- | ------- |
| Master    | Whether master lock should be used instead of a specific id | true    | false   |
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
            "master": {
              "type": "boolean",
              "description": "Whether master lock should be used instead of a specific id",
              "examples": [
                true
              ],
              "default": false
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
