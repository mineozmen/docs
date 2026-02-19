---
description: Create a minimal UI and source mapping for the /test CRUD endpoint.
---

# Exercise: Test UI

This exercise creates a tiny admin UI for the `test` collection. It assumes you already created the backend CRUD endpoint in [Exercise: Test State](exercise-test-state.md).

### Before you start

* You can access the **Design** app.
* The `/test` CRUD endpoint exists under `train_crud`.
* You can reach the UI at `https://[YOUR_ADMIN_UI_DOMAIN]`.

### What you’ll build

* A new UI with ID `test`.
* A lister that shows test records.
* An editor with a single `name` field.
* A source mapping that points the UI to the `train_crud/test` endpoint.
* A menu entry under the `Training` app.

{% stepper %}
{% step %}
### Create a new UI

Open the [UI](../../design/user-interface/uis/) screen from the [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app.

Unless you changed routing, the UI is at `https://[YOUR_ADMIN_UI_DOMAIN]/app/design/common/ui`.

Click **Create New**.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption><p>UI Editor</p></figcaption></figure>
{% endstep %}

{% step %}
### Configure UI basics

Set the UI **ID** to `test`. This becomes the UI path segment.

Fill in name and status. Set the ID and name field mappings as shown.

Enable **Customizable ID**. This allows creating new records with custom IDs.

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption><p>Test UI Configuration</p></figcaption></figure>
{% endstep %}

{% step %}
### Configure the lister

Open the **Lister** tab.

Select the **Menu** lister. Set a title.

This lister type is simple. It works well for small datasets.

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption><p>Test Lister Configuration</p></figcaption></figure>
{% endstep %}

{% step %}
### Add the editor

Open the **Tabs** tab.

Add a tab named `Definition`. Add a grid inside it.

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption><p>Test Tab &#x26; Grid</p></figcaption></figure>

Click **Add** and create the `name` editor shown below.

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption><p>Name Editor</p></figcaption></figure>

Click **Apply**. Then click **Save**.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption><p>Created Test UI</p></figcaption></figure>
{% endstep %}

{% step %}
### Create a source mapping

Open the **Source** screen from the [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app.

Unless you changed routing, the UI is at `https://[YOUR_ADMIN_UI_DOMAIN]/app/design/common/source`.

Click **Create New**.

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption><p>Test Source</p></figcaption></figure>

Configure the source to target the `train_crud/test` endpoint. This is what connects your UI actions to CRUD calls.

{% hint style="info" %}
Fastest path is duplicating the existing `dummy` source. Then change any `dummy` path segments to `test`. Save as a new source.
{% endhint %}
{% endstep %}

{% step %}
### Add it to the Training app menu

Open the [App](../../design/user-interface/apps.md) screen from the [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app.

Unless you changed routing, the UI is at `https://[YOUR_ADMIN_UI_DOMAIN]/app/design/common/app`.

Select the existing **Training** app. Add a menu entry for the new `test` UI.

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption><p>Test Menu</p></figcaption></figure>
{% endstep %}

{% step %}
### Run it

Go to `[{UI_SERVER}]/app/train`.

Open the **Test** menu entry. Create a record. Save it.

You should see it appear in the lister afterwards.
{% endstep %}
{% endstepper %}

### Troubleshooting

* **Menu entry not visible**: confirm you edited the `Training` app. Save it.
* **Lister is empty after creating records**: confirm source points to `train_crud/test`.
* **Save fails**: confirm the backend `/test` endpoint exists. Retry after rebuild.
