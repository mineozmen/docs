---
description: >-
  These actions provide a state-based authentication implementation using
  existing states as a credential store.
---

# State Based

## State Based Actions

### Login

Extra event metadata parameters for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter  | Definition                                                         | Example | Default                 |
| ---------- | ------------------------------------------------------------------ | ------- | ----------------------- |
| Expiration | Seconds for expiration of access token for a specific login action | 1200    | Handler's configuration |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "State Based Login action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "expiration": {
              "type": "number",
              "definition": "Seconds for expiration of access token for a specific login action",
              "example": 1200,
              "default": "Handler's configuration"
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

### Refresh

Extra event metadata parameters for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter          | Definition                                                                         | Example | Default |
| ------------------ | ---------------------------------------------------------------------------------- | ------- | ------- |
| Allow Unregistered | Whether refresh tokens should be valid if they don't belong to users in auth.state | true    | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "State Based Refresh action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "allowUnregistered": {
              "type": "boolean",
              "definition": "Whether refresh tokens should be valid if they don't belong to users in auth.state",
              "example": true,
              "default": false
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
Allowing unregistered user refresh can be useful where user registration is optional and stateless authentication is used.
{% endhint %}
