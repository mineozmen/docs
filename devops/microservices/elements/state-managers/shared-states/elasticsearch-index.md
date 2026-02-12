---
description: >-
  This state manager (com.rierino.state.manager.elastic.ElasticStateManager)
  uses Elasticsearch as an aggregate store, which is typically used as the
  search engine.
---

# Elasticsearch Index

## Manager Parameters

| Parameter | Definition                                                                                                                                   | Example                       | Default                 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- | ----------------------- |
| system    | Name of the ES [system ](../../systems.md#elasticsearch)with configuration details (url, pathPrefix, username, password, token, key, secret) | {url:"http://localhost:9200"} | master.elasticsearch.\* |
| index     | Name of the index to use for storing aggregates                                                                                              | product\_search               | -                       |
