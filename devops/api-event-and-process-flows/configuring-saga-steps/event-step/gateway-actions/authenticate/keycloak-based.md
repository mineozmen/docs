---
description: These actions provide a Keycloak-based authentication implementation.
---

# Keycloak Based

## Keycloak Based Actions

### Register

Supports additional credential types provided as `credential_type` in the request, in addition to `password`.

### Login

Supports additional credential types provided as `grant_type` in the request, in addition to `password`.

### UpdatePassword

Allows updating password for a user represented by `access_token`, using `password` field.

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Keycloak Based UpdatePassword action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload",
          "example": "auth",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### ExecuteActionEmail

Triggers an action email through Keycloak for a specific action type such as `VALIDATE_EMAIL` or `FORGOT_EMAIL` for a given `username`.

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Keycloak Based ExecuteActionEmail action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload",
          "example": "auth",
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
| Parameter | Definition        | Example       | Default |
| --------- | ----------------- | ------------- | ------- |
| Action    | Action to execute | FORGOT\_EMAIL | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Keycloak Based ExecuteActionEmail action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "action": {
              "type": "string",
              "definition": "Action to execute",
              "example": "FORGOT_EMAIL",
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

### ForgotPasswordAction

Resets the password for a user to a given `password`, using `action_token` for user verification.

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Keycloak Based ForgotPasswordAction action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload",
          "example": "auth",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### VerifyEmailAction

Verifies a user email using `action_token` for user verification.

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Keycloak Based VerifyEmailAction action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in event payload",
          "example": "auth",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
