---
description: 'Build a minimal saga on Train RPC that returns {message: "Hello World"}.'
---

# Exercise: Hello World API

This exercise creates a tiny saga and exposes it as an API endpoint. You will run it through the **Train RPC** runner. The flow uses only three steps: `Start` → `Transform` → `Success`.

### Before you start

* You can access the **Devops** app and the **Saga** screen.
* The training deployment (including `train_rpc`) is installed and running.
* You know your API base URL: `https://[YOUR_ADMIN_API_DOMAIN]`.

### What you’ll build

* **Saga path**: `/HelloWorld`
* **Runner**: `train_rpc` (via **Allowed For: Train RPC**)
* **Response**: HTTP `200` with body `{message: "Hello World"}`

{% stepper %}
{% step %}
### Open the Saga screen

Open the [Saga](../../../devops/api-flows/) screen from the [Devops](/broken/pages/PWyjQCLF01E9OngBbsr8) app.

Unless you changed routing, the UI is at `https://[YOUR_ADMIN_UI_DOMAIN]/app/devops/common/saga`.

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Open.png" alt=""><figcaption><p>Saga Screen</p></figcaption></figure>
{% endstep %}

{% step %}
### Create a new saga

Click **CREATE NEW** in the left menu area. This clears the editor and opens a blank saga.

You’ll define the saga metadata first. Then you’ll design the step graph.

<figure><img src="../../../.gitbook/assets/Create_Button.png" alt=""><figcaption><p>Create New Button</p></figcaption></figure>
{% endstep %}

{% step %}
### Assign an ID

Click the pencil next to **ID:** and set a unique identifier.

For this exercise, use: `hello_world-0001`.

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_ID.png" alt=""><figcaption><p>Saga ID Assignment</p></figcaption></figure>

<details>

<summary>ID conventions (optional)</summary>

IDs can be auto-generated if an ID generator is configured. For core assets like sagas and queries, human-readable IDs make debugging and audits much easier.

{% hint style="info" %}
If you assign IDs manually, prefer alphanumerics plus `-` and `_`. For high-volume assets, consider a 4‑digit partition suffix (like `-0001`). For low-volume assets, you can skip partitioning.
{% endhint %}

</details>
{% endstep %}

{% step %}
### Define the saga (name, path, allowed runner)

Click the **Definition** icon (circled edit icon) to open saga metadata.

<figure><img src="../../../.gitbook/assets/Definition_Button.png" alt=""><figcaption><p>Definition Button</p></figcaption></figure>

Fill in these fields:

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Hello World Saga</p></figcaption></figure>

* **Saga Name:** `Hello World`
* **Saga Domain (optional):** `util`
* **Status:** `ACTIVE`
* **Saga Description (optional):** `Returns "Hello World"`
* **Saga Path:** `/HelloWorld`
* **Version:** `0`
* **Allowed For:** `Train RPC`
* **Auto Fail:** `true`

This means requests to `/HelloWorld` routed through `train_rpc` will execute this saga.

{% hint style="info" %}
**Allowed For** limits which runner can execute the saga. Leaving it empty allows any runner to execute it.
{% endhint %}
{% endstep %}

{% step %}
### Add steps (Start → Transform → Success)

Drag a **Start** step from the stencil.

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Start_Step.png" alt=""><figcaption><p>Saga Stencil</p></figcaption></figure>

Drag a **Transform** step and a **Success** step.

Your canvas should look like this:

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_All_Steps.png" alt=""><figcaption><p>Saga Steps</p></figcaption></figure>

Connect `Start` → `Transform`.

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Link.png" alt=""><figcaption><p>Saga Link</p></figcaption></figure>

Connect `Transform` → `Success`.

Your graph should look like this:

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Links.png" alt=""><figcaption><p>Saga Links</p></figcaption></figure>
{% endstep %}

{% step %}
### Configure the Transform step

The flow runs now, but it does not yet return `"Hello World"`. You’ll add a static value to the payload.

Select the [Transform](../../../devops/api-flows/configuring-saga-steps/transform-step/) step. Click its pencil icon to edit.

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Transform_Icon.png" alt=""><figcaption><p>Transform Step Icons</p></figcaption></figure>

Set these values:

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Transform_Define.png" alt=""><figcaption><p>Transform Definition Screen</p></figcaption></figure>

* **Step Name:** `Say Hello World`
* **Step Description (optional):** `This step adds a static "Hello World" message.`
* **Transform Class:** `Add Value to Payload`
* **Transform Parameters:**
  * **Key:** `value`
  * **Value:** `{message: "Hello World"}`

Click **Apply** to save the step configuration.

{% hint style="info" %}
**Why does Transform have a “Class”?**

Instead of adding a separate stencil shape for every transformation type, the step stays the same. You switch behavior by selecting a different **Class** (or **Action**) in the step config.

This makes it safer to evolve flows over time. You can change behavior without rebuilding the graph and breaking tracing continuity.
{% endhint %}

The step label should update on the canvas:

<figure><img src="../../../.gitbook/assets/Hello_World_Saga_Transform_Set.png" alt=""><figcaption><p>Defined Transform Step</p></figcaption></figure>
{% endstep %}

{% step %}
### Save the saga

Click **SAVE** in the top-right corner.

<figure><img src="../../../.gitbook/assets/Save_Button.png" alt=""><figcaption><p>Save Button</p></figcaption></figure>

Wait for the runner to pick up changes. In most training setups this is quick. If your environment has a reload interval, give it \~10–30 seconds.

You should see a confirmation notification.
{% endstep %}

{% step %}
### Test the endpoint

Call the API endpoint from your REST client:

`https://[YOUR_ADMIN_API_DOMAIN]/api/request/train_rpc/HelloWorld`

Expected result:

* HTTP `200 OK`
* Body: `{message: "Hello World"}`

{% hint style="info" %}
Remember the saga path is case-sensitive. Use `/HelloWorld` exactly as configured.
{% endhint %}
{% endstep %}
{% endstepper %}

### Troubleshooting

* **404 Not Found**: confirm **Saga Path** and that you saved the saga. Also confirm it’s allowed for **Train RPC**.
* **403/401**: gateway auth is blocking the request. Check your token/session.
* **Saga does not trigger**: confirm the request is routed through `train_rpc` (URL contains `/train_rpc/`).
