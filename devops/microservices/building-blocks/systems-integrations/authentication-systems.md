---
description: Built-in authentication system configurations beyond REST APIs
---

# Authentication Systems

## Keycloak

Includes settings required for connecting to a Keycloak server for authentication handlers.

| Setting           | Definition                                                                        | Example                                      | Default |
| ----------------- | --------------------------------------------------------------------------------- | -------------------------------------------- | ------- |
| config            | Json string for Keycloak adapter configuration                                    | {"realm":"test", ...}                        | -       |
| authServerUrl     | Url endpoint for Keycloak server (if not provided as config already)              | https://localhost/auth/                      | -       |
| realm             | Authentication realm to use (if not provided as config already)                   | admin-user                                   | -       |
| resource          | Authentication client resource to use (if not provided as config already)         | rierino-auth                                 | -       |
| credential.\[key] | Keycloak server access credentials as KV pair (if not provided as config already) | provider=secret, username=admin, secret=pass | -       |
| roles             | Default roles to assign to each new user                                          | user                                         | -       |
| idToken           | Whether to return id\_token when resolving tokens                                 | true                                         | false   |

{% embed url="https://www.keycloak.org" %}
Keycloak Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'keycloak', version:"${rierinoVersion}")
```

## LDAP

Includes settings required for connecting to an LDAP v3 directory (OpenLDAP, Active Directory, or any JNDI reachable server) for authentication handlers.

#### Connection

| Setting             | Definition                                                                | Example                   | Default |
| ------------------- | ------------------------------------------------------------------------- | ------------------------- | ------- |
| ldapURL             | Url endpoint for the directory, whitespace separated for failover         | ldaps://ldap.acme.com:636 | -       |
| ldapBaseDn          | Base distinguished name for all user and group searches                   | dc=acme,dc=com            | -       |
| userOrgUnit         | Organizational unit holding user entries, used when composing the user DN | people                    | users   |
| referral            | Referral handling mode as ignore, follow or throw                         | follow                    | ignore  |
| connectTimeoutInMs  | Tcp connect timeout in milliseconds                                       | 5000                      | 5000    |
| readTimeoutInMs     | Socket read timeout in milliseconds                                       | 10000                     | 10000   |
| searchTimeLimitInMs | Server side time limit applied to every search                            | 10000                     | 10000   |

#### Transport Security

| Setting                | Definition                                                                                                     | Example | Default |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- | ------- | ------- |
| startTLS               | Whether to connect anonymously and upgrade the channel with StartTLS before binding, ignored for ldaps:// urls | true    | false   |
| requireSecureTransport | Whether to fail startup when the connection is neither ldaps:// nor StartTLS enabled                           | true    | false   |

Plain `ldap://` is accepted so that deployments without TLS still work, but a simple bind over an unencrypted connection puts the user password on the wire in cleartext and is logged as a warning at startup. Use `requireSecureTransport` to turn that warning into a startup failure.

#### Bind and Identity

| Setting            | Definition                                                                                | Example                         | Default |
| ------------------ | ----------------------------------------------------------------------------------------- | ------------------------------- | ------- |
| adminBindDn        | Service account distinguished name used for admin reads and refresh time re-reads         | cn=svc,ou=system,dc=acme,dc=com | -       |
| adminBindPassword  | Password of the service account, required whenever adminBindDn is provided                | pass                            | -       |
| userIdAttribute    | Attribute carrying the login name, uid in OpenLDAP and sAMAccountName in Active Directory | sAMAccountName                  | uid     |
| securityAuthMethod | Bind method as simple, none or strong                                                     | simple                          | simple  |
| securityMechanism  | Sasl mechanism to use, required when securityAuthMethod is strong                         | DIGEST-MD5                      | -       |
| realm              | Sasl realm used with DIGEST-MD5, GSSAPI (kerberos) and similar mechanisms                 | ACME.COM                        | -       |
| provideRealm       | Whether to pass realm to the sasl layer, requires realm to be provided                    | true                            | false   |

`adminBindDn` is optional. Without it, login still works as the user's own authenticated context is used to read the entry, but user listing and lookup are rejected, and refresh falls back to the attribute snapshot carried inside the refresh token instead of re-reading the directory.

#### Attributes and Groups

| Setting               | Definition                                                                                           | Example          | Default |
| --------------------- | ---------------------------------------------------------------------------------------------------- | ---------------- | ------- |
| userAttributes        | Comma separated attributes to read from the user entry, all attributes when not provided             | cn,mail,memberOf | -       |
| groupSearchFilterName | Group membership attribute, member in Active Directory and memberUid in OpenLDAP                     | memberUid        | member  |
| searchGroupsBy        | Value matched against the membership attribute as userDn in Active Directory or username in OpenLDAP | username         | userDn  |
| groupNameAttribute    | Attribute holding the group name, collected into the roles claim                                     | cn               | cn      |
| listLimit             | Maximum number of entries returned by the list action                                                | 500              | 500     |

Attribute names are validated against `^[A-Za-z][A-Za-z0-9;.\-]*$` at startup to prevent filter and dn injection through configuration values.

Credential shaped attributes are dropped from every response and every token, whatever the directory returns: `userPassword`, `unicodePwd`, `ntPassword`, `lmPassword`, `dbcsPwd`, `sambaNTPassword`, `sambaLMPassword`, `sambaPasswordHistory`, `authPassword`, `pwdHistory`, `krbPrincipalKey`, `userPKCS12`, `msDS-KeyCredentialLink` and `supplementalCredentials`. Binary values such as `objectSid` or `jpegPhoto` are dropped as well. Multi valued attributes keep all of their values and are returned as arrays.

