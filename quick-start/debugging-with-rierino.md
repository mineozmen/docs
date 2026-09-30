---
description: >-
  Built-in testing and debugging features give you visibility across all
  deployments
icon: bug
---

# Debugging with Rierino

Rierino includes several built-in helpers that make development and debugging easier, such as AI development agents and expression and template validators. For structured, end-to-end debugging beyond these, there are two main approaches: **audit runs** and **log monitoring**.

## Audit Runs

Audit runs are recommended for development environments. They let you review the inputs, outputs and error stack traces of a saga flow step by step.

You can run audits in two ways.

### Through API

Any gateway channel or path configured to allow auditing can be called through the special `/api/test` endpoints (for example, `/api/test/rpc/Ping`). These calls offer two things that regular `/api/request` endpoints don't:

1. **Unmodified payloads:** The request body is passed to backend services exactly as sent. This lets you imitate payloads from users or internal systems.
2. **Step-by-step details:** With the `rierino-audit-path` header, the response includes the input, output and metadata of every step executed in the saga flow, plus a short stack trace for any errors.

{% hint style="info" %}
If the backend service is configured to store audit results, the audit details are also saved in the database.
{% endhint %}

### Through Saga UI

If you'd rather not debug through code, the Saga UI gives you tools to run different audit scenarios and inspect how they executed.

<figure><img src="../.gitbook/assets/image (186).png" alt="Saga Audit Dialog"><figcaption><p>Saga Audit Dialog</p></figcaption></figure>

The Saga Audit dialog:

* Lists every audit run recorded for the current saga.
* Lets you start new audit scenarios.
* Offers AI assistance that explains error logs and recommends fixes to the saga flow.

You can also view audit results for a single step. This gives you quick access to the step's input data schema and shows how the step behaves with different data.

<figure><img src="../.gitbook/assets/image (187).png" alt="Step Audit Dialog"><figcaption><p>Step Audit Dialog</p></figcaption></figure>

## Log Monitoring

Audit runs are mainly for development and testing, so they don't show what happened in requests that weren't audited, including ones that failed. For those cases, Rierino writes detailed logs with a rich, customizable MDC (Mapped Diagnostic Context) that includes request details such as:

* Request ID
* Requester Detail
* API Gateway
* Runner
* Saga ID
* Step ID
* API Path
* Handler
* Action
* State / Domain

Logs also include the error details. At more detailed log levels, you can also trace the event payload at each saga step. Use this only in development, so that sensitive data doesn't end up in your logs.

You can send logs, traces and metrics to your organization's APM and SIEM tools through adapters or OpenTelemetry feeds. For setup details, see [Logs, Traces & Metrics configurations](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/administration/logs-and-traces-and-metrics).
