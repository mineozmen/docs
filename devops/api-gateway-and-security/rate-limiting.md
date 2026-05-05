---
description: Rierino provides built-in user & IP based rate limiting features
---

# Rate Limiting

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
