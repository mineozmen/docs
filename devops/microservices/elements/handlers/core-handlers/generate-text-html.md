---
description: >-
  This handler (com.rierino.handler.TemplateEventHandler) provides ability to
  produce text outputs using templates and input payload.
---

# Generate Text/Html

## Handler Parameters

This [handler is](#user-content-fn-1)[^1] [cacheable](../#handler-caching).

| Parameter  | Definition                                                 | Example                                                  | Default       |
| ---------- | ---------------------------------------------------------- | -------------------------------------------------------- | ------------- |
| code.state | Name of the state manager with template definitions        | template\_code                                           | handler\_code |
| code.field | Json path for the data element of code record for template | template                                                 | code          |
| helper     | Class name for the template helper to use                  | com.rierino.handler.util.helper.HandlebarsTemplateHelper | -             |

{% file src="../../../../../.gitbook/assets/handler-handlebars-0001.json" %}
Example Template Handler Definition (Can be Imported on Element Screen)
{% endfile %}

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'handlebars', version:"${rierinoVersion}")
```

## Actions

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
| Output Structure | Structure of output to produce (map[^2], array[^3], text[^4]) | map     | -       |
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

## Helpers

### HandlebarsTemplateHelper

This helper (_com.rierino.handler.util.helper.HandlebarsTemplateHelper_) uses [Handlebars](https://handlebarsjs.com/) library for processing given template.

<details>

<summary>Example Template</summary>

```handlebars
<header><h1>{{course}}</h1></header>
{{#each chapters}}
    <h2>{{name}}</h2>
    {{#each sections}}
        <h3>{{name}}</h3>
    {{/each}}
{{/each}}
```

</details>

### FreemarkerTemplateHelper

This helper (_com.rierino.handler.util.helper.FreemarkerTemplateHelper_) uses [Freemarker](https://freemarker.apache.org/) library for processing given template.

[^1]: Since version 0.4.1

[^2]: {section:\[{templateId, result}]}

[^3]: \[{section, templateId, result}]

[^4]: Concatenated result text
