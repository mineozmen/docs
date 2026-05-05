---
description: >-
  This state manager (com.rierino.state.manager.SamzaStateManager) uses
  Samzalocal store (LevelDB/RocksDB) for storing and reading data.
---

# Samza Based

This state manager is only available for Samza event runners which have matching stores configured.

## Manager Parameters

| Parameter | Definition                                      | Example      | Default |
| --------- | ----------------------------------------------- | ------------ | ------- |
| store     | Name of the Samza store which stores aggregates | query\_local | -       |

{% embed url="https://samza.apache.org/learn/documentation/latest/architecture/architecture-overview.html#state-management" %}
Samza Architecture
{% endembed %}
