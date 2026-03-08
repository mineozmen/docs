---
description: >-
  Systems are used for defining key configuration details shared across elements
  which are based on a common data store or external tool.
---

# Systems

Systems are used for connecting 3rd party components to Rierino platform, by configuring their access details, such as:

* **Data systems:** Including shared connection and authorization details for state and query managers
* **API based systems:** Including connection and authorization details for REST API integrations
* **Other systems:** Including connections to event streams, email servers, file systems and more

Each of these systems can be configured as elements just once and added to as many runners as required for allowing such runners communicate with an external component.

{% hint style="info" %}
It is recommended to configure and use value lookups (e.g. from k8s ConfigMap, etcd or similar) for infrastructure related settings (e.g. server IP address) and secret lookups (e.g. from k8s Secret, HashiCorp Vault or similar) for credential settings (e.g. API authentication token).

Value lookups can be referenced using $\{{VALUE\_PATH\}} notation whereas secret lookups can be referenced using #\{{SECRET\_PATH\}} notation when deployments are configured to use value and secret loaders.

Some systems also support auto reconnect functionality with "ephemeral" lookups. Such systems replace $!\{{VALUE\_PATH\}} and #!\{{SECRET\_PATH\}} notations with their respective values on each reconnect attempt. This feature allows changing a target system IP address or connection credentials without having to restart microservices connecting to such systems.
{% endhint %}

{% hint style="info" %}
A predefined "noop" system exists in Rierino, which is considered to be a dummy system. Any stream mapping to this system name ignores all send message requests, acting as a blackhole.
{% endhint %}
