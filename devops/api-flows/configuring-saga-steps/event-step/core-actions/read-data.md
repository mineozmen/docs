---
description: >-
  These actions provide ability to select one, multiple or all records from a
  state manager on demand, facilitating common REST API read calls.
---

# Read Data

## Read Data Actions

### Get

Reads and returns aggregate for a single ID from a specific state manager. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the state manager to read data from        | product    | -       |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | product    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data Get action eventMeta fields",
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
          "example": "product"
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
| Parameter      | Definition                                                           | Example                                               | Default |
| -------------- | -------------------------------------------------------------------- | ----------------------------------------------------- | ------- |
| Output Pattern | JMESPath pattern to apply on data output, before returning response  | {id:id, name:data.name, description:data.description} | -       |
| ID Path        | Json path for the id field in input element                          | product.id                                            | id      |
| ID Value       | Static ID to read instead of idPath value                            | 1234                                                  | -       |
| Fields         | Comma separated list of fields to keep in response                   | data.name,data.description                            | -       |
| Form           | Form of response to produce (i.e. full=including custom data fields) | full                                                  | -       |
| Customize By   | Comma separated list of customizations to apply to data              | luxury,tech\_savvy                                    | -       |
| Version Type   | Type of version data to return (history[^1], snapshot[^2], none[^3]) | history                                               | none    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data Get action eventMeta.parameters",
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
            "idPath": {
              "type": "string",
              "definition": "Json path for the id field in input element",
              "default": "id",
              "example": "product.id"
            },
            "idValue": {
              "type": ["string", "integer"],
              "definition": "Static ID to read instead of idPath value",
              "example": 1234
            },
            "fields": {
              "type": "string",
              "definition": "Comma separated list of fields to keep in response",
              "example": "data.name,data.description"
            },
            "form": {
              "type": "string",
              "definition": "Form of response to produce (i.e. full=including custom data fields)",
              "example": "full"
            },
            "customizeBy": {
              "type": "string",
              "definition": "Comma separated list of customizations to apply to data",
              "example": "luxury,tech_savvy"
            },
            "versionType": {
              "type": "string",
              "definition": "Type of version data to return (history, snapshot, none)",
              "default": "none",
              "example": "history"
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
        "id": "given-id"
    }
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (133).png>)

</details>

### GetList

Reads and returns aggregates for a list of ID from a specific state manager. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example             | Default |
| -------------- | -------------------------------------------------- | ------------------- | ------- |
| Domain         | Name of the state manager to read data from        | product             | -       |
| Input Element  | Json path for the input in request event payload   | parameters.products | -       |
| Output Element | Json path for the output in response event payload | list                | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data GetList action eventMeta fields",
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
          "example": "parameters.products"
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "list"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If input element ends with .\*, it allows iteration of an array of {id:""} objects for injection of their details. Otherwise, this action expects a list of ids in \[""] form.
{% endhint %}

With event metadata parameters as:

{% tabs %}
{% tab title="Table" %}
| Parameter      | Definition                                                               | Example                                               | Default |
| -------------- | ------------------------------------------------------------------------ | ----------------------------------------------------- | ------- |
| Output Pattern | JMESPath pattern to apply on each data output, before returning response | {id:id, name:data.name, description:data.description} | -       |
| Fields         | Comma separated list of fields to keep in response                       | data.name,data.description                            | -       |
| Form           | Form of response to produce (i.e. full=including custom data fields)     | full                                                  | -       |
| Customize By   | Comma separated list of customizations to apply to data                  | luxury,tech\_savvy                                    | -       |
| Skip           | Number of rows to skip in results                                        | 20                                                    | -       |
| Limit          | Maximum number of rows to return                                         | 10                                                    | -       |
| Version Type   | Type of version data to return (history[^1], snapshot[^2], none[^3])     | history                                               | none    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data GetList action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on each data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}"
            },
            "fields": {
              "type": "string",
              "definition": "Comma separated list of fields to keep in response",
              "example": "data.name,data.description"
            },
            "form": {
              "type": "string",
              "definition": "Form of response to produce (i.e. full=including custom data fields)",
              "example": "full"
            },
            "customizeBy": {
              "type": "string",
              "definition": "Comma separated list of customizations to apply to data",
              "example": "luxury,tech_savvy"
            },
            "skip": {
              "type": "integer",
              "definition": "Number of rows to skip in results",
              "example": 20
            },
            "limit": {
              "type": "integer",
              "definition": "Maximum number of rows to return",
              "example": 10
            },
            "versionType": {
              "type": "string",
              "definition": "Type of version data to return (history, snapshot, none)",
              "default": "none",
              "example": "history"
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
For complex use cases with skip, limit, fields or outputPattern using QueryEventHandler could provide better performance, as it benefits from pass-thru query execution.
{% endhint %}

### GetAll

Reads and returns all aggregates from a specific state manager. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Domain         | Name of the state manager to read data from        | product | -       |
| Output Element | Json path for the output in response event payload | list    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data GetAll action eventMeta fields",
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
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "list"
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
| Parameter      | Definition                                                               | Example                                               | Default |
| -------------- | ------------------------------------------------------------------------ | ----------------------------------------------------- | ------- |
| Output Pattern | JMESPath pattern to apply on each data output, before returning response | {id:id, name:data.name, description:data.description} | -       |
| Fields         | Comma separated list of fields to keep in response                       | data.name,data.description                            | -       |
| Form           | Form of response to produce (i.e. full=including custom data fields)     | full                                                  | -       |
| Customize By   | Comma separated list of customizations to apply to data                  | luxury,tech\_savvy                                    | -       |
| Skip           | Number of rows to skip in results                                        | 20                                                    | -       |
| Limit          | Maximum number of rows to return                                         | 10                                                    | -       |
| Order By       | Order by field for results with direction                                | name desc                                             | -       |
| Count As       | Json path to return total number of rows on event payload                | total                                                 | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read Data GetAll action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on each data output, before returning response",
              "example": "{id:id, name:data.name, description:data.description}"
            },
            "fields": {
              "type": "string",
              "definition": "Comma separated list of fields to keep in response",
              "example": "data.name,data.description"
            },
            "form": {
              "type": "string",
              "definition": "Form of response to produce (i.e. full=including custom data fields)",
              "example": "full"
            },
            "customizeBy": {
              "type": "string",
              "definition": "Comma separated list of customizations to apply to data",
              "example": "luxury,tech_savvy"
            },
            "skip": {
              "type": "integer",
              "definition": "Number of rows to skip in results",
              "example": 20
            },
            "limit": {
              "type": "integer",
              "definition": "Maximum number of rows to return",
              "example": 10
            },
            "orderBy": {
              "type": "string",
              "definition": "Order by field for results with direction",
              "example": "name desc"
            },
            "countAs": {
              "type": "string",
              "definition": "Json path to return total number of rows on event payload",
              "example": "total"
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

[^1]: Returning all "versions" in result

[^2]: Returning single snapshot requested in "version" or "versionBase64" input payload

[^3]: Standard aggregate without version history
