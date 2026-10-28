---
description: >-
  This state manager (com.rierino.state.manager.JooqStateManager) uses a SQL
  database, mapping Json data into one or more tables.
---

# Jooq (SQL) Table

## Manager Parameters

| Parameter         | Definition                                                                                                                                            | Example             | Default            |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------ |
| system            | Name of the [system ](../../systems-integrations/data-systems.md#jdbc)which defines remote service details (dbms, uri, connectionPropertiesmergeInto) | {dbms:"ORACLE"}     | -                  |
| maxColumnNameSize | Max number of characters allowed for column names                                                                                                     | 255                 | 100                |
| rootName          | Root table name                                                                                                                                       | product             | State manager name |
| schema.name       | Database schema name                                                                                                                                  | public              | Root name          |
| journalTable      | Table for storing change journal records                                                                                                              | product\_journal    | -                  |
| schema.state      | State manager keeping json schemas                                                                                                                    | schema              | -                  |
| schema            | Json string representing data schema (if schema. state is not used)                                                                                   | {id:{type:string\}} | -                  |

This state manager supports [CallSP](../../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/write-data.md#callsp) action of write event handler, where command is the parameterized command to apply and the inputs are passed as parameters to this command.

{% hint style="info" %}
`com.rierino.state.manager.JooqNestedStateManager` is an experimental variant for nested data models. It uses the same parameters.
{% endhint %}
