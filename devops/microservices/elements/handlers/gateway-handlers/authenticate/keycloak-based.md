---
description: >-
  This handler (com.rierino.handler.auth.keycloak.KeycloakEventHandler) provides
  a Keycloak based implementation of AuthEventHandler.
---

# Keycloak Based

Keycloak provides capabilities such as social login, user federation and support for OpenID Connect, OAuth 2.0, and SAML. This handler is only available in Rierino Core+ version.

This handler uses the following extra configurations, actions and parameters:

## Handler Parameters

| Parameter | Definition                                 | Example         | Default |
| --------- | ------------------------------------------ | --------------- | ------- |
| system    | Name of Keycloak system for access details | admin\_keycloak | -       |

{% file src="../../../../../../.gitbook/assets/handler-keycloak-0001.json" %}
Example Keycloak Handler Definition (Can be Imported on Element Screen)
{% endfile %}

This handler requires the following dependency added to [deployment contents](../../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'keycloak', version:"${rierinoVersion}")
```

## Actions

### Register

This handler supports using additional credential types provided as "credential\_type" in request in addition to "password" option.

### Login

This handler supports using additional credential types provided as "grant\_type" in request in addition to "password" options.

### UpdatePassword

Allows updating password for a user represented by the "access\_token" using "password" field.

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

Triggers an action e-mail through Keycloak system for a specific action type (e.g. VALIDATE\_EMAIL, FORGOT\_EMAIL) for a given "username".

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

Resets the password for a user to a given "password", using "action\_token" for user verification:

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

Verifies a user e-mail (if required in realm) using "action\_token" for user verification:

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

{% hint style="info" %}
Keycloak stores firstname, lastname, email as user profile data. User attributes are also stored as profile data, except for attributes starting with # character. Attributes starting with # are stored as access data, hence considered as data not managed by the users themselves (e.g. assigned user groups).
{% endhint %}
