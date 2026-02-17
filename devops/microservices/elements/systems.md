---
description: >-
  Systems are used for defining key configuration details shared across elements
  which are based on a common data store or external tool.
---

# Systems

Systems are used for connecting 3rd party components to Rierino platform, by configuring their access details, such as:

* **Database systems:** Including shared connection and authorization details for state and query managers
* **Event streaming systems:** Including shared connection and authorization details for streams
* **Authentication systems:** Including connection details for authentication and authorization systems
* **API based systems:** Including connection and authorization details for REST API integrations

Each of these systems can be configured as elements just once and added to as many runners as required for allowing such runners communicate with an external component.

{% hint style="info" %}
It is recommended to configure and use value lookups (e.g. from k8s ConfigMap, etcd or similar) for infrastructure related settings (e.g. server IP address) and secret lookups (e.g. from k8s Secret, HashiCorp Vault or similar) for credential settings (e.g. API authentication token).

Value lookups can be referenced using $\{{VALUE\_PATH\}} notation whereas secret lookups can be referenced using #\{{SECRET\_PATH\}} notation when deployments are configured to use value and secret loaders.

Some systems also support auto reconnect functionality with "ephemeral" lookups. Such systems replace $!\{{VALUE\_PATH\}} and #!\{{SECRET\_PATH\}} notations with their respective values on each reconnect attempt. This feature allows changing a target system IP address or connection credentials without having to restart microservices connecting to such systems.
{% endhint %}

List of applicable settings when configuring a system element depends on the type of system (e.g. a database system requiring JDBC connection details, a rest system requiring connection URL), such as:

## REST

Includes settings required for connecting with an http server for standard REST API calls.

| Setting               | Definition                                                                                                               | Example                                           | Default                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- | ----------------------------- |
| url                   | Base URL for REST endpoint                                                                                               | http://localhost:8080/api                         | -                             |
| contentType           | Media content type for REST communications ("query" for url parameters, "none" for no content)                           | application/xml                                   | application/json              |
| trust                 | Whether endpoint should be trusted without SSL validation                                                                | true                                              | false                         |
| auth.method           | Method to use for authentication to endpoint (input, basic, bearer, scribe, jwt)                                         | basic                                             | -                             |
| auth.token.header[^1] | Header to include generated token in                                                                                     | custom-auth                                       | Authorization                 |
| auth.prefix[^1]       | Prefix to include for generated token                                                                                    | none (for no prefix)                              | Bearer (Basic for basic auth) |
| header.\[header]      | Additional header to send in system communications                                                                       | special-header=value                              | -                             |
| clientPrefix          | Client library specific prefix to use for setting client properties                                                      | jersey.config.client                              | -                             |
| client.\[property]    | Client library specific property to include                                                                              | async.threadPoolSize=10                           | -                             |
| alternateUrls         | Comma separated list of alternate URLs for fallback                                                                      | http://back1.example.com,http://back2.example.com | -                             |
| retries               | Maximum number of retries allowed in case of server failure                                                              | 5                                                 | 0                             |
| backoffMs             | Milliseconds to wait between retries                                                                                     | 500                                               | 1000                          |
| failTTLMs             | Milliseconds to wait until reconsidering a base or alternate URL                                                         | 10000                                             | 5000                          |
| responseCookies       | Json path to return cookie details received in response (in {path, value, domain, expiry, \[name]} format)               | cookies                                           | -                             |
| responseHeaders       | Json path to return header details received in response (in {\[name]:\[]} format)                                        | headers                                           | -                             |
| connector             | Rest conector to use (default for multi part body support, apache for native PATCH calls without method header override) | apache                                            | default                       |

{% embed url="https://www.youtube.com/watch?v=4KXbjt1qylk" %}

{% file src="../../../.gitbook/assets/system-shopify-0001.json" %}
Example Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

