---
description: >-
  This handler (com.rierino.handler.JDBCTransactionEventHandler) executes
  multiple state manipulation journals as a single database transaction.
---

# Perform DB Transaction

## Handler Parameters

<table><thead><tr><th>Parameter</th><th>Definition</th><th width="222">Example</th><th>Default</th></tr></thead><tbody><tr><td>transaction.timeout</td><td>Seconds to consider a transaction timed out</td><td>60</td><td>20</td></tr></tbody></table>

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'jdbc', version:"${rierinoVersion}")
```

## Actions

### ExecuteTransaction

Executes database statements on target JDBC state managers as a single transaction, using array of journals provided in "journals" path of the event payload.

### StartTransaction

Starts a database transaction on current local thread, which can be used as part of a local saga flow for multiple database operations.

### CommitTransaction

Commits current database transaction started on local thread, which must be called after a "StartTransaction" action.

### RollbackTransaction

Cancels and rollsback current database transaction started on local thread, which must be called after a "StartTransaction" action.

{% hint style="info" %}
This event handler can only be used on JDBC state managers.
{% endhint %}
