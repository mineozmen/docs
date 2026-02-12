---
description: >-
  Sagas are the API, event and process flow definitions across the platform,
  which can be distributed across multiple servers for execution.
icon: arrow-progress
---

# API Flows

![Saga UI](../../.gitbook/assets/UI_Saga.png)

A typical saga flow responds to requests on a specific path, executed required actions locally or in a distributed manner on different runners, and returns results to the requestor. A saga is built with a no-code approach, by linking microservices and business logic through a drag and drop interface.

{% hint style="info" %}
Changes in a saga flow or creation of a new saga are deployed in real-time, regardless of how distributed the saga event runners are.
{% endhint %}
