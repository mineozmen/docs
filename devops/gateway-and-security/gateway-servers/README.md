---
description: >-
  A gateway server acts as the gatekeeper and controller of flow between
  front-end requests and back-end runners.
---

# Gateway Servers

Gateway servers also coordinate with session and authentication servers to sessionize requests and manage authorization. It is possible to implement new session servers using different technologies, as long as the API endpoints are implemented and requests can be forwarded to the right runners.

## Rierino Gateway Server

The gateway server provided within Rierino platform provides all required endpoints and is based on Spring Webflux that can be configured using application.properties file with the typical Spring settings.

Following Rierino specific properties are also applicable to these gateways, which are configured in gateway server application properties files during its installation:

| Property                              | Definition                                                                                     | Example                                                    | Default                                                 |
| ------------------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------- |
| rierino.gateway.id                    | Id of the gateway (used for selecting gateway components)                                      | admin                                                      | -                                                       |
| rierino.gateway.refreshMs             | Milliseconds to automatically reload gateway components (never reloads if set to -1)           | -1                                                         | 15000                                                   |
| rierino.gateway.allowRequestId        | Whether client or LB/Ingress should be allowed to dictate request id using X-Request-Id header | true                                                       | false                                                   |
| rierino.gateway.requestIdHeader       | Header to return request Id for traceability                                                   | X-Rierino-Request-Id                                       | -                                                       |
| rierino.gateway.requestIdHeaderOn     | Whether returning request Id is allowed (none, always, error)                                  | error                                                      | none                                                    |
| rierino.gateway.traceIdHeaderOn       | Whether returning trace Id is allowed (none, always, error)                                    | always                                                     | none                                                    |
| rierino.gateway.traceIdHeader         | Header to return trace Id for traceability                                                     | X-Rierino-Trace-Id                                         | -                                                       |
| rierino.gateway.allowBranch           | Whether gateway should allow making calls to 'branches' other than main                        | false (e.g. on Production)                                 | true                                                    |
| rierino.request.controller.enabled    | Whether the gateway should have Request APIs                                                   | false                                                      | true                                                    |
| rierino.auth.controller.enabled       | Whether the gateway should have Auth APIs                                                      | false                                                      | true                                                    |
| rierino.control.controller.enabled    | Whether the gateway should have Control APIs                                                   | false                                                      | true                                                    |
| rierino.tracker.controller.enabled    | Whether the gateway should have Tracker APIs                                                   | false                                                      | true                                                    |
| rierino.command.controller.enabled    | Whether the gateway should have Command APIs                                                   | false                                                      | true                                                    |
| rierino.file.controller.enabled       | Whether the gateway should have File APIs                                                      | false                                                      | true                                                    |
| rierino.stream.controller.enabled     | Whether server sent events are enabled on the gateway                                          | false                                                      | true                                                    |
| rierino.stream.controller.wait        | Milliseconds to wait between runner polls for SSE calls                                        | 5000                                                       | 10000                                                   |
| rierino.command.kafka.enabled         | Whether the gateway should be able to send commands thru Kafka                                 | true                                                       | false                                                   |
| rierino.command.kafka.default.service | Default Kafka service to send commands                                                         | kafka\_command                                             | -                                                       |
| rierino.command.kafka.default.topic   | Default Kafka topic to send commands                                                           | command                                                    | -                                                       |
| rierino.id.prefix                     | Prefix for all request ids from the gateway                                                    | admin\_request                                             | gateway id value                                        |
| rierino.id.instanceNum.class          | Fully qualified class name for gateway instance number (used in request id generation)         | com.rierino.handler.util.generator.ConstantNumberGenerator | com.rierino.handler.util.generator.EpochNumberGenerator |
| rierino.id.instanceNum.\*             | Parameters for gateway instance number class                                                   | value=0                                                    | -                                                       |
| rierino.telemetry.trace.input.enabled | Whether gateway should accept trace & span ids from requestor client for correlations          | true                                                       | false                                                   |

{% hint style="info" %}
Gateway creates request ids using rierino.id settings, which produce ids in the following form:

\[prefix]-\[instance number]-\[local increment]\[partition suffix]

such as:

"admin\_prod-123-50001" for an "admin\_prod" gateway with "123" constant instance number and "1" as its assigned partition on its 5th request.
{% endhint %}

Typically, Spring Consul and Vault properties are also set in these files, in case client side load balancing and authentication token features are used.

## Configuration Loader Properties

In addition to the properties file, these gateway servers load their component definitions (i.e. system, channel, service, token) dynamically, which can be refreshed during runtime.

| Property                                | Definition                                                       | Example                                                           | Default               |
| --------------------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------- | --------------------- |
| rierino.config.loader.\*                | Configuration REST server parameters                             | baseUrl=http://localhost/crud,timeout=5000,connectionTimeout=5000 | -                     |
| rierino.config.loader.\[component].path | Path for gateway component (e.g. system, channel) configurations | admin\_gateway\_system                                            | gateway\_\[component] |

## Security Properties

All gateway servers also share the following origin related security settings:

| Property                                     | Definition                                                         | Example                 | Default |
| -------------------------------------------- | ------------------------------------------------------------------ | ----------------------- | ------- |
| rierino.security.cors.enabled                | Whether CORS policy is enabled or not                              | true                    | false   |
| rierino.security.cors.maxAge                 | Max seconds for  caching preflight response (when CORS is enabled) | 3600                    | -       |
| rierino.security.cors.credentials            | Whether credentials are allowed or not (when CORS is enabled)      | false                   | true    |
| rierino.security.cors.allowed.headers        | List of allowed headers  (when CORS is enabled)                    | header1, header2        | -       |
| rierino.security.cors.allowed.originPatterns | List of allowed origin patterns (when CORS is enabled)             | https://\*.example.com  | -       |
| rierino.security.cors.allowed.origins        | List of allowed origins (when CORS is enabled)                     | https://www.example.com | -       |
| rierino.security.cors.allowed.methods        | List of allowed methods (when CORS is enabled)                     | GET, POST               | -       |
| rierino.security.csrf.enabled                | Whether CSRF protection is enabled                                 | true                    | false   |
| rierino.security.csrf.cookie                 | Whether CSRF token is sent as cookie                               | true                    | false   |
| rierino.security.actuator.allowed.ips        | List of whitelisted IPs for actuator access                        | 127.0.0.1               | -       |
