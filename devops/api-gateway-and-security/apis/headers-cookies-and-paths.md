---
description: >-
  For all requests, certain details can be provided through headers, paths or
  cookies
---

# Headers, Cookies & Paths

## Request Origin

Identifies the source of a request (such as application, user geolocation), and can be populated using:

* "Rierino-Origin" Header: As base64 encoded Json string
* "Rierino-Origin" Cookie: As base64 encoded Json string
* "User-Agent" Header: Sets origin.agent
* "Referrer" Header: Sets origin.referrer
* "X-Forwarded-For", "Forwarded" Headers, Request Address: Sets origin.ip

## Authentication

Provides authentication details (such as credentials, api key or tokens), and can be populated using:

* "Rierino-Auth.\[parameter]" Header: Sets auth.parameter
* "Rierino-Auth.\[parameter]" Cookie: Sets auth.parameter
* "Bearer" Authorization Header: As base64 encoded Json string prefixed with "base64="
* "Bearer" Authorization Header: As comma separated "parameter:value,parameter:value" pairs prefixed with "multi="
* "Basic" Authorization Header: As base64 encoded username:password parameters
* "\[parameter]" Authorization Header: Sets auth.parameter where comma separated "parameter value, parameter value" pairs are allowed

## Branch

For sending requests for a specific "branch", two alternative methods can be used:

* {channel} can be suffixed with @{branch}, such as rpc@beta
* Sending branch as "X-Branch" header, such as X-Branch=beta

## Async Requests

For making API calls that should fire & forget, a special "X-Async" header can be used with 2 alternative values:

* **gateway:** The request is received at the API gateway and immediately returned to the requestor with a request ID (even if the backend is not accessible). The gateway keeps communication ongoing with the backend as long as the process runs.
* **backend:** The request is forwarded to the backend as a fire & forget saga call, and the backend returns a dedicated process ID. The backend keeps running but the communication between gateway and backend is completed immediately.

{% hint style="info" %}
In addition to async request header, sagas can be individually configured to be fire & forget in nature, which immediately return regardless of this header.
{% endhint %}
