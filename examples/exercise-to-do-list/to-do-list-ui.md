---
description: >-
  Once your new runner is accessible through your gateway, you can create a
  dedicated app and UI for it.
---

# To-do List UI

## Open the App Screen

Open the [App ](../../design/user-interface/apps.md)screen from your [Design ](/broken/pages/R6CwbBwzD5B7a9dcS18z)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/app.

<figure><img src="../../.gitbook/assets/Todo_UI_App.png" alt=""><figcaption><p>App Screen</p></figcaption></figure>

## Create an App

Next, you will create a new App, which will have its own home page and route in admin UI.

Click on "CREATE NEW" button on top of the menu lister and enter "example" to ID field. This ID will be used in the app URL path.

Enter the following details:

<figure><img src="../../.gitbook/assets/Todo_UI_App_Define.png" alt=""><figcaption><p>App Definition Screen</p></figcaption></figure>

* **DEFINITION**
  * **App Name:** Example
  * **App Status:** ACTIVE
  * **Description (optional):** Application dedicated to list of examples.
  * **Home Image:** /image/configuration.png
  * **Home Title:** Example Navigator
  * **Home Description:** Everything you need to manage your example services.
* **MENUS**
  * **Label:** Todo
  * **Icon:** rules
  * **Path:** /common/todo

Save your new App and browse to https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/example. You should now see your new App home page as follows:

<figure><img src="../../.gitbook/assets/Todo_UI_App_Home.png" alt=""><figcaption><p>Example App Home Page</p></figcaption></figure>

## Create a Source

Click on the Source icon on navigation bar to switch to the [Source ](../../design/api-mapping/)screen on your [Design ](/broken/pages/R6CwbBwzD5B7a9dcS18z)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/source.

<figure><img src="../../.gitbook/assets/Todo_UI_Source.png" alt=""><figcaption><p>Source Screen</p></figcaption></figure>

Click on "CREATE NEW" button on top of the menu lister of Source screen and provide the unique identifier "todo" and enter the following details as the source information:

<figure><img src="../../.gitbook/assets/Todo_UI_Source_Define.png" alt=""><figcaption><p>Source Definition Screen</p></figcaption></figure>

* **Source Name:** Todo
* **Source Status:** ACTIVE
* **Urls:**
  * **Action:** default
  * **URL:** request/todo\_crud/todo

These settings ensure that our UI requests for todo are directed to /api/request/todo\_crud/todo endpoint we've created in previous sections.

{% hint style="info" %}
**Why do you need the Source?**

Rierino core platform supports hyper-distributed applications, where each API endpoint could be maintained independent of the rest and serviced over a completely different server or even cluster. Having source mappings allows us to isolate such distribution from end users. It also simplifies the process of changing endpoints centrally.

Even when a Source does not require custom URLs (e.g. uses standard request/crud/\[ui] endpoint), it is still mandatory to create a Source, as otherwise the platform will consider CRUD endpoint disabled for access.
{% endhint %}

## Create a Data Schema

Click on the Schema icon on navigation bar to switch to the [Schema ](../../design/data-schema/)screen on your [Design ](/broken/pages/R6CwbBwzD5B7a9dcS18z)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/schema.

<figure><img src="../../.gitbook/assets/Todo_UI_Schema.png" alt=""><figcaption><p>Schema Screen</p></figcaption></figure>

Click on "CREATE NEW" button on top of the menu lister of Schema screen and provide the unique identifier "todo" and enter the following details as the schema information:

<figure><img src="../../.gitbook/assets/Todo_UI_Schema_Define.png" alt=""><figcaption><p>Schema Definition Screen</p></figcaption></figure>

```json
{
	"title": "Todo",
	"description": "A todo definition",
	"type": "object",
	"properties": {
		"id": {
			"title": "Todo ID",
			"description": "The unique identifier for a todo",
			"type": "string",
			"readOnly": true
		},
		"data": {
			"title": "Todo Information",
			"description": "Properties of the todo",
			"type": "object",
			"properties": {
				"name": {
					"title": "Todo Name",
					"description": "Name of the todo",
					"type": "string"
				},
				"project": {
					"title": "Project",
					"description": "Related project for the todo",
					"type": "string"
				}
			}
		}
	}
}
```

After editing the definition, save your schema. Title and description information entered in this screen populate labels in UI screens.

{% hint style="info" %}
Our data schema definitions follow JSON Schema standards, providing ability to use them in other platforms as well.
{% endhint %}

## Create the UI

Click on the UI icon on navigation bar to switch to the [UI](../../design/user-interface/uis/) screen on your [Design ](/broken/pages/R6CwbBwzD5B7a9dcS18z)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/ui.

Click on "CREATE NEW" button on top of the menu lister of UI screen and provide the unique identifier "todo" and enter the following details as the UI information:

<figure><img src="../../.gitbook/assets/Todo_UI_UI_Define.png" alt=""><figcaption><p>UI Definition Screen</p></figcaption></figure>

* **DEFINITION**
  * **Name:** Todo
  * **Status:** ACTIVE
  * **Description (optional):** UI design for todo list management
  * **ID Field:** id
  * **Name Field:** data.name
* **LISTER**
  * **Lister:** Grouped
  * **Lister Title:** Todo List
  * **Group By:** data.project

Switch to the "TABS" tab and follow below steps:

1. Click on "Add Tab" to create a new tab.
2. Hover on the new tab label, click on edit icon and set tab label as "DEFINITION" on displayed dialog.
3. Click on "Add Grid" to create a new grid.
4. Click on "Add" to add a new field inside the grid.
5. Hover on the new field, click on edit icon and set its path to "data.name" and select "Text" widget for it on displayed dialog.
6. Click on "Add" to add another field inside the grid.
7. Hover on the new field, click on edit icon and set its path to "data.project" and select "Text" widget for it on displayed dialog.

Your screen should look like the following:

<figure><img src="../../.gitbook/assets/Todo_UI_UI_Tabs.png" alt=""><figcaption><p>UI Tabs Screen</p></figcaption></figure>

Save your UI design.

{% hint style="info" %}
**Why is UI separate from Schema?**

Keeping UI and Schema definitions separate allows us to create multiple visualizations for the same data, which can be required for different organizational teams (e.g. one having access to only small part of data). It also makes sure that even our administration dashboard can be used as headless, without relying on our internal UI configurations.
{% endhint %}

## Test your UI

For testing your new UI, you can simply go back to your Example app (e.g. https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/example) and click on the Todo card. This should display the todo entry we've created in previous steps and allow you to add, edit and remove new todo items, grouped by projects.

<figure><img src="../../.gitbook/assets/Todo_UI_Test.png" alt=""><figcaption><p>Todo Screen</p></figcaption></figure>
