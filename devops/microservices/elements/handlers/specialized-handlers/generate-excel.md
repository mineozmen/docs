---
description: >-
  This handler (com.rierino.handler.ExcelExportEventHandler) provides ability to
  produce well formatted Excel files using templates and input payload.
---

# Generate Excel

## Handler Parameters

| Parameter      | Definition                                          | Example        | Default        |
| -------------- | --------------------------------------------------- | -------------- | -------------- |
| template.state | Name of the state manager with template definitions | template\_xlsx | handler\_excel |
| output.system  | Alias of the system to store Excel file produced on | s3\_default    | -              |

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'excel', version:"${rierinoVersion}")
```

## Actions

### ExportXLSX

Produces Excel file from given input data and template id. Event metadata fields applicable for this action are as follows:

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
  "title": "Generate Excel ExportXLSX action eventMeta fields",
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
| Parameter     | Definition                                                                                             | Example        | Default |
| ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ------- |
| Template      | Id of the template to use from code.state                                                              | landing\_page  | -       |
| Input Pattern | JMESPath pattern to apply on input element                                                             | {sheets: list} | -       |
| Sheets Path   | Json path in payload including data for sheets (in {\[sheet\_name]: \[ {\[field]: \[value]} ]} format) | list           | sheets  |
| Output Path   | Json path in payload defining the output file path on target file system                               | file\_path     | path    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate Excel ExportXLSX action eventMeta.parameters",
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
              "definition": "Id of the template to use from code.state",
              "example": "landing_page",
              "default": null
            },
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on input element",
              "example": "{sheets: list}",
              "default": null
            },
            "sheetsPath": {
              "type": "string",
              "definition": "Json path in payload including data for sheets (in {[sheet_name]: [ {[field]: [value]} ]} format)",
              "example": "list",
              "default": "sheets"
            },
            "outputPath": {
              "type": "string",
              "definition": "Json path in payload defining the output file path on target file system",
              "example": "file_path",
              "default": "path"
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

Templates used by this handler have the following data format:

```json
{
    "name": "NAME",
    "header": {},
    "columns":[
        {"title": "TITLE", "field": "FIELD", "titleStyle": {}, "rowStyle": {}}
    ],
    "footer": {}
}
```

titleStyle and rowStyle are provided using [Apache POI cell style](https://poi.apache.org/apidocs/dev/org/apache/poi/ss/util/CellUtil.html) keys.
