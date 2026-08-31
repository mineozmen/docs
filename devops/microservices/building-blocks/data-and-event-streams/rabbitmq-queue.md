---
description: Maps input/output connections to a queue on a specific RabbitMQ
---

# RabbitMQ Queue

Includes settings required for consuming from or producing to a RabbitMQ queue.

Publishers send to an _exchange_ with a _routing key_; the exchange copies the message into every queue whose binding matches. Consumers read from a _queue_. Leaving `parameter.exchange` empty selects the default exchange, where the routing key is the queue name and a message reaches exactly one queue.

## Input Streams

#### Queue and bindings

Applied on connect when `parameter.declare` is true. Both streams must declare a queue with identical settings, otherwise the broker rejects the second declare with `PRECONDITION_FAILED`; the simplest arrangement is to let the input stream own the topology and set `parameter.declare` to false on the output stream.

| Setting                           | Definition                                                      | Example                                       | Default      |
| --------------------------------- | --------------------------------------------------------------- | --------------------------------------------- | ------------ |
| parameter.queue                   | Queue to consume                                                | ecp.product.price.preprod                     | Stream alias |
| parameter.queue.type              | classic \| quorum                                               | quorum                                        | classic      |
| parameter.exchange                | Exchange to bind the queue to, empty binds nothing              | ecp.product.price                             | -            |
| parameter.exchange.type           | topic \| direct \| fanout \| headers                            | fanout                                        | topic        |
| parameter.routing.key             | Binding key                                                     | price.delta                                   | Stream alias |
| parameter.routing.keys            | Several binding keys, comma separated, overrides routing.key    | price.delta,price.rollback                    | -            |
| parameter.declare                 | Declare and bind on connect                                     | true                                          | false        |
| parameter.declare.passive         | Only verify existence, never create                             | true                                          | false        |
| parameter.durable                 | Durable queue and exchange, survives broker restart             | false                                         | true         |
| parameter.queue.auto.delete       | Delete the queue when the last consumer disconnects             | true                                          | false        |
| parameter.queue.exclusive         | Queue is private to this connection                             | true                                          | false        |
| parameter.exchange.auto.delete    | Delete the exchange when the last binding is removed            | true                                          | false        |
| parameter.exchange.internal       | Exchange accepts messages only from other exchanges             | true                                          | false        |
| parameter.dead.letter.exchange    | Where rejected, expired or overflowing messages go              | ecp.product.price.dlx                         | -            |
| parameter.dead.letter.routing.key | Routing key used when dead lettering                            | price.delta.failed                            | -            |
| parameter.message.ttl.ms          | Discard messages older than this                                | 3600000                                       | -            |
| parameter.expires.ms              | Delete the queue after this long unused                         | 86400000                                      | -            |
| parameter.max.length              | Max messages held before overflow applies                       | 100000                                        | -            |
| parameter.max.length.bytes        | Max bytes held before overflow applies                          | 1073741824                                    | -            |
| parameter.overflow                | drop-head \| reject-publish \| reject-publish-dlx               | reject-publish                                | drop-head    |
| parameter.max.priority            | Enable priority queueing up to this level, classic only         | 10                                            | -            |
| parameter.delivery.limit          | Dead letter a message after this many redeliveries, quorum only | 5                                             | -            |
| parameter.queue.arg.\<name>       | Any other raw queue argument                                    | parameter.queue.arg.x-consumer-timeout=600000 | -            |
| parameter.exchange.arg.\<name>    | Any other raw exchange argument                                 | -                                             | -            |
| parameter.binding.arg.\<name>     | Binding argument, used by headers exchanges                     | parameter.binding.arg.x-match=all             | -            |

#### Consumer

| Setting                        | Definition                                                                        | Example | Default |
| ------------------------------ | --------------------------------------------------------------------------------- | ------- | ------- |
| parameter.prefetch             | Max unacked messages in flight, the main back-pressure control                    | 100     | 1024    |
| parameter.max.inflight         | Alias of prefetch                                                                 | 100     | 1024    |
| parameter.prefetch.global      | Apply prefetch across the channel instead of per consumer                         | true    | false   |
| parameter.single.active        | Only one consumer receives messages at a time, others stand by and order is kept  | true    | false   |
| parameter.consumer.priority    | Higher priority consumers are served first while they have capacity               | 10      | -       |
| parameter.consumer.exclusive   | No other consumer may attach to the queue                                         | true    | false   |
| parameter.consumer.arg.\<name> | Any other raw consumer argument                                                   | -       | -       |
| parameter.manual.ack           | Ack explicitly once the record is buffered, false acks on delivery and risks loss | false   | true    |
| parameter.requeue.on.error     | Requeue unparseable messages instead of dead lettering or dropping them           | true    | false   |
| parameter.payload.format       | json \| string                                                                    | string  | json    |

