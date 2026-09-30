---
description: >-
  Gateway handlers facilitate authentication and session management and are
  mostly used on dedicated runners called by the API gateway
---

# Gateway Handlers

Gateway handlers cover authentication and session management for gateway-facing flows.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Implementation-specific behavior

Use [Gateway Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/) for action-level saga step fields and parameters.

### Authenticate

Base class: `com.rierino.handler.auth.AuthEventHandler`

Provides the common authentication structure used by gateway authentication handlers.

#### Shared handler parameters

| Parameter              | Definition                                  | Example        | Default |
| ---------------------- | ------------------------------------------- | -------------- | ------- |
| `attempt.state`        | State manager storing login attempt history | `auth_attempt` | -       |
| `registration.enabled` | Whether user registration is enabled        | `true`         | `false` |
| `initial.disabled`     | Whether newly created users start disabled  | `true`         | `false` |
| `apikey.length`        | API key length to generate                  | `64`           | `32`    |
| `apikey.secret`        | Secret used to hash API keys                | -              | -       |

Action details: [Authenticate](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/)

#### No Authentication

Class: `com.rierino.handler.auth.NoopAuthEventHandler`

Development-only implementation that accepts credentials without performing actual authentication.

This handler has no additional parameters.

#### State Based

Class: `com.rierino.handler.auth.StatesAuthEventHandler`

Uses a state manager as the credential store with salted passwords and token handling.

**Handler parameters**

| Parameter                | Definition                                   | Example                | Default                |
| ------------------------ | -------------------------------------------- | ---------------------- | ---------------------- |
| `auth.state`             | State manager storing credentials            | `auth_store`           | -                      |
| `auth.secret`            | Secret used for hashing passwords and tokens | -                      | -                      |
| `auth.expiration`        | Access token expiration in seconds           | `900`                  | `600`                  |
| `auth.refreshExpiration` | Refresh token expiration in seconds          | `9000`                 | `6000`                 |
| `auth.iterations`        | Password salting iterations                  | `5`                    | `1`                    |
| `auth.saltLength`        | Salt string length                           | `32`                   | `16`                   |
| `auth.keyLength`         | PBKDF2 key length                            | `1024`                 | `512`                  |
| `auth.algorithm`         | Password hashing algorithm                   | `PBKDF2WithHmacSHA256` | `PBKDF2WithHmacSHA256` |
| `auth.issuer`            | Issuer included in generated tokens          | `Rierino`              | -                      |
| `auth.minPassLength`     | Minimum allowed password length              | 8                      | 4                      |
| `auth.maxPassLength`     | Maximum allowed password length              | 20                     | 512                    |
| `auth.passRegex`         | Regex to validate password                   | -                      | -                      |
| `auth.minSecretLength`   | Minimum allowed secret length                | 8                      | 4                      |
| `auth.maxSecretLength`   | Maximum allowed secret length                | 20                     | 512                    |
| `auth.secretRegex`       | Regex to validate secret                     | -                      | -                      |

Action details: [State Based](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/state-based.md)

#### Keycloak Based

Class: `com.rierino.handler.auth.keycloak.KeycloakEventHandler`

Uses Keycloak for authentication, federation, social login, and standards-based identity flows.

**Handler parameters**

| Parameter | Definition                              | Example          | Default |
| --------- | --------------------------------------- | ---------------- | ------- |
| `system`  | Keycloak system name for access details | `admin_keycloak` | -       |

**Runtime dependency**

```gradle
implementation (group:'com.rierino.custom', name: 'keycloak', version:"${rierinoVersion}")
```

Action details: [Keycloak Based](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/keycloak-based.md)

#### LDAP Based

Class: `com.rierino.handler.auth.ldap.LDAPEventHandler`

The handler binds as the end user to verify credentials, reads the user entry and its group memberships, and packs the requested attributes into the tokens.

The directory is treated as read only: registration, user modification and user deletion are not supported.

