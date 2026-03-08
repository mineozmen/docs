---
description: >-
  This state manager (com.rierino.state.manager.SingleJournalStoreManager) is an
  event store coordinator, which uses a core state.
---

# Event Store Coordinator

State manager works as a coordinator, keeping event journal entries for each aggregate as "entries" data element in a core state. This manager can convert any standard state manager into an event store, allowing retrieval of historical versions of an aggregate.

## Manager Parameters

| Parameter  | Definition                                       | Example | Default |
| ---------- | ------------------------------------------------ | ------- | ------- |
| core.state | Name of the state manager which acts as the core | product | -       |

