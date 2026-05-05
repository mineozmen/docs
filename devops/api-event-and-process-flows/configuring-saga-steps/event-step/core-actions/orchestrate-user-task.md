---
description: >-
  These actions provide ability to include human actions and time delays in API
  calls.
---

# Orchestrate User Task

## Orchestrate User Task Actions

### StartTask

Records current event as a process entry assigned to a user or group, to proceed after a trigger or timeout after given time. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                                                 | Example      | Default |
| ------------- | ---------------------------------------------------------- | ------------ | ------- |
| Domain        | Name of the state manager to store process data in         | workflow     | -       |
| Input Element | Json path for storing data to return on proceeding process | process.data | -       |
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
          "description": "Name of the state manager to store process data in",
          "example": "workflow"
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for storing data to return on proceeding process",
          "example": "process.data"
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
| Parameter     | Definition                                                                   | Example              | Default |
| ------------- | ---------------------------------------------------------------------------- | -------------------- | ------- |
| Input Pattern | JMESPath pattern to apply on data input                                      | {data: process.data} | -       |
| Task Id Path  | [Json path which defines id for storing the process](#user-content-fn-1)[^1] | process.id           | taskId  |
| Task Name     | Descriptive name for the task                                                | Job Post Approval    | -       |
| Task Spec     | Task specification (i.e. reference name for the process flow)                | job\_post\_approval  | -       |
| Task Action   | Referential name of the action                                               | Approve              | -       |
| Task Roles    | Comma separated list of user roles allowed to process task                   | admin,manager        | -       |
| Task User     | ID of user allowed to process task                                           | user123              | -       |
| Timeout       | Milliseconds to timeout started process after                                | 60000                | -       |
| Timeout On    | Epoch time in ms to timeout started process on                               | 1666765654           | -       |
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
              "description": "JMESPath pattern to apply on data input",
              "example": "{data: process.data}"
            },
            "taskIdPath": {
              "type": "string",
              "description": "Json path which defines id for storing the process",
              "default": "taskId",
              "example": "process.id"
            },
            "taskName": {
              "type": "string",
              "description": "Descriptive name for the task",
              "example": "Job Post Approval"
            },
            "taskSpec": {
              "type": "string",
              "description": "Task specification (i.e. reference name for the process flow)",
              "example": "job_post_approval"
            },
            "taskAction": {
              "type": "string",
              "description": "Referential name of the action",
              "example": "Approve"
            },
            "taskRoles": {
              "type": "string",
              "description": "Comma separated list of user roles allowed to process task",
              "example": "admin,manager"
            },
            "taskUser": {
              "type": "string",
              "description": "ID of user allowed to process task",
              "example": "user123"
            },
            "timeout": {
              "type": "integer",
              "description": "Milliseconds to timeout started process after",
              "example": 60000
            },
            "timeoutOn": {
              "type": "integer",
              "description": "Epoch time in ms to timeout started process on",
              "example": 1666765654
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
        "productId": "product-1",
        "brief": "Image and video creation with summer vibes" 
    }
}
```

Event Metadata

### ![](<../../../../../.gitbook/assets/image (104).png>)

</details>

{% hint style="info" %}
StartTask action can be followed with an event status condition step in sagas, for escalation of tasks not completed in timeout duration. The "TIMEOUT" condition value can be used for such flows.
{% endhint %}

### ProceedTask

Triggers proceeding of an existing process entry, enriching with stored event data. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the state manager keeping process data     | workflow   | -       |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | process    | -       |
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
          "description": "Name of the state manager keeping process data",
          "example": "workflow"
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "process"
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
| Parameter      | Definition                                                                 | Example           | Default                                      |
| -------------- | -------------------------------------------------------------------------- | ----------------- | -------------------------------------------- |
| Input Pattern  | JMESPath pattern to apply on data input                                    | {data: newData}   | -                                            |
| Output Pattern | JMESPath pattern to apply on enrich data output, before returning response | {stored: oldData} | -                                            |
| Task Id Path   | Json path which defines id for proceeding the process                      | id                | \[requestId]:\[sagaStepId]\[runnerPartition] |
| Enrich Path    | Json path to use for enriching proceeded process event data                | newData           | -                                            |
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
              "description": "JMESPath pattern to apply on data input",
              "example": "{data: newData}"
            },
            "outputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on enrich data output, before returning response",
              "example": "{stored: oldData}"
            },
            "taskIdPath": {
              "type": "string",
              "description": "Json path which defines id for proceeding the process",
              "default": "[requestId]:[sagaStepId][runnerPartition]",
              "example": "id"
            },
            "enrichPath": {
              "type": "string",
              "description": "Json path to use for enriching proceeded process event data",
              "example": "newData"
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
Typically a global, generic /ProceedTask endpoint is used for triggering progression of tasks assigned to users across all sagas.

However, it is typically preferred to have the ProceedTask action running on the same runner performing StartTask sagas, as they typically need to have access to the same state managers (e.g. approval states).
{% endhint %}

### TimeoutTask

Triggers timeout of an already started process, enriching with input event data. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the state manager keeping process data     | workflow   | -       |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | process    | -       |
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
          "description": "Name of the state manager keeping process data",
          "example": "workflow"
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "process"
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
| Parameter      | Definition                                                                 | Example           | Default                                      |
| -------------- | -------------------------------------------------------------------------- | ----------------- | -------------------------------------------- |
| Input Pattern  | JMESPath pattern to apply on data input                                    | {data: newData}   | -                                            |
| Output Pattern | JMESPath pattern to apply on enrich data output, before returning response | {stored: oldData} | -                                            |
| Task Id Path   | Json path which defines id for timed out process                           | id                | \[requestId]:\[sagaStepId]\[runnerPartition] |
| Enrich Path    | Json path to use for enriching timed out process event data                | newData           | -                                            |
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
              "description": "JMESPath pattern to apply on data input",
              "example": "{data: newData}"
            },
            "outputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on enrich data output, before returning response",
              "example": "{stored: oldData}"
            },
            "taskIdPath": {
              "type": "string",
              "description": "Json path which defines id for timed out process",
              "default": "[requestId]:[sagaStepId][runnerPartition]",
              "example": "id"
            },
            "enrichPath": {
              "type": "string",
              "description": "Json path to use for enriching timed out process event data",
              "example": "newData"
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
Tasks that are created with StartTask automatically timeout based on their parameter values. This task provides an extra facility to timeout records manually.
{% endhint %}

### TimeoutBetween

Triggers timeout of all processes with timeout time within the given range. Event metadata fields applicable for this action are as follows:

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
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
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
| Parameter      | Definition                                                 | Example | Default      |
| -------------- | ---------------------------------------------------------- | ------- | ------------ |
| From Time Path | Json path for "from time" of timeouts                      | start   | Last timeout |
| To Time Path   | Json path for "to time" of timeouts (updates last timeout) | end     | -            |
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
            "fromTimePath": {
              "type": "string",
              "description": "Json path for \"from time\" of timeouts",
              "default": "Last timeout",
              "example": "start"
            },
            "toTimePath": {
              "type": "string",
              "description": "Json path for \"to time\" of timeouts (updates last timeout)",
              "example": "end"
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

[^1]: If given path is null, \[requestId]:\[sagaStepId]\[runnerPartition] is used
