---
description: >-
  Rierino automatically produces OpenAPI specification from runner & saga
  configurations.
---

# OpenAPI Specification

Devops application includes a link to auto generated API "Documentation", which can be viewed through an interactive UI or downloaded as OpenAPI specification for external tools.

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption><p>OpenAPI Documentation</p></figcaption></figure>

This specification is automatically generated using the following configurations:

* **Gateway Channels:** Mapping runners to individual gateways to list only relevant APIs for a selected gateway
* **Gateway Tokens:** Mapping authentication tokens to API endpoints
* **Runners:** Mapping state managers to standard list of CRUD endpoints
* **Schemas:** Mapping JSON schemas to CRUD endpoints and Saga references
* **Sagas:** Mapping custom API endpoints, using "allowed on" stream information

{% hint style="info" %}
If JSON schema for a state is not configured, it is not displayed for the mapping CRUD runner.
{% endhint %}
