---
description: >-
  These queries are converted to Elasticsearch search parameters and
  aggregations by ElasticQueryProducer.
---

# Elasticsearch Queries

These queries support simple, aggregation, command and bundle query types.

Elasticsearch specific features for special use cases are as follows:

## Query Parameters

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>global</td><td>Whether the query is a global aggregation or not</td></tr></tbody></table>

## Order Field Parameters

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>sorter</td><td>Type of sorter to use (field for sort by field name, script for sort by script evaluation)</td></tr></tbody></table>

## Condition Parameters

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>boost</td><td>Boost factor for condition priority in search scores</td></tr><tr><td>queryName</td><td>Name to use for condition</td></tr><tr><td>caseInsensitive</td><td>(For EQ &#x26; CONTAINS simple conditions) whether check should be case sensitive or not</td></tr><tr><td>score</td><td>(For AND &#x26; CUSTOM complex conditions) whether inner conditions should be scored (must) or not (filter)</td></tr><tr><td>scoreMode</td><td>(For array conditions) score mode to apply on nested query</td></tr><tr><td>innerHit</td><td>(For array conditions) inner hit statement for nested query</td></tr><tr><td>innerHit.inject</td><td>Whether "innerhit" parameter should be injected with variables</td></tr></tbody></table>

## General Parameters

For queries, order fields, custom aggregation fields and custom conditions, it is possible to pass Elasticsearch specific statements, using following parameters:

<table><thead><tr><th width="319">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>elastic</td><td>Json object to be used as the field expression</td></tr><tr><td>elastic.inject</td><td>Whether "elastic" parameter should be injected with variables</td></tr><tr><td>elasticJson</td><td>Json string to be used as the field expression</td></tr><tr><td>elasticJson.inject</td><td>Whether "elasticJson" parameter should be injected with variables</td></tr></tbody></table>

## Custom Condition Operators

For simple conditions, Elasticsearch supports following custom operators:

* **query\_string:** Using "elastic", "elasticSearch" parameters or the command value, creates a query\_string type query
* **simple\_query\_string:** Using "elastic", "elasticSearch" parameters or the command value, creates a simple\_query\_string type query
* **match:** Using "elastic", "elasticSearch" parameters or the command & values, creates a match type query
* **multi\_match:** Using "elastic", "elasticSearch" parameters or the command & values, creates a multi\_match type query
* **prefix:** Using "elastic", "elasticSearch" parameters or the command & values, creates a prefix type query
* **terms\_set:** Using "elastic", "elasticSearch" parameters or the command & values, creates a terms\_set type query. When command is used, "data.required\_matches" field is used as the "minimum should match" field.
* **more\_like\_this:** Using "elastic", "elasticSearch" parameters or the command & values, creates a more\_like\_this type query. When command is used, minimum term frequency and document frequency are both set to 1.
* **script:** Using "elastic", "elasticSearch" parameters or the command, creates a script type query
* **generic:** Using "elastic", "elasticSearch" parameters, creates a query, based on "type" condition parameter

{% embed url="https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html" %}
Elasticsearch Query Types
{% endembed %}
