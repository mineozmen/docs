---
description: >-
  These actions provide ability to generate and execute system specific queries
  on a query manager on demand, from simple select statements to complex
  requests.
---

# Query Data

## Query Data Actions

### GetQuery

Executes and returns results for a single query from a specific query manager. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                   | Example    | Default |
| -------------- | ------------------------------------------------------------ | ---------- | ------- |
| Domain         | Name of the query manager to read data from                  | product    | -       |
| Input Element  | Json path for the input in event payload for query variables | parameters | -       |
| Output Element | Json path for the output in response event payload           | $.list     | -       |


{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Query Data GetQuery action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the query manager to read data from",
          "example": "product"
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload for query variables",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "$.list"
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
| Query ID       | Id of the query to execute                                          | product\_search\_0001                                 | -       |
| Query Name     | Name of the query to execute                                        | Product Search                                        | -       |
| Query Json     | Full Json representation of query to execute                        | -                                                     | -       |
| First          | Whether to return first record only from results                    | true                                                  | false   |
| Sort Path      | Json path in query results to be used for sorting                   | product.id                                            | -       |
| Sorter Path    | Json path in request event payload to be used for sorting           | productids                                            | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Query Data GetQuery action eventMeta.parameters",
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
              "example": "{id:id, name:data.name, description:data.description}"
            },
            "queryId": {
              "type": "string",
              "definition": "Id of the query to execute",
              "example": "product_search_0001"
            },
            "queryName": {
              "type": "string",
              "definition": "Name of the query to execute",
              "example": "Product Search"
            },
            "queryJson": {
              "type": "object",
              "definition": "Full Json representation of query to execute"
            },
            "first": {
              "type": "boolean",
              "definition": "Whether to return first record only from results",
              "default": false
            },
            "sortPath": {
              "type": "string",
              "definition": "Json path in query results to be used for sorting",
              "example": "product.id"
            },
            "sorterPath": {
              "type": "string",
              "definition": "Json path in request event payload to be used for sorting",
              "example": "productids"
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
        "search": "test"
    }
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (134).png>)

</details>

{% hint style="info" %}
sortPath and sorterPath parameters are used to allow sorting of results by an already prioritized list of ids (such as product ids sorted based on search releavence).
{% endhint %}

### ProduceQuery

Generates a query statement for a specific query manager, which is typically used by 3rd party systems in converting query objects into system specific requests. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                   | Example    | Default |
| -------------- | ------------------------------------------------------------ | ---------- | ------- |
| Domain         | Name of the query manager to produce statement for           | product    | -       |
| Input Element  | Json path for the input in event payload for query variables | parameters | -       |
| Output Element | Json path for the output in response event payload           | $.query    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Query Data ProduceQuery action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the query manager to produce statement for",
          "example": "product"
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload for query variables",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "$.query"
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
| Parameter  | Definition                                   | Example               | Default |
| ---------- | -------------------------------------------- | --------------------- | ------- |
| Query ID   | Id of the query to execute                   | product\_search\_0001 | -       |
| Query Name | Name of the query to execute                 | Product Search        | -       |
| Query Json | Full Json representation of query to execute | -                     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Query Data ProduceQuery action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "queryId": {
              "type": "string",
              "definition": "Id of the query to execute",
              "example": "product_search_0001"
            },
            "queryJson": {
              "type": "object",
              "definition": "Full Json representation of query to execute"
            },
            "queryName": {
              "type": "string",
              "definition": "Name of the query to execute",
              "example": "Product Search"
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
Produced statement is returned as a "query" field of the outputElement.
{% endhint %}
