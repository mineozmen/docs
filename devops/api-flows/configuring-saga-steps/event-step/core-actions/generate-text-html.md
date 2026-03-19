---
description: >-
  These actions provide ability to produce text outputs using templates and
  input payload.
---

# Generate Text/Html

## Generate Text/Html Actions

### ApplyTemplate

Produces text (e.g. plain, html) from given input data and template id. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example  | Default |
| -------------- | -------------------------------------------------- | -------- | ------- |
| Input Element  | Json path for the input in request event payload   | customer | -       |
| Output Element | Json path for the output in response event payload | html     | -       |
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
          "example": "customer"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "html"
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
| Parameter        | Definition                                       | Example             | Default |
| ---------------- | ------------------------------------------------ | ------------------- | ------- |
| Template ID      | Id of the template to use from code.state        | landing\_page       | -       |
| Template ID Path | Json path of the template in event payload       | parameters.template | -       |
| Result Path      | Json path to add produced text on output element | output              | result  |
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
            "templateId": {
              "type": "string",
              "description": "Id of the template to use from code.state",
              "example": "landing_page"
            },
            "templateIdPath": {
              "type": "string",
              "description": "Json path of the template in event payload",
              "example": "parameters.template"
            },
            "resultPath": {
              "type": "string",
              "description": "Json path to add produced text on output element",
              "default": "result",
              "example": "output"
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
        "course": "Rierino 101",
        "chapters": [
            {"name": "Introduction"}
        ] 
    }
}
```

Event Metadata

### ![](<../../../../../.gitbook/assets/image (106).png>)

</details>

### ApplyTemplateList

Produces text from a given array of input data and template ids. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | list    | -       |
| Output Element | Json path for the output in response event payload | html    | -       |
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
          "example": "list"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "html"
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
| Parameter        | Definition                                                    | Example | Default |
| ---------------- | ------------------------------------------------------------- | ------- | ------- |
| Output Structure | Structure of output to produce (map[^1], array[^2], text[^3]) | map     | -       |
| Output Delimiter | Delimiter for concatenating results if structure is "text"    | \n      | -       |
| Input Path       | Json path to get list of requests from                        | list    | input   |
| Result Path      | Json path to add produced text on output element              | output  | result  |
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
            "structure": {
              "type": "string",
              "description": "Structure of output to produce (map, array, text)",
              "enum": ["map", "array", "text"],
              "example": "map"
            },
            "delimiter": {
              "type": "string",
              "description": "Delimiter for concatenating results if structure is \"text\"",
              "example": "\\n"
            },
            "inputPath": {
              "type": "string",
              "description": "Json path to get list of requests from",
              "default": "input",
              "example": "list"
            },
            "resultPath": {
              "type": "string",
              "description": "Json path to add produced text on output element",
              "default": "result",
              "example": "output"
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

[^1]: {section:\[{templateId, result}]}

[^2]: \[{section, templateId, result}]

[^3]: Concatenated result text
