---
description: >-
  Selection menus perform actions on a selected list of items, typically
  iterating through the list.
---

# Selection Menu Actions

## Select All

Selects all currently listed records on screen.

## Unselect All

Clears current selection.

## Delete Selection

Deletes list of items selected one by one.

This action has the following special properties:

* **Keep Selection:** Whether to keep or unselect current selection list after the event.

## Local Export Selection

Exports contents of currently selected list of items, first retrieving their contents through an API call.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call (if no url is provided, uses current source's "list" entry).
* **Method:** REST method to use for calling API endpoint.
* **Extra Body:** Extra payload to add to the API call.
* **IDs:** Parameter to use for sending ids to the API call (defaults to "ids").
* **Keep Selection:** Whether to keep or unselect current selection list after the event.

## Call Selection API

Calls an API endpoint for each of the selected items one by one.

&#x20;This action has the following special properties in addition to [API action properties](./):

* **ID:** Parameter to use for sending each id to the API call (defaults to "id").
* **Keep Selection:** Whether to keep or unselect current selection list after the event.
* **Refresh:** Whether the list should be refreshed after API call or not.
* **Close on Success:** Whether the result screen should be automatically closed if all calls are successful.

## Patch Selection

Patches list of items selected one by one, first displaying a dialog to specify which fields should be updated.

&#x20;This action has the following special properties:

* **Schema ID:** Id of the schema to use for selecting data elements (optional, uses current UI's schema when not provided).
* **ID:** Parameter to use for sending each id to the API call (defaults to "id").
* **Keep Selection:** Whether to keep or unselect current selection list after the event.
* **Execute Condition:**  [Condition configuration](../extended-scope/conditional-display.md) to allow execution of API call (i.e. for validation of inputs before request).

{% hint style="info" %}
In addition to constant values, it is possible to use jmespath evaluations on existing data fields using "=" notation (e.g. =join(' ', \[data.name, data.surname]))
{% endhint %}

## Run Code Selection

Runs a custom JavaScript code with access to specific widget functions and data as props (e.g. props.selection).

This action has the following special properties:

* **Code:** JS code to execute.
