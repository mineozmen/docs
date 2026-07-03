---
description: >-
  Reference for configuring runner I/O, partition selection, and runner-level
  settings such as rebuild, commit, logging, and handler access.
---

# Managing Runner Settings

<figure><img src="../../../.gitbook/assets/image (122).png" alt=""><figcaption><p>Runner I/O</p></figcaption></figure>

Each runner specifies the list of streams it consumes and produces to via its I/O settings:

* **Input Streams:** List of streams the runner should consume from.
* **Output Streams:** List of streams the runner should produce to.
* **Partition Stream:** Name of the stream which should be used for selecting runner partition, unless the runner is assigned a static partition.

These settings are mainly required for event streams (e.g. Kafka), and are not critical for REST or websocket based I/O operations. When a runner starts, it connects to the event streams listed in these configurations, which can not be modified while the runner is running (as it would affect partition assignments of runners using the same streams).

Partition stream is important when a runner is listening to multiple event streams, some of which may be used in broadcasting mode. Selecting a specific stream to assign partition to the runner aligns state partition selection strategies with stream feeds, so, when required you should be picking the main data stream for this entry.

<figure><img src="../../../.gitbook/assets/image (123).png" alt=""><figcaption><p>Runner Settings</p></figcaption></figure>

The final tab of runner definition screen is the settings tab, which allows entry of runner-level settings when required (such as rebuildMs).&#x20;

A typical use case for this tab is for assigning default stream to RPC runners (e.g. defaultStream = saga), which allows making requests directly to saga paths, without specifying the stream name.

All runners share the following settings that can be also configured from this screen:

| Parameter     | Definition                                                                                                        | Example   |
| ------------- | ----------------------------------------------------------------------------------------------------------------- | --------- |
| rebuildMs     | Milliseconds period to check whether runner is updated and automatically rebuild                                  | 30000     |
| commitMs[^1]  | Milliseconds period to commit current records / resume tokens                                                     | 10000     |
| logDetail     | When to perform detailed logging for the runner (overrides log\_level setting, with options as "never", "always") | always    |
| allowHandlers | Whether the runner should allow use of handlers not mapped on to individual streams for event calls               | crud-0001 |

## Partitioned Runners

For use cases such as consuming message queues, instances of a single runner may need to process only selected partition(s) of their input streams. Some runner types like Samza event runner can automatically assign partitions to runner instances, for others, it is possible to assign individual partitions to individual instances through configuration:

* **Runner Partition:** A runner instance can be assigned to a specific partition (which is used by stream managers such as Kafka), by setting "runner.partition" value in application.properties configuration or RUNNER\_PARTITION environment variable (which has higher priority).
* **Runner Partition Modulo:** When assigning partitions, runners also need to know the total number of partitions (or modulo for assignment of partitions to IDs), which can be assigned by setting "runner.partitionMod" value in application.properties configuration or RUNNER\_PARTITION\_MOD environment variable (which has higher priority).

[^1]: Should not be used with Samza runners since Samza has its custom implementation of checkpoints
