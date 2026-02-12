---
description: >-
  These queries are converted to MongoDB filters and aggregations by
  MongoQueryProducer.
---

# MongoDB Queries

These queries support all available query types and convert them to Bson maps or lists for execution. Pipeline queries are treated as aggregate pipelines and bundled queries are treated as facets.

MongoDB specific features for special use cases are as follows:

## Query Parameters

<table><thead><tr><th width="267.88983220943845">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>matchAfterJoin</td><td>Whether the where condition should execute after joins or before (i.e. on main collection), defaults to false</td></tr><tr><td>pipe</td><td>Whether the query should be generated as an aggregate pipeline instead of a find query </td></tr></tbody></table>

{% hint style="info" %}
When there is a join condition on a query, it is automatically produced as an aggregate pipeline. The "pipe" parameter is only recommended when some aggregate operators will be used on an otherwise basic query.
{% endhint %}

## Join Parameters

MongoDB query manager translates joins into $lookup statements, with 2 alternative methods:

{% embed url="https://www.mongodb.com/docs/manual/reference/operator/aggregation/lookup" %}
MongoDB $lookup
{% endembed %}

### Simple Lookup

The simple lookup method uses only a simple "EQ" type condition, where localField of "from" collection maps to "Expression" and foreignField of "join" collection maps to "Value".

<figure><img src="../../../.gitbook/assets/image (136).png" alt=""><figcaption><p>Example Simple Lookup</p></figcaption></figure>

### Complex Lookup

The complex method uses pipeline feature of MongoDB lookups, where 2 alternative approaches can be used:

#### Using Parameters

With this approach, lookup pipeline can be manually configured using the following condition parameters:

<table><thead><tr><th>Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>let</td><td>Map of variables to expressions as "let" statement in "$lookup" stage (e.g. {"id":"$_id"})</td></tr><tr><td>pipeline</td><td>List of steps for "pipeline" to run on joined collection in "$lookup" stage (as array node or string) </td></tr></tbody></table>

#### Using Join Condition

When a complex condition is used for joining with a collection in query designer (e.g. using AND, OR or anything other than EQ operator), MongoDB query manager automatically translates the join into a pipeline based lookup. This option allows using regular conditions (including ability to use parameters such as required variables), which will be automatically translated into $match statements.&#x20;

When using this approach, it is possible to add the following parameters:

<table><thead><tr><th>Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>let</td><td>Map of variables to expressions as "let" statement in "$lookup" stage (e.g. {"id":"$_id"})</td></tr><tr><td>extra</td><td>List of steps for adding to "pipeline" after the produced $match statement (as array node or string) </td></tr></tbody></table>

## Common Mongo Parameters

Following parameters are applicable in various building blocks of a MongoDB query:

<table><thead><tr><th width="319">Parameter</th><th width="200">Definition</th></tr></thead><tbody><tr><td>mongo</td><td>Json object to be used as the field expression</td></tr><tr><td>mongo.inject</td><td>Whether "mongo" parameter should be injected with variables</td></tr><tr><td>mongoJson</td><td>Json string to be used as the field expression</td></tr><tr><td><a data-footnote-ref href="#user-content-fn-1">mongoJson.parseFirst</a></td><td>Whether "mongoJson" parameter should be parsed into a json object before injection</td></tr><tr><td>mongoJson.inject</td><td>Whether "mongoJson" parameter should be injected with variables</td></tr></tbody></table>

## Field Parameters

For both custom simple and aggregation fields, "Common Mongo Parameters" are applicable.

{% embed url="https://www.mongodb.com/docs/manual/reference/operator" %}
MongoDB Operators
{% endembed %}

{% hint style="info" %}
In defining field expressions for simple and aggregation fields, it is possible to use any MongoDB expression.&#x20;

However, if the first character is a "$", it should be omitted (e.g. instead of $id, expression should be id, instead of \$$ROOT, expression should be $ROOT). Query manager automatically adds a "$" character, if the first character is not "{". This convention is used to allow more standardized expressions between different systems.
{% endhint %}

## Custom Condition Operators

For simple conditions, MongoDB supports following custom operators:

* **value:** Uses "filter" parameter (e.g. $eq) as the check between rhs and lhs values of the condition.
* **values:** Same as value operator, accepting multiple values as the rhs.&#x20;
* **bson:** Uses "Common Mongo Parameters" to build a Bson statement as the condition.

It is also possible to use any mongodb operator (e.g. $exists) as a custom operator, which uses a single rhs value (e.g. "true") along with the lhs (e.g. data.records).&#x20;

{% hint style="warning" %}
MongoDB Java driver automatically hides "$and" operator in certain scenarios in older versions of the library (i.e. when multiple entries with same key doesn't exist). This can result in issues in rare complex condition scenarios (e.g. $not allowed as a root level operator). In such case, conditions inside the AND operator can be written as custom expressions instead.
{% endhint %}

## Custom Pipeline Steps

For pipeline steps, MongoDB supports following types:

* **custom:** Pipeline step is produced directly from "Common Mongo Parameters" of the step content.
* **condition:** Step content represents "where" condition of a query.
* **join:** Step content represents "join" condition of a query.
* **select:** Step content represents "fields" of a simple query.
* **aggregate:** Step content represents "fields" of an aggregation query.
* **orderBy:** Step content represents "orderBy" of a query.
* **skip:** Uses "value" text field of the step content to skip number of records.
* **limit:** Uses "value" text field of the step content to limit number of records.

## Examples

{% file src="../../../.gitbook/assets/customer_segments.json" %}
Select Customer Segment by Id (Can be Imported on Query Screen)
{% endfile %}

{% file src="../../../.gitbook/assets/product_detail_query.json" %}
Product Details with Joins and Expressions (Can be Imported on Query Screen)
{% endfile %}

{% file src="../../../.gitbook/assets/product_list_bundle.json" %}
Product List with Bundled Total Count (Can be Imported on Query Screen)
{% endfile %}

{% file src="../../../.gitbook/assets/product_variant_feed_es.json" %}
Pipeline with Unwind (Can be Imported on Query Screen)
{% endfile %}

[^1]: Typically used with older versions of MongoDB which doesn't allow storing $ prefixed fields, causing error on "mongo" parameters
