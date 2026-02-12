---
description: >-
  This state manager (com.rierino.state.manager.LazyCachedStateManager) is a
  cache coordinator, which uses a loader state and a cache state.
---

# Lazy Cache Coordinator

State manager searches for requested ids in cache first, and the loader afterwards, if cache does not hold a copy. It works as a lazy cache, only copying data to the cache state after the first miss.

## Manager Parameters

| Parameter         | Definition                                                                               | Example         | Default |
| ----------------- | ---------------------------------------------------------------------------------------- | --------------- | ------- |
| cache.state       | Name of the state manager which acts as the cache                                        | variable\_local | -       |
| expirationSeconds | Optional TTL seconds to ignore old cache entries (typically used when CDC is not active) | 60              | -       |

