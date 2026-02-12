---
description: Listener elements are used for tracking changes in states manager by a runner.
---

# Listeners

A state listener can trigger actions before or after aggregate updates and/or full state reloads. A number of settings are shared across all listeners:

| Setting  | Definition                                                                        | Example | Default |
| -------- | --------------------------------------------------------------------------------- | ------- | ------- |
| priority | A numeric value for prioritizing listener triggers (higher priority called first) | 10      | 0       |

{% hint style="info" %}
In addition to developing customized state listeners, it is possible to use them programmatically for building handlers responsive to state data changes.
{% endhint %}
