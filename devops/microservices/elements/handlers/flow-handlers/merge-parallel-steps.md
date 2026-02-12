---
description: >-
  This handler (com.rierino.handler.MergeEventHandler) provides ability to merge
  parallel saga steps in distributed flow executions.
---

# Merge Parallel Steps

## Handler Parameters

| Parameter            | Definition                                                            | Example                                 | Default |
| -------------------- | --------------------------------------------------------------------- | --------------------------------------- | ------- |
| merge.locker         | Fully qualified class name for the locker to use for thread safety.   | com.rierino.state.lock.RedisStateLocker | -       |
| merge.locker.timeout | Default milliseconds to wait for acquiring a lock                     | 1000                                    | -       |
| merge.action         | Whether merge completion should delete, update or ignore state record | delete                                  | update  |
| merge.locker.expiry  | Default TTL of a lock in milliseconds (-1 for non-expiring locks)     | 10000                                   | -1      |

MergeEventHandler also supports timer and timeout parameters as in [TaskEventHandler](../core-handlers/orchestrate-user-task.md).

## Actions

### Merge

Merges multiple parallel running branches in a saga flow, storing merge status in a state manager. Each merge instance is identified by a unique taskId, and contributing branches are identified by their mergeIds. When all required merge ids are received, the merge step continues.

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                                         | Example       | Default |
| ------ | -------------------------------------------------- | ------------- | ------- |
| Domain | Name of state manager for coordinating merge state | search\_merge | -       |
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
        "domain": {
          "type": "string",
          "definition": "Name of state manager for coordinating merge state",
          "example": "search_merge",
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
| Parameter          | Definition                                                                                         | Example | Default |
| ------------------ | -------------------------------------------------------------------------------------------------- | ------- | ------- |
| Merge Path         | [Json path of the payload that should be merged with other merge branches](#user-content-fn-1)[^1] | results | -       |
| Required Merge Ids | Comma separated list of merge ids that are required to complete the merge                          | 1,2,3   | -       |
| Merge Id Path      | Json path which defines id for merge branch                                                        | id      | mergeId |
| Task Id Path       | [Json path which defines id for merge instance](#user-content-fn-2)[^2]                            | mainId  | taskId  |


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
            "mergePath": {
              "type": "string",
              "definition": "Json path of the payload that should be merged with other merge branches",
              "example": "results",
              "default": null
            },
            "requiredMergeIds": {
              "type": "string",
              "definition": "Comma separated list of merge ids that are required to complete the merge",
              "example": "1,2,3",
              "default": null
            },
            "mergeIdPath": {
              "type": "string",
              "definition": "Json path which defines id for merge branch",
              "example": "id",
              "default": "mergeId"
            },
            "taskIdPath": {
              "type": "string",
              "definition": "Json path which defines id for merge instance",
              "example": "mainId",
              "default": "taskId"
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

Merge handler also allows timing out merge tasks, in case some branches do not send their results in time. For this purpose, all timeout roles and commands of the TaskEventHandler are also supported.

## Merge Record

Current state of a merge activity is stored in the state manager with the following details:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "id": {
      "type": "integer",
      "definition": "Unique merge id",
      "example": 123,
      "default": null
    },
    "data": {
      "type": "object",
      "properties": {
        "event": {
          "type": "object",
          "definition": "Event record that started the merge operation",
          "example": {},
          "default": null
        },
        "requiredMergeIds": {
          "type": "string",
          "definition": "Comma separated list of mergeids required to complete the operation",
          "example": "1,2,3",
          "default": null
        },
        "waitingMergePaths": {
          "type": "array",
          "definition": "Array of mergeids still waiting to be completed",
          "items": {
            "type": "integer"
          },
          "example": [1, 3],
          "default": null
        },
        "mergeElements": {
          "type": "object",
          "definition": "Current contents of the merge elements",
          "example": {},
          "default": null
        },
        "timeout": {
          "type": "integer",
          "definition": "Epoch time of the timeout for current operation",
          "example": 1686154822136,
          "default": null
        },
        "isProcessed": {
          "type": "boolean",
          "definition": "Whether merge process is already completed or not",
          "example": false,
          "default": null
        }
      }
    }
  }
}
```

{% hint style="info" %}
In case of local saga flows, steps are executed in sequence, so ForEachEventHandler should be used for creating parallelism instead of MergeEventHandler.
{% endhint %}

[^1]: Also used for returning merge results in output event

[^2]: If given path is null, \[requestId]:\[sagaStepId]\[runnerPartition] is used
