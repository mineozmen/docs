---
description: >-
  These state managers (com.rierino.state.manager.odata2.OdataStateManager and
  com.rierino.state.manager.odata4.OdataStateManager) use a remote Odata service
  as a data source.
---

# Odata Service

## Manager Parameters

| Parameter | Definition                                                                           | Example                          | Default |
| --------- | ------------------------------------------------------------------------------------ | -------------------------------- | ------- |
| system    | Name of the rest system which defines remote service details (url, auth, allowPatch) | {url:"http://odata.example.com"} | -       |
| path      | Path of the resource on target system                                                | /odata                           | -       |
| entity    | Name of the resource on target system                                                | product                          | -       |

{% hint style="info" %}
Selected state manager library version should match target web service's odata version.
{% endhint %}

This manager requires one of the following dependencies added to [deployment contents](../../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'odata2', version:"${rierinoVersion}")
implementation (group:'com.rierino.custom', name: 'odata4', version:"${rierinoVersion}")
```
