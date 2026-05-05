---
description: >-
  This state manager (com.rierino.state.manager.MongoStateManager) uses MongoDB
  for storing and reading data, typically as the master data store.
---

# MongoDB Collection

## Manager Parameters

| Parameter           | Definition                                                                                                         | Example                     | Default                   |
| ------------------- | ------------------------------------------------------------------------------------------------------------------ | --------------------------- | ------------------------- |
| system              | Name of the [system ](../../systems-integrations/#mongodb)which defines MongoDB service details (uri, database)    | {uri:"mongodb://127.0.0.1"} | master.mongo.uri/database |
| collection          | Name of the MongoDB collection to store details                                                                    | product                     | \[state manager alias]    |
| journalCollection   | Name of the MongoDB collection to store state changes                                                              | product\_journal            | -                         |
| keepJournal         | Alternative to journalCollection parameter, automatically using "draft\_journal" suffix for the journal collection | true                        | false                     |
| bypassDocValidation | Whether MongoDB should bypass document validation during updates                                                   | true                        | false                     |

This state manager supports [CallSP](/broken/pages/0a3jLM4TdCuK7kEQqgi2) action of write event handler, where command can be either of deleteOne, deleteMany, updateOne or updateMany and input contents can include following fields:

* **filter:** Filter to pass for deleting or updating records (e.g. {"data.status": "D"})
* **update:** Update contents to pass for updating records (e.g. {"$set": {"data.status": "A"\}})
* **options:** Delete or update options to pass (e.g. {"upsert": true})
