---
description: >-
  This state manager (com.rierino.state.manager.CouchbaseStateManager) uses
  Couchbase for storing and reading data.
---

# Couchbase Collection

## Manager Parameters

| Parameter         | Definition                                                                                                         | Example                       | Default                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------ | ----------------------------- | ---------------------- |
| system            | Name of the [system](../../systems/#couchbase) which defines Couchbase service details (uri, bucket)               | {uri:"couchbase://127.0.0.1"} | -                      |
| scope             | Name of the Couchbase scope                                                                                        | core\_scope                   | \_default              |
| collection        | Name of the Couchbase collection to store details                                                                  | product                       | \[state manager alias] |
| journalCollection | Name of the Couchbase collection to store state changes                                                            | product\_journal              | -                      |
| keepJournal       | Alternative to journalCollection parameter, automatically using "draft\_journal" suffix for the journal collection | true                          | false                  |