Connection, bind, attribute and token settings come from the referenced LDAP system.

**Handler parameters**

| Parameter | Definition                                       | Example  | Default |
| --------- | ------------------------------------------------ | -------- | ------- |
| system    | Name of the LDAP system configuration to be used | corpLdap | -       |

**Issued token claims**

| Token          | Claims                                                                                                           |
| -------------- | ---------------------------------------------------------------------------------------------------------------- |
| access\_token  | `sub`, `username`, `token_use=access`, `roles`, plus every attribute listed in `accessTokenClaims`               |
| id\_token      | `sub`, `token_use=id`, plus every attribute listed in `idTokenClaims`                                            |
| refresh\_token | `sub`, `username`, `token_use=refresh`, plus `snap` (the attribute snapshot) when no `adminBindDn` is configured |

**Runtime dependency**

```gradle
implementation (group:'com.rierino.custom', name: 'ldap', version:"${rierinoVersion}")
```

Action details: [LDAP Based](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/ldap-based.md)

#### SAML Based

Class: `com.rierino.handler.auth.saml.SAMLEventHandler`

Acts as a SAML 2.0 Service Provider, validating assertions from any SAML identity provider such as Entra ID, Okta, ADFS or Keycloak. The browser exchange is owned by the frontend, so this handler only builds AuthnRequests and validates the SAMLResponse forwarded to it, returning the resolved identity. SAML issues no tokens, so it is used together with a token / session handler that trusts its Login and Refresh output.

Handler parameters

| Parameter              | Definition                                                                                                                                   | Example                                | Default            |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | ------------------ |
| `system`               | SAML system name for identity provider and service provider details                                                                          | `corporate_saml`                       | -                  |
| `state.state`          | State manager used for storing request state server side (single use). Either this or `state.secret` is required                             | `saml_state`                           | -                  |
| `state.secret`         | Base64 encoded HMAC key used for signing request state returned to the frontend, when `state.state` is not used                              | `c2VjcmV0LWtleS0zMi1ieXRlcy1sb25nIQ==` | -                  |
| `state.ttl`            | Seconds a SAML request state stays valid                                                                                                     | `300`                                  | `600`              |
| `replay.state`         | State manager used for rejecting reused assertions across instances. Falls back to an in-memory cache (single instance only) if not provided | `saml_replay`                          | -                  |
| `replay.ttl`           | Seconds assertions are kept in the in-memory replay cache                                                                                    | `1800`                                 | `900`              |
| `idpInitiated.enabled` | Whether unsolicited (IdP initiated) responses without a request state are accepted                                                           | `true`                                 | `false`            |
| `user.idPrefix`        | Prefix added to the NameID to produce the user id                                                                                            | `saml:`                                | -                  |
| `login.pattern`        | JMESPath pattern applied on the validated assertion to build Login / Refresh output                                                          | `{"user": {"id": userId}}`             | See action details |

Runtime dependency

```
implementation (group:'com.rierino.custom', name: 'saml', version:"${rierinoVersion}")
```

Action details: [SAML Based](https://docs.rierino.com/devops/api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/saml-based)

### Sessionize

Class: `com.rierino.handler.SessionEventHandler`

Creates, extends, stitches, and expires user sessions for gateway use cases.

#### Handler parameters

| Parameter        | Definition                                       | Example        | Default    |
| ---------------- | ------------------------------------------------ | -------------- | ---------- |
| `store.state`    | State manager storing session details            | `session`      | -          |
| `init.stream`    | Stream receiving session initialization triggers | `session_init` | -          |
| `stitchPriority` | Strategy used when stitching sessions            | `old`          | `existing` |
| `ttl`            | Session inactivity timeout in milliseconds       | `900000`       | `60000`    |

Action details: [Sessionize](../../../api-event-and-process-flows/configuring-saga-steps/event-step/gateway-actions/sessionize.md)
