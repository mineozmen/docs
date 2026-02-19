---
description: This example can be viewed from UI and Source screens in Design app
---

# UI Example

This training UI is a small admin screen on top of the `dummy` data model. The data is served by the `train_crud` runner. It is a quick way to see how a UI, a source mapping, and a backend endpoint fit together.

You can browse the configuration in the Design app:

* **UIs**: the screen layout, lister, editors, and widgets.
* **Source**: the API endpoint mapping used by the UI.
* **Apps**: a minimal `training` app that exposes only this screen in its menu.

If you are new to UI building, skim [Apps](../../../design/user-interface/apps.md), [UIs](../../../design/user-interface/uis/), and [API Mapping](../../../design/api-mapping/) first.

### Record editor (tabs + widget types)

The record editor uses tabs to group fields. It combines a few common widget types. You can compare how each widget reads and writes values.

This example includes:

* a text editor for simple string fields
* a select editor backed by option values
* a Markdown editor for rich text
* a JSON editor for the full record payload

<figure><img src="../../../.gitbook/assets/image (44).png" alt=""><figcaption><p>Tabs Example</p></figcaption></figure>

### Lister (columns + filters)

The list view uses a table lister. The visible columns are configured in the UI definition. The same goes for which filter inputs are available to users.

This is the part to copy when you want a “browse + search + click to edit” admin experience. See [Listers](../../../design/user-interface/uis/listers.md) for the available types and options.

<figure><img src="../../../.gitbook/assets/image (45).png" alt=""><figcaption><p>Lister Example</p></figcaption></figure>

### Source mapping (CRUD via `train_crud/dummy`)

On the Source screen, the `train_crud/dummy` endpoint is set as the UI’s data source. This enables standard CRUD behavior without writing custom API calls per action. The UI can list records, open a record, create, update, and delete using the default method mappings.

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption><p>Source Example</p></figcaption></figure>

### App packaging (`training`)

A small `training` app is included as a container for the menu and navigation. It exposes only the `dummy` UI. This is the simplest pattern for shipping a focused admin experience to a specific user group.

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption><p>Training App Design</p></figcaption></figure>

### Run it

Navigate to `[{UI_SERVER}]/app/train` to open the app. You should see the lister first. From there you can filter, open a record, edit fields, and save changes.

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption><p>Training App</p></figcaption></figure>

{% hint style="info" %}
If the screen is empty or CRUD operations fail, verify the `train_crud` runner and gateway route are reachable first. The UI is only a client. It depends on the backend endpoint.
{% endhint %}
