---
description: >-
  This state manager (com.rierino.state.manager.CaffeineCacheStateManager) uses
  Caffeine to provide a local in-memory cache with expiration duration and max
  size.
---

# Caffeine Cache

## Manager Parameters

| Parameter         | Definition                                       | Example | Default |
| ----------------- | ------------------------------------------------ | ------- | ------- |
| expirationSeconds | Seconds after the last access to expire an entry | 300     | 60      |
| maxSize           | Maximum number of records allowed in the cache   | 2000    | 1000    |

{% embed url="https://github.com/ben-manes/caffeine" %}
Caffeine Repository
{% endembed %}
