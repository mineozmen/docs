---
description: >-
  This handler (com.rierino.handler.JsoupEventHandler) provides ability to parse
  HTML documents and return their contents as structured JSON.
---

# Parse Html

## Handler Parameters

This handler requires no parameters.

## Actions

### ParseHtml

Produces JSON from given input html text or page url. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                                                       | Example | Default |
| -------------- | ------------------------------------------------------------------------------------------------ | ------- | ------- |
| Input Element  | Json path for the input in request event payload (should include "html" or "url" field)          | content | -       |
| Output Element | Json path for the output in response event payload (includes "elements" as the list of children) | parsed  | -       |
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
          "description": "Json path for the input in request event payload (should include \"html\" or \"url\" field)",
          "example": "content"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload (includes \"elements\" as the list of children)",
          "example": "parsed"
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
| Parameter | Definition                                            | Example   | Default |
| --------- | ----------------------------------------------------- | --------- | ------- |
| XPath     | XPath expression to select elements from document     | -         | -       |
| CSS Query | CSS query expression to select elements from document | #products | -       |
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
            "xpath": {
              "type": "string",
              "description": "XPath expression to select elements from document"
            },
            "cssQuery": {
              "type": "string",
              "description": "CSS query expression to select elements from document",
              "example": "#products"
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
