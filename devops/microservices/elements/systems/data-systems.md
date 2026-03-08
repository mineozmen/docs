---
description: Data systems provide shared connection details for state and query managers.
---

# Data Systems

## MongoDB

Includes settings required for connecting to a MongoDB database.

| Setting  | Definition                                                                                         | Example                   | Default |
| -------- | -------------------------------------------------------------------------------------------------- | ------------------------- | ------- |
| uri      | Connection string in URI format for connecting to MongoDB system (can be multiple comma separated) | mongodb://localhost:27017 | -       |
| database | Name of the database to connect                                                                    | master                    | -       |

{% file src="../../../../.gitbook/assets/system-mongo_master-0001.json" %}
Example Mongo System Definition (Can be Imported on Element Screen)
{% endfile %}

{% embed url="https://www.mongodb.com" %}
MongoDB Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'mongodb', version:"${rierinoVersion}")
```

## JDBC

Includes settings required for connecting to a database using JDBC.&#x20;

| Setting              | Definition                                                                   | Example                                       | Default |
| -------------------- | ---------------------------------------------------------------------------- | --------------------------------------------- | ------- |
| uri                  | JDBC data source uri                                                         | jdbc:sqlite:test.db                           | -       |
| connectionProperties | JDBC connection properties                                                   | DriverClassName=org.sqlite.JDBC,Username=user | -       |
| dataSourceClassName  | [XA data source class name for transaction handling](#user-content-fn-1)[^1] | org.postgresql.xa.PGXADataSource              | -       |

{% embed url="https://commons.apache.org/proper/commons-dbcp/configuration.html" %}
Connection Properties
{% endembed %}

For Jooq based JDBC state managers, the following system settings are also applicable:

| Setting   | Definition                                                 | Example  | Default |
| --------- | ---------------------------------------------------------- | -------- | ------- |
| dbms      | Jooq DBMS name for SQLDialect                              | POSTGRES | DEFAULT |
| mergeInto | Whether the Jooq database class supports merge into or not | false    | true    |

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'jdbc', version:"${rierinoVersion}")
```

## Elasticsearch

Includes settings required for connecting to an Elasticsearch server.

| Setting    | Definition                                                                    | Example               | Default |
| ---------- | ----------------------------------------------------------------------------- | --------------------- | ------- |
| url        | HTTP endpoint to access ES through REST API (can be multiple comma separated) | http://localhost:9200 | -       |
| pathPrefix | Path prefix to include for ES rest client builder                             | test                  | -       |
| username   | Username for accessing ES with basic authentication                           | \*\*\*\*\*            | -       |
| password   | Password for accessing ES with basic authentication                           | \*\*\*\*\*            | -       |
| token      | Service token to access ES with token authentication                          | \*\*\*\*\*            | -       |
| key        | API key to access ES for key authentication                                   | \*\*\*\*\*            | -       |
| secret     | API secret to access ES for key authentication                                | \*\*\*\*\*            | -       |

{% embed url="https://www.elastic.co/elasticsearch" %}
Elasticsearch Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'elastic', version:"${rierinoVersion}")
```

## Redis

Includes settings required for connecting to a Redis store.

| Setting | Definition                                                                                       | Example                | Default |
| ------- | ------------------------------------------------------------------------------------------------ | ---------------------- | ------- |
| uri     | Connection string in URI format for connecting to Redis system (can be multiple comma separated) | redis://localhost:6379 | -       |
| master  | Master name if using sentinel pool                                                               | master                 | -       |

{% embed url="https://redis.io" %}
Redis Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'redis', version:"${rierinoVersion}")
```

## Couchbase

Includes settings required for connecting to a Couchbase database.

| Setting     | Definition                                                         | Example               | Default |
| ----------- | ------------------------------------------------------------------ | --------------------- | ------- |
| uri         | Connection string in URI format for connecting to Couchbase system | couchbase://localhost | -       |
| username    | Username for access                                                | user                  | -       |
| password    | Password for access                                                | pass                  | -       |
| bucket      | Bucket to access                                                   | bucket\_1             | -       |
| bucket.wait | Bucket wait duration                                               | 30                    | 10      |

{% embed url="https://www.couchbase.com/" %}
Couchbase Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'couchbase', version:"${rierinoVersion}")
```

## etcd

Includes settings required for connecting to an etcd server.

| Setting  | Definition                                           | Example                                      | Default |
| -------- | ---------------------------------------------------- | -------------------------------------------- | ------- |
| url      | Comma separated list of endpoints for etcd server(s) | http://localhost:2379, http://localhost:2378 | -       |
| user     | Username for etcd connection                         | \*\*\*\*                                     | -       |
| password | Password for etcd connection                         | \*\*\*\*                                     | -       |

{% embed url="https://etcd.io" %}

This system requires the following dependency added to [deployment contents](../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'etcd', version:"${rierinoVersion}")
```

[^1]: JDBC system connections use Atomikos for transaction handling, hence also require its configuration parameters.&#x20;
