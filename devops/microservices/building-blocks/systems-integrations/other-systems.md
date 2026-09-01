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

## RabbitMQ

Includes settings required for connecting to Rabbit MQ system.

Connection settings:

| Setting                | Definition                                              | Example                          | Default             |
| ---------------------- | ------------------------------------------------------- | -------------------------------- | ------------------- |
| uri                    | Full connection uri, overrides host/port/credentials    | amqp://user:pass@host:5672/vhost | -                   |
| host                   | Broker host                                             | rabbit-preprod                   | localhost           |
| port                   | Broker port                                             | 5672                             | 5672, 5671 with tls |
| addresses              | Cluster nodes, comma separated, overrides host/port     | node1:5672,node2:5672            | -                   |
| username               | User                                                    | rierino                          | guest               |
| password               | Password                                                | -                                | guest               |
| vhost                  | Virtual host                                            | /preprod                         | /                   |
| client.id              | Connection name, also the consumer tag on input streams | core-consumer                    | prefix-UUID         |
| client.prefix          | Prefix for default client.id                            | core                             | System alias        |
| connect.wait.ms        | TCP connection timeout                                  | 10000                            | 2000                |
| handshake.timeout.ms   | AMQP handshake timeout                                  | 20000                            | 10000               |
| shutdown.timeout.ms    | Socket close timeout                                    | 5000                             | 10000               |
| channel.rpc.timeout.ms | Timeout for declares and other channel operations       | 30000                            | 10000               |
| heartbeat.sec          | Heartbeat interval, 0 disables                          | 60                               | 30                  |
| automatic.recovery     | Use client-side recovery instead of the runner's        | true                             | false               |
| topology.recovery      | Redeclare queues and bindings on client-side recovery   | false                            | true                |
| network.recovery.ms    | Client-side retry interval, needs automatic.recovery    | 10000                            | 5000                |

TLS settings:

| Setting                 | Definition                                                                        | Example                     | Default |
| ----------------------- | --------------------------------------------------------------------------------- | --------------------------- | ------- |
| ssl.enabled             | Enable TLS, also flips the default port to 5671                                   | true                        | false   |
| ssl.protocol            | TLS protocol version                                                              | TLSv1.3                     | TLSv1.2 |
| ssl.verify.hostname     | Verify the broker hostname against its certificate                                | false                       | true    |
| ssl.trust.all           | Skip certificate validation, encrypts but authenticates nothing, development only | true                        | false   |
| ssl.truststore.path     | Truststore holding the broker certificate, empty uses the JDK default             | /etc/rierino/truststore.jks | -       |
| ssl.truststore.password | Truststore password                                                               | -                           | -       |
| ssl.truststore.type     | Truststore format                                                                 | PKCS12                      | JKS     |
| ssl.keystore.path       | Client certificate keystore for mutual TLS                                        | /etc/rierino/client.p12     | -       |
| ssl.keystore.password   | Keystore password                                                                 | -                           | -       |
| ssl.keystore.type       | Keystore format                                                                   | PKCS12                      | JKS     |

## STAN

Includes settings required for connecting to a NATS Streaming (NATS) system.

| Setting         | Definition                   | Example | Default               |
| --------------- | ---------------------------- | ------- | --------------------- |
| nats.url        | NATS URL(s), comma separated | -       | nats://localhost:4222 |
| cluster.id      | STAN cluster id              | prod    | -                     |
| client.id       | STAN client id               | -       | prefix-UUID           |
| client.prefix   | Prefix for default client.id | core    | System alias          |
| connect.wait.ms | Connection timeout           | 10000   | 2000                  |

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

## LDAP

Includes settings required for connecting to an LDAP v3 directory (OpenLDAP, Active Directory, or any JNDI reachable server) for authentication handlers.

#### Connection

| Setting             | Definition                                                                | Example                   | Default |
| ------------------- | ------------------------------------------------------------------------- | ------------------------- | ------- |
| ldapURL             | Url endpoint for the directory, whitespace separated for failover         | ldaps://ldap.acme.com:636 | -       |
| ldapBaseDn          | Base distinguished name for all user and group searches                   | dc=acme,dc=com            | -       |
| userOrgUnit         | Organizational unit holding user entries, used when composing the user DN | people                    | users   |
| referral            | Referral handling mode as ignore, follow or throw                         | follow                    | ignore  |
| connectTimeoutInMs  | Tcp connect timeout in milliseconds                                       | 5000                      | 5000    |
| readTimeoutInMs     | Socket read timeout in milliseconds                                       | 10000                     | 10000   |
| searchTimeLimitInMs | Server side time limit applied to every search                            | 10000                     | 10000   |

#### Transport Security

| Setting                | Definition                                                                                                     | Example | Default |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- | ------- | ------- |
| startTLS               | Whether to connect anonymously and upgrade the channel with StartTLS before binding, ignored for ldaps:// urls | true    | false   |
| requireSecureTransport | Whether to fail startup when the connection is neither ldaps:// nor StartTLS enabled                           | true    | false   |

