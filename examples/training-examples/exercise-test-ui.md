---
description: >-
  This example describes how to create a new UI for managing contents of the new
  Test state.
---

# Exercise: Test UI

## Create a New UI

Open the [UI](../../design/user-interface/uis/) screen from your [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/ui. Click on 'Create New' button to create a new UI entry.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption><p>UI Editor</p></figcaption></figure>

## Configure UI & Lister

Set ID of the new UI as 'test', which will be used to define the URL path for accessing this new UI. Set name, status, id & name field values and switch 'Customizable ID' field as true.

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption><p>Test UI Configuration</p></figcaption></figure>

On the 'Lister' tab, select 'Menu' lister and enter a lister title, which will list all test entries on a simple navigation list.

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption><p>Test Lister Configuration</p></figcaption></figure>

## Add Editors

Switch to 'Tabs' tab, add a new tab named 'Definition' and add a new grid to this tab.

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption><p>Test Tab &#x26; Grid</p></figcaption></figure>

Click on 'Add' button to add the following new editor to this grid:

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption><p>Name Editor</p></figcaption></figure>

'Apply' and click on 'Save' button to create the simple UI.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption><p>Created Test UI</p></figcaption></figure>

## Create a New Source

Open the Source screen from your [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/source. Click on 'Create New' button to create a new Source entry.

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption><p>Test Source</p></figcaption></figure>

{% hint style="info" %}
You can also open 'dummy' source and select 'Duplicate' from its action menu to quickly create a replica and edit as well.
{% endhint %}

## Add to Training App

Open the [App](../../design/user-interface/apps.md) screen from your [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z) app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/app. Select already available 'Training' app and add a new menu entry for the 'Test' UI.

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption><p>Test Menu</p></figcaption></figure>

## Test Your UI

Navigating to \[UI\_SERVER]/app/train, you can now see your new UI in action.