#### Connection Pool

| Setting                 | Definition                                                        | Example   | Default |
| ----------------------- | ----------------------------------------------------------------- | --------- | ------- |
| connectionPool.enabled  | Whether to pool admin connections, disabled when startTLS is used | false     | true    |
| connectionPool.timeout  | Idle timeout in milliseconds for pooled connections               | 300000    | -       |
| connectionPool.maxsize  | Maximum number of pooled connections                              | 20        | -       |
| connectionPool.prefsize | Preferred pool size                                               | 5         | -       |
| connectionPool.protocol | Protocols to pool                                                 | plain ssl | -       |

Admin lookups go through the JNDI connection pool as `DirContext` is not thread safe. User binds are never pooled, since pooling per user principal would grow the pool with the user count.

#### Tokens

| Setting                   | Definition                                                                                 | Example           | Default |
| ------------------------- | ------------------------------------------------------------------------------------------ | ----------------- | ------- |
| tokenSecretKey            | Base64 encoded signing key, must decode to at least 64 bytes for HS512                     | c2VjcmV0...       | -       |
| requireSignedTokens       | Whether to fail startup when tokenSecretKey is missing, instead of issuing unsigned tokens | true              | false   |
| tokenLifetimeInSec        | Access and id token lifetime in seconds                                                    | 3600              | 3600    |
| refreshTokenLifetimeInSec | Refresh token lifetime in seconds                                                          | 86400             | 86400   |
| accessTokenClaims         | Comma separated user attributes to copy into the access token                              | mail,department   | -       |
| idToken                   | Whether to return id\_token when resolving tokens                                          | true              | false   |
| idTokenClaims             | Comma separated user attributes to copy into the id token                                  | cn,mail,givenName | -       |

When `tokenSecretKey` is not provided, tokens are issued unsigned (`alg=none`) and any caller reaching the gateway can forge claims. This is intended for the default environment only; provide a secret or set `requireSignedTokens` everywhere else.

This system requires the following dependency added to deployment contents:

```gradle
implementation (group:'com.rierino.custom', name: 'ldap', version:"${rierinoVersion}")
```

## SAML

Includes settings required for connecting to a SAML 2.0 identity provider as a service provider for authentication handlers.

| Setting                        | Definition                                                                              | Example                                                                                      | Default                                                  |
| ------------------------------ | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| metadata.url                   | Url for the identity provider metadata XML (one of metadata settings is required)       | https://login.microsoftonline.com/{tenant}/federationmetadata/2007-06/federationmetadata.xml | -                                                        |
| metadata.xml                   | Identity provider metadata XML content (if not provided as url)                         | \<EntityDescriptor ...>                                                                      | -                                                        |
| metadata.file                  | File path for the identity provider metadata XML (if not provided as url or xml)        | /etc/saml/idp-metadata.xml                                                                   | -                                                        |
| sp.entityId                    | Entity id of the application as registered at the identity provider                     | https://app.example.com/saml                                                                 | -                                                        |
| sp.acsUrl                      | Frontend assertion consumer service url, exactly as registered at the identity provider | https://app.example.com/auth/saml/acs                                                        | -                                                        |
| sp.keystore.path               | Keystore file path holding the service provider signing / decryption key                | /etc/saml/sp.jks                                                                             | -                                                        |
| sp.keystore.password           | Keystore password                                                                       | changeit                                                                                     | -                                                        |
| sp.keystore.privateKeyPassword | Private key password in the keystore                                                    | changeit                                                                                     | -                                                        |
| sp.keystore.alias              | Alias of the key to use in the keystore                                                 | saml-sp                                                                                      | -                                                        |
| sp.keystore.generatePath       | Path for a generated self-signed keystore, when sp.keystore.path is not provided        | /data/saml-sp.jks                                                                            | \[java.io.tmpdir]/saml-\[system].jks                     |
| sp.signRequests                | Whether AuthnRequests are signed                                                        | true                                                                                         | false                                                    |
| idp.requestBinding             | Binding used for sending AuthnRequests to the identity provider                         | urn:oasis:names:flag\_tc:SAML:2.0:bindings:HTTP-POST                                         | urn:oasis:names:flag\_tc:SAML:2.0:bindings:HTTP-Redirect |
| nameIdFormat                   | Requested NameID format                                                                 | urn:oasis:names:flag\_tc:SAML:1.1:nameid-format:emailAddress                                 | -                                                        |
| maxAuthenticationLifetime      | Maximum seconds allowed since user authentication at the identity provider              | 7200                                                                                         | 3600                                                     |
| roles                          | Default roles to assign to each user                                                    | user                                                                                         | -                                                        |

Assertions are always required to be signed. When no keystore path is provided, a self-signed keystore is generated, which is suitable only if the identity provider does not verify request signatures or encrypt assertions. Identity provider metadata is read once at startup, so handlers should be reconfigured when the identity provider rotates its certificate.

This system requires the following dependency added to [deployment contents](https://docs.rierino.com/devops/microservices/deployment-packages/defining-a-deployment):

```
implementation (group:'com.rierino.custom', name: 'saml', version:"${rierinoVersion}")
```
