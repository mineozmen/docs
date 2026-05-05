---
description: >-
  Flow handlers are included in all Rierino platform versions, providing ability
  to combine and orchestrate various actions
---

# Flow Handlers

Flow handlers coordinate orchestration logic across events, steps, roles, and streams.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Roles and commands

Use [Flow Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/) for action-level saga step fields and parameters.

### Orchestrate Saga

Class: `com.rierino.handler.saga.SagaEventHandler`

Coordinates end-to-end saga execution across local or distributed runners.

#### Handler parameters

| Parameter            | Definition                                                                             | Example            | Default                      |
| -------------------- | -------------------------------------------------------------------------------------- | ------------------ | ---------------------------- |
| `saga.state`         | State manager keeping saga definitions                                                 | `customSaga`       | `saga`                       |
| `response.stream`    | Stream used for saga completion or failure results                                     | `response_product` | `response`                   |
| `saga.stream`        | Stream used to track saga step results                                                 | `saga_product`     | `saga`                       |
| `saga.system`        | System used to track saga step results                                                 | `kafka_default`    | -                            |
| `event.system`       | Default system used to post requests to event runners                                  | `kafka_default`    | -                            |
| `saga.timeout`       | Timeout in milliseconds for saga calls                                                 | `5000`             | `10000`                      |
| `timer.pool`         | Thread count for scheduled sagas                                                       | `10`               | -                            |
| `roles.variable`     | Variable name used for allowed role matching                                           | `userRoles`        | `user.roles` path in payload |
| `offset.state`       | State manager used for storing offsets for "self waiting" sagas                        | `cdc_offset`       | -                            |
| `offset.partitioned` | Whether offsets are stored as partitioned (i.e. multiple replicas running as saga CDC) | `true`             | `false`                      |

Action details: [Orchestrate Saga](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/orchestrate-saga.md)

### Execute Predefined Actions

Class: `com.rierino.handler.ActionEventHandler`

Executes reusable predefined actions as part of a saga flow.

#### Handler parameters

| Parameter      | Definition                           | Example         | Default  |
| -------------- | ------------------------------------ | --------------- | -------- |
| `action.state` | State manager keeping action records | `custom_action` | `action` |

Action details: [Execute Predefined Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/execute-predefined-actions.md)

### Loop Each Entry

Class: `com.rierino.handler.ForEachEventHandler`

Repeats an action for each entry in a payload array.

#### Handler parameters

This handler requires no parameters.

Action details: [Loop Each Entry](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/loop-each-entry.md)

### Run Multiple Steps

Class: `com.rierino.handler.CompositeEventHandler`

Runs multiple actions in sequence and passes the output of each step to the next.

#### Handler parameters

This handler requires no parameters.

Action details: [Run Multiple Steps](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/run-multiple-steps.md)

### Buffer Payloads

Class: `com.rierino.handler.BufferEventHandler`

Buffers multiple incoming payloads and emits them as a single batch.

#### Handler parameters

This handler requires no parameters.

Action details: [Buffer Payloads](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/buffer-payloads.md)

### Merge Parallel Steps

Class: `com.rierino.handler.MergeEventHandler`

Waits for parallel branches to complete, then merges their results and continues the flow.

#### Handler parameters

| Parameter              | Definition                                           | Example                                   | Default  |
| ---------------------- | ---------------------------------------------------- | ----------------------------------------- | -------- |
| `merge.locker`         | Locker class used for thread-safe merge coordination | `com.rierino.state.lock.RedisStateLocker` | -        |
| `merge.locker.timeout` | Milliseconds to wait for acquiring a lock            | `1000`                                    | -        |
| `merge.action`         | What to do with the merge state record on completion | `delete`                                  | `update` |
| `merge.locker.expiry`  | Lock TTL in milliseconds                             | `10000`                                   | `-1`     |

This handler also supports timeout-oriented task controls for merge activities.

Action details: [Merge Parallel Steps](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/merge-parallel-steps.md)

### Log Event

Class: `com.rierino.handler.LogEventHandler`

Logs event or payload contents for debugging and tracing.

#### Handler parameters

This handler requires no parameters.

Action details: [Log Event](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/log-event.md)

### Send Event

Class: `com.rierino.handler.SendEventHandler`

Sends an event to another stream.

#### Handler parameters

This handler requires no parameters.

Action details: [Send Event](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/send-event.md)

### Validate Event

Class: `com.rierino.handler.ValidationEventHandler`

Validates event input using a configured validator.

#### Handler parameters

