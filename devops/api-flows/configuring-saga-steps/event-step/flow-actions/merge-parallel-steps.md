---
description: >-
  These actions provide ability to merge parallel saga steps in distributed flow
  executions.
---

# Merge Parallel Steps

## Merge Parallel Steps Actions

### Merge

Merges multiple parallel running branches in a saga flow, storing merge status in a state manager. Each merge instance is identified by a unique `taskId`, and contributing branches are identified by their `mergeIds`. When all required merge ids are received, the merge step continues.

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

{% hint style="info" %}
In local saga flows, steps run in sequence. Use `ForEachEventHandler` to create parallelism there instead of `MergeEventHandler`.
{% endhint %}

[^1]: Also used for returning merge results in output event

[^2]: If given path is null, `[requestId]:[sagaStepId][runnerPartition]` is used