Plain `ldap://` is accepted so that deployments without TLS still work, but a simple bind over an unencrypted connection puts the user password on the wire in cleartext and is logged as a warning at startup. Use `requireSecureTransport` to turn that warning into a startup failure.

#### Bind and Identity

| Setting            | Definition                                                                                | Example                         | Default |
| ------------------ | ----------------------------------------------------------------------------------------- | ------------------------------- | ------- |
| adminBindDn        | Service account distinguished name used for admin reads and refresh time re-reads         | cn=svc,ou=system,dc=acme,dc=com | -       |
| adminBindPassword  | Password of the service account, required whenever adminBindDn is provided                | pass                            | -       |
| userIdAttribute    | Attribute carrying the login name, uid in OpenLDAP and sAMAccountName in Active Directory | sAMAccountName                  | uid     |
| securityAuthMethod | Bind method as simple, none or strong                                                     | simple                          | simple  |
| securityMechanism  | Sasl mechanism to use, required when securityAuthMethod is strong                         | DIGEST-MD5                      | -       |
| realm              | Sasl realm used with DIGEST-MD5, GSSAPI (kerberos) and similar mechanisms                 | ACME.COM                        | -       |
| provideRealm       | Whether to pass realm to the sasl layer, requires realm to be provided                    | true                            | false   |

`adminBindDn` is optional. Without it, login still works as the user's own authenticated context is used to read the entry, but user listing and lookup are rejected, and refresh falls back to the attribute snapshot carried inside the refresh token instead of re-reading the directory.

#### Attributes and Groups

| Setting               | Definition                                                                                           | Example          | Default |
| --------------------- | ---------------------------------------------------------------------------------------------------- | ---------------- | ------- |
| userAttributes        | Comma separated attributes to read from the user entry, all attributes when not provided             | cn,mail,memberOf | -       |
| groupSearchFilterName | Group membership attribute, member in Active Directory and memberUid in OpenLDAP                     | memberUid        | member  |
| searchGroupsBy        | Value matched against the membership attribute as userDn in Active Directory or username in OpenLDAP | username         | userDn  |
| groupNameAttribute    | Attribute holding the group name, collected into the roles claim                                     | cn               | cn      |
| listLimit             | Maximum number of entries returned by the list action                                                | 500              | 500     |

Attribute names are validated against `^[A-Za-z][A-Za-z0-9;.\-]*$` at startup to prevent filter and dn injection through configuration values.

Credential shaped attributes are dropped from every response and every token, whatever the directory returns: `userPassword`, `unicodePwd`, `ntPassword`, `lmPassword`, `dbcsPwd`, `sambaNTPassword`, `sambaLMPassword`, `sambaPasswordHistory`, `authPassword`, `pwdHistory`, `krbPrincipalKey`, `userPKCS12`, `msDS-KeyCredentialLink` and `supplementalCredentials`. Binary values such as `objectSid` or `jpegPhoto` are dropped as well. Multi valued attributes keep all of their values and are returned as arrays.

#### Connection Pool

| Setting                 | Definition                                                        | Example   | Default |
| ----------------------- | ----------------------------------------------------------------- | --------- | ------- |
| connectionPool.enabled  | Whether to pool admin connections, disabled when startTLS is used | false     | true    |
| connectionPool.timeout  | Idle timeout in milliseconds for pooled connections               | 300000    | -       |
| connectionPool.maxsize  | Maximum number of pooled connections                              | 20        | -       |
| connectionPool.prefsize | Preferred pool size                                               | 5         | -       |
| connectionPool.protocol | Protocols to pool                                                 | plain ssl | -       |

Admin lookups go through the JNDI connection pool as `DirContext` is not thread safe. User binds are never pooled, since pooling per user principal would grow the pool with the user count.

#### Tokens

| Setting                   | Definition                                                                                 | Example           | Default |
| ------------------------- | ------------------------------------------------------------------------------------------ | ----------------- | ------- |
| tokenSecretKey            | Base64 encoded signing key, must decode to at least 64 bytes for HS512                     | c2VjcmV0...       | -       |
| requireSignedTokens       | Whether to fail startup when tokenSecretKey is missing, instead of issuing unsigned tokens | true              | false   |
| tokenLifetimeInSec        | Access and id token lifetime in seconds                                                    | 3600              | 3600    |
| refreshTokenLifetimeInSec | Refresh token lifetime in seconds                                                          | 86400             | 86400   |
| accessTokenClaims         | Comma separated user attributes to copy into the access token                              | mail,department   | -       |
| idToken                   | Whether to return id\_token when resolving tokens                                          | true              | false   |
| idTokenClaims             | Comma separated user attributes to copy into the id token                                  | cn,mail,givenName | -       |

When `tokenSecretKey` is not provided, tokens are issued unsigned (`alg=none`) and any caller reaching the gateway can forge claims. This is intended for the default environment only; provide a secret or set `requireSignedTokens` everywhere else.

This system requires the following dependency added to deployment contents:

```gradle
implementation (group:'com.rierino.custom', name: 'ldap', version:"${rierinoVersion}")
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
