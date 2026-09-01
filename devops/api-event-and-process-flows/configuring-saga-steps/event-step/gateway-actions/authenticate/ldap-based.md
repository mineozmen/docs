---
description: Authenticates users against an LDAP directory and issues Rierino JWTs
---

# LDAP Based

## LDAP Based Actions

### Login

Authenticates the user with a bind against the directory and returns the token set. Only `password` is accepted as `grant_type`, which is also the default when the field is absent.

Both `username` and `password` are mandatory. A blank password is an _unauthenticated_ bind under RFC 4513 and most directories answer success to it, so it is rejected before reaching the server. The username is passed as a filter argument and escaped per RFC 4515, so values such as `*` or `)(uid=admin` cannot rewrite the search.

Request fields are `grant_type`, `username` and `password`; the response carries `access_token`, `expires_in`, `refresh_token`, `refresh_expires_in`, and `id_token` when the system has `idToken` enabled.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                               | Example | Default |
| -------------- | ---------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in event payload | auth    | -       |
| \{% endtab %\} |                                          |         |         |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LDAP Based Login action eventMeta fields",
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

### Validate

Validates the `access_token` and, when provided, the `id_token`. Each token must carry the matching `token_use` claim.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                               | Example | Default |
| -------------- | ---------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in event payload | auth    | -       |
| \{% endtab %\} |                                          |         |         |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LDAP Based Validate action eventMeta fields",
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

### Refresh

Exchanges a `refresh_token` for a new token set. The subject is taken from the `sub` claim, falling back to `username`.

When the system has `adminBindDn` configured, the directory is read again so revoked group memberships and deleted accounts take effect on refresh, and a user who no longer exists is rejected. Without a service account the attribute snapshot carried in the refresh token is reused instead, so role changes only take effect once the refresh token expires.

{% tabs %}
{% tab title="Table" %}
| Field          | Definition                               | Example | Default |
| -------------- | ---------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in event payload | auth    | -       |
| \{% endtab %\} |                                          |         |         |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LDAP Based Refresh action eventMeta fields",
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

### List

Returns directory users page by page, using the service account. Requires `adminBindDn` and `adminBindPassword` on the system, and fails otherwise.

Entries are searched with `(<userIdAttribute>=*)` over the whole subtree below `ldapBaseDn`. The requested `limit` is capped at the configured `listLimit`, which is also used when no limit is given. When the id attribute is missing from an entry, its full name in namespace is used as the username.

### Get

Returns a single user matched on `userIdAttribute`, together with the `roles` collected from group membership. Requires the service account, and an unknown id is reported as not found.
