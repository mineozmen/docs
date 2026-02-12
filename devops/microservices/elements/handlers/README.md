---
description: >-
  Handlers are used for defining key classes responsible for processing data and
  responding to requests.
---

# Handlers

Handlers represent the main functional blocks in Rierino, where all events are actioned on using a specific handler. In built-in Rierino elements, handlers correspond to Java classes extending an abstract Rierino core class. However, it is possible to configure and use elements with different programming languages as long as they can consume and produce the same event data format.

As Rierino is a highly distributed platform with specialized microservices, handlers provide means for loading and enabling the use of only selected functionality on each runner. Without such construct, each microservice would have to load all available functionality, resulting in inefficient use of resources and limited functional governance.

Handlers are only created when there is a new custom class implementation. And, this is a relatively rare activity, since Rierino handlers are designed to provide rich set of capabilities, ranging from basic CRUD operations to real-time ML inference.

## Handler Settings

A number of settings are shared across all handler types:

| Setting                | Definition                                                                       | Example                              | Default |
| ---------------------- | -------------------------------------------------------------------------------- | ------------------------------------ | ------- |
| class                  | Fully qualified name of the class for initializing this handler                  | com.rierino.handler.ReadEventHandler | -       |
| parameter.\[parameter] | Handler specific parameter value (see below and individual handlers for details) | query.state=query                    | -       |

Each handler can be responsible for one or multiple actions, which typically map to the same or similar named functions.

{% file src="../../../../.gitbook/assets/handler-read-0001.json" %}
Example Handler Definition (Can be Imported on Element Screen)
{% endfile %}

### Common Role Handler Parameters

Following parameters are shared among all role & event handlers:

| Parameter           | Definition                                                                                                      | Example      | Default           |
| ------------------- | --------------------------------------------------------------------------------------------------------------- | ------------ | ----------------- |
| cache.use           | Whether handler is allowed to use caching                                                                       | true         | false             |
| cache.write         | Whether handler is allowed to write to cache states (in case there is a dedicated writer)                       | false        | true              |
| logDetail           | Whether detailed trace logs should be performed (always[^1]/never[^2]) independent of the current logging level | always       | -                 |
| role.lock           | Default locking type for all roles (NONE, OPTIMISTIC or SAFE), used by handlers that require locking            | NONE         | SAFE              |
| role.\[role].lock   | Locking type for a specific role type                                                                           | journal=SAFE | role.lock value   |
| role.onFail         | Default strategy for requests that fail (REPLY, SKIP, DLQ, FATAL)                                               | REPLY        | SKIP              |
| role.\[role].onFail | Strategy for requests that fail for a specific role type                                                        | journal=DLQ  | role.onFail value |
| telemetry.enabled   | Whether handler should produce OpenTelemetry traces & metrics                                                   | false        | true              |
| fireForget.state    | State manager that should store fireForget call status and results (optional)                                   | ff\_task     | -                 |

### Common Event Handler Parameters

Following parameters are shared among all event handlers:

| Parameter             | Definition                                                                        | Example    | Default             |
| --------------------- | --------------------------------------------------------------------------------- | ---------- | ------------------- |
| action.lock           | Default locking type for all actions, used by event handlers that require locking | NONE       | SAFE                |
| action.\[role].lock   | Locking type for a specific action                                                | Write=SAFE | action.lock value   |
| action.onFail         | Default strategy for requests that fail                                           | REPLY      | SKIP                |
| action.\[role].onFail | Strategy for requests that fail for a specific action                             | Write=DLQ  | action.onFail value |

## Common Action Parameters

While all handler actions use their own set of parameters, the following event metadata parameters are shared between all actions:

