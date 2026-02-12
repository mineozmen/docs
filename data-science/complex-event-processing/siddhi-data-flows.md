---
description: These queries are converted to Siddhi statements by SiddhiCodeProducer.
---

# Siddhi Data Flows

Siddhi has the following special attributes for data flow definitions:

* **Persisted:** Whether data flow contents should be persisted for resilience or kept in memory only

## Mutation Parameters

All [Siddhi query](../../configuration/queries/query-platforms/siddhi-queries.md) parameters are applicable to mutation queries, and the following special parameters can be applied at mutation level as well:

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>annotation</td><td>annotation to add to the mutation code</td></tr><tr><td>matchOn</td><td>(for DELETE &#x26; UPDATE) condition for matching queries to target tables</td></tr></tbody></table>