| Parameter     | Definition                            | Example                                                  | Default |
| ------------- | ------------------------------------- | -------------------------------------------------------- | ------- |
| `validator`   | Fully qualified validator class name  | `com.rierino.handler.util.validator.JsonSchemaValidator` | -       |
| `validator.*` | Additional validator class parameters | `state=schema`                                           | -       |

#### Built-in validators

* `JMESValidator`
* `JsonSchemaValidator`

Action details: [Validate Event](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/validate-event.md)

### Transform Event

Class: `com.rierino.handler.TransformEventHandler`

Applies a transformation class to an incoming event payload.

#### Handler parameters

This handler requires no parameters.

Action details: [Transform Event](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/transform-event.md)

### Lock & Unlock

Class: `com.rierino.handler.LockEventHandler`

Creates, checks, and releases locks for a given domain and id.

#### Handler parameters

| Parameter | Definition                                          | Example                                   | Default |
| --------- | --------------------------------------------------- | ----------------------------------------- | ------- |
| `locker`  | `StateLocker` class used to issue and release locks | `com.rierino.state.lock.RedisStateLocker` | -       |
| `timeout` | Default milliseconds to wait for acquiring a lock   | `1000`                                    | -       |
| `expiry`  | Default TTL of a lock in milliseconds               | `60000`                                   | `-1`    |

Action details: [Lock & Unlock](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/lock-and-unlock.md)

### Perform DB Transaction

Class: `com.rierino.handler.JDBCTransactionEventHandler`

Executes multiple JDBC state changes as a single transaction.

#### Handler parameters

| Parameter             | Definition                                           | Example | Default |
| --------------------- | ---------------------------------------------------- | ------- | ------- |
| `transaction.timeout` | Seconds before a transaction is considered timed out | `60`    | `20`    |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.processors', name: 'jdbc', version:"${rierinoVersion}")
```

Action details: [Perform DB Transaction](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/perform-db-transaction.md)

### Trigger Runner Command

Class: `com.rierino.handler.CommandEventHandler`

Executes a runner command from an event payload. This is mainly used for development or simple single-runner setups.

#### Handler parameters

This handler requires no parameters.

Action details: [Trigger Runner Command](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/trigger-runner-command.md)

### Do Nothing

Class: `com.rierino.handler.NoopEventHandler`

Passes event or role data through without modification.

#### Handler parameters

This handler requires no parameters.

#### Roles

* `noop`

Action details: [Do Nothing](../../../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/do-nothing.md)

### Modify Role Data

Class: `com.rierino.handler.JMESDataRoleHandler`

Transforms and routes role data using a JMESPath pattern.

#### Handler parameters

| Parameter | Definition                                            | Example                                                                                                                        | Default |
| --------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------- |
| `pattern` | JMESPath pattern with data, route, and filter details | `[{ "data":@, "route":{"system":'kafka',"stream":'customer',"key":payload.customer.id}, "filter":payload.customer.id!=null }]` | -       |

#### Roles

* `processData`

### Enrich Role Data

Class: `com.rierino.handler.ConversionRoleHandler`

Enriches or converts pulse and journal records using state or query data.

#### Handler parameters

| Parameter        | Definition                                  | Example           | Default  |
| ---------------- | ------------------------------------------- | ----------------- | -------- |
| `source.state`   | State manager used to enrich data           | `product`         | -        |
| `output.journal` | Stream used to send outputs                 | `variant_journal` | -        |
| `trigger.state`  | State manager whose changes trigger outputs | `product_local`   | -        |
| `query.state`    | State manager keeping query records         | `query`           | -        |
| `query.id`       | Query id used for enrichment                | `variant_query`   | -        |
| `buffer`         | Number of records buffered before sending   | `100`             | -        |
| `reload`         | Whether handler allows reload from source   | `false`           | `true`   |
| `journalAction`  | Journal action used for source feeds        | `CREATE`          | `UPSERT` |

#### Roles

* `conversionPulse`
* `conversionJournal`

#### Commands

* `FULLRELOAD`
* `OFFSETRELOAD`
* `IDRELOAD`

### Convert Pulse to Journal

Class: `com.rierino.handler.CDCRoleHandler`

Converts CDC pulse records into journal entries and enriches them from state data.

#### Handler parameters

| Parameter       | Definition                                                 | Example   | Default |
| --------------- | ---------------------------------------------------------- | --------- | ------- |
| `cdc.state`     | State manager used to produce journal data                 | `product` | -       |
| `allowExternal` | Allow externally originated pulses without journal records | `true`    | `false` |

#### Roles

* `produceJournal`
