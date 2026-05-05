---
description: >-
  This state manager (com.rierino.state.manager.JooqStateManager) uses a SQL
  database, mapping Json data into one or more tables.
---

# Jooq (SQL) Table

## Manager Parameters

| Parameter         | Definition                                                                                                                             | Example             | Default            |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------ |
| system            | Name of the [system ](../../systems-integrations/#jdbc)which defines remote service details (dbms, uri, connectionPropertiesmergeInto) | {dbms:"ORACLE"}     | -                  |
| maxColumnNameSize | Max number of characters allowed for column names                                                                                      | 255                 | 100                |
| rootName          | Root table name                                                                                                                        | product             | State manager name |
| journalTable      | Table for storing change journal records                                                                                               | product\_journal    | -                  |
| schema.state      | State manager keeping json schemas                                                                                                     | schema              | -                  |
| schema            | Json string representing data schema (if schema. state is not used)                                                                    | {id:{type:string\}} | -                  |

This state manager supports [CallSP](/broken/pages/0a3jLM4TdCuK7kEQqgi2) action of write event handler, where command is the parameterized command to apply and the inputs are passed as parameters to this command.
