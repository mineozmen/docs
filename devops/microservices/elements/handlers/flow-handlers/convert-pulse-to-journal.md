---
description: >-
  This handler (com.rierino.handler.CDCRoleHandler) provides ability to convert
  CDC pulse records into journal entries, enriching them using state data.
---

# Convert Pulse to Journal

## Handler Parameters

| Parameter     | Definition                                                                                        | Example | Default |
| ------------- | ------------------------------------------------------------------------------------------------- | ------- | ------- |
| cdc.state     | Name of the state manager to use for producing journal data                                       | product | -       |
| allowExternal | Whether pulses originating from external edits (i.e. without journal records) should be also used | true    | false   |

## Roles

### produceJournal

Converts received pulse record into journal using cdc.state. If state manager has a journal entry recorded, it is used, otherwise a new journal entry is created using the current aggregate available in state manager.&#x20;
