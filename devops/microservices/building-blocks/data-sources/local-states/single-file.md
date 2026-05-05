---
description: >-
  This state manager (com.rierino.state.manager.SingleFileStateManager) provides
  read-only access to a local json file, mainly used for test purposes.
---

# Single File

## Manager Parameters

| Parameter | Definition                                                                         | Example          | Default |
| --------- | ---------------------------------------------------------------------------------- | ---------------- | ------- |
| path      | File path for the state contents                                                   | states/test.json | -       |
| dynamic   | Whether state manager reads file on each get request or only once at the beginning | true             | false   |
