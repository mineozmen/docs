---
description: >-
  This state manager (com.rierino.state.manager.CachedWriteStateManager)
  provides a cache layer on top of any writeable state manager, which is updated
  upon write requests.
---

# Write thru Coordinator

Unlike LazyCachedStateManager, this state manager updates the cache values immediately.

## Manager Parameters

| Parameter       | Definition                                                                  | Example       | Default |
| --------------- | --------------------------------------------------------------------------- | ------------- | ------- |
| write.state     | Name of the state manager which is the persistent write layer               | basket        | -       |
| cache.state     | Name of the state manager which acts as the cache layer                     | basket\_cache | -       |
| cacheAfterWrite | Whether caching should happen before or after write to the persistent layer | true          | -       |
