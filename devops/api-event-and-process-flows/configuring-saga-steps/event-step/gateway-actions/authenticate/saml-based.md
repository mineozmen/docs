---
description: >-
  These actions provide a SAML 2.0 Service Provider implementation, where the
  frontend handles browser redirects and posts the identity provider response
  back for validation.
---

# SAML Based

## SAML Based Actions

### SamlRequest

Builds a SAML AuthnRequest for the identity provider. Accepts optional `relay_state`, `passive` (`true` to request silent re-authentication without any IdP UI) and `force_authn` (`true` to force the user to re-enter credentials) in the request.

Returns `redirect_url` for the frontend to navigate to (or `post_content` as an auto-submitting HTML form when the IdP uses HTTP-POST binding), together with an opaque `saml_state` value, which must be sent back unchanged with the IdP response. Passive requests are marked in this state as session extensions.

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
  "title": "SAML Based SamlRequest action eventMeta fields",
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

### SamlMetadata

Returns the service provider metadata XML as `metadata`, to be shared with identity provider administrators for registering the application.

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
  "title": "SAML Based SamlMetadata action eventMeta fields",
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

### Login

Validates the `saml_response` (base64 SAMLResponse posted by the IdP to the frontend ACS route) together with `saml_state` returned by SamlRequest and optional `relay_state`. Validation covers signature, issuer, audience, destination, time window, InResponseTo, status, decryption and assertion replay. Responses without `saml_state` are rejected unless IdP initiated login is enabled on the handler.

Returns the resolved identity (no tokens) by applying the login pattern on the validated assertion, which has the following fields: `userId`, `nameId`, `nameIdFormat`, `sessionIndex`, `sessionNotOnOrAfter`, `idp`, `mode` (`login` or `refresh`) and `attributes` (each IdP attribute as a list of strings).

If `saml_state` belongs to a passive request, the call is treated as a session extension, and `expected_user_id` must be provided and match the user in the assertion. This value should be set by the flow from the verified session, and not taken from the client request.

Default login pattern:

```
{"user": {"id": userId, "roles": attributes.memberOf}, "profile": {"username": nameId}, "saml": {"nameId": nameId, "nameIdFormat": nameIdFormat, "sessionIndex": sessionIndex, "sessionNotOnOrAfter": sessionNotOnOrAfter, "idp": idp, "mode": mode}}
```

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
  "title": "SAML Based Login action eventMeta fields",
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
| Parameter     | Definition                                                                  | Example                                               | Default               |
| ------------- | --------------------------------------------------------------------------- | ----------------------------------------------------- | --------------------- |
| Login Pattern | JMESPath pattern applied on the validated assertion, overriding the handler | {"user": {"id": userId, "roles": attributes.groups\}} | Handler login.pattern |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SAML Based Login action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "loginPattern": {
              "type": "string",
              "definition": "JMESPath pattern applied on the validated assertion, overriding the handler",
              "example": "{\"user\": {\"id\": userId, \"roles\": attributes.groups}}",
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

### Refresh

Behaves the same as Login and accepts the same fields and parameters. Whether the call is a fresh login or a session extension is decided by `saml_state`, not the action, so a refresh requires a SamlRequest with `passive=true` followed by `expected_user_id` matching the user of the session being extended. A failed silent re-authentication (e.g. IdP session expired) is rejected as unauthorized.

### Unsupported Actions

Validate and Logout are not supported, as SAML issues no tokens (use the session handler for these). Register and user management actions (List, Get, Set, Delete) are not supported, as users are managed by the identity provider.
