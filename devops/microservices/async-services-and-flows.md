---
description: >-
  Choose the right async pattern in Rierino based on trigger type, recovery
  model, and operational needs.
---

# Async Services and Flows

Rierino supports several async patterns. Each solves a different problem.

Pick the pattern by answering two questions:

1. What starts the work?
2. Who is responsible for recovery?

{% hint style="info" %}
Most async patterns are at-least-once in practice. Design handlers and sagas to be idempotent.
{% endhint %}

### 1. Fire & Forget sagas

Fire & Forget runs a saga asynchronously and returns early.

You can enable it in three places:

* In the saga definition with **Fire & Forget**
* On an API request with the `X-Async` header
* On nested saga calls with `fireForget=true`

#### How it behaves

At saga level, **Fire & Forget** always returns immediately.

If fire & forget state is configured, Rierino stores status and result data.

The response includes a `reference` value. Use it to query progress later.

At request level, `X-Async` supports two modes:

* `gateway` returns from the gateway immediately
* `backend` forwards an async saga call to the backend

`gateway` returns a request ID even if the backend is unavailable.

`backend` returns a backend process ID after forwarding the request.

#### Best fit

* Use this for short-running background work.
* Use it when the caller can track status and retry safely.
* Single-step or low-fanout sagas are the safest fit.

#### Tradeoffs

* Fire & Forget does not resume automatically from a partial step.
* Recovery is usually caller-driven.
* If a failure happens mid-flow, retry logic must handle duplicates safely.

#### Design notes

* Keep the flow small and deterministic.
* Persist a business key with the returned reference.
* Make downstream writes idempotent.

Related docs:

* [Defining a Saga](../api-event-and-process-flows/defining-a-saga.md)
* [Headers, Cookies & Paths](../api-gateway-and-security/apis/headers-cookies-and-paths.md)
* [Orchestrate Saga](../api-event-and-process-flows/configuring-saga-steps/event-step/flow-actions/orchestrate-saga.md)

### 2. CDC-driven flows

CDC flows start from data changes, not API calls.

Typical pattern:

1. Store records to process in a table or state
2. Let CDC publish change records
3. Consume those records in a runner
4. Run business logic from each change

#### How it behaves

CDC managers track resume tokens or offsets.

On restart, they resume from the last stored checkpoint when supported.

Spring runners can store CDC resume state with `offset.state`.

CDC stream settings also support `offset.partitioned`, `offset.initial`, `pollMs`, and `asPulse`.

This gives automatic recovery across restarts.

#### Best fit

* Use CDC when persistence is the source of truth.
* Use it for outbox-style integration and incremental sync.
* Use it when new or changed records should trigger processing.

#### Tradeoffs

* CDC is usually at-least-once.
* A record can be processed more than once after retry or restart.
* Business logic must tolerate duplicates and partial repeats.

#### Operational notes

* CDC managers should usually run on a single replica.
* They do not partition consumed changes by themselves.
* If you need scale-out processing, publish CDC outputs to Kafka first.
* Then run business logic on partitioned consumers.

Related docs:

* [CDC Feed](building-blocks/data-and-event-streams/cdc-feed.md)
* [Other Systems → CDC](building-blocks/systems-integrations/other-systems.md)
* [Flow Handlers](building-blocks/execution-handlers/flow-handlers.md)

### 3. Message queues and event streams

This pattern sends work to a stream and processes it asynchronously.

Kafka is the common example. Samza runners are a common execution model.

#### How it behaves

The producer publishes an event and returns.

One or more runners consume the event later.

Checkpointing depends on the broker and runner type.

Delivery is commonly at-least-once.

Ordering also depends on the runner:

* **Sync Samza Event Runner** preserves message order
* **Async Samza Event Runner** increases parallelism and does not preserve order

#### Best fit

* Use streams for high-throughput async services.
* Use them when producers and consumers should evolve independently.
* Use them when buffering traffic spikes matters.

#### Tradeoffs

* You accept broker-specific delivery and retry behavior.
* Consumers must be idempotent.
* If order matters, choose the runner type carefully.

#### Design notes

* Use stable message keys when partition order matters.
* Keep messages self-contained when possible.
* Route failures to DLQ or compensating flows when needed.

Related docs:

* [Samza Runners](service-runners/deploying-runners/samza-runners.md)
* [Other Systems → Kafka](building-blocks/systems-integrations/other-systems.md)
* [Event Step](../api-event-and-process-flows/configuring-saga-steps/event-step/)

### 4. Scheduled and self-waiting sagas

Scheduled sagas start from time, not from an external request.

They are Rierino’s built-in timer-based async pattern.

#### How it behaves

Scheduling is configured on the saga definition.

You can use:

* **Period** and **Period Unit**
* **Initial Delay**
* **Max Repeat Count**
* **Cron Expression**
* **Cron Time Zone**

The runner timer triggers the saga directly.

This works well for polling external systems.

It also works for internal housekeeping jobs.

Rierino also supports self-waiting saga patterns.

Saga handler settings include `offset.state` for storing self-waiting offsets.

#### Best fit

* Use scheduled sagas for time-based or self-timing jobs.
* Use them for polling REST systems without push support.
* Use them for short and simple recurring tasks.

#### Tradeoffs

* Scheduling can duplicate work on replicated runners.
* Timers do not use streams for coordination.
* You must scope execution to the correct runners.

#### Operational notes

* Always set **Allowed For Runners** on scheduled sagas.
* Otherwise, every timer-enabled runner can trigger the same saga.
* To avoid duplicate runs:
  * Run a single replica
  * Or trigger only from partition `0` in a StatefulSet
  * Use Python batch runners for long-running or complex scheduled jobs.

Related docs:

* [Defining a Saga](../api-event-and-process-flows/defining-a-saga.md)
* [Flow Handlers](building-blocks/execution-handlers/flow-handlers.md)
* [Batch Tasks](../batch-tasks/)

### 5. Human-assigned flows

Human-assigned flows pause automation and wait for a person.

They use the task orchestration actions.

#### How it behaves

`StartTask` stores the current process as a task record.

You can assign the task to:

* Specific roles
* A specific user
* A timeout path

You can also define:

* `taskName`
* `taskSpec`
* `taskAction`
* `timeout`
* `timeoutOn`

Later, `ProceedTask` resumes the process.

If the timeout expires, timeout handling can continue the flow differently.

Saga conditions can branch on `TIMEOUT`.

#### Best fit

* Use this for approvals and manual exceptions.
* Use it when an operator must review data before continuing.
* Use it when automation should pause safely for human input.

#### Tradeoffs

* This pattern adds operational latency by design.
* Task state must stay consistent and queryable.
* Proceed and timeout flows should run near the same task state.

#### Design notes

* Store enough context for the assignee.
* Keep the resumed payload small and explicit.
* Design clear timeout and escalation paths.

Related docs:

* [Orchestrate User Task](../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/orchestrate-user-task.md)
* [Core Handlers](building-blocks/execution-handlers/core-handlers.md)

### How to choose the right pattern

Choose **Fire & Forget** when an API needs fast acknowledgement.

Choose **CDC** when data persistence should trigger the work.

Choose **Event streams** when you need buffering and loose coupling.

Choose **Scheduled sagas** when time or loop should drive execution.

Choose **Human-assigned tasks** when a person must continue the process.
