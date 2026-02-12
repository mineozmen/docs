# Defining a Saga

<figure><img src="../../.gitbook/assets/image (157).png" alt=""><figcaption><p>Saga Definition</p></figcaption></figure>

Clicking on the edit icon displays saga definition form, which has the following data fields across tabs:

## **DEFINITION**

* **Name:** Descriptive name for the saga, which will be used as the label on saga screen and listings.
* **Domain:** Used for grouping saga flows within logical business domains (e.g. customer, product).
* **Status\*:** Whether the saga is currently active or not. A saga is used for accepting requests only when it is set as active.
* **Description:** Verbal description of the saga flow, used for referential purposes and documentation.
* **Path\*:** URL path for triggering the saga flow from API endpoint, which starts with '/' character and is an alphanumerical entry without any spaces. Saga runners select the flow to execute based on the URL path received in request metadata, hence this is a critical setting. More than one sagas can be assigned to the same URL, especially if they are allowed on different streams. It is also possible to trigger special saga flows on event streams such as change data captures, where "/On\_\[stream]" paths represent flows to trigger when data is received on a specific \[stream].
* **Version:** Numerical version for the saga, typically used when there are more than one flows created for the same path.
* **Allowed On Streams\*:** Streams on which the saga can be executed (e.g. Kafka topic, HTTP stream).&#x20;
* **Allowed For Runners\*:** Runners for which the saga is enabled.  This setting can be used on its own if only one stream processes sagas on the given runners, or can be combined with "allowed on streams" setting for more refined configuration.
* **Allowed Roles:** List of user roles that are allowed to execute the saga. (The same configuration can be applied on API gateway instead, which allows limiting an extra call to the microservices.)&#x20;
* **Allowed Methods:** REST methods allowed for triggering this saga flow from API endpoint (e.g. GET, POST). This is mainly useful for disabling front-end caching via GET requests.
* **Auto Fail:** Whether an error in any saga step should automatically result in failure of the complete flow (i.e. respond with an error code). If this is not set to true, saga flows must handle error cases on each step (e.g. directing them to a failure node or recovering them).
* **Fire & Forget:** Whether the saga should immediately return response, creating an asynchronous task for executing the actual flow or not (status and results are recorded in fire & forget state if configured).&#x20;

## **CONFIGURATION**

* **Redirect To:** Uses another saga definition for this flow, regardless of whether the current saga has steps defined or not. This is mainly used for assigning the same flow to multiple URL paths.
* **On Fail Saga:** Uses another saga in case this saga fails, typically for recovery actions or to log details of the error in database.
* **Extra Payload:** Static payload to add to received events for each saga call (can be also used in combination with redirect to).

### **Caching**

While it is possible to cache individual step results for specific event handlers, Saga level caching allows caching of final output from individual API calls, based on input parameters mapped to a cache key.

* **Use Cache:** Whether saga calls can be cached on the runner.
* **Cache State:** Alias of the state manager which will be used for caching saga calls (if use cache is set as true).
* **Cache Key Pattern:** Jmespath pattern for calculating the cache key from saga event payload.

### Resilience

While it is possible to set resilience parameters through the API gateway configuration, saga runners also allow definining circuit breaker logic for each individual saga flow. This is an optional configuration, but is especially useful when certain saga orchestrate microservices which are more susceptible to failure (e.g. API calls to external systems). It is possible to define the following resilience settings for each saga flow:

* **Min. Number of Calls:** Minimum number of requests to be received before the circuit breaker calculates error rates. If the saga flow receives less number of requests, it will not apply circuit breaker.
* **Sliding Window Threshold:** Number of most recent requests that will be used for calculating the error rates.
* **No Reply Timeout Duration:** Milliseconds between the latest request to and response from a saga flow to consider it not responding.
* **Failure Rate Threshold:** Minimum percent of requests that should fail to stop processing new requests (i.e. open the circuit).
* **Slow Call Duration Threshold:** Milliseconds between the request and end time of a saga flow to be considered as a slow call.
* **Slow Call Rate Threshold:** Minimum percent of requests that should take longer than the threshold duration to stop processing new requests (i.e. open the circuit).
* **Wait Duration in Open State:** Milliseconds to keep circuit breaker in open state (e.g. not processing new requests) before switching to half-open state (e.g. start processing again to decide if problem is resolved).
* **Calls Permitted in Half Open:** Number of requests allowed to decide on whether the problem is resolved when circuit breaker is in half-open state.

