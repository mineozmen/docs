---
description: These examples can be viewed from Runner and Deployment screens in Devops app
---

# Microservice Examples

## Runners

### Train CRUD

This CRUD event runner provides read/write access to a single MongoDB collection named 'dummy' on 'master' database, through /dummy GET, POST, PUT, PATCH, DELETE endpoints. Full list of operations and parameters that can be used with this runner can be found [here](../../devops/microservices/runners/deploying-runners/spring-runners.md#crud-event-runner).

This runner is mapped to "CRUD Train" gateway system and "Train CRUD" gateway channel, accessible through \[API\_SERVER]/api/request/train\_crud/\[PATH].

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption><p>CRUD Runner</p></figcaption></figure>

A standard CRUD event runner includes three handlers: read, write, and condition. These handlers enable operations on states added to the runner. Advanced settings are available, such as restricting read/write access to specific states or using saga flows for custom logic in CRUD operations. More details are provided in the subsequent DevOps sections.

### Train RPC

This RPC event runner provides read, write, query, secret and rest handlers as well as dynamic code execution capabilities while orchestating sagas listed in [API flow examples](api-flow-examples.md).

This runner is mapped to "RPC Train" gateway system and "Train RPC" gateway channel, accessible through \[API\_SERVER]/api/request/train\_rpc/\[PATH] URL.

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption><p>RPC Runner</p></figcaption></figure>

A typical RPC event runner requires an input stream (request\_train in training example) and a saga handler at a minimum to grant the runner rights to execute saga flows. All other elements are added based on the intended capabilities and access rights for the runner.&#x20;

The train RPC runner includes the following elements as an example:

* read, write handlers to perform basic CRUD operations in saga flows on 'dummy' collection on 'master' MongoDB database
* query handler to run queries on 'master' MongoDB database
* rest handler to perform API calls on 'datausa' external system, with predefined 'action' definitions as well
* script, template\_hb and openhft handlers to execute custom scripts and code, which is stored in 'handler\_code' collection on 'devops' MongoDB database and cached locally on an in-memory Map for fast access
* rule handler to perform rule calculations, which is stored in 'rule\_training' collection on 'config' MongoDB database and used directly without a cache &#x20;

### Train CDC

This CDC event runner listens to changes on the 'dummy' collection of 'master' database in MongoDB and triggers /On\_dummy saga flow to process these changes.&#x20;

This runner is not accessible through APIs and only uses CDC triggers as requests.

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption><p>CDC Runner</p></figcaption></figure>

A typical CDC event runner includes one or more CDC streams (such as the 'dummy' collection change stream on 'master' MongoDB database in training example), as well as a mechanism for storing and retrieving offsets to allow continuation of CDC flows on service restart. For Spring based runners, a state manager is typically used, whereas Samza based runners have their own checkpointing configurations. A CDC stream can be linked to various roles, such as triggering any saga flow as in the training example, or publishing enriched CDC records on a Kafka topic.

## Deployments

### Training Core

This deployment includes all event runners and is configured for deployment in admin-backend namespace.&#x20;

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption><p>Deployment Runner List</p></figcaption></figure>

{% hint style="info" %}
The training core deployment is typically installed automatically with the core platform, so it does not require additional deployment actions.
{% endhint %}
