---
description: >-
  This handler (com.rierino.handler.RestAPIEventHandler) provides ability to
  make REST based calls to other internal or external services and return their
  results for API-based integrations.
---

# Call Rest API

## Handler Parameters

This handler requires no parameters. Instead, target [rest system](../../systems/#rest) referred to by name using 'domain' field defines the base URL and credentials for all calls. This [handler is](#user-content-fn-1)[^1] [cacheable](../#handler-caching).

## Actions

### CallRest

Makes a REST call and returns the result. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                                                | Example    | Default |
| -------------- | ----------------------------------------------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the system to make REST call to (url and auth parameters of this system are used) | erp        | -       |
| Input Element  | Json path for the input in request event payload                                          | parameters | -       |
| Output Element | Json path for the output in response event payload                                        | $.product  | -       |
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
        "domain": {
          "type": "string",
          "description": "Name of the system to make REST call to (url and auth parameters of this system are used)",
          "example": "erp"
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "example": "parameters"
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "example": "$.product"
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
| Parameter       | Definition                                                                                                                                                 | Example                   | Default          |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ---------------- |
| Input Pattern   | JMESPath pattern to apply on data input                                                                                                                    | {id:parameters.productid} | -                |
| Output Pattern  | JMESPath pattern to apply on data output, before returning response                                                                                        | {id:id, stock:totalUnits} | -                |
| Url             | Url path on the target system for REST call                                                                                                                | GetInventory              | -                |
| Append Url Path | Payload element to add to the end of REST call url path                                                                                                    | parameters.language       | -                |
| Auth Path       | Payload element to use for auth.\* parameters of REST system                                                                                               | parameters.auth           | -                |
| Method          | Call method to use                                                                                                                                         | POST                      | GET              |
| Content Type    | Content type for REST calls (none, query, application/json, application/xml, text/xml, text/plain, multipart/form-data, application/x-www-form-urlencoded) | application/xml           | application/json |
| Headers         | Json string defining list of headers to add to the request                                                                                                 | {"content-encoding":"br"} | -                |
| Query.\*        | Query parameters to pass on the request URL                                                                                                                | language=en-US            | -                |
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
            "inputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data input",
              "example": "{id:parameters.productid}"
            },
            "outputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data output, before returning response",
              "example": "{id:id, stock:totalUnits}"
            },
            "url": {
              "type": "string",
              "description": "Url path on the target system for REST call",
              "example": "GetInventory"
            },
            "urlPath": {
              "type": "string",
              "description": "Payload element to add to the end of REST call url path",
              "example": "parameters.language"
            },
            "authPath": {
              "type": "string",
              "description": "Payload element to use for auth.* parameters of REST system",
              "example": "parameters.auth"
            },
            "method": {
              "type": "string",
              "description": "Call method to use",
              "default": "GET",
              "example": "POST"
            },
            "contentType": {
              "type": "string",
              "description": "Content type for REST calls",
              "enum": [
                "none",
                "query",
                "application/json",
                "application/xml",
                "text/xml",
                "text/plain",
                "multipart/form-data",
                "application/x-www-form-urlencoded"
              ],
              "default": "application/json",
              "example": "application/xml"
            },
            "headers": {
              "type": "string",
              "description": "Json string defining list of headers to add to the request",
              "example": "{\"content-encoding\":\"br\"}"
            }
          },
          "patternProperties": {
            "^query\\..+$": {
              "type": "string",
              "description": "Query parameters to pass on the request URL (use keys like \"query.language\")",
              "example": "en-US"
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
        "measures": "Population"
    }
}
```

Event Metadata

## ![](<../../../../../.gitbook/assets/image (135).png>)

</details>

For contentType = "multipart/form-data", files can be added as fields using the following fields (all other fields are sent as text fields):

* **$type:** Should be set as "FILE"
* **body:** Text or JSON body to be sent as file contents
* **fileName:** File name to use for sending contents
* **mimeType:** Mime-type for the file contents

**Example:** {"$type": "FILE", "body": {test: true}, "fileName": "test.json"}

Depending on the response format, this handler returns the following data:

<table><thead><tr><th width="169">Format</th><th>Process</th></tr></thead><tbody><tr><td>json</td><td>Response is returned as is</td></tr><tr><td>xml</td><td>Response is converted to json format</td></tr><tr><td><a data-footnote-ref href="#user-content-fn-2">plain</a></td><td>Response is returned as {"text": [text]}</td></tr><tr><td><a data-footnote-ref href="#user-content-fn-2">html</a></td><td>Response is returned as {"html": [html]}</td></tr></tbody></table>

### ProduceRest

Produces rest call details without executing them for debugging purposes. Uses the same parameters as CallRest action.

## Commands

### RECONFIGURE

Reloads all REST target system properties, injecting connection and credential parameters with latest values. Typically used if the base URLs or API credentials change for the target system.

[^1]: Since version 0.4.1

[^2]: since 0.5.2
