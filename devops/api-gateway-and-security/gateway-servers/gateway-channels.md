---
description: >-
  A gateway channels defines path segment(s) mappings to systems for
  distributing requests across different runners.
---

# Gateway Channels

<figure><img src="../../../.gitbook/assets/image (156).png" alt="Gateway Channel UI"><figcaption><p>Gateway Channel UI</p></figcaption></figure>

All channels share the following settings:

* **Target:** Path segment(s) representing this channel (e.g. /api/crud).
* **System:** Id of the gateway system that communicates for this channel. As an alternative, system parameters can be defined on the channel as well.
* **Method:** Method for communicating through the gateway system (e.g. POST).
* **Gateways:** List of gateway ids which are allowed to communicate with this system.

## Channel Authentication

Authentication requirements for a channel are defined with the following settings:

* **Auth Token:** Id of the gateway token to use for user authentication
* **Anonymous Token:** Id of the gateway token to use for guest identification
* **Can Audit All:** Whether all paths should allow auditing
* **Paths:** Customized authentication settings for different paths on the channel:
  * **Path:** Path segment for customization (e.g. product). \* is used as a wildcard meaning all paths. \[path]/\* is used to allow all subpaths of a given path.
  * **Is Public:** Whether path can be accessed without authentication
  * **Is Disabled:** Whether the path is disabled for access (useful for temporarily disabling paths without removing them)
  * **Log Detail as Info:** Whether request payload details should be logged as "Info" level instead of "Trace" (useful for temporarily logging details on select paths for debugging)
  * **Return Error Payload:** Whether response should include error payload details even when the log level does not allow it (useful for non-sensitive endpoints)
  * **Can Audit:** Whether path should return details on execution step inputs and outputs for auditing/debugging purposes (using rierino-audit-path header for requests)
  * **Access Token Verification Required:** Whether gateway can rely on its own tokens for authentication, or it should ask authentication server for each request
  * **Methods:** Customized authentication settings for different call methods on the channel:
    * **Method:** Name of the method (e.g. GET). \* is used as a wildcard meaning all methods.
    * **Accessing Roles:** List of user roles which are allowed access for this method (e.g. admin). Leaving this list empty or including \* as a role allows access for all users regardless of their roles.

## Path Aliases

Aliases allow redirecting specific paths to a target URL for renaming or shortening API endpoints (e.g. landing -> /GenerateLandingPage) with:

* **Alias:** Root level alias to use for redirecting
* **Target:** Target path to redirect to on this channel
* **Header Map:** Header name - Jmespath expression pairs where incoming header value can be used to produce new headers or replace its value (e.g. "API-key": "{ "Authorization": join(' ', \['API-key', value]) }").

## Response Headers

Custom response headers can be configured for each channel as well as path & method serviced under that channel with:

* **Header Key:** Header to send
* **Header Value:** Value to send for the header

## Channel Resilience

Resilience requirements for a channel are defined with the following settings:

* **Rate Limit:** Rate limit settings based on resilience4j parameters.
* **Paths:** Customized resilience settings for different path segments.
  * **Path:** Path segment for customization (e.g. product). \* is used as a wildcard meaning all paths.
  * **Circuit Break:** Circuit breaker settings based on resilience4j parameters.
* **Retries:** Automated retries in case of failures in accessing runner endpoints with path & method level configurations.&#x20;

{% embed url="https://resilience4j.readme.io/docs/circuitbreaker" %}
Resilience4j Circuit Breaker
{% endembed %}

In addition, channels can be configured to apply [user based rate limits](../rate-limiting.md).
