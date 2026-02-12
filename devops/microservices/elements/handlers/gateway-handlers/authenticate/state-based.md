---
description: >-
  This handler (com.rierino.handler.auth.StatesAuthEventHandler) provides a
  state based implementation of AuthEventHandler, using existing states as
  credential store with salted passwords.
---

# State Based

This handler uses the following extra configurations and parameters:

## Handler Parameters

| Parameter              | Definition                                     | Example              | Default              |
| ---------------------- | ---------------------------------------------- | -------------------- | -------------------- |
| auth.state             | Name of state manager to store credentials     | auth\_store          | -                    |
| auth.secret            | Secret used for hashing passwords and tokens   | -                    | -                    |
| auth.expiration        | Seconds for expiration of any new access token | 900                  | 600                  |
| auth.refreshExpiration | Seconds for expiration of refresh tokens       | 9000                 | 6000                 |
| auth.iterations        | Number of iterations to salt passwords         | 5                    | 1                    |
| auth.saltLength        | Length of the salt string                      | 32                   | 16                   |
| auth.keyLength         | Key length for PBKDF2 algorithms               | 1024                 | 512                  |
| auth.algorithm         | Hashing algorithm to use for storing passwords | PBKDF2WithHmacSHA256 | PBKDF2WithHmacSHA256 |
| auth.issuer            | Name of issuer to include in generated tokens  | Rierino              | -                    |

{% file src="../../../../../../.gitbook/assets/handler-auth_admin-0001.json" %}
Example State Auth Handler Definition (Can be Imported on Element Screen)
{% endfile %}

## Actions

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
Allowing unregistered user refresh can be used in scenarios where user registration is optional and stateless authentication mechanisms are used (e.g. OTP only login without accounts).
{% endhint %}
