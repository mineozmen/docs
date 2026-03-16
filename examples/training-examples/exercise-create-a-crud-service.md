---
description: Add a new MongoDB-backed state to Train CRUD and expose it at /test.
---

# Exercise: Test State

This exercise extends the existing **Train CRUD** runner with a new state. The result is a new CRUD endpoint under the runner. The endpoint reads and writes to a MongoDB collection automatically.

### Before you start

* You can access the **Devops** app.
* The training deployment is installed and `train_crud` is running.
* You know your API base URL: `https://[YOUR_ADMIN_API_DOMAIN]`.

### What you’ll build

* A new state with alias `test`.
* A new endpoint: `https://[YOUR_ADMIN_API_DOMAIN]/api/request/train_crud/test`

{% stepper %}
{% step %}
### Open the Runner screen

Open the [Runner](../../devops/microservices/runners/) screen from the [Devops](/broken/pages/PWyjQCLF01E9OngBbsr8) app.

Unless you changed routing, the UI is at `https://[YOUR_ADMIN_UI_DOMAIN]/app/devops/common/runner`.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption><p>Runner UI</p></figcaption></figure>
{% endstep %}

{% step %}
### Select Train CRUD

In the left runner list, filter and select **Train CRUD**.

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption><p>Train CRUD Runner</p></figcaption></figure>
{% endstep %}

{% step %}
### Add a new State element

Drag a **State** element (table icon) from the stencil onto the runner diagram.

Select the new element. Click the pencil icon to edit it.

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>New State Manager</p></figcaption></figure>
{% endstep %}

{% step %}
### Configure it as Generic Master + alias `test`

In the editor:

* Choose **Generic Master**.
* Set **alias** to `test`.

`Generic Master` creates a simple MongoDB-backed state manager on the `master` database. The **alias** becomes:

* the CRUD URL segment (`/test`)
* the MongoDB collection name (`test`)

This is the simplest “new CRUD resource” configuration. For advanced options, use the Devops docs referenced from the CRUD runner section.
{% endstep %}

{% step %}
### Wait for rebuild and test the endpoint

The training Train CRUD runner is typically configured to rebuild every \~30 seconds. After you save, give it a moment to apply the new state.

Then call:

`https://[YOUR_ADMIN_API_DOMAIN]/api/request/train_crud/test`

Expected result:

* HTTP `200 OK`
* Body: `{list: []}` (the collection starts empty)

Full list of supported CRUD operations and parameters is in [CRUD event runner](../../devops/microservices/runners/deploying-runners/spring-runners.md#crud-event-runner).
{% endstep %}
{% endstepper %}

### Troubleshooting

* **404 Not Found**: confirm the alias is `test` and that you saved the runner. Then wait for the rebuild interval.
* **500 errors**: `Generic Master` points to a MongoDB connection. Check the master DB is reachable.
* **Endpoint exists but returns nothing**: verify you are hitting `train_crud` (URL contains `/train_crud/`).
