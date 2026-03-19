---
description: >-
  These actions provide ability to produce formatted PDF files from HTML
  content.
---

# Generate PDF

## Generate PDF Actions

### ExportPDF

Produces a PDF file from given input data and template id. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                                       | Example  | Default |
| ------------- | ------------------------------------------------ | -------- | ------- |
| Input Element | Json path for the input in request event payload | customer | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate PDF ExportPDF action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "customer",
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
| Parameter     | Definition                                                               | Example                     | Default |
| ------------- | ------------------------------------------------------------------------ | --------------------------- | ------- |
| Input Pattern | JMESPath pattern to apply on input element                               | {html: text}                | -       |
| Html Path     | Json path in payload including html text to convert to PDF               | input                       | html    |
| Output Path   | Json path in payload defining the output file path on target file system | file\_path                  | path    |
| Font Paths    | Comma separated list of font file paths to use in rendering              | /app/fonts/custom\_font.ttf | -       |
| Html Base     | Base URL root to use for converting html contents into PDF               | https://example.com         | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate PDF ExportPDF action eventMeta.parameters",
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
              "example": "{html: text}",
              "default": null
            },
            "htmlPath": {
              "type": "string",
              "definition": "Json path in payload including html text to convert to PDF",
              "example": "input",
              "default": "html"
            },
            "outputPath": {
              "type": "string",
              "definition": "Json path in payload defining the output file path on target file system",
              "example": "file_path",
              "default": "path"
            },
            "fontPaths": {
              "type": "string",
              "definition": "Comma separated list of font file paths to use in rendering",
              "example": "/app/fonts/custom_font.ttf",
              "default": null
            },
            "htmlBase": {
              "type": "string",
              "definition": "Base URL root to use for converting html contents into PDF",
              "example": "https://example.com",
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
This action is typically used together with [Generate Text/Html](../core-actions/generate-text-html.md) to produce formatted PDF files.
{% endhint %}
