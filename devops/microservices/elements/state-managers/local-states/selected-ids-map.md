---
description: >-
  This state manager (com.rierino.state.manager.IDMapStateManager) provides a
  local in-memory map for storing and reading a specific list of aggregates with
  very low latency.
---

# Selected IDs Map

## Manager Parameters

| Parameter | Definition                                                              | Example           | Default |
| --------- | ----------------------------------------------------------------------- | ----------------- | ------- |
| id        | Id of the aggregate to keep in state (if state will keep only 1 record) | query\_1          | -       |
| ids       | Comma separated list of ids of the aggregate to keep in state           | query\_1,query\_2 | -       |
