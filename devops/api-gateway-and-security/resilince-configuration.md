---
description: >-
  Rierino provides built-in features for granular resilience in API gateway
  layer, including rate limiter, circuit breaker and bulkhead
---

# Resilince Configuration

Resilience parameters are set at gateway channel level, providing more granular control based on importance and resource availability of individual backend microservices. Each channel can be configured to have the following resilience controls:

## Global Rate Limiting

Gateway channels can be configured to limit global number of requests they accept for a given period of time, to protect backend from traffic spikes it is not resourced for. The following settings can be configured for the global rate limiters:

* **Refresh Period:** Milliseconds between refill of the limit
* **Period Limit:** Number of requests allowed during the period
* **Timeout Duration:** Milliseconds the requestor is allowed to wait for obtaining acceptance

{% hint style="info" %}
Global rate limits are applied per API gateway instance (not shared across them), and should be planned according to the number of replicas this layer has.
{% endhint %}

## User Based Rate Limiting

While it is possible to perform rate limiting using an external service component, API gateway also provides this feature out of box.

Following settings start with "rierino.rateLimiter." prefix and are set in application properties of the relevant gateway, for optimizing the performance of API calls, avoiding central lookup on each call.

| Property      | Description                                                                                                        | Default |
| ------------- | ------------------------------------------------------------------------------------------------------------------ | ------- |
| cache.maxSize | Size of the local cache to use for rate limiting, to avoid checking on central rate limit storage on each API call | 10000   |
| cache.expiry  | Minutes to expire the local cache                                                                                  | 2       |

Gateway rate limiters require configuration of a gateway service, as they typically require access to external services (such as a Redis cache) for centralization of rate counting. Parameters used for these services are as follows:

| Parameter              | Description                                                                                                         | Example                                                  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| persistenceHelperClass | Fully qualified class name for the central rate limit storage helper                                                | com.rierino.api.gateway.helper.CaffeinePersistenceHelper |
| \*                     | Additional configuration parameters used by the persistence class (such as configString for RedisPersistenceHelper) | -                                                        |

Once the gateway is configured to support user based rate limiting, the following configurations can be applied on any gateway channel to restrict rate per channel and method:

* **Is Enabled:** Whether channel applies rate limiting
* **Period:** Rate limiting period (minutes as default)
* **Identifier:** user, ip or both options to restrict based on user id, IP address or both
* **Max Unsynchronized Tokens:** Number of tokens that can be consumed locally without synchronization with external storage (5 as default)
* **Max Timeout Between Sunchronization:** How long bucket proxy can act locally without synchronization with external storage (30 as default)
* **Use Batching:** Whether rate limiting should batch requests for optimization
* **Global Default:** Global default rate limit per period (-1 as unlimited by default)
* **Paths Default:** Default for each path per period (-1 as unlimited by default)
* **Global Types:** Global limit per type (user/ip)
* **Paths Types:** Path based limit per type (user/ip)
* **For each path:**
  * **Default Rate:** Allowed limit per period (-1 as unlimited by default)
  * **Types:** Limit per type (user/ip)

## Concurrency Limiting

Gateway channels can be also configured to restrict the number of concurrent incoming connections using bulkhead settings, to avoid exhaustion of threads and resources. The following settings can be configured for the bulkhead:

* **Max Concurrent Calls:** Number of maximum concurrent calls allowed
* **Max Wait:** Milliseconds to wait for establishing a connection before rejection

## Circuit Breaker

Circuit breakers are configured per gateway channel path (e.g. API URL), allowing blocking of incoming calls for endpoints that are continuously failing. The following settings can be configured for the circuit breaker per path:

* **Sliding Window Type:** Type of sliding window used to record calls and calculate failure rates
* **Sliding Window Size:** Number of calls or time period included in the sliding window
* **Failure Rate Threshold:** Percentage of failed calls that opens the circuit breaker
* **Minimum Number of Calls:** Minimum number of calls required before failure rates are calculated
* **Automatic Transition from Open to Half Open:** Enables automatic transition to half-open after the open wait duration
* **Max Wait Duration in Half Open:** Maximum time to remain half-open before returning to open state
* **Slow Call Duration Threshold:** Maximum call duration before a call is recorded as slow
* **Permitted Calls in Half Open:** Number of test calls allowed while the circuit breaker is half-open
* **Wait Duration in Open:** Milliseconds to remain open before transitioning to half-open

## Automated Retries

Gateway channel paths as well as methods called on those paths can be configured to perform automated retries from API gateway to backend services through the following configuration:

* **Retry After:** Milliseconds to wait between retries
* **Retry Count:** Number of retries allowed per call
* **Retry Filter:** Following filters are applied as an OR condition to retry specific type of errors only:
  * **Retry Error Codes:** List of Rierino error codes allowed to be retried
  * **Retry HTTP Status Codes:** List of HTTP status codes allowed to be retried
  * **Retry Error Texts:** List of Rierino error messages allowed to be retried

{% hint style="info" %}
These configurations are performed on the API gateway layer. For requests coming from other channels to sagas (e.g. through event streams), saga flows can be configured to have their own circuit breakers as well as custom designed flows for additional control.
{% endhint %}