## Output Streams

#### Exchange and bindings

| Setting                        | Definition                                                         | Example                    | Default                         |
| ------------------------------ | ------------------------------------------------------------------ | -------------------------- | ------------------------------- |
| parameter.exchange             | Exchange to publish to, empty uses the default exchange            | ecp.product.price          | -                               |
| parameter.exchange.type        | topic \| direct \| fanout \| headers                               | fanout                     | topic                           |
| parameter.routing.key          | Routing key stamped on every message                               | price.delta                | Stream alias                    |
| parameter.queue                | Queue to declare and bind so published messages have a destination | ecp.product.price.preprod  | Routing key on default exchange |
| parameter.declare              | Declare and bind on connect                                        | true                       | false                           |
| parameter.declare.passive      | Only verify existence, never create                                | true                       | false                           |
| parameter.durable              | Durable exchange and queue, survives broker restart                | false                      | true                            |
| parameter.exchange.auto.delete | Delete the exchange when the last binding is removed               | true                       | false                           |
| parameter.exchange.internal    | Exchange accepts messages only from other exchanges                | true                       | false                           |
| parameter.alternate.exchange   | Where messages that match no binding go instead of being discarded | ecp.product.price.unrouted | -                               |
| parameter.queue.type           | classic \| quorum                                                  | quorum                     | classic                         |
| parameter.queue.auto.delete    | Delete the queue when the last consumer disconnects                | true                       | false                           |
| parameter.queue.arg.\<name>    | Any other raw queue argument                                       | -                          | -                               |
| parameter.exchange.arg.\<name> | Any other raw exchange argument                                    | -                          | -                               |
| parameter.binding.arg.\<name>  | Binding argument, used by headers exchanges                        | -                          | -                               |

#### Publishing

| Setting                      | Definition                                                                           | Example                     | Default          |
| ---------------------------- | ------------------------------------------------------------------------------------ | --------------------------- | ---------------- |
| parameter.confirm.mode       | sync \| batch \| off, sync waits for a broker confirm on every publish               | batch                       | sync             |
| parameter.confirm.batch.size | Messages published per confirm in batch mode                                         | 500                         | 100              |
| parameter.confirm.wait.ms    | Confirm timeout                                                                      | 30000                       | 10000            |
| parameter.publisher.confirms | Legacy switch, false is equivalent to confirm.mode=off                               | false                       | true             |
| parameter.mandatory          | Fail the publish when the message matches no binding instead of dropping it silently | true                        | false            |
| parameter.persistent         | Write messages to disk, needs a durable queue to survive a restart                   | false                       | true             |
| parameter.content.type       | Content type header                                                                  | text/plain                  | application/json |
| parameter.priority           | Message priority, needs max.priority on the queue                                    | 5                           | -                |
| parameter.expiration.ms      | Per-message TTL                                                                      | 60000                       | -                |
| parameter.message.id         | Stamp a unique id on every message, for consumer-side deduplication                  | true                        | false            |
| parameter.timestamp          | Stamp the publish time, read back as event time by input streams                     | false                       | true             |
| parameter.correlation.id     | Correlation id stamped on every message                                              | -                           | -                |
| parameter.reply.to           | Reply queue stamped on every message                                                 | -                           | -                |
| parameter.message.type       | Message type stamped on every message                                                | price.delta                 | -                |
| parameter.app.id             | Application id stamped on every message                                              | ecp-core                    | -                |
| parameter.header.\<name>     | Static header stamped on every message                                               | parameter.header.source=ecp | -                |

## Notes

**A queue must exist before a message is published to it.** Exchanges hold nothing; a message that matches no binding is discarded with no error. Set `parameter.mandatory` on the output stream to turn that silent drop into a failure.

**Consuming removes the message.** An acked message is deleted, so a new consumer sees only what arrives after it connects. To let several consumers each receive every message, bind a separate queue per consumer to a fanout or topic exchange rather than sharing one queue.

**Batch confirms report success before the broker has confirmed.** A crash mid-batch loses that window, so keep `parameter.confirm.mode` at sync unless throughput demands otherwise.
