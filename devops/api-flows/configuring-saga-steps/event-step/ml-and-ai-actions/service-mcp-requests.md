---
description: >-
  These actions provide ability to expose existing microservice capabilities
  over MCP.
---

# Service MCP Requests

## Service MCP Requests Actions

### CallRPC

Responds to a request using MCP protocol. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example                 | Default |
| -------------- | -------------------------------------------------- | ----------------------- | ------- |
| Domain         | ID of the MCP server to interact with              | procurement\_specialist | -       |
| Input Element  | Json path for the input in request event payload   | message                 | -       |
| Output Element | Json path for the output in response event payload | output                  | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
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
          "description": "ID of the MCP server to interact with",
          "examples": ["procurement_specialist"],
          "default": null
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "examples": ["message"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
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
{% tab title="Parameters (table)" %}
| Parameter     | Definition                                                   | Example | Default |
| ------------- | ------------------------------------------------------------ | ------- | ------- |
| Input Pattern | Jmespath expression for converting input payload to MCP call | -       | -       |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
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
              "description": "Jmespath expression for converting input payload to MCP call",
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

Input contents should match MCP server calls such as:

#### Initialize Request

```json
{
    "jsonrpc": "2.0",
    "method": "initialize"
}
```

#### Tool Listing Request

```json
{
    "jsonrpc": "2.0",
    "method": "tools/list"
}
```

#### Tool Call Request

```json
{
    "jsonrpc": "2.0",
    "method": "tools/call",
    "params": {
        "name": "Hello",
        "arguments": {}
    }
}
```
