---
description: >-
  This state manager (com.rierino.state.manager.RedisStateManager) uses Redis
  for storing and reading data, typically for shared caching purposes.
---

# Redis Map

## Manager Parameters

| Parameter         | Definition                                                                             | Example                        | Default          |
| ----------------- | -------------------------------------------------------------------------------------- | ------------------------------ | ---------------- |
| system            | Name of the [system ](../../systems/#redis)which defines Redis service details (uri)   | {uri:"redis://127.0.0.1:6379"} | master.redis.uri |
| collection        | Name of the collection to store details, which is used as a prefix in Redis keys       | product                        | -                |
| journalCollection | Name of the collection to store state changes, which is used as a prefix in Redis keys | product\_journal               | -                |
