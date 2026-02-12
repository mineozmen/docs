---
description: >-
  This handler (com.rierino.handler.WriteEventHandler) provides ability to
  create, update, delete records on a state manager on demand, facilitating
  common REST API write calls.
---

# Write Data

## Handler Parameters

| Parameter | Definition                                                                                                                                              | Example | Default |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------- |
| useDiff   | Whether handler should generate journals keeping only actual updates from current data by default (i.e. removing fields which are same as current data) | true    | false   |

{% file src="../../../../../.gitbook/assets/handler-write-0001.json" %}
Example Write Handler Definition (Can be Imported on Element Screen)
{% endfile %}

## Actions

Most actions of this handler utilize the following event metadata fields:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the state manager to read data from        | product    | -       |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | $.product  | -       |


{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Write Data action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the state manager to read data from",
          "example": "product"
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "$.product"
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
| Parameter       | Definition                                                                                                                                                                                            | Example                                               | Default |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | ------- |
| Input Pattern   | JMESPath pattern to apply on data input                                                                                                                                                               | {id:parameters.id, data:parameters.data}              | -       |
| Output Pattern  | JMESPath pattern to apply on data output, before returning response                                                                                                                                   | {id:id, name:data.name, description:data.description} | -       |
| Use Diff        | Whether handler should generate journals keeping only actual updates from current data for the event                                                                                                  | true                                                  | false   |
| If Pattern      | JMES path pattern evaluating condition on current data to decide whether to execute update                                                                                                            | data.status == 'A'                                    | -       |
| Fail Stale      | Whether to fail in case a request with old instance version or offset arrives                                                                                                                         | false                                                 | true    |
| Return Result   | Whether the action should return updated data after the change                                                                                                                                        | false                                                 | true    |
| Immediate       | Whether the action should be executed immediately, or can be buffered for bulk execution                                                                                                              | false                                                 | true    |
| Keep Impact     | Whether the action should also return the impact created on current record                                                                                                                            | true                                                  | false   |
| Keep Undo       | Whether the action should also return the undo action required to revert the impact                                                                                                                   | true                                                  | false   |
| Structure       | Type of input to apply updates for (i.e. [array ](#user-content-fn-1)[^1]or [each ](#user-content-fn-2)[^2]= \[{id, data}, {id, data}], multi[^3] = \[ids: \[id, id], data: {}], single = {id, data}) | array                                                 | single  |
| Data Path       | Json path for the data to use in update, when form is "multi"                                                                                                                                         | newData                                               | data    |
| IDs Path        | Json path for the ids to update, when form is "multi"                                                                                                                                                 | productids                                            | ids     |
| Item Path       | Json path for the array field to update (for SetElement and RemoveElement)                                                                                                                            | parameters                                            | -       |
| Allow Manual ID | When creating a new record, whether IDs can be given manually, if there is already an ID generator                                                                                                    | true                                                  | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Write Data action eventMeta.parameters",
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
              "example": "{id:parameters.id, data:parameters.data}"
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}"
            },
            "useDiff": {
              "type": "boolean",
              "definition": "Whether handler should generate journals keeping only actual updates from current data for the event",
              "default": false,
              "example": true
            },
            "ifPattern": {
              "type": "string",
              "definition": "JMES path pattern evaluating condition on current data to decide whether to execute update",
              "example": "data.status == 'A'"
            },
            "failStale": {
              "type": "boolean",
              "definition": "Whether to fail in case a request with old instance version or offset arrives",
              "default": true,
              "example": false
            },
            "returnResult": {
              "type": "boolean",
              "definition": "Whether the action should return updated data after the change",
              "default": true,
              "example": false
            },
            "immediate": {
              "type": "boolean",
              "definition": "Whether the action should be executed immediately, or can be buffered for bulk execution",
              "default": true,
              "example": false
            },
            "keepImpact": {
              "type": "boolean",
              "definition": "Whether the action should also return the impact created on current record",
              "default": false,
              "example": true
            },
            "keepUndo": {
              "type": "boolean",
              "definition": "Whether the action should also return the undo action required to revert the impact",
              "default": false,
              "example": true
            },
            "structure": {
              "type": "string",
              "definition": "Type of input to apply updates for (array / each / multi / single)",
              "default": "single",
              "example": "array"
            },
            "dataPath": {
              "type": "string",
              "definition": "Json path for the data to use in update, when form is \"multi\"",
              "default": "data",
              "example": "newData"
            },
            "idsPath": {
              "type": "string",
              "definition": "Json path for the ids to update, when form is \"multi\"",
              "default": "ids",
              "example": "productids"
            },
            "itemPath": {
              "type": "string",
              "definition": "Json path for the array field to update (for SetElement and RemoveElement)",
              "example": "parameters"
            },
            "allowManualId": {
              "type": "boolean",
              "definition": "When creating a new record, whether IDs can be given manually, if there is already an ID generator",
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

<details>

<summary>Example</summary>

Input

```json
{
    "parameters": {
        "id": "given-id",
        "data":{
            "name": "Test",
            "description": "Test Entry"
        }
    }
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (132).png>)

</details>

### Create

Creates a new aggregate on a specific state manager.

### Update

Patches an existing aggregate on a specific state manager.

### Set

Completely updates an existing aggregate on a specific state manager.

### Upsert

Completely updates or creates an aggregate on a specific state manager.

### Delete

Marks an aggregate as deleted, hiding it on a specific state manager.

### Undelete

Unmarks an aggregate as deleted, making it accessible again on a specific state manager.

### Remove

Completely removes an aggregate from a specific state manager.

### SetElement

Adds to or sets element of an array field of an aggregate on a specific state manager.

### RemoveElement

Removes element of an array field of an aggregate on a specific state manager.

### CallSP

This is a special action, which makes calls to stored procedures in state managers which support the functionality (such as Jooq state manager for RDBMS). Special parameters used by this action are:

{% tabs %}
{% tab title="Table" %}
| Parameter      | Definition                                                 | Example                             | Default |
| -------------- | ---------------------------------------------------------- | ----------------------------------- | ------- |
| Command        | Procedure call command with parameters                     | CALL update\_status({0}, {1})       | -       |
| Input Pattern  | JMESPath for transforming input into call parameters array | \[parameters.id, parameters.status] | -       |
| Output Pattern | JMESPath for transforming procedure call results           | -                                   | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Write Data CallSP action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "command": {
              "type": "string",
              "definition": "Procedure call command with parameters",
              "example": "CALL update_status({0}, {1})"
            },
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath for transforming input into call parameters array",
              "example": "[parameters.id, parameters.status]"
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath for transforming procedure call results"
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

[^1]: Executes all impacts at once

[^2]: Executes each impact entry individually as a loop (typically used when id generator requires previous entries executed first)

[^3]: Executes same impact on multiple records at once