{% hint style="info" %}
When using XML contents, json body elements starting with [- character are converted](#user-content-fn-1)[^1] to attributes. E.g.&#x20;

{"soapenv:Envelope": {"-xmls:soapenv": "[http://schemas.xmlsoap.org/soap/envelope/](http://schemas.xmlsoap.org/soap/envelope/)"} }&#x20;

generates

\<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
{% endhint %}

{% hint style="info" %}
Cookies can be sent using "header.cookie" header parameter with "\[name]=\[value];\[name]=\[value]" format as the header value, similar to regular headers.
{% endhint %}



Based on auth.method, additional system parameters are used:

### Input Authentication Method

Authentication with a request body element:

| Parameter    | Definition                                             | Example      | Default |
| ------------ | ------------------------------------------------------ | ------------ | ------- |
| auth.key     | Key to include in each request body for authentication | cmVhbGx5Pw== | -       |
| auth.element | Json path to include authentication key in body        | user.apiKey  | -       |

{% file src="../../../.gitbook/assets/system-translate-0001.json" %}
Example Input Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### Basic Authentication Method

Username / password based basic authentication:

| Parameter     | Definition                          | Example | Default |
| ------------- | ----------------------------------- | ------- | ------- |
| auth.username | Username to include in each request | admin   | -       |
| auth.password | Password to include in each request | pass    | -       |

{% file src="../../../.gitbook/assets/system-jenkins_default-0001.json" %}
Example Basic Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### Bearer Authentication Method

Authentication with a bearer token:

| Parameter   | Definition                           | Example        | Default |
| ----------- | ------------------------------------ | -------------- | ------- |
| auth.token  | Token to include in each request     | cmVhbGx5Pw==   | -       |
| auth.prefix | Bearer prefix to use in each request | Special\_Token | Bearer  |

{% file src="../../../.gitbook/assets/system-deployer_default-0001.json" %}
Example Bearer Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### Scribe Authentication Method

Scribe based OAuth token authentication:

| Parameter                    | Definition                                                                                                                     | Example                                             | Default              |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------- | -------------------- |
| auth.prefix                  | Bearer prefix to use for access token in each request                                                                          | Special\_Token                                      | Bearer               |
| auth.provider.authentication | Client authentication model (if required, none, basic or input)                                                                | input                                               | basic                |
| auth.provider.key            | Key to use for client authentication                                                                                           | somekey                                             | -                    |
| auth.provider.secret         | Secret to use for client authentication                                                                                        | somesecret                                          | -                    |
| auth.provider.url            | Url of OAuth provider                                                                                                          | http://example.com/oauth                            | -                    |
| auth.provider.refreshUrl     | Url for getting new tokens when useRefresh is true                                                                             | http://example.com/oauth/refresh                    | \[auth.provider.url] |
| auth.provider.mapping[^1]    | JMESPath pattern for converting response from provider url to standard OAuth                                                   | {"access\_token": data.accessToken}                 | -                    |
| auth.token.grant             | Grant type (password, clientCredentials, refreshToken, custom)                                                                 | password                                            | -                    |
| auth.token.scope             | Authentication scope to apply                                                                                                  | customer.read                                       | -                    |
| auth.token.useRefresh        | Whether the refresh\_token received from OAuth endpoint should be used for [getting new access tokens](#user-content-fn-2)[^2] | true                                                | false                |
| auth.token.service           | Class name for OAuth provider service if custom class is needed                                                                | com.github.scribejava.core.builder.api.DefaultApi20 | -                    |

Depending on the grant type parameter, additional settings are applicable as follows:

#### Password Grant Type

| Setting       | Definition                | Example | Default |
| ------------- | ------------------------- | ------- | ------- |
| auth.username | Username for scribe grant | admin   | -       |
| auth.password | Password for scribe grant | pass    | -       |

#### Refresh Token Grant Type

| Setting           | Definition               | Example | Default |
| ----------------- | ------------------------ | ------- | ------- |
| auth.refreshToken | Refresh token for scripe | -       | -       |

#### Custom Grant Type

| Setting                        | Definition                                                                             | Example                                                 | Default |
| ------------------------------ | -------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------- |
| auth.provider.system           | Name of [runner system](#user-content-fn-3)[^3] to use for token requests              | custom\_auth                                            | -       |
| provider.custom.url            | Url endpoint on custom system to call for access tokens                                | /GetToken                                               | -       |
| provider.custom.method         | Http method to use for access token calls                                              | GET                                                     | -       |
| provider.custom.content        | Content type for access token calls                                                    | query                                                   | -       |
| provider.custom.input          | Json string to use as call body/parameters for access tokens                           | { username: "user", password: $\{{rierinoKV.secret\}} } | -       |
| provider.custom.refreshUrl     | Url endpoint on custom system to call for refreshing tokens                            | /RefreshToken                                           | -       |
| provider.custom.refreshMethod  | Http method to use for token refresh calls                                             | GET                                                     | -       |
| provider.custom.refreshContent | Content type for token refresh calls                                                   | query                                                   | -       |
| provider.custom.refreshInput   | Json string to use as call body/parameters for token refresh                           | -                                                       | -       |
| provider.custom.refreshPath    | Json path to add refresh token received from access token call to call body/parameters | refresh\_token                                          | -       |

{% file src="../../../.gitbook/assets/system-ebayseller_default-0001.json" %}
Example Scribe Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### JWT Authentication Method

Authentication generating JWT token wiith a private key:

| Parameter       | Definition                                              | Example        | Default |
| --------------- | ------------------------------------------------------- | -------------- | ------- |
| auth.prefix     | Bearer prefix to use in each request                    | Special\_Token | Bearer  |
| auth.ephemeral  | Whether each request should generate a new access token | true           | false   |
| auth.issuer     | Issuer of the token                                     | rierino        | -       |
| auth.subject    | Subject of the token                                    | 12345          | -       |
| auth.audience   | Target audience for the token                           | google         | -       |
| auth.payload    | Payload of the token                                    | foo            | -       |
| auth.id         | Id of the token                                         | 12345          | -       |
| auth.ttl        | TTL of token in seconds                                 | 36000          | 3600    |
| auth.claim.\*   | Claims of the token                                     | user=123       | -       |
| auth.header.\*  | Header of the token                                     | typ=JWT        | -       |
| auth.privateKey | Private key for signing token                           | AAAAAAAA       | -       |
| auth.algorithm  | Algorithm for signing token                             | RS256          | RS256   |

In addition to parameters, it is possible to send token inputs (e.g. issuer, ttl, claim, header) using input authentication payload.

{% file src="../../../.gitbook/assets/system-translatev3-0001.json" %}
Example JWT Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

## MongoDB

Includes settings required for connecting to a MongoDB database.

| Setting  | Definition                                                                                         | Example                   | Default |
| -------- | -------------------------------------------------------------------------------------------------- | ------------------------- | ------- |
| uri      | Connection string in URI format for connecting to MongoDB system (can be multiple comma separated) | mongodb://localhost:27017 | -       |
| database | Name of the database to connect                                                                    | master                    | -       |

{% file src="../../../.gitbook/assets/system-mongo_master-0001.json" %}
Example Mongo System Definition (Can be Imported on Element Screen)
{% endfile %}

{% embed url="https://www.mongodb.com" %}
MongoDB Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'mongodb', version:"${rierinoVersion}")
```

## File System

Includes settings required for connecting to a file system. Additional HDFS settings can be applied using site.xml files.

| Setting           | Definition                                                            | Example                                            | Default |
| ----------------- | --------------------------------------------------------------------- | -------------------------------------------------- | ------- |
| uri               | Filesystem root address                                               | hdfs://localhost:8020/master                       | -       |
| fsspec.protocol   | Fsspec protocol when using file system with a Py4J handler            | sftp                                               | -       |
| fsspec.options    | Json representation of fsspec options when using with a Py4J handler  | {host:"", port:22, username:"", password:""}       | -       |
| hdfs.\[parameter] | Filesystem parameters when using with an FSEventHandler               | fs.s3a.impl=com.rierino.util.fs.CustomS3FileSystem | -       |

{% hint style="info" %}
Custom file systems listed in "Gateway Services" can be also used with FSEventHandler.
{% endhint %}

{% embed url="https://hadoop.apache.org" %}
Hadoop Page
{% endembed %}

When writing to sequence files with FSEventHandler, this system also uses the following settings:

| Setting          | Definition                                                                                    | Example                                             | Default                                             |
| ---------------- | --------------------------------------------------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------- |
| path.writer      | Full class name of the path writer to use for generating file paths                           | com.rierino.handler.util.helper.hdfs.DatePathWriter | com.rierino.handler.util.helper.hdfs.DatePathWriter |
| path.maxRows     | [Maximum records ](#user-content-fn-4)[^4]to include in each sequence file (-1 for unlimited) | 10000                                               | -1                                                  |
| path.bufferSize  | Buffer size for sequence file writer                                                          | 1000                                                | -1                                                  |
| path.blockSize   | Block size for sequence file writer                                                           | 100                                                 | -1                                                  |
| path.compression | Compression to apply on sequence file writer                                                  | BLOCK                                               | NONE                                                |
| path.asBytes     | Whether to write contents as bytes or Text.class                                              | false                                               | true                                                |
| path.format      | Sequence path format to use for DatePathWriter (e.g. one folder per hour)                     | yyyy/MM/dd/hh                                       | yyyy/MM/dd                                          |

## CDC

Includes settings required for connecting to a database or a similar system for change data capture. CDC managers produce CDCRecord entries and publish them on a given stream, which can be consumed by a [CDCRoleHandler ](handlers/flow-handlers/convert-pulse-to-journal.md)to convert them into pulse and journal records.

Spring event runners provide support for CDC managers, where each CDC stream linked to a CDC manager can define an offset state (using offset.state parameter of the stream), which is updated based on the specified commit duration (using commitMs parameter of the runner) for managing resume tokens on restart.

{% file src="../../../.gitbook/assets/cdc_batch-0001.json" %}
Example Spring Runner with CDC Manager
{% endfile %}

Samza event runners on the other hand, provide more native support for CDC managers, treating them as consumers with input streams with a specific way of configuring access to them.

{% embed url="https://samza.apache.org/learn/documentation/latest/jobs/configuration-table.html" %}
Samza Systems Configurations
{% endembed %}

| Samza Setting                                              | Spring Setting   | Definition                                                                                       | Example                               | Default                        |
| ---------------------------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------- | ------------------------------ |
| systems.$alias.consumer.manager                            | manager          | Fully qualified class name for the CDC manager                                                   | com.rierino.state.cdc.MongoCDCManager | -                              |
| systems.$alias.consumer.dlq.suffix                         | dlq.suffix       | Suffix to add to CDC stream names for dead letter queues                                         | \_fail                                | -                              |
| systems.$alias.consumer.dlq.enrich                         | dlq.enrich       | Whether dead letter queue entries should include CDC content                                     | true                                  | false                          |
| systems.$alias.consumer.offset.type                        | offset.type      | Type of resume token / offset value (long, comparable or unordered)                              | long                                  | unordered                      |
| systems.$alias.consumer.pollMs                             | -[^5]            | Milliseconds to wait before polling new records                                                  | 5000                                  | 1000                           |
| systems.$alias.consumer.asPulse                            | -[^5]            | Whether CDC should produce records as pulse instead of CDC records                               | false                                 | true                           |
| systems.$alias.consumer.manager.parameter.ignoreTerminate  | ignoreTerminate  | Whether the system should stop listening if a TERMINATE operation is received                    | true                                  | false                          |
| systems.$alias.consumer.manager.parameter.onResumeFail     | onResumeFail     | Type of action when CDC manager can not resume from last checkpoint (SKIP, MUTE or FATAL)        | FATAL                                 | SKIP                           |
| systems.$alias.consumer.manager.parameter.onRecordFail     | onRecordFail     | Type of action when CDC manager can not process current change record (SKIP, DLQ, MUTE or FATAL) | FATAL                                 | SKIP                           |
| systems.$alias.consumer.manager.parameter.ignoreResume     | ignoreResume     | Whether the system should ignore current resume token and start as if it is missing              | true                                  | false                          |
| systems.$alias.consumer.manager.parameter.resumeReset      | resumeReset      | Type of strategy to follow on missing resume token (OLDEST or NEWEST)                            | NEWEST                                | OLDEST                         |
| systems.$alias.consumer.manager.parameter.disableReconnect | disableReconnect | Whether reconnecting on failure should be disabled or not                                        | true                                  | false                          |
| systems.$alias.consumer.manager.parameter.retriesPerStep   | retriesPerStep   | Number of reconnect retries on each backoff step                                                 | 3                                     | 1                              |
| systems.$alias.consumer.manager.parameter.backoffSteps     | backoffSteps     | Milliseconds to wait at each backoff step as comma separated values                              | 1000,30000                            | 10,100,200,500,1000,1000,10000 |

In addition to these shared settings, the following CDC managers have additional settings, which are similar to system settings (e.g. systems.$alias.consumer.manager.parameter.uri for MongoDB uri):

* **com.rierino.state.cdc.NoopCDCManager**: Uses "ms" setting for configuring milliseconds to wait between creating a new CDC record with an incremental aggregate ID.
* **com.rierino.state.cdc.ActionCDCManager**: Uses "action" setting for making a call to action path on each iteration and an optional "source.stream" setting for defining source for the action call. Processed event payload can contain 3 main fields:
  * **wait:** If set to true, the action is not triggered till the CDC manager is polled again
  * **offset:** Used as the resume token, which is provided in event payload on the next action call
  * **content:** Stored in content of the produced CDC record
* **com.rierino.state.cdc.MongoCDCManager**: Uses "uri" and "database" settings.
* **com.rierino.state.cdc.RedisCDCManager**: Uses "uri" and "master" settings.
* **com.rierino.state.cdc.EtcdCDCManager**: Uses "url", "namespace", "user", "password" settings.
* **com.rierino.state.cdc.DebeziumCDCManager**: Uses all settings applicable to Debezium connectors.

This manager requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'debezium', version:"${rierinoVersion}")
```

* **com.rierino.state.cdc.HDFSCDCManager**: Uses "uri" and all settings applicable to HDFS file systems for file system change data capture.
* **com.rierino.state.cdc.odata4.OdataCDCManager:** Uses "url" and "path" settings and delta logic of odata v4 endpoints for change data capture.
* **com.rierino.state.cdc.MailCDCManager:** Uses[^6] "mail.\*" settings and UID logic of email servers to fetch new emails as change data capture.

{% hint style="info" %}
Runners using CDC managers should be deployed with single replicas since managers consume all records coming from a CDC stream without applying any partitioning. To apply partitioning on these records, the runners should output records to Kafka topics and run business logic on runners consuming these topics.
{% endhint %}

## Keycloak

Includes settings required for connecting to a Keycloak server for authentication handlers.

| Setting           | Definition                                                                        | Example                                      | Default |
| ----------------- | --------------------------------------------------------------------------------- | -------------------------------------------- | ------- |
| config            | Json string for Keycloak adapter configuration                                    | {"realm":"test", ...}                        | -       |
| authServerUrl     | Url endpoint for Keycloak server (if not provided as config already)              | https://localhost/auth/                      | -       |
| realm             | Authentication realm to use (if not provided as config already)                   | admin-user                                   | -       |
| resource          | Authentication client resource to use (if not provided as config already)         | rierino-auth                                 | -       |
| credential.\[key] | Keycloak server access credentials as KV pair (if not provided as config already) | provider=secret, username=admin, secret=pass | -       |
| roles             | Default roles to assign to each new user                                          | user                                         | -       |
| idToken           | Whether to return id\_token when resolving tokens                                 | true                                         | false   |

{% embed url="https://www.keycloak.org" %}
Keycloak Page
{% endembed %}

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'keycloak', version:"${rierinoVersion}")
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

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

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

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

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

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'couchbase', version:"${rierinoVersion}")
```

## JDBC

Includes settings required for connecting to a database using JDBC.&#x20;

| Setting              | Definition                                                                   | Example                                       | Default |
| -------------------- | ---------------------------------------------------------------------------- | --------------------------------------------- | ------- |
| uri                  | JDBC data source uri                                                         | jdbc:sqlite:test.db                           | -       |
| connectionProperties | JDBC connection properties                                                   | DriverClassName=org.sqlite.JDBC,Username=user | -       |
| dataSourceClassName  | [XA data source class name for transaction handling](#user-content-fn-7)[^7] | org.postgresql.xa.PGXADataSource              | -       |

{% embed url="https://commons.apache.org/proper/commons-dbcp/configuration.html" %}
Connection Properties
{% endembed %}

For Jooq based JDBC state managers, the following system settings are also applicable:

| Setting   | Definition                                                 | Example  | Default |
| --------- | ---------------------------------------------------------- | -------- | ------- |
| dbms      | Jooq DBMS name for SQLDialect                              | POSTGRES | DEFAULT |
| mergeInto | Whether the Jooq database class supports merge into or not | false    | true    |

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'jdbc', version:"${rierinoVersion}")
```

