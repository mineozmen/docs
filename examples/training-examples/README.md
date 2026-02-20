---
description: >-
  Standard Rierino installations include training entries which can be used as
  starter templates.
icon: books
---

# Training Examples

Training examples ship with a standard Core installation. Use them as starter templates. Use them to validate your setup end-to-end.

Most examples are intentionally small. They show one idea at a time. You can copy the pattern into real services.

{% hint style="info" %}
This section covers **Core** examples. Core is available to all users.

For Core vs Core+ feature comparison, see [Rierino Packages](../../troubleshooting/rierino-packages.md).
{% endhint %}

### What’s included

You get three sets of assets. They map to the three main “build surfaces” in Rierino.

<table data-view="cards"><thead><tr><th>Title</th><th data-card-target data-type="content-ref">Target</th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td>API Flow Examples</td><td><a href="api-flow-examples.md">api-flow-examples.md</a></td><td data-object-fit="contain"><a href="../../.gitbook/assets/Nav_saga.svg">Nav_saga.svg</a></td></tr><tr><td>Microservice Examples</td><td><a href="microservice-examples.md">microservice-examples.md</a></td><td data-object-fit="contain"><a href="../../.gitbook/assets/Nav_runner.svg">Nav_runner.svg</a></td></tr><tr><td>UI Example</td><td><a href="ui-example.md">ui-example.md</a></td><td data-object-fit="contain"><a href="../../.gitbook/assets/Nav_ui.svg">Nav_ui.svg</a></td></tr></tbody></table>

### Recommended path (30–60 minutes)

Do these in order. Each step builds on the previous one.

1. Build a minimal API flow: [Exercise: Hello World API](exercise-hello-world-api.md)
2. Expose a new CRUD collection: [Exercise: Test State](exercise-test-state.md)
3. Add a UI on top of that CRUD endpoint: [Exercise: Test UI](exercise-test-ui.md)

{% hint style="info" %}
If you already have a working environment, start with **Hello World API**. It is the fastest routing smoke test.
{% endhint %}

### Where to find the training assets

#### Devops → Sagas (API Flows)

Open the **Devops** app, then **Saga**.

Training sagas are grouped under the `training` domain. Start with:

* `/train_ping` (echo / smoke test)
* `/train_hello` (transform → response)
* `/train_get` and `/train_set` (read/write against state)

Full list and screenshots: [API Flow Examples](api-flow-examples.md).

#### Devops → Runners + Deployments (Microservices)

Open the **Devops** app, then **Runner** and **Deployment**.

The training deployment typically includes:

* `train_rpc`: executes sagas as APIs
* `train_crud`: generic CRUD microservice
* `train_cdc`: listens to DB changes and triggers a saga

Details and videos: [Microservice Examples](microservice-examples.md).

#### Design → UIs + Source + Apps (Admin UI)

Open the **Design** app.

The built-in training UI shows how to wire a lister + editor to a CRUD backend:

* **UI**: screen layout (tabs, widgets, lister columns)
* **Source**: API mapping (typically `train_crud/dummy`)
* **App**: menu packaging (a minimal `training` app)

Walkthrough: [UI Example](ui-example.md).

### API endpoints cheat sheet

Use these patterns when testing from Postman or curl. Replace `YOUR_ADMIN_API_DOMAIN` with your gateway domain.

```
# Run a saga (RPC runner)
https://[YOUR_ADMIN_API_DOMAIN]/api/request/train_rpc/<SAGA_PATH>

# CRUD collection endpoint (CRUD runner)
https://[YOUR_ADMIN_API_DOMAIN]/api/request/train_crud/<ALIAS>
```

{% hint style="warning" %}
Sagas are case-sensitive. Runner configs may reload on a short interval (often \~10–30 seconds).
{% endhint %}

### Next step

Once these patterns feel familiar, build something end-to-end with the larger guided exercise:

* [In-depth Exercise](../in-depth-exercise/)
