---
description: These queries are converted to Odata API calls by OdataQueryProducer.
---

# Odata Queries

Odata v4 queries support simple, aggregation and command query types, whereas v2 supports only simple and command queries.

Odata specific features for special use cases are as follows:

## Query Parameters

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>path</td><td>Service endpoint path for running the query (except for entity name, which is provided in "from" expression of the query)</td></tr></tbody></table>

## Join Parameters

<table><thead><tr><th>Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>option</td><td>Extra options to add to an expand() statement (v4 only)</td></tr></tbody></table>

{% hint style="info" %}
Odata v2 allows automatically joining entities without any conditions, whereas v4 allows using join conditions, which are translated into $filter statements inside expand.
{% endhint %}
