---
description: >-
  These runners are based on Samza library, receiving requests from Samza
  consumers (e.g. Kafka) and returning results on Samza producers (e.g.
  Kinesis).
---

# Samza Runners

All Samza configurations are applicable, and can be passed on to these runners using global runner elements.

## Sync Samza Event Runner

SyncSamzaEventRunner(_com.rierino.runner.samza.SyncSamzaEventRunner_) guarantees message order, with blocking properties for each message received.

Name of the input topic is used as the stream and the messages are used as the full event contents.

## Async Samza Event Runner

AsyncSamzaEventRunner(_com.rierino.runner.samza.AsyncSamzaEventRunner_) does not guarantee message order, as it provides certain level of parallelization.

Name of the input topic is used as the stream and the messages are used as the full event contents.

{% embed url="https://samza.apache.org" %}
Samza Page
{% endembed %}

{% hint style="info" %}
Samza runners can also communicate with Restful event runners (e.g. as a step inside a Saga flow), which results in synchronous event exchange with the remote runner.
{% endhint %}
