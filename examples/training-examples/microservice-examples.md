---
description: These examples can be viewed from Runner and Deployment screens in Devops app
---

# Microservice Examples

## Runners

### Train CRUD

Train CRUD is a basic HTTP CRUD microservice. It exposes standard `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` operations on a single MongoDB collection named `dummy` in the `master` database. It is the fastest way to see “data in / data out” behavior across gateway → runner → state manager.

It supports the generic CRUD operation set and parameters described in [CRUD event runner](../../devops/microservices/runners/deploying-runners/spring-runners.md#crud-event-runner). Use that reference when you want to understand supported query params, request bodies, and how partial updates work.

This runner is mapped to the `CRUD Train` gateway system and the `Train CRUD` gateway channel. You can reach it at `[{API_SERVER}]/api/request/train_crud/[PATH]`.

<figure><img src="../../.gitbook/assets/image (141).png" alt=""><figcaption><p>CRUD Runner</p></figcaption></figure>

A typical CRUD runner has three core handlers:

* **Read**: resolves `GET` operations.
* **Write**: handles create/update/delete operations.
* **Condition**: enables simple branching and checks during CRUD calls.

You can tighten access by limiting which states are readable or writable. You can also route certain operations through sagas when you need custom logic. This is the usual upgrade path from “generic CRUD” to “domain-aware API”.

{% hint style="info" %}
To create a new fully functional CRUD endpoint, all you need to do is add a `STATE` element named "Generic Master w/ ID", give it an alias and click on save for this runner. Within 30 seconds (rebuild window), your endpoints will be available on /train\_crud/\[alias].
{% endhint %}

To learn how to extend this CRUD runner, please watch the following video:

{% embed url="https://www.youtube.com/watch?v=iW_klLfvJuk" %}

### Train RPC

Train RPC is a general-purpose RPC runner that executes sagas. It also ships with a broad set of handlers for “do some work and return a response”. Most training sagas are executed through this runner, including the ones listed under [API Flow Examples](api-flow-examples.md).

This runner is mapped to the `RPC Train` gateway system and the `Train RPC` gateway channel. You can reach it at `[{API_SERVER}]/api/request/train_rpc/[PATH]`.

<figure><img src="../../.gitbook/assets/image (140).png" alt=""><figcaption><p>RPC Runner</p></figcaption></figure>

A minimal RPC runner needs two things:

* An **input stream** (in training, `request_train`) to receive requests.
* A **saga handler** to load and execute the selected saga flow.

Everything else is optional. You add elements based on what the sagas are allowed to do. That keeps permissions explicit and makes runners easier to reason about.

This training runner includes the following elements:

* **Read + Write handlers** for basic state operations on the `dummy` collection in `master` (used by many sagas).
* **Query handler** to execute stored queries against `master`.
* **REST handler** to call an external system (`datausa`). It also includes predefined `Action` records for reusable call configs.
* **Script / Handlebars template / OpenHFT handlers** for dynamic logic. Code is stored in `handler_code` on the `devops` database. It is cached in-memory for fast reloads.
* **Rule handler** to evaluate training rules stored in `rule_training` on the `config` database. These are typically used directly, without a cache.

To learn how to extend this RPC runner, please watch the following video:

{% embed url="https://www.youtube.com/watch?v=qIJNBnN6v8U" %}

### Train CDC

Train CDC is a change-data-capture runner. It listens to changes on the `dummy` collection in the `master` MongoDB database. On each change, it triggers the `/On_dummy` saga to process the event.

This runner is not called through the API gateway. Its “requests” are CDC records emitted by the underlying database stream.

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption><p>CDC Runner</p></figcaption></figure>

A CDC runner usually includes:

* One or more **CDC streams** (here, the `dummy` collection change stream).
* An **offset/checkpoint mechanism** so it can resume after restarts.

Spring-based runners often store offsets in a state manager. Samza-based runners typically rely on their own checkpointing model. After you have CDC events, you can route them in multiple ways. You can trigger sagas (like this example). You can also enrich the change record and publish it downstream, like to a Kafka topic.

To learn how to extend this CDC runner, please watch the following video:

{% embed url="https://www.youtube.com/watch?v=MCvRGhUhijg" %}

## Deployments

### Training Core

Training Core is the deployment that bundles the training runners together. It is configured for the `admin-backend` namespace. This deployment is meant for learning, demos, and local validation. It gives you a complete working baseline without building your own runners first.

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption><p>Deployment Runner List</p></figcaption></figure>

{% hint style="info" %}
Training Core is typically installed automatically with the Core platform. You usually do not need to deploy it manually.
{% endhint %}
