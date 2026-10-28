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
* **Can Stop All:** Whether all paths should allow stopping at any step during audit
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
    * **Is Public:** Whether the method can be called without authentication (to override Path configuration)

## Path Aliases

Aliases map incoming paths to endpoints on a channel. You can use them to rename or shorten API endpoints (e.g. `/landing` → `/GenerateLandingPage`) or to serve fixed content directly from the gateway. Routing is internal: the request is forwarded to `/api/request/{channelId}{target}` without an HTTP redirect, so the client's URL stays the same.

Each alias has these fields:

* **Alias**: The path to match. It can be an exact path (e.g. `/landing`) or a path pattern with variables and wildcards (e.g. `/products/{id}`, `/docs/**`).
* **Target**: The path to forward to on this channel, starting with `/` (e.g. `/GenerateLandingPage`). The original query parameters are kept. For pattern aliases, each path variable is also added as a query parameter (e.g. `/products/123` → `/GetProduct?id=123`). Query parameters are used because every endpoint accepts them, while body formats vary.
* **Request Header Map**: Pairs of header name and JMESPath expression. They transform incoming headers and payload into new headers before the request is forwarded:
  * The expression is evaluated against `{"value": "<incoming header value>"}`. Only the first value of the header is used.
  * The original header is removed. Each field of the resulting object is added as a new header, so a single expression can produce several headers.
  * If the expression returns null, the header is just dropped. If the request doesn't have the header, nothing happens.
  * Example: `"API-key": "{ \"Authorization\": join(' ', ['API-key', value]) }"` turns `API-key: abc` into `Authorization: API-key abc`.
* **Request Payload Map:** Similar to request header map, modifies incoming payload and header into new payload to be forwarded.
* **Response Header Map**: Allows mapping of payload & headers in backend response into new headers.
* **Response Payload Map**: Allows mapping of payload & headers in backend response into new payload elements.
* **Static Content**: Text to return directly instead of forwarding (e.g. for constant `.well-known` endpoints). The response is `200 OK` with `Content-Length` set, and HEAD requests get only the headers. The Header Map is not applied to static responses.
* **Static Content Type**: The content type for static content (e.g. `application/json`, `text/plain; charset=utf-8`). It defaults to `application/json`, and an invalid value also falls back to `application/json`. The charset in the content type is used to encode the content, with UTF-8 as the default.

In addition to the path configurations, channels allow configuring method specific (e.g. GET, POST) override of all these parameters.

## Response Headers

Custom response headers can be configured for each channel as well as path & method serviced under that channel with:

* **Header Key:** Header to send
* **Header Value:** Value to send for the header

In addition to global configurations, it is possible to define path specific response headers and values.
