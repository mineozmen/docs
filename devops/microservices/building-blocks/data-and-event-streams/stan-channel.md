---
description: Maps input/output connections to a channel on a specific STAN cluster
---

# STAN Channel

Includes settings required for consuming or producing to a NATS Streaming (STAN) channel.

| Setting                       | Definition                                                   | Example    | Default                |
| ----------------------------- | ------------------------------------------------------------ | ---------- | ---------------------- |
| parameter.system              | Alias of STAN system to connect to                           | stan\_prod | -                      |
| parameter.subject             | Channel/subject to consume                                   | command    | Stream alias           |
| parameter.durable.name        | Durable subscription name                                    | -          | -                      |
| parameter.queue               | Queue group name                                             | -          | -                      |
| parameter.start.position      | new-only \| first \| last-received \| sequence \| time-delta | -          | sequence (if provided) |
| parameter.start.sequence      | Sequence to start at (used when start.position=sequence)     | -          | -                      |
| parameter.start.delta.seconds | Seconds to look back (used when start.position=time-delta)   | -          | 0                      |
| parameter.ack.wait.ms         | Server redelivery timeout                                    | -          | 30000                  |
| parameter.max.inflight        | Max unacked messages in flight                               | -          | 1024                   |
| parameter.manual.ack          | Manually ack on poll                                         | -          | true                   |
| parameter.payload.format      | json \| string                                               | -          | json                   |
