---
description: >-
  This query manager (com.rierino.query.manager.LuceneQueryManager) provides
  ability to query a local Lucene state
---

# Lucene

## Manager Parameters

| Parameter    | Definition                                                                                                                      | Example        | Default |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------- |
| state        | Name of the [Lucene state manager](../state-managers/local-states/lucene-based.md) which is used as the data source for queries | product\_local | -       |
| defaultLimit | Maximum records to return when no limit is provided in query                                                                    | 1000           | 100     |
