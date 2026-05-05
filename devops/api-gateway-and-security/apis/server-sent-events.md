---
description: Stream saga responses over a single HTTP connection using /api/stream.
---

# Server Sent Events

API gateways support Server-Sent Events (SSE) for any backend microservice endpoint.

SSE keeps one HTTP connection open. The gateway then pushes a sequence of events to the client.

### Endpoint mapping

You typically already have an API under:

* `/api/request/[PATH]` for a one-shot response.

You can stream the same backend logic by calling:

* `/api/stream/[PATH]` for an SSE feed.

Authentication, request method, and request body stay the same.

{% hint style="info" %}
`/api/stream/...` returns `text/event-stream`. Response format negotiation (XML/HTML/CSV) applies to `/api/request/...` endpoints. See [Response Formats](response-formats.md).
{% endhint %}

### How streaming works

The gateway runs a simple poll-and-stream loop. The backend controls the loop using a few response fields.

{% stepper %}
{% step %}
### 1) Gateway calls your backend normally

The gateway calls the mapped backend runner on `[PATH]`. It receives a regular JSON response.
{% endstep %}

{% step %}
### 2) Gateway emits events

If the backend response contains a top-level `list` array, the gateway emits:

* one SSE event per entry in `list`

This is the main mechanism for streaming results incrementally.
{% endstep %}

{% step %}
### 3) Backend decides whether to continue

If the backend response contains `continue: true`, the gateway will make another request.

The gateway waits between calls. The backend can control the wait by returning `wait`. If `wait` is missing, the gateway uses its default.
{% endstep %}

{% step %}
### 4) Gateway passes state back to the backend

On every follow-up request, the gateway sends a `previous` value to the runner.

`previous` is set to the last `next` value returned by the backend. This enables cursor-based iteration and “resume token” patterns.
{% endstep %}

{% step %}
### 5) Loop ends

The loop ends when either:

* the client closes the SSE connection, or
* the backend stops returning `continue: true`
{% endstep %}
{% endstepper %}

### Request and response fields

#### Request (gateway → backend runner)

* `previous` (optional): cursor from the prior iteration.
  * Not present on the first call.
  * Set to the last seen `next` value.

#### Response (backend runner → gateway)

* `list` (optional): array of records to stream.
  * Each entry becomes a separate SSE event.
* `continue` (optional): boolean.
  * When `true`, the gateway repeats the request.
* `wait` (optional): wait duration before the next request.
  * Interpreted by the gateway.
  * Falls back to a gateway default when missing.
* `next` (optional): cursor value for the next iteration.
  * Returned to the backend later as `previous`.

### Common use cases

* **Notifications feed:** return new notifications in `list`, set `continue: true`.
* **Live monitoring:** stream changing metrics or health events as individual list items.
* **Cursor iteration:** return `next` tokens to page through large datasets safely.