These metrics are calculated and the circuit breakers are applied at a runner partition level, meaning that if a saga flow is distributed across multiple partitions and only a single partition is frequently failing, it doesn't affect the execution of remaining partitions, allowing isolation of the problem.

### Scheduling

While complex and batch jobs are typically scheduled using cron jobs using Rierino Python processor libraries, sagas can be also scheduled directly on the runners with timer configurations, for simpler, short-running use cases.&#x20;

* **Period:** Period (e.g. number of seconds) which will trigger execution of the saga.
* **Period Unit:** Unit of time measure for the period (e.g. seconds, minutes).
* **Initial Delay (ms):** Initial delay for the first execution, if required.
* **Max Repeat Count:** Maximum number of repetitions allowed when the runner starts executing this saga schedule. Every runner restart or rebuild resets this counter.
* **Cron Expression:** Cron expression to use for configuring more sophisticated schedules, instead of "period" parameter.&#x20;
* **Cron Time Zone:** The time zone to execute cron expression on (if left empty, uses server time zone).

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

## SCHEMA

Schema configuration defines input & output data model for the saga, for documentation as well as validation purposes. OpenAPI specification is automatically produced from this schema configuration.

* **Method in Documentation:** HTTP method to use for the saga in OpenAPI specification.
* **Exclude in Documentation:** Whether saga should be excluded from OpenAPI specification.
* **Mock Output:** Whether saga should automatically generated mock data as response, using examples provided in "Output Schema".
* **Validated:** Whether saga calls should be validated against the "Input Schema", triggering error response for invalid calls.&#x20;
* **Input Schema:** Json schema for the input data model for saga calls.
* **Output Schema:** Json schema for the output data model for saga responses.

{% file src="../../.gitbook/assets/train_query.json" %}
Example Saga Definition (Can be Imported on Saga Screen)
{% endfile %}

### Validation

When a saga flow is configured to be "Validated", it automatically validates all incoming requests against the "Input Schema", supporting JSON Schema specification standard such as "minimum", "minLength" and "pattern".&#x20;

{% embed url="https://json-schema.org/specification" %}
JSON Schema Specification
{% endembed %}

To allow more advanced validation checks automatically using "Input Schema", a custom validation keyword "x-validation" is also added as an extension to JSON Schema standards. This keyword allows addition of any custom validators with the following configuration:

```json
"x-validation":{
    VALIDATOR_1_NAME: {
        "validator": VALIDATOR_CLASS,
        "scope":{
            "type": TYPE,
            "fields": [FIELD_ARRAY] 
        },
        "props": { VALIDATOR_SPECIFIC_PROPERTIES }
    },
    ...
    VALIDATOR_n_NAME: {...},
    ...
}
```

* **VALIDATOR\_NAME:** Can be any logical name assigned to a validator to be applied.
* **VALIDATOR\_CLASS:** Fully qualified name of the Java class that extends "BaseJsonValidator", which will apply custom validation rules on the field(s).
* **Scope:** Optional configuration that allows inclusion of multiple nested fields inside an "object" or "array" type field. When not specified, the validator only checks the current element value.
  * **TYPE:** Data type, such as "string", which means automated validation of each nested field matching this TYPE inside the current element
  * **FIELD\_ARRAY:** List of Json paths for nexted fields, which will be automatically validated
* **VALIDATOR\_SPECIFIC\_PROPERTIES:** Json object to pass additional configuration properties, if required by a validator

As an example, a "JsoupValidator" is available out of box, which allows automated detection of unsafe tags inside an input:

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

AI configuration allows customization of how saga is used by AI agents as a tool. While this configuration is optional, it provides more granular control over tool calls.

* **Tool Name:** Name that will be used for the tool generated for this saga. When left empty, a unique ID is generated by default. The name must be compatible with AI agent tool naming conventions.
* **Tool Description:** Description of the saga as a tool for the AI agent. When left empty, saga's own definition is used by default. This configuration allows AI specific descriptions, keeping original description for human use and documentation.
* **Tool Response Pattern:** JMESPath expression for translating saga output into a text message for the AI agent. When left empty, AI agent expects a "message" data field in saga result.
* **Tool Restriction:** Controls over repeated calls of this saga by an agent with the same parameters.
