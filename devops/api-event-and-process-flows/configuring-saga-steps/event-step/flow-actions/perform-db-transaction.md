---
description: >-
  These actions provide ability to execute multiple state manipulation journals
  as a single database transaction.
---

# Perform DB Transaction

## Perform DB Transaction Actions

### ExecuteTransaction

Executes database statements on target JDBC state managers as a single transaction, using an array of journals provided in the `journals` path of the event payload.

### StartTransaction

Starts a database transaction on the current local thread, which can be used as part of a local saga flow for multiple database operations.

### CommitTransaction

Commits the current database transaction started on the local thread. Call this after `StartTransaction`.

### RollbackTransaction

Cancels and rolls back the current database transaction started on the local thread. Call this after `StartTransaction`.

{% hint style="info" %}
This handler can only be used on JDBC state managers.
{% endhint %}
