---
description: These runners are based on Java Spring library.
---

# Spring Runners

All relevant Spring configurations are applicable, and can be passed on to these runners using properties files.

All Spring runners accept following configurations, in addition to the common security configurations mentioned in [Gateway & Security](../../../api-gateway-and-security/):



| Parameter                                     | Definition                                                     | Example                                                    | Default |
| --------------------------------------------- | -------------------------------------------------------------- | ---------------------------------------------------------- | ------- |
| rierino.runner.\[name].offset.class           | Fully qualified class name for assigning offsets               | com.rierino.handler.util.generator.ConstantNumberGenerator | -       |
| rierino.runner.\[name].offset.\[parameter]    | Offset generator specific parameters                           | value=5                                                    | -       |
| rierino.runner.\[name].partition              | Partition assigned to runner (used for non-broadcasted topics) | 1                                                          | -       |
| rierino.runner.\[name].authentication.enabled | Whether runner requires authentication for API calls           | true                                                       | false   |
| rierino.runner.\[name].authentication.role    | Role required (on service account) for making API calls        | internal                                                   | -       |

## Authentication

If authentication is enabled on a runner (which is different than authentication on API gateways), the following parameters are also applicable, based on preferred authentication method:

### Simple Authentication

Simplest form of authentication, using a predefined API key without hashing and roles:

| Parameter                                   | Definition                               | Example | Default |
| ------------------------------------------- | ---------------------------------------- | ------- | ------- |
| rierino.runner.\[name].authentication.token | API key used for authenticating requests | DEMO    | -       |

### Hashed Authentication

| Parameter                                         | Definition                                                                                            | Example       | Default |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------- | ------- |
| rierino.runner.\[name].authentication.secret      | Secret to use for hashing API keys                                                                    | -             | -       |
| rierino.runner.\[name].authentication.algorithm   | Algorithm for hashing API keys                                                                        | MD5           | SHA-256 |
| rierino.runner.\[name].authentication.iterations  | Number of iterations for hashing API keys                                                             | 100           | 1       |
| rierino.runner.\[name].authentication.cache.state | State manager which keeps acceptable hashed keys with their role mappings (should manage its own TTL) | key\_store    | -       |
| rierino.runner.\[name].authentication.system      | System to run a saga on, for validating a hashed API key and returning its roles                      | admin\_core   | -       |
| rierino.runner.\[name].authentication.stream      | Stream to run a saga on, for validating a hashed API key and returning its roles                      | rpc           | -       |
| rierino.runner.\[name].authentication.action      | Action (i.e. saga path) to run, for validating a hashed API key and returning its roles               | ProcessAPIKey | -       |

{% hint style="info" %}
Authentication cache state can be a shared state manager, allowing use of predefined and hashed list of API keys and roles without using Sagas to produce them.
{% endhint %}

## CRUD Event Runner

CRUDEventRunner (_com.rierino.runner.spring.CRUDEventRunner_) is a Spring WebFlux based runner, which accepts typical REST calls through http or https protocols for common CRUD operations.

Each request method and path is mapped on to a specific action and processed based on the elements configured on the runner. Path mappings are as follows:

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}" method="get" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/{id}" method="get" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}" method="post" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/{id}" method="post" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/{id}" method="put" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/{id}" method="patch" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/{id}" method="delete" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% openapi src="../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml" path="/{runner}/{state}/@" method="post" %}
[CRUD Runner OpenAPI.yaml](<../../../../.gitbook/assets/CRUD Runner OpenAPI.yaml>)
{% endopenapi %}

{% hint style="info" %}
CRUD event runner also supports using /\[action]\_\[state] Saga flows for these requests if the input stream is mapped to a Saga event handler.
{% endhint %}

This event runner accepts request metadata in "request\_metadata" header, as a base64 encoded Json string.

## RPC Event Runner

RPCEventRunner (_com.rierino.runner.spring.RPCEventRunner_) is a Spring WebFlux based runner, which accepts remote procedure call type requests through http or https protocols for more customized use cases.

The request path defines the stream and action in /api/\[runner]/\[stream]/\[action] form, and the request body is used as the full event contents, including request metadata and payload itself.

Using /api\_direct instead of /api in calls to these runners allows sending requests in payload form, without requiring full event data format. This option is usable only when making calls directly to the runners from inside the cluster, without passing them through the API gateways.&#x20;

## RSocket Event Runner

RSocketEventRunner(_com.rierino.runner.spring.RSocketEventRunner_) is a RSocket based runner, which accepts continuous requests through socket connections for low latency and frequent use cases.

Message path defines the stream and action in /api/\[runner]/\[stream]/\[action] form, and the message payload is used as the full event contents.

{% embed url="https://spring.io" %}
Spring Page
{% endembed %}

{% hint style="info" %}
Spring runners can listen to Kafka streams, but only for command, journal and pulses (and not events or other role data).
{% endhint %}