## Kafka

Includes settings required for connecting to a Kafka cluster.

| Setting                           | Definition                                                                                  | Example                                                        | Default |
| --------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ------- |
| binary                            | Whether the system uses binary or text data format                                          | true                                                           | false   |
| key.class                         | Fully qualified classname for Kafka system record keys                                      | java.lang.String                                               | -       |
| msg.class                         | Fully qualified classname for Kafka system record contents                                  | java.lang.String                                               | -       |
| msg.class.inner                   | Fully qualified inner classname for Kafka system record contents, if msg.class is a generic | java.lang.String                                               | -       |
| rierino.system.$alias.consumer.\* | Kafka consumer configurations (except for Samza)                                            | bootstrap.servers=localhost:9092                               | -       |
| rierino.system.$alias.producer.\* | Kafka producer configurations (except for Samza)                                            | batch.size=1                                                   | -       |
| systems.$alias.\*                 | Samza specific connection configurations                                                    | samza.factory=org.apache.samza.system.kafka.KafkaSystemFactory | -       |
| parameter.consumer.\[property]    | Kafka consumer properties                                                                   | auto.offset.reset=earliest                                     | -       |
| parameter.producer.\[property]    | Kafka producer properties                                                                   | acks=0                                                         | -       |
| parameter.output.backupSystem     | Name of backup system to use if a stream of this system fails                               | kafka\_backup                                                  | -       |
| parameter.output.backupStream     | Name of backup stream to use if a stream of this system fails                               | journal\_backup                                                | -       |

