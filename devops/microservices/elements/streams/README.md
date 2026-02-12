---
description: >-
  Streams are used for defining input and output data flows (such as Events,
  Pulses, Journals) between different runners.
---

# Streams

Streams configure input / output relations between different runners via different communication channels (such as Kafka topics, API routes). It is optional to define and add streams explicitly to runners except for event streams (i.e. Kafka), which require active set-up of connections during runner start-up.

Most stream definitions only require a system name mapping, but it is also possible to add more specialized configurations (such as offset reset details for Kafka consumers) when creating stream elements.&#x20;

## Stream Partitions

A key concept in using streams, especially with Kafka or similar systems is stream partitioning. Partitioning splits a stream into a set of sub-streams, each with an assigned unique partition id, responsible for transporting events only related to that partition id. Partitioning of event streams provides efficient scalability for the services consuming such streams.

Stream partition assignment and management varies between the event runner type used. For example, the Samza event runner automatically distributes partitions to runner instances and is able to reassign partitions in case of instance failures, if its configured to use a service coordinator (e.g. Zookeeper). On the other hand, Spring event runners typically only consume broadcasted event streams (such as commands, pulses, journals), as they primarily use REST / socket channels for communications.

It is possible to utilize partitions in various use cases with Rierino:

* **CDC:** Change data records (i.e. pulses, journals) for large data sets are typically partitioned, based on partitionId field of the aggregate record. This allows synchronization of distributed runners and state managers, always receiving the updates for their records from the same stream partition and in the right order.
* **Requests:** All requests from the API gateway to event stream based runners are sent in a partitioned manner, where each API gateway instance is assigned a partition id (e.g. stateful set number if on kubernetes). All requests coming from these gateways are assigned requests ids in the same partition as their assigned partition id (e.g. their request ids ending with the partition id) and they receive their responses on the same partition of their response event streams.
* **Saga Flows:** Every step of a saga flow allows selection of the partitioning method (i.e. key strategy), to ensure that the runner responsible for that step receives the request on the right partition (e.g. if the runner is responsible for records of a specific partition only). Step results are always returned to the saga runner on the same partition (i.e. partition of the request id), so that the same runner can manage each API call end to end.
* **Stateful Runners:** In case a runner is managing a local state or cache (e.g. caching calculation results for a customer), partitions allow ensuring that the same runner which holds local cache receives upcoming requests of the same nature (e.g. from the same customer).

A number of settings are shared across all stream types:

| Setting                | Definition                                                                  | Example        | Default |
| ---------------------- | --------------------------------------------------------------------------- | -------------- | ------- |
| system                 | Name of the system this stream belongs to                                   | kafka\_default | -       |
| parameter.ignoreOffset | Whether stream offset should be used or not for checks (i.e. is sequential) | true           | false   |
| meta.\[field].default  | Default event metadata field values for requests received on this topic     | action=Get     | -       |
| meta.\[field].override | Final event metadata field values for requests received on this topic       | domain=product | -       |

{% file src="../../../../.gitbook/assets/stream-saga-0001.json" %}
Example Stream Definition (Can be Imported on Element Screen)
{% endfile %}

It is possible to define any number and type of new streams with specialized configurations for different event runners.