{% tabs %}
{% tab title="Table" %}
| Parameter     | Definition                                                                                                                                          | Example          | Default |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------- |
| Log Detail    | Whether handler should perform detailed logging for the event (always=display event data on DEBUG level logging, never=don't display detailed data) | always           | -       |
| Skip Pattern  | JMES path expression for deciding whether to skip executing action (calculated on full payload)                                                     | data.status=='A' | -       |
| Delay Ms      | Milliseconds to wait between executing actions                                                                                                      | 500              | -       |
| Delay ID      | Unique identifier of the actions which should wait each other with delayMs (if not specified delayMs is applied on each call)                       | amazon\_rest     | -       |
| Ignore Errors | Comma separated list of error codes to be treated as successful execution                                                                           | 11002,11006      | -       |
| Fire & Forget | Whether the action should run async (returns "reference" with unique task id in response payload only)                                              | true             | false   |
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
            "logDetail": {
              "type": "string",
              "description": "Whether handler should perform detailed logging for the event (always=display event data on DEBUG level logging, never=don't display detailed data).",
              "enum": ["always", "never"],
              "example": "always"
            },
            "skipPattern": {
              "type": "string",
              "description": "JMES path expression for deciding whether to skip executing action (calculated on full payload).",
              "example": "data.status=='A'"
            },
            "delayMs": {
              "type": "integer",
              "description": "Milliseconds to wait between executing actions.",
              "example": 500
            },
            "delayId": {
              "type": "string",
              "description": "Unique identifier of the actions which should wait each other with delayMs (if not specified delayMs is applied on each call).",
              "example": "amazon_rest"
            },
            "ignoreErrors": {
              "type": "string",
              "description": "Comma separated list of error codes to be treated as successful execution.",
              "example": "11002,11006"
            },
            "fireForget": {
              "type": "boolean",
              "description": "Whether the action should run async (returns \"reference\" with unique task id in response payload only).",
              "default": false,
              "example": true
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
delayMs and delayId parameters are used for enabling rate-limiting, which may be required when target systems (such as REST or database) have performance bottlenecks or API throttling constraings, providing a runner level speed limit across actions grouped by delayId.
{% endhint %}

### Action Caching

Some handlers (such as ReadEventHandler, QueryEventHandler) allow caching of results in state managers based on input parameters. Following event metadata parameters are shared among these cacheable event handlers:

{% tabs %}
{% tab title="Table" %}
| Parameter         | Definition                                                                                 | Example              | Default |
| ----------------- | ------------------------------------------------------------------------------------------ | -------------------- | ------- |
| Use Cache         | Whether specific event should use cache                                                    | true                 | false   |
| Cache State       | Name of the state manager for caching event data                                           | product\_cache       | -       |
| Cache Key Pattern | JMES path expression for producing cache key (in case handler supports dynamic cache keys) | join(\[id, version]) | -       |
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
            "useCache": {
              "type": "boolean",
              "description": "Whether specific event should use cache.",
              "default": false,
              "example": true
            },
            "cacheState": {
              "type": "string",
              "description": "Name of the state manager for caching event data.",
              "example": "product_cache"
            },
            "cacheKeyPattern": {
              "type": "string",
              "description": "JMES path expression for producing cache key (in case handler supports dynamic cache keys).",
              "example": "join([id, version])"
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

It is possible to clear cache using TTL or journal / pulse feeds with typical settings of the state manager used for caching.

{% hint style="info" %}
Handlers can be also versioned, where the version specified in event metadata defines which version of the handler action implementation should be used.
{% endhint %}

### Action Iteration

All actions can run as sequential or parallel iterations without explicit loops configured as steps on a saga, where input and output are arrays. Following event metadata parameters are used for configuring these iterations:

{% tabs %}
{% tab title="Table" %}
| Parameter           | Definition                                                                                      | Example | Default |
| ------------------- | ----------------------------------------------------------------------------------------------- | ------- | ------- |
| For Each            | Whether action should be iterated or not                                                        | true    | false   |
| For Each Parallel   | Whether iteration should run as in parallel or not                                              | true    | false   |
| For Each Root As    | Element to have original event payload repeated in for each loop                                | main    | $root   |
| For Each Index As   | Element to have index of the array added in iteration payload                                   | idx     | $index  |
| For Each Input      | Input element to use within the loop                                                            | id      | -       |
| For Each Output     | Output element to produce within the loop                                                       | output  | -       |
| For Each Full Resut | Whether each iteration should return full results (excluding root & index) or only output field | true    | false   |
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
            "forEach": {
              "type": "boolean",
              "description": "Whether action should be iterated or not.",
              "default": false,
              "example": true
            },
            "parallel": {
              "type": "boolean",
              "description": "Whether iteration should run as in parallel or not.",
              "default": false,
              "example": true
            },
            "rootAs": {
              "type": "string",
              "description": "Element to have original event payload repeated in for each loop.",
              "default": "$root",
              "example": "main"
            },
            "indexAs": {
              "type": "string",
              "description": "Element to have index of the array added in iteration payload.",
              "default": "$index",
              "example": "idx"
            },
            "forEachInput": {
              "type": "string",
              "description": "Input element to use within the loop.",
              "example": "id"
            },
            "forEachOutput": {
              "type": "string",
              "description": "Output element to produce within the loop.",
              "example": "output"
            },
            "forEachFullResult": {
              "type": "boolean",
              "description": "Whether each iteration should return full results (excluding root & index) or only output field.",
              "default": false,
              "example": true
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

## Handler Selection

When a runner receives an Event or Role data, it uses the following steps in selecting the right handler for processing it:

1. If it is received from a "local" stream as part of a Saga flow and has a "handler" assigned to it (i.e. configured in Saga UI), that specific handler is used
2. If it is received from any other stream as part of a Saga flow and has a "handler" assigned to it, that specific handler is used
3. If the received event's action has its "handler" parameter defined (i.e. configured in Element UI), that specific handler is used
4. If the received stream is part of the input streams of a specific role with a specific handler (i.e. configured in Element UI) that handler is used
5. If all fails, the first handler which supports event's action is used

{% hint style="info" %}
Except for 1st case, if the identified handler does not have the received stream as part of its inputs list, it will not process the message. This allows discarding of messages received from streams which are not supposed to make such requests. To override this behavior, it is possible to set "allowHandlers" property to "true" for any runner.
{% endhint %}

[^1]: Typically used for debugged event handlers

[^2]: Typically used for sensitive event handlers
