---
description: >-
  This state manager (com.rierino.state.manager.IndexedMapStateManager) provides
  a local in-memory map for storing and reading aggregates which is also indexed
  with a specific field for quick filtering.
---

# Indexed Map

## Manager Parameters

| Parameter | Definition                         | Example    | Default |
| --------- | ---------------------------------- | ---------- | ------- |
| index     | Data field to use for quick access | data.refId | -       |
