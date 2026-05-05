---
description: Other systems range from event streaming to custom Camel based connectors
---

# Other Systems

## CDC

Includes settings required for connecting to a database or a similar system for change data capture. CDC managers produce CDCRecord entries and publish them on a given stream, which can be consumed by a [CDCRoleHandler ](/broken/pages/QMAwCDov0qa30B9EOZvG)to convert them into pulse and journal records.

Spring event runners provide support for CDC managers, where each CDC stream linked to a CDC manager can define an offset state (using offset.state parameter of the stream), which is updated based on the specified commit duration (using commitMs parameter of the runner) for managing resume tokens on restart.

{% file src="../../../../.gitbook/assets/cdc_batch-0001.json" %}
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
| systems.$alias.consumer.pollMs                             | -[^1]            | Milliseconds to wait before polling new records                                                  | 5000                                  | 1000                           |
| systems.$alias.consumer.asPulse                            | -[^1]            | Whether CDC should produce records as pulse instead of CDC records                               | false                                 | true                           |
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

This manager requires the following dependency added to [deployment contents](../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'debezium', version:"${rierinoVersion}")
```

* **com.rierino.state.cdc.HDFSCDCManager**: Uses "uri" and all settings applicable to HDFS file systems for file system change data capture.
* **com.rierino.state.cdc.odata4.OdataCDCManager:** Uses "url" and "path" settings and delta logic of odata v4 endpoints for change data capture.
* **com.rierino.state.cdc.MailCDCManager:** Uses "mail.\*" settings and UID logic of email servers to fetch new emails as change data capture.

{% hint style="info" %}
Runners using CDC managers should be deployed with single replicas since managers consume all records coming from a CDC stream without applying any partitioning. To apply partitioning on these records, the runners should output records to Kafka topics and run business logic on runners consuming these topics.
{% endhint %}

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

## Email

Includes settings required for connecting to an email server system.

| Setting         | Definition                                                              | Example                  | Default |
| --------------- | ----------------------------------------------------------------------- | ------------------------ | ------- |
| mail.\*         | Jakarta mail settings to apply                                          | mail.store.protocol=imap | -       |
| mail.rierino.\* | Rierino OAuth2Auth authentication settings (when mechanism is XOAUTH2)  | -                        | -       |

{% embed url="https://jakarta.ee/specifications/mail/" %}
Jakarta Mail
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.processors', name: 'mail', version:"${rierinoVersion}")
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
| path.maxRows     | [Maximum records ](#user-content-fn-2)[^2]to include in each sequence file (-1 for unlimited) | 10000                                               | -1                                                  |
| path.bufferSize  | Buffer size for sequence file writer                                                          | 1000                                                | -1                                                  |
| path.blockSize   | Block size for sequence file writer                                                           | 100                                                 | -1                                                  |
| path.compression | Compression to apply on sequence file writer                                                  | BLOCK                                               | NONE                                                |
| path.asBytes     | Whether to write contents as bytes or Text.class                                              | false                                               | true                                                |
| path.format      | Sequence path format to use for DatePathWriter (e.g. one folder per hour)                     | yyyy/MM/dd/hh                                       | yyyy/MM/dd                                          |

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

This system requires the following dependency added to [deployment contents](../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'keycloak', version:"${rierinoVersion}")
```

## Camel

Includes settings required for connecting to an Apache Camel system.&#x20;

| Setting    | Definition                        | Example  | Default |
| ---------- | --------------------------------- | -------- | ------- |
| camelRoute | Uri for the Camel system endpoint | mock:out | -       |

{% embed url="https://camel.apache.org/" %}
Apache Camel
{% endembed %}

This system requires the following dependency added to [deployment contents](../../deployment-packages/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.runner', name: 'camel', version:"${rierinoVersion}")
```

[^1]: Defined at stream level instead

[^2]: File is closed only after this number of records are produced        &#x20;
