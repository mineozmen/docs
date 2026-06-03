---
description: >-
  Widget menus are used to perform actions on individual data elements,
  accessible from icons inline with editor labels.
---

# Widget Menu Actions

## Copy Value

Copies current value of the editor as JSON to clipboard, which can be used for viewing contents of complex data or copying the same between different items.&#x20;

## Paste Value

Pastes current contents of clipboard as JSON to the editor, which can be used for importing partial complex data or pasting data copied from a different item.&#x20;

## Copy Local Value

Special to localized editor, this action copies currently displayed contents of the editor as JSON to clipboard.&#x20;

## Paste Local Value

Special to localized editor, this action pastes contents of the clipboard on currently displayed editor as JSON for replicating contents of a language or locale to another one.&#x20;

## Set Value

Sets value of the editor to a given constant JSON value or evaluated pattern, which can be used for clearing contents of complex data editors as well as setting them to a predefined or calculated value.&#x20;

This action has the following special properties:

* **Value:** JSON value (e.g. "Test") or a pattern (e.g. =join(' ', \[data.name, data.surname])). If not defined, this action clears contents of the editor.

## Open Link

Opens content from a new URL using currently displayed value.

This action has the following special properties:

* **URL Template:** URL path of the content to display with %%value%% replaced with currently displayed value.
* **Target:** Target window to display content on (e.g. \_blank).
* **Window Features:** Features of the window to display content on (e.g. noopener,noreferrer).

## Query Value API

Sends a request to given API endpoint with the current value and displays results in a table with provided column details.

This action has the following special properties in addition to [API action properties](./):

* **As:** Data element to send value in request (e.g. "value").
* **Columns:** List of table columns to display.
* **Style:** CSS style of table to display.

## Call Value API

Sends a request to given API endpoint with the current value and optionally replace current value with returned data.

This action has the following special properties in addition to [API action properties](./):

* **Require Value:** Whether editor should have some value for this action to be active.
* **As:** Data element to send editor value on API call.
* **Item Path:** Item expression for sending extra data (e.g. =id).
* **Item Path As:** Data element to send data calculated with item path (e.g. itemId).
* **Replace With:** Data element in response to use for replacing current value with ($ for root).
* **Replace Path:** Json path to update value on (e.g. child.value).
* **Refresh:** Whether editor should refresh itself upon receiving API response or not.
* **Input Contents:** Contents array for getting additional inputs to be sent along with editor value.
* **Execute Condition:**  [Condition configuration](../extended-scope/conditional-display.md) to allow execution of API call (i.e. for validation of inputs before request).
* **Output Contents:** Contents array for displaying API response details.

## Refresh Value

Refreshes contents or lookup values related to current editor (e.g. updating items listed from another source in a drop down).&#x20;

## Translate Value

Translates current value(s) displayed using a translation API endpoint.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call.
* **Method:** REST method to use for calling API endpoint.
* **Exclude:** List of JSON paths to exclude from translation (if value is an object).
* **Overwrite:** Whether to replace already existing language values or skip them in translation.

## Toggle Action Value

Toggles a specific action on the current value using a specific function its editor supports.

This action has the following special properties:

* **Function(fn):** Function to call on current editor.&#x20;
* **Arguments(args):** Parameters to pass on to called function.&#x20;

## Run Code Value

Runs a custom JavaScript code with access to specific widget functions and data as props (e.g. props.data).

This action has the following special properties:

* **Code:** JS code to execute.

## Export XLSX

Exports and downloads current editor data as an Excel file with styling.

This action has the following special properties:

* **Sheets:** List of sheets with column configurations and mappings to export.

Following alternatives also exist, for exporting data in different file formats:

* Export CSV
* Export XML
* Export Handlebars

## Import XLSX

Imports data from a local Excel file to an editor.

This action has the following special properties:

* **Sheets:** List of sheets with column mappings to import.

## Validate Handlebars

Validates the handlebars template in specific widget displaying error details on failure.

## Test Jmespath

Tests a JMESPath expression for a given sample input and displays result or error details.

This action has the following special properties:

* **Remote:** Whether to test using server-side engine instead of client-side. When true, API call special properties are also applicable, with request/rpc/ApplyJmesPath being used as the default test endpoint URL. &#x20;
