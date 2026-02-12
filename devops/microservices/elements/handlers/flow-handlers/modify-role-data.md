---
description: >-
  This handler (com.rierino.handler.JMESDataRoleHandler) provides ability to
  modify and route any role data.
---

# Modify Role Data

## Handler Parameters

<table><thead><tr><th>Parameter</th><th>Definition</th><th width="222">Example</th><th>Default</th></tr></thead><tbody><tr><td>pattern</td><td>JMESPath pattern to apply with data, route, filter details</td><td>[ { "data":@, "route":{"system":'kafka', "stream":'customer', "key":payload.customer.id}, "filter":payload.customer.id!=null }, { "data":@, "route":{"system":'kafka', "stream":'session', "key":requestMeta.sessionId}, "filter":requestMeta.sessionId!=null }]</td><td>-</td></tr></tbody></table>

## Roles

### processData

Transforms received data with handler's JMESPath pattern, checks if its "filter" field is true and redirects it based on its "route" field (system, stream, key).

{% hint style="info" %}
One input can be directed towards multiple routes at once, allowing partitioning by different keys or conditional routing of the output.
{% endhint %}
