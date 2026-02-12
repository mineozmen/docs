---
description: >-
  This handler (com.rierino.handler.py4j.Py4JEventHandler) provides ability to
  handle events using Python libraries.
---

# Run Python Procedure

## Handler Parameters

This handler has no special parameters.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'py4j', version:"${rierinoVersion}")
```

## Actions

### Process / ProcessPython

Passes requested event to Python interface for processing. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                             | Example    | Default |
| -------------- | -------------------------------------- | ---------- | ------- |
| Input Element  | Json path of payload input to pass     | parameters | -       |
| Output Element | Json path for output to add on payload | $          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure Process/ProcessPython action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path of payload input to pass",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for output to add on payload",
          "example": "$",
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
| Parameter      | Definition                                 | Example         | Default |
| -------------- | ------------------------------------------ | --------------- | ------- |
| Input Pattern  | JMESPath pattern to apply on input element | {data:contents} | -       |
| Python Package | Name of the Python package to use          | rierino\_runner | -       |
| Python Module  | Name of the Python module to use           | CustomHandler   | -       |
| Python Action  | Name of the Python function to call        | Calculate       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure Process/ProcessPython action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on input element",
              "example": "{data:contents}",
              "default": null
            },
            "pyPackage": {
              "type": "string",
              "definition": "Name of the Python package to use",
              "example": "rierino_runner",
              "default": null
            },
            "pyModule": {
              "type": "string",
              "definition": "Name of the Python module to use",
              "example": "CustomHandler",
              "default": null
            },
            "pyAction": {
              "type": "string",
              "definition": "Name of the Python function to call",
              "example": "Calculate",
              "default": null
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
Python function receives java handler, process input as dict, and event metadata parameters as input.
{% endhint %}

{% hint style="info" %}
Using Py4JEventHandler requires deployment of an extra container along with the runner, such as "ghcr.io/rierino-open/py-runner" image, configured from useExtraContainer and extraImage parameters of Deployment UI.
{% endhint %}

## Python Modules

The following python packages and modules are currently available out of box:

### NoopEventHandler

Used for Py4J gateway testing purposes only.

* **pyPackage:** rierino\_runner
* **pyModule:** NoopEventHandler
* **pyAction:** hello

### ProcessEventHandler

Generic handler for triggering any Python Process class (such as rierino\_runner.IterateProcess, rierino\_media.MediaEventProcess, rierino\_tensor.TFModelProcess).

* **pyPackage:** rierino\_runner
* **pyModule:** ProcessEventHandler
* **pyAction:** execute

{% tabs %}
{% tab title="Table" %}
| Parameter         | Definition                                                                                                                        | Example                                                                                | Default |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------- |
| sync              | Whether handler should wait for task completion to return a response or not                                                       | true                                                                                   | false   |
| systems           | Comma separated list of system names for which parameters shall be received from Java runner                                      | fs\_media,s3\_model                                                                    | -       |
| connections       | Json array containing details of connections to pass to handler (system configurations on Java runner is usually preferred)       | \[{name: "fs\_test", type: "fs", dir: "/tmp"}]                                         | -       |
| silent            | Whether handler should log process updates or not                                                                                 | true                                                                                   | false   |
| module            | Process module to execute (view [list](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/artifacts/docker-repository/python-images)) | rierino\_runner.IterateProcess                                                         | -       |
| \[module\_params] | Process module specific parameters                                                                                                | {"iterator":{ "module": "rierino\_runner.iterator.DataIterator", "element": "list" \}} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure ProcessEventHandler eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "sync": {
              "type": "boolean",
              "definition": "Whether handler should wait for task completion to return a response or not",
              "example": true,
              "default": false
            },
            "systems": {
              "type": "string",
              "definition": "Comma separated list of system names for which parameters shall be received from Java runner",
              "example": "fs_media,s3_model",
              "default": null
            },
            "connections": {
              "type": "array",
              "definition": "Json array containing details of connections to pass to handler (system configurations on Java runner is usually preferred)",
              "example": [
                {
                  "name": "fs_test",
                  "type": "fs",
                  "dir": "/tmp"
                }
              ],
              "default": null,
              "items": {
                "type": "object"
              }
            },
            "silent": {
              "type": "boolean",
              "definition": "Whether handler should log process updates or not",
              "example": true,
              "default": false
            },
            "module": {
              "type": "string",
              "definition": "Process module to execute",
              "example": "rierino_runner.IterateProcess",
              "default": null
            },
            "module_params": {
              "type": "object",
              "definition": "Process module specific parameters",
              "example": {
                "iterator": {
                  "module": "rierino_runner.iterator.DataIterator",
                  "element": "list"
                }
              },
              "default": null
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

### DQEventHandler

Performs data quality assessment of given inputs against a configured list of checks.

* **pyPackage:** rierino\_dq
* **pyModule:** DQEventHandler
* **pyAction:** eval

Has the following set of additional event metadata parameters:

{% tabs %}
{% tab title="Table" %}
| Parameter   | Definition                                                                          | Example               | Default |
| ----------- | ----------------------------------------------------------------------------------- | --------------------- | ------- |
| configPath  | Json path in input payload for DQ check configuration (if not using a config state) | parameters.dq\_config | -       |
| configState | Name of the runner state manager storing DQ check configs                           | dq\_main              | dq      |
| configId    | Id of the config stored in state manager                                            | product\_dq           | default |
| reload      | Whether config should be reloaded from state                                        | true                  | false   |
| summarize   | Whether results should be summarized                                                | true                  | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Python Procedure DQEventHandler eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "configPath": {
              "type": "string",
              "definition": "Json path in input payload for DQ check configuration (if not using a config state)",
              "example": "parameters.dq_config",
              "default": null
            },
            "configState": {
              "type": "string",
              "definition": "Name of the runner state manager storing DQ check configs",
              "example": "dq_main",
              "default": "dq"
            },
            "configId": {
              "type": "string",
              "definition": "Id of the config stored in state manager",
              "example": "product_dq",
              "default": "default"
            },
            "reload": {
              "type": "boolean",
              "definition": "Whether config should be reloaded from state",
              "example": true,
              "default": false
            },
            "summarize": {
              "type": "boolean",
              "definition": "Whether results should be summarized",
              "example": true,
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

<details>

<summary>Example</summary>

Input

```json
{
    "parameters": {
        "id": "product-1",
        "data": {
             "name": "Test Product",
             "description": "Too Short"
        }
    }
}
```

Event Metadata

### ![](<../../../../../.gitbook/assets/image (129).png>)

</details>

{% hint style="info" %}
Additional python classes can be implemented and deploy using py-runner library for use with Py4JEventHandler, simply having the following two functions:

* def init(self, javaHandler)
* def \[FUNCTION NAME]\(self, javaHandler, processDict, parameters)
{% endhint %}
