---
description: >-
  These actions provide ability to execute sequential actions using different
  event handlers at once.
---

# Run Multiple Steps

## Run Multiple Steps Actions

### RunSteps

Runs multiple steps sequentially and passes each one's result to the next. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                                                                                | Example    | Default |
| -------------- | ----------------------------------------------------------------------------------------- | ---------- | ------- |
| Domain         | Name of the system to make REST call to (url and auth parameters of this system are used) | erp        | -       |
| Input Element  | Json path for the input in request event payload                                          | parameters | -       |
| Output Element | Json path for the output in response event payload                                        | product    | -       |
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
          "definition": "Name of the system to make REST call to (url and auth parameters of this system are used)",
          "example": "erp",
          "default": null
        },
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "parameters",
          "default": null
        },
        "outputElement": {
          "type": "string",
          "definition": "Json path for the output in response event payload",
          "example": "product",
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
| Parameter     | Definition                                       | Example                   | Default |
| ------------- | ------------------------------------------------ | ------------------------- | ------- |
| Common Fields | Event metadata fields applicable for all steps   | inputElement=parameters   | -       |
| Step Action   | Name of the action to run for step \[step]       | GetQuery                  | -       |
| Step Fields   | Event metadata field applicable for step \[step] | {id:parameters.productid} | -       |
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
            "common.[field]": {
              "type": "string",
              "definition": "Event metadata fields applicable for all steps",
              "example": "inputElement=parameters",
              "default": null
            },
            "step_[step]_action": {
              "type": "string",
              "definition": "Name of the action to run for step [step]",
              "example": "GetQuery",
              "default": null
            },
            "step_[step].[field]": {
              "type": "string",
              "definition": "Event metadata field applicable for step [step]",
              "example": "{id:parameters.productid}",
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

<details>

<summary>Example</summary>

Input

```json
{
    "parameters": {
        "language": "enUS"
    },
    "facets":{
        "brands": ["brand-1"],
        "categories": ["cat-1"]
    }

}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (109).png>)

</details>

{% hint style="info" %}
Common event metadata fields are used as default and merged with step specific metadata fields such as inputElement, handler or parameters.
{% endhint %}
