---
description: >-
  This handler (com.rierino.handler.soap.SoapEventHandler) provides ability to
  use SOAP services for 3rd party integrations.
---

# Call SOAP API

## Handler Parameters

| Parameter      | Definition                                                                                                     | Example        | Default           |
| -------------- | -------------------------------------------------------------------------------------------------------------- | -------------- | ----------------- |
| soap.state     | Name of the state manager keeping soap service definitions (files, soapVersion, serviceName, serviceClassName) | soap\_services | handler\_soap     |
| soap.domain    | Domain name for filtering entries in soap.state                                                                | pricing        | -                 |
| soap.local.dir | Local directory to use for storing service jar files                                                           | /tmp           | \[java.io.tmpdir] |

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'soap', version:"${rierinoVersion}")
```

## Actions

### GetAvailableOperations

Gets list of available operations/methods for a SOAP service. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Output Element | Json path for the output in response event payload | ops     | list    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Call SOAP API GetAvailableOperations action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "ops",
          "default": "list"
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
| Parameter    | Definition                                | Example           | Default |
| ------------ | ----------------------------------------- | ----------------- | ------- |
| Service Name | Name of the service to get operations for | price\_calculator | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Call SOAP API GetAvailableOperations action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "serviceName": {
              "type": "string",
              "definition": "Name of the service to get operations for",
              "example": "price_calculator",
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

### Invoke

Invokes a specific operation on a SOAP service. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | ops        | list    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Call SOAP API Invoke action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "ops",
          "default": "list"
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
| Parameter      | Definition                               | Example                   | Default |
| -------------- | ---------------------------------------- | ------------------------- | ------- |
| Input Pattern  | JMESPath pattern to apply on data input  | {"item": product}         | -       |
| Output Pattern | JMESPath pattern to apply on data output | {"product\_price": price} | -       |
| Service Name   | Name of the service to use               | price\_calculator         | -       |
| Operation      | Name of the operation to call            | calculate                 | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Call SOAP API Invoke action eventMeta.parameters",
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
              "definition": "JMESPath pattern to apply on data input",
              "example": "{\"item\": product}",
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on data output",
              "example": "{\"product_price\": price}",
              "default": null
            },
            "serviceName": {
              "type": "string",
              "definition": "Name of the service to use",
              "example": "price_calculator",
              "default": null
            },
            "operation": {
              "type": "string",
              "definition": "Name of the operation to call",
              "example": "calculate",
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
Input element of the event payload should match parameter names required by the SOAP operation.
{% endhint %}
