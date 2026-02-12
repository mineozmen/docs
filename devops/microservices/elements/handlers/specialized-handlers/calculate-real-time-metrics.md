---
description: >-
  This handler (com.rierino.handler.siddhi.JsonSiddhiEventHandler) provides
  ability to build real-time intelligence and CEP capabilities using Siddhi
  library.
---

# Calculate Real-time Metrics

## Handler Parameters

| Parameter             | Definition                                                           | Example                    | Default        |
| --------------------- | -------------------------------------------------------------------- | -------------------------- | -------------- |
| dataflow.state        | Name of the state manager with dataflow configurations               | dataflow                   | -              |
| dataflow.ids          | Comma separated list of dataflow ids to run on this Siddhi instance  | session\_dna,customer\_dna | -              |
| partition.stream      | Stream, from which the data partition information should be received | sessionq                   | -              |
| dataflow.store.system | Name of the system to store Siddhi persistence data                  | hdfs\_default              | -              |
| dataflow.local.dir    | Local directory to store interim Siddhi persistence data             | /tmp                       | java.io.tmpdir |

{% file src="../../../../../.gitbook/assets/handler-siddhi-0001.json" %}
Example Siddhi Handler Definition (Can be Imported on Element Screen)
{% endfile %}

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'siddhi', version:"${rierinoVersion}")
```

## Actions

### Get / GetRT

Returns stored real-time data for a given id from Siddhi tables. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example         | Default |
| -------------- | -------------------------------------------------- | --------------- | ------- |
| Domain         | Name of the Siddhi table to query on               | session\_latest | -       |
| Input Element  | Json path for the input in request event payload   | basket          | -       |
| Output Element | Json path for the output in response event payload | basket          | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Calculate Real-time Metrics Get/GetRT action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the Siddhi table to query on",
          "example": "session_latest",
          "default": null
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "basket",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "basket",
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
| Parameter      | Definition                                                          | Example                        | Default |
| -------------- | ------------------------------------------------------------------- | ------------------------------ | ------- |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | {totalPrice:basket.totalPrice} | -       |
| Dataflow       | Id of the dataflow from which data will be received                 | session\_dna                   | -       |
| Id Field       | Name of the id field to query on Siddhi table                       | sessionid                      | id      |
| Id Path        | Json path for the id field in input element                         | session.id                     | id      |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Calculate Real-time Metrics Get/GetRT action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{totalPrice:basket.totalPrice}",
              "default": null
            },
            "dataflow": {
              "type": "string",
              "definition": "Id of the dataflow from which data will be received",
              "example": "session_dna",
              "default": null
            },
            "idField": {
              "type": "string",
              "definition": "Name of the id field to query on Siddhi table",
              "example": "sessionid",
              "default": "id"
            },
            "idPath": {
              "type": "string",
              "definition": "Json path for the id field in input element",
              "example": "session.id",
              "default": "id"
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
        "id": "path-1"
    }
}
```

Event Metadata

### ![](<../../../../../.gitbook/assets/image (126).png>)

</details>

### GetQuery / GetQueryRT

Returns stored real-time data for a given id from Siddhi tables. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | basket  | -       |
| Output Element | Json path for the output in response event payload | product | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Calculate Real-time Metrics GetQuery/GetQueryRT action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "basket",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "product",
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
| Parameter      | Definition                                                          | Example                                               | Default |
| -------------- | ------------------------------------------------------------------- | ----------------------------------------------------- | ------- |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | {id:id, name:data.name, description:data.description} | -       |
| Dataflow       | Id of the dataflow from which data will be received                 | session\_dna                                          | -       |
| Query Id       | Id of the query to execute                                          | product\_search\_0001                                 | -       |
| Query Name     | Name of the query to execute                                        | Product Search                                        | -       |
| Query Json     | Full Json representation of query to execute                        | -                                                     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Calculate Real-time Metrics GetQuery/GetQueryRT action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}",
              "default": null
            },
            "dataflow": {
              "type": "string",
              "definition": "Id of the dataflow from which data will be received",
              "example": "session_dna",
              "default": null
            },
            "queryId": {
              "type": "string",
              "definition": "Id of the query to execute",
              "example": "product_search_0001",
              "default": null
            },
            "queryJson": {
              "type": "object",
              "definition": "Full Json representation of query to execute",
              "default": null
            },
            "queryName": {
              "type": "string",
              "definition": "Name of the query to execute",
              "example": "Product Search",
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

<details>

<summary>Example</summary>

Input

```json
{
    "parameters": {
        "category": "cat-1"
    }
}
```

Event Metadata

## ![](<../../../../../.gitbook/assets/image (127).png>)

</details>

## Roles

### processRole

Passes a Json object to Siddhi, mapping it to a source stream across all data flows with a matching input, which triggers real-time update of metric calculations.
