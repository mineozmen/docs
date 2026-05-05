---
description: >-
  Gateway server APIs are the main request endpoints for Rierino platform, when
  deployed in full.
---

# APIs

## Request APIs

Request APIs provide ability to communicate with micro-services, where an executor responsible for the given channel sends the request and returns response to the caller.

The method of request (e.g. GET vs POST) is used only by some executors (e.g. CRUDExecutor), whereas the others treat all methods the same (e.g. KafkaExecutor).

### Special Headers, Cookies & Paths

For all requests, certain details can be provided through headers, paths or cookies:

#### Request Origin

Identifies the source of a request (such as application, user geolocation), and can be populated using:

* "Rierino-Origin" Header: As base64 encoded Json string
* "Rierino-Origin" Cookie: As base64 encoded Json string
* "User-Agent" Header: Sets origin.agent
* "Referrer" Header: Sets origin.referrer
* "X-Forwarded-For", "Forwarded" Headers, Request Address: Sets origin.ip

#### Authentication

Provides authentication details (such as credentials, api key or tokens), and can be populated using:

* "Rierino-Auth.\[parameter]" Header: Sets auth.parameter
* "Rierino-Auth.\[parameter]" Cookie: Sets auth.parameter
* "Bearer" Authorization Header: As base64 encoded Json string prefixed with "base64="
* "Bearer" Authorization Header: As comma separated "parameter:value,parameter:value" pairs prefixed with "multi="
* "Basic" Authorization Header: As base64 encoded username:password parameters
* "\[parameter]" Authorization Header: Sets auth.parameter where comma separated "parameter value, parameter value" pairs are allowed

#### Branch

For sending requests for a specific "branch", two alternative methods can be used:

* {channel} can be suffixed with @{branch}, such as rpc@beta
* Sending branch as "X-Branch" header, such as X-Branch=beta

#### Async Requests

For making API calls that should fire & forget, a special "X-Async" header can be used with 2 alternative values:

* **gateway:** The request is received at the API gateway and immediately returned to the requestor with a request ID (even if the backend is not accessible). The gateway keeps communication ongoing with the backend as long as the process runs.
* **backend:** The request is forwarded to the backend as a fire & forget saga call, and the backend returns a dedicated process ID. The backend keeps running but the communication between gateway and backend is completed immediately.

{% hint style="info" %}
In addition to async request header, sagas can be individually configured to be fire & forget in nature, which immediately return regardless of this header.
{% endhint %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/{channel}/{path}" method="get" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/full/{channel}/{path}" method="get" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/full/e/{channel}/{path}" method="get" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/{channel}/{path}" method="post" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/full/{channel}/{path}" method="post" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/full/e/{channel}/{path}" method="post" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/{channel}/{path}" method="put" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/{channel}/{path}" method="patch" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Request APIs.yml" path="/api/request/{channel}/{path}" method="delete" %}
[Request APIs.yml](<../../../.gitbook/assets/Request APIs.yml>)
{% endopenapi %}

## Auth APIs

Auth APIs coordinate authentication flows with authentication servers across different gateway channels.

{% openapi src="../../../.gitbook/assets/Auth APIs.yml" path="/api/auth/register/{channel}/{type}" method="post" %}
[Auth APIs.yml](<../../../.gitbook/assets/Auth APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Auth APIs.yml" path="/api/auth/login/{channel}/{type}" method="post" %}
[Auth APIs.yml](<../../../.gitbook/assets/Auth APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Auth APIs.yml" path="/api/auth/refresh/{channel}/{type}" method="post" %}
[Auth APIs.yml](<../../../.gitbook/assets/Auth APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Auth APIs.yml" path="/api/auth/logout/{channel}/{type}" method="post" %}
[Auth APIs.yml](<../../../.gitbook/assets/Auth APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Auth APIs.yml" path="/api/auth/delete/{channel}/{type}" method="post" %}
[Auth APIs.yml](<../../../.gitbook/assets/Auth APIs.yml>)
{% endopenapi %}

## Tracker APIs

Tracker APIs provide ability to post front-end traffic data, in case back-end tracking is not possible for a specific use case. Example uses include front-end or CDN caching of API responses, front-end events that do not require API calls (such as widget interactions) or Rierino platform integration with other back-end modules that do not provide tracking capabilities.

{% openapi src="../../../.gitbook/assets/Tracker APIs.yml" path="/api/track/{channel}/{path}" method="post" %}
[Tracker APIs.yml](<../../../.gitbook/assets/Tracker APIs.yml>)
{% endopenapi %}

## File APIs

File APIs provide ability to interact with file system services, listing, uploading and deleting files, typically used for export/import operations. For all APIs, user authentication is done using headers and cookies similar to other gateway APIs. All APIs also have a api/file/sudo/\* version, which allows root level access to a file system for admin users.

{% openapi src="../../../.gitbook/assets/File APIs.yml" path="/api/file/{service}" method="get" %}
[File APIs.yml](<../../../.gitbook/assets/File APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/File APIs.yml" path="/api/file/{service}/{filePath}" method="get" %}
[File APIs.yml](<../../../.gitbook/assets/File APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/File APIs.yml" path="/api/file/{service}" method="post" %}
[File APIs.yml](<../../../.gitbook/assets/File APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/File APIs.yml" path="/api/file/{service}/{filePath}" method="post" %}
[File APIs.yml](<../../../.gitbook/assets/File APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/File APIs.yml" path="/api/file/{service}/{filePath}" method="delete" %}
[File APIs.yml](<../../../.gitbook/assets/File APIs.yml>)
{% endopenapi %}

## Control APIs

Control APIs provide ability to query and refresh gateway components (system, channel, service, token) in real-time.

{% openapi src="../../../.gitbook/assets/Control APIs.yml" path="/api/control/{component}/{id}" method="get" %}
[Control APIs.yml](<../../../.gitbook/assets/Control APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Control APIs.yml" path="/api/control/{component}/{id}" method="post" %}
[Control APIs.yml](<../../../.gitbook/assets/Control APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Control APIs.yml" path="/api/control" method="post" %}
[Control APIs.yml](<../../../.gitbook/assets/Control APIs.yml>)
{% endopenapi %}

## Command APIs

Command APIs provide ability to centrally issue commands to all runners for their administration. All commands are processed and responded to in an async manner, so their acknowledgement and results are typically reported using a separate API request.

Similar to events, commands also have request metadata, which is enriched using same approach as gateway APIs, with headers and cookies for authentication. Users need to have roles specified for gateway services used by commands to be able to issue them.

{% openapi src="../../../.gitbook/assets/Command APIs.yml" path="/api/command/" method="post" %}
[Command APIs.yml](<../../../.gitbook/assets/Command APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Command APIs.yml" path="/api/command/{path}" method="post" %}
[Command APIs.yml](<../../../.gitbook/assets/Command APIs.yml>)
{% endopenapi %}

## Message APIs

Message APIs provide ability to send messages to any Kafka service and topic for adhoc use cases.

Messages are authenticated using request metadata generated from headers and cookies of the request. Users need to have roles specified for the gateway service specified to be able to send messages.

{% openapi src="../../../.gitbook/assets/Message APIs.yml" path="/api/send/{service}/{topic}/{key}" method="post" %}
[Message APIs.yml](<../../../.gitbook/assets/Message APIs.yml>)
{% endopenapi %}

{% openapi src="../../../.gitbook/assets/Message APIs.yml" path="/api/send/{service}/{topic}/mod/{mod}/{key}" method="post" %}
[Message APIs.yml](<../../../.gitbook/assets/Message APIs.yml>)
{% endopenapi %}
