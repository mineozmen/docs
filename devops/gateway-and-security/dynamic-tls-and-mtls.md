---
description: >-
  API gateway and web based runners can use dynamic configurations for enabling
  TLS & mTLS
---

# Dynamic TLS & mTLS

In addition to built-in Spring security configurations, ingress/loadbalancer based TLS and the ability to use side-cars for enabling mTLS, Rierino provides ability to load and rotate private keys and public certificates on the fly for more dynamic security. This dynamic security capability has 4 main building blocks:

## Server Side

Server side configurations include TLS or mTLS configurations where the gateway / runner acts as a server, exposing its APIs to various clients. When the "trusted" configuration is not enabled, this acts as a regular server SSL configuration.

### General & Polling Configuration

These settings configure the polling behavior for refreshing server side keys and certificates. These settings start with "rierino.security.mtls.server" prefix.

| Property          | Description                                                                                         | Default |
| ----------------- | --------------------------------------------------------------------------------------------------- | ------- |
| keyId             | ID of the private key to use as a server                                                            | -       |
| useClient         | Whether server configuration should use client side configuration instead of own settings           | false   |
| pollingIntervalMs | Milliseconds between each key refresh attempt                                                       | 10000   |
| pollingGraceMs    | Milliseconds to wait before updating server private key (to allow clients to pick the update first) | 11000   |

### Context Configuration

These settings configure the TLS communication details, starting with "rierino.security.mtls.server.context" prefix.

| Property         | Description                                                                 | Default |
| ---------------- | --------------------------------------------------------------------------- | ------- |
| protocols        | Comma separated list of TLS protocol versions to enable                     | -       |
| ciphers          | Comma separated list of cipher suites to enable, in the order of preference | -       |
| enableOcsp       | Enables OCSP stapling                                                       | -       |
| sessionCacheSize | Sets the size of the cache used for storing SSL session objects             | -       |
| sessionTimeout   | Sets the timeout for the cached SSL session objects, in seconds             | -       |
| startTls         | Whether the first write request shouldn't be encrypted                      | -       |
| options          | Json string for additional options to apply                                 | -       |

### Self Configuration

Server side self configurations reflect server's own private key and certificate management settings. These settings start with "rierino.security.mtls.server.self" prefix.

| Property | Description                                    | Default |
| -------- | ---------------------------------------------- | ------- |
| enabled  | Whether server should have TLS enabled         | false   |
| class    | Java class for loading keys and certificates   | -       |
| \*       | Properties that are specific to selected class | -       |

### Trusted Configuration

Server side trusted configurations reflect server's trusted client certificate management settings, where web client configurations allow selecting whether that specific client will use mTLS or not. These settings start with "rierino.security.mtls.server.trusted" prefix.

| Property | Description                                                             | Default |
| -------- | ----------------------------------------------------------------------- | ------- |
| enabled  | Whether server should have client certificates enabled                  | false   |
| auth     | Level of enforcement of client authentication (NONE, OPTIONAL, REQUIRE) | NONE    |
| class    | Java class for loading keys and certificates                            | -       |
| \*       | Properties that are specific to selected class                          | -       |

## Client Side

Client side configurations include mTLS configurations where the gateway / runner acts as a client, connecting to mTLS enabled servers. Client side configurations follow the same structure as server side configurations, with the main difference being use of "rierino.security.mtls.client" prefixes instead of "rierino.security.mtls.server".

## TLS Configuration Loaders

### Local Folder

This loader (com.rierino.util.mtls.loader.LocalSSLConfigLoader) loads private key and certificate files from a local folder, typically attached to Secrets and ConfigMaps in K8s deployments. If the self certificate requires a chain of certificates (i.e. with intermediate and CA certificates), naming should define the order in chain (i.e. leaf = private-0, intermediate = private-1, etc.).&#x20;

| Property      | Description                                                            | Default               |
| ------------- | ---------------------------------------------------------------------- | --------------------- |
| selfFolder    | Path to folder containing private.key file and \*.crt self certificate | /app/secrets          |
| trustedFolder | Path to folder containing \*.crt trusted certificates                  | /app/globalconfigcert |

### Bouncy Castle

This loader (com.rierino.util.mtls.loader.BouncySSLConfigLoader) generates new self-signed certificates when they are close to expiry, and is only used for generating self certificates, where trusted certificates should be loaded using other loaders if required.

| Property               | Description                                                                                                                                                            | Default                   |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| provider               | Security provider to use, defaults to Bouncy Castle                                                                                                                    | BC                        |
| certificateDN          | DN to assign to generated certificates                                                                                                                                 | CN=rierino.com, O=Rierino |
| algorithm              | Algorithm to use for generating keys                                                                                                                                   | RSA                       |
| signatureAlgorithm     | Algorithm to use for signing certificates                                                                                                                              | SHA256withRSA             |
| keySize                | Size of keys to use                                                                                                                                                    | 2048                      |
| certificateLifeTime    | Lifetime for generated certificates in days                                                                                                                            | 1                         |
| certificateRenewalTime | Time to wait until renewal of current certificate in days (unless already expired) - uses 30 sec grace period (0 value forces new certificate generation on each poll) | 0                         |

### State Manager

This loader (com.rierino.util.mtls.loader.StateSSLConfigLoader) can use any state manager to read certificates and keys from.

The state should include {algorithm, sslPrivateKey, trustedBy, certificate: {type, certificate}, certificateChain: \[{type, certificate}]} format in data bodies where trustedBy matches the trusting server's keyId.

| Property | Description                             | Default |
| -------- | --------------------------------------- | ------- |
| manager  | Class name for the state manager        | -       |
| \*       | Parameters to pass to the state manager | -       |

### Rest Services

This loader (com.rierino.util.mtls.loader.RestSSLConfigLoader) allows using an external API to load certificates and keys.&#x20;

The request includes {id: keyId} and expects a response in {algorithm, sslPrivateKey, certificate: {type, certificate\}} or {algorithm, sslPrivateKey, certificateChain: \[{type, certificate}]} format for self requests and {type, certificate} for trusted requests.

| Property       | Description                                                             | Default          |
| -------------- | ----------------------------------------------------------------------- | ---------------- |
| domain         | Name of the system domain                                               | ssl              |
| url            | URL to pull information from                                            | -                |
| method         | Method to use in API call                                               | POST             |
| contentType    | Content type header to send                                             | application/json |
| authNodeStr    | Custom authentication details to send                                   | -                |
| headersNodeStr | Custom headers to send                                                  | -                |
| asEvent        | Whether request should be sent as event (to a runner) or a regular call | true             |
