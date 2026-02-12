---
description: >-
  This state manager (com.rierino.state.manager.LuceneStateManager) provides a
  Lucene state for local search capabilities.
---

# Lucene Based

## Manager Parameters

| Parameter        | Definition                                                                         | Example                                                        | Default |
| ---------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------- | ------- |
| default.analyzer | Default analyzer to use on fields when not specified                               | org.apache.lucene.analysis.standard.StandardAnalyzer           | -       |
| field.analyzers  | Comma separated list of field:analyzerClass pairs for using non-default analyzers  | data.name:org.apache.lucene.analysis.standard.StandardAnalyzer | -       |
| indexFields      | Comma separated list of field:type pairs for indexing                              | data.name:text                                                 | -       |

This manager requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'lucene', version:"${rierinoVersion}")
```
