---
description: >-
  Configure gateway components, API exposure, authentication, streaming, rate
  limits, and TLS or mTLS for secure access to Rierino services.
icon: swap-arrows
---

# API Gateway & Security

The gateway layer exposes backend capabilities to clients and applies the controls around them. It maps requests to runners, enforces authentication, shapes responses, and adds resilience and transport security.

### What this section covers

* [Gateway Servers](gateway-servers/) covers the gateway runtime, controller endpoints, request and trace IDs, refresh behavior, and shared security settings.
  * [Gateway Tokens](gateway-servers/gateway-tokens.md) define token lifetimes, claims, cookies, auth providers, and session settings.
  * [Gateway Channels](gateway-servers/gateway-channels.md) map public paths to backend systems and apply per-path auth, aliases, headers, retries, and resilience rules.
  * [Gateway Services](gateway-servers/gateway-services.md) configure direct gateway integrations for Kafka and file operations.
  * [Gateway Systems](gateway-servers/gateway-systems.md) define how the gateway reaches runners over RPC, CRUD, Kafka, or RSocket.
* [APIs](apis/) covers the exposed gateway endpoints for requests, auth, tracking, files, control, commands, and ad hoc messages.
  * [OpenAPI Specification](apis/openapi-specification.md) explains how gateway, runner, schema, and saga configuration become generated API docs.
  * [Response Formats](apis/response-formats.md) explains JSON, XML, HTML, plain text, and CSV responses.
* [Server Sent Events](apis/server-sent-events.md) explains how `/api/stream/...` turns repeated saga calls into an SSE feed using `list`, `continue`, `wait`, and `next`.
* [Resilience Configuration](resilince-configuration.md) covers built-in rate limiting, bulk head, circuit breaker anbulkheadd retry logic at gateway channel level.
* [Dynamic TLS & mTLS](dynamic-tls-and-mtls.md) explains runtime certificate loading and rotation for both server and client connections.

### How the pieces fit together

1. A client calls a gateway API on a channel.
2. The channel applies auth, path rules, headers, and resilience.
3. The channel uses a system to reach the target runner or service.
4. Tokens and sessions enrich the request with identity and claims.
5. The gateway returns JSON by default, or another supported format.
6. Optional controls such as rate limits, SSE, and mTLS apply at the edge.

### Common use cases

* Expose a saga or CRUD endpoint through a public API.
* Protect paths with token-based auth and role checks.
* Add retries, circuit breakers, and rate limits for unstable dependencies.
* Stream incremental results to clients over SSE.
* Secure gateway-to-runner traffic with rotated TLS or mTLS certificates.
* Publish generated OpenAPI docs for API consumers.

### Start here

* Start with [Gateway Servers](gateway-servers/) if you are wiring a new gateway.
* Go to [Gateway Channels](gateway-servers/gateway-channels.md) and [Gateway Systems](gateway-servers/gateway-systems.md) when exposing a runner.
* Go to [Gateway Tokens](gateway-servers/gateway-tokens.md) when setting up login, claims, or cookies.
* Go to [Resilience Configuration](resilince-configuration.md) or [Dynamic TLS & mTLS](dynamic-tls-and-mtls.md) when hardening production traffic.
