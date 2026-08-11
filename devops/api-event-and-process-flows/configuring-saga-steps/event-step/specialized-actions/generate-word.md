---
description: >-
  These actions provide ability to produce formatted Word files using templates
  and input payload.
---

# Generate Word

## Generate Word Actions

### ExportDOCX

Produces a Word (`.docx`) file from given input data. The document can be produced in three ways: from an existing template using **content controls**, from an existing template using **mail merge** (MERGEFIELDs), or from scratch by converting **HTML** supplied in the input payload. Event metadata fields applicable for this action are as follows:

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
  "title": "Generate Word ExportDOCX action eventMeta fields",
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
| Parameter       | Definition                                                                                                                                                                      | Example                 | Default    |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ---------- |
| Output Path     | Path on the target file system to write the generated document to                                                                                                               | output/contract.docx    | -          |
| Template Path   | Path on the target file system to read the template document from. If omitted, the document is built from the `html` field in the input                                         | templates/contract.docx | -          |
| Template Format | How the template is populated: `controls` fills content controls matched by tag; any other value performs a mail merge on MERGEFIELDs. Only applies when a template path is set | controls                | mail merge |
| Input Pattern   | JMESPath pattern to apply on input element                                                                                                                                      | {name: name}            | -          |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate Word ExportDOCX action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "outputPath": {
              "type": "string",
              "definition": "Path on the target file system to write the generated document to",
              "example": "output/contract.docx",
              "default": null
            },
            "templatePath": {
              "type": "string",
              "definition": "Path on the target file system to read the template document from; if omitted, the document is built from the html field in the input",
              "example": "templates/contract.docx",
              "default": null
            },
            "templateFormat": {
              "type": "string",
              "definition": "How the template is populated: 'controls' fills content controls matched by tag, any other value performs a mail merge on MERGEFIELDs",
              "example": "controls",
              "default": null
            },
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on input element",
              "example": "{name: name}",
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

## Content Format

The shape of the input payload depends on how the document is produced.

**Content controls** — used when `templateFormat` is `controls`. Each top-level field in the input maps to a Word content control whose **tag** equals the field name; the control's text is replaced with the field value. Controls in the document body and in headers/footers are both filled. Only non-null fields are applied.

```json
{
    "customerName": "Acme Corp",
    "orderDate": "2026-01-15",
    "total": "$1,299.00"
}
```

**Mail merge** — the default when a template path is set (any `templateFormat` other than `controls`). Each top-level field maps to a `MERGEFIELD` of the same name in the template.

```json
{
    "customerName": "Acme Corp",
    "orderDate": "2026-01-15",
    "total": "$1,299.00"
}
```

**HTML** — used when no template path is given. The input must contain an `html` field whose value is converted into the document body.

```json
{
    "html": "<h1>Invoice</h1><p>Dear customer, ...</p>"
}
```
