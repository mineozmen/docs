---
description: >-
  Maps input/output connections to a topic on a specific Kafka cluster,
  typically used for async operations using a Samza event runner
---

# Kafka Topic

Includes settings required for consuming or producing to a Kafka topic.

| Setting                        | Definition                                                    | Example                       | Default |
| ------------------------------ | ------------------------------------------------------------- | ----------------------------- | ------- |
| streams.$alias.\*              | Samza specific settings for the Kafka topic                   | samza.offset.default=upcoming | -       |
| parameter.broadcast            | Whether topic should be broadcasted or consumed as partitions | true                          | false   |
| parameter.consumer.\[property] | Kafka consumer properties                                     | auto.offset.reset=earliest    | -       |
| parameter.producer.\[property] | Kafka producer properties                                     | acks=0                        | -       |
| parameter.output.backupSystem  | Name of backup system to use if this stream fails             | kafka\_backup                 | -       |
| parameter.output.backupStream  | Name of backup stream to use if this stream fails             | journal\_backup               | -       |
