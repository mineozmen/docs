---
description: >-
  This state manager
  (com.rierino.state.manager.elastic.ElasticJoinedStateManager) is similar to
  ElasticStateManager, but can join multiple aggregates into a single index for
  efficient search.
---

# Elasticsearch Joined

## Manager Parameters

| Parameter | Definition                                                                                                                                   | Example                       | Default                 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- | ----------------------- |
| system    | Name of the ES [system ](../../systems.md#elasticsearch)with configuration details (url, pathPrefix, username, password, token, key, secret) | {url:"http://localhost:9200"} | master.elasticsearch.\* |
| index     | Name of the index to use for storing aggregates                                                                                              | product\_search               | -                       |
| as        | Name of the attribute to which aggregates should be mapped on the index                                                                      | product\_variants             | base                    |
| master    | Whether this state is allowed to remove the complete document when deleted                                                                   | true                          | false                   |
