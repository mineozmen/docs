# Query Types

There are 5 main types of queries, some of which may not be supported by the selected query platform (e.g. Odata not supporting bundle queries):

## Command Queries

This type of queries only define a free text "command", which is highly platform specific and interpreted as DSL. An example would be full SQL select statement on a JDBC query manager.

Some platforms accept templated command texts, using FreeMarker template engine, using variables as its data model.

{% embed url="https://freemarker.apache.org" %}
FreeMarker Page
{% endembed %}

Platforms supporting template use typically have "templated" parameter to indicate that command text should be interpreted by the template engine. Typically such platforms also support templating of condition commands as well.

## Simple Queries

This type of queries are used for executing simplest form of requests and typically have the following attributes:

* **Fields:** List of columns to be returned in results.
* **Where Condition:** Condition to be applied for filtering result set.
* **Joins:** List of additional data sources (e.g. tables) to join with.&#x20;
* **Order Bys:** List of fields and sort directions to order results.
* **Skip:** Number of rows to skip in results.
* **Limit:** Number of rows to return in results.&#x20;

## Aggregation Queries

This type of queries are used for grouping data sources to calculate aggregations (e.g. count, sum) and typically have the following attributes:

* **Fields:** List of aggregations to be returned in results.
* **Having:** Condition to be applied on aggregated values for filtering result set.

## Bundle Queries

This type of queries bundle one or more aggregation queries with a simple query, improving response times in running multiple, dependent queries. Only some query managers (such as [ElasticQueryManager](../../devops/microservices/building-blocks/query-and-search-sources/elasticsearch.md)) support this type of queries. Bundle query configurations start same as simple queries, where bundles are added as new queries to it next.

## Pipeline Queries

This type of queries run a series of query steps on target platform and return the final result set, improving response times and flexibility in query plans. Only some query managers (such as [MongoQueryManager](../../devops/microservices/building-blocks/query-and-search-sources/mongodb.md)) support this type of queries. Pipeline queries are defined as a list of pipeline steps, each having:

* **Name:** Descriptive name and alias of the step.
* **Description:** Detailed description of the step.
* **Type:** Platform specific nature of the step.
* **Content:** Platform specific command contents for the step.
* **Inject:** Whether step should be injected with variables or not.
* **Required Variables:** List of mandatory variables for the step to execute (others will be considered optional and omitted if missing).
