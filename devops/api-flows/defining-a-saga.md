# Defining a Saga

<figure><img src="../../.gitbook/assets/image (157).png" alt=""><figcaption><p>Saga Definition</p></figcaption></figure>

Click the edit icon on the Saga screen. This opens the saga definition form. The form is split into tabs.

## Definition

Fields marked with `*` are required.

* **Name:** Human-friendly name shown in lists and on the graph.
* **Domain:** Logical grouping (for example `customer` or `product`).
* **Status\*:** Only active sagas accept requests.
* **Description:** Short explanation for documentation and maintenance.
* **Path\*:** URL path that triggers this saga.
  * Must start with `/`.
  * Use alphanumerics and no spaces.
  * Runners select the saga by matching this path in request metadata.
  * You can keep multiple sagas on one path.
  * You can also define stream-driven sagas using `/On_[stream]`.
* **Version:** Numeric version for maintaining multiple flows per path.
* **Allowed On Streams:** Streams where this saga may run (HTTP, Kafka, etc.).
* **Allowed For Runners\*:** Runners allowed to execute this saga.
  * Use this alone if sagas run on a single stream.
  * Combine with streams for tighter control.
* **Allowed Roles:** Roles permitted to execute this saga.
  * You can also enforce this at the gateway.
  * Gateway enforcement can avoid an extra microservice call.
* **Allowed Methods:** REST methods allowed for this path (GET/POST/etc.).
  * Useful when you want to prevent GET-based caching.
* **Auto Fail:** Fail the entire saga when any step fails.
  * If disabled, handle failures per step and route explicitly.
* **Fire & Forget:** Return immediately and run the flow asynchronously.
  * Status and results are recorded if Fire & Forget state is configured.

## Configuration

* **Redirect To:** Always execute another saga instead of this one.
  * Useful for mapping multiple URLs to one flow.
* **On Fail Saga:** Execute another saga when this saga fails.
  * Useful for recovery actions or persistent error logging.
* **Extra Payload:** Add a static payload to every call.
  * Works with Redirect To.

### Caching

You can cache individual steps in some handlers. Saga-level caching caches the final response. The cache key is derived from the input payload.

* **Use Cache:** Enable caching on the runner.
* **Cache State:** State manager alias used as the cache store.
* **Cache Key Pattern:** JMESPath expression that produces the cache key.

### Resilience

You can configure resilience at the API gateway. You can also configure it per saga on the runner.

This is optional. It helps when a saga calls unstable dependencies. External APIs are a common example.

Circuit breakers support these settings:

* **Min. Number of Calls:** Minimum requests before error rate is calculated.
* **Sliding Window Threshold:** How many recent calls to evaluate.
* **No Reply Timeout Duration:** Milliseconds to consider the saga non-responsive.
* **Failure Rate Threshold:** Failure percentage that opens the circuit.
* **Slow Call Duration Threshold:** Milliseconds that count as a slow call.
* **Slow Call Rate Threshold:** Slow-call percentage that opens the circuit.
* **Wait Duration in Open State:** Milliseconds to stay open before half-open.
* **Calls Permitted in Half Open:** Test calls allowed in half-open.

Circuit breakers run at runner partition level. If one partition fails often, others are not blocked. This isolates the blast radius in distributed deployments.

### Scheduling

Use Python processors for complex scheduled jobs. Use saga scheduling for simpler and short-running tasks. Scheduling runs directly on the runner via timers.

* **Period:** Trigger interval (for example every 10 seconds).
* **Period Unit:** Seconds, minutes, and so on.
* **Initial Delay (ms):** Delay before the first run.
* **Max Repeat Count:** Maximum runs before stopping.
  * This counter resets on runner restart or rebuild.
* **Cron Expression:** Cron schedule for more complex timing.
* **Cron Time Zone:** Time zone for cron evaluation.
  * Defaults to server time zone.

{% hint style="warning" %}
To avoid same saga being scheduled on multiple replicas of a runner:

* Either, deploy the runner with a single replica
* Or, deploy the runner as a StatefulSet and configure only the partition 0 with a timer

Otherwise, the saga would be executed multiple times with each period.
{% endhint %}

{% hint style="warning" %}
When scheduling a saga, it is important to configure the "Allowed For Runners" setting, since scheduling does not use streams for triggering saga flows.

Otherwise, all runners with timers would trigger the same saga with each period.
{% endhint %}

## Schema

Schema defines the saga input and output models. It is used for documentation and optional validation. OpenAPI is generated from this configuration.

* **Method in Documentation:** HTTP method shown in OpenAPI.
* **Exclude in Documentation:** Hide this saga from OpenAPI.
* **Mock Output:** Return mock responses from Output Schema examples.
* **Validated:** Validate requests against Input Schema.
  * Invalid requests return an error response.
* **Input Schema:** JSON Schema for request payload.
* **Output Schema:** JSON Schema for response payload.

{% file src="../../.gitbook/assets/train_query.json" %}
Example Saga Definition (Can be Imported on Saga Screen)
{% endfile %}

### Validation

When **Validated** is enabled, requests are checked against Input Schema. Standard JSON Schema constraints are supported. Examples include `minimum`, `minLength`, and `pattern`.

{% embed url="https://json-schema.org/specification" %}
JSON Schema Specification
{% endembed %}

For advanced validation, Rierino adds an `x-validation` extension keyword. It lets you attach custom validators to a schema. Use this structure (placeholders shown in ALL CAPS):

```json
"x-validation":{
    "VALIDATOR_1_NAME": {
        "validator": "VALIDATOR_CLASS",
        "scope":{
            "type": "TYPE",
            "fields": ["FIELD_PATH_1", "FIELD_PATH_2"]
        },
        "props": { "VALIDATOR_SPECIFIC_PROPERTIES": "..." }
    },
    ...
    "VALIDATOR_N_NAME": {...}
    ...
}
```

* **VALIDATOR\_NAME:** Any logical name for this validator instance.
* **VALIDATOR\_CLASS:** Fully qualified Java class extending `BaseJsonValidator`.
* **Scope (optional):** Apply the validator to nested fields.
  * **type:** Data type to match (for example `string`).
  * **fields:** JSON paths of nested fields to validate.
* **props:** Extra validator-specific configuration object.

Example: `JsoupValidator` detects unsafe HTML content.

```json
"x-validation":{
    "safeHtml": {
        "validator": "com.rierino.handler.util.jsonschema.JsoupValidator",
        "scope":{
            "type": "string"
        },
        "props": { 
            "safelist": "relaxed"
         }
    }
}
```

## AI

AI settings control how this saga is exposed as an agent tool. This is optional. It gives you tighter control over tool calls and responses.

* **Tool Name:** Tool name generated for this saga.
  * If empty, a unique name is generated.
  * Must match agent tool naming rules.
* **Tool Description:** Tool description shown to the agent.
  * If empty, the saga description is used.
  * Use this when you need AI-specific wording.
* **Tool Response Pattern:** JMESPath expression that maps saga output to text.
  * If empty, the agent expects a `message` field.
* **Tool Restriction:** Controls repeated calls with identical parameters.