{% embed url="https://kafka.apache.org" %}
Kafka Page
{% endembed %}

## etcd

Includes settings required for connecting to an etcd server.

| Setting  | Definition                                           | Example                                      | Default |
| -------- | ---------------------------------------------------- | -------------------------------------------- | ------- |
| url      | Comma separated list of endpoints for etcd server(s) | http://localhost:2379, http://localhost:2378 | -       |
| user     | Username for etcd connection                         | \*\*\*\*                                     | -       |
| password | Password for etcd connection                         | \*\*\*\*                                     | -       |

{% embed url="https://etcd.io" %}

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'etcd', version:"${rierinoVersion}")
```

## Camel

Includes settings required for connecting to an Apache Camel system.&#x20;

| Setting    | Definition                        | Example  | Default |
| ---------- | --------------------------------- | -------- | ------- |
| camelRoute | Uri for the Camel system endpoint | mock:out | -       |

{% embed url="https://camel.apache.org/" %}
Apache Camel
{% endembed %}

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.runner', name: 'camel', version:"${rierinoVersion}")
```

## Email

Includes settings required for connecting to an email server system.

| Setting         | Definition                                                              | Example                  | Default |
| --------------- | ----------------------------------------------------------------------- | ------------------------ | ------- |
| mail.\*         | Jakarta mail settings to apply                                          | mail.store.protocol=imap | -       |
| mail.rierino.\* | Rierino OAuth2Auth authentication settings (when mechanism is XOAUTH2)  | -                        | -       |

{% embed url="https://jakarta.ee/specifications/mail/" %}
Jakarta Mail
{% endembed %}

This system requires the following dependency added to [deployment contents](../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'mail', version:"${rierinoVersion}")
```

## noop

A predefined "noop" system exists in Rierino, which is considered to be a dummy system. Any stream mapping to this system name ignores all send message requests, acting as a blackhole.

[^1]: Since 0.3.4

[^2]: When set to false, original method (e.g. password, client credentials) is used for getting new access tokens each time instead

[^3]: All connection details are loaded from the given runner system element for access and refresh token calls

[^4]: File is closed only after this number of records are produced        &#x20;

[^5]: Defined at stream level instead

[^6]: since 0.5.2

[^7]: JDBC system connections use Atomikos for transaction handling, hence also require its configuration parameters.&#x20;
