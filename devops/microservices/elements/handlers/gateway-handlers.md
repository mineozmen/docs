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

Use [Gateway Actions](../../../api-flows/configuring-saga-steps/event-step/gateway-actions/) for action-level saga step fields and parameters.

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

Action details: [Authenticate](../../../api-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/)

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

Action details: [State Based](../../../api-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/state-based.md)

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

Action details: [Keycloak Based](../../../api-flows/configuring-saga-steps/event-step/gateway-actions/authenticate/keycloak-based.md)

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

Action details: [Sessionize](../../../api-flows/configuring-saga-steps/event-step/gateway-actions/sessionize.md)
