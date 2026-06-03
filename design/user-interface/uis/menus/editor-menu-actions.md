---
description: >-
  Editor menus perform actions on the current item being edited and are
  accessible through drop-down menu of object editors.
---

# Editor Menu Actions

## Delete

Deletes currently displayed item.

## Duplicate

Copies currently displayed item as a new record with empty ID.

## Export

Exports currently displayed item to a local JSON file.

This action has the following special properties:

* **With Details:** Whether export should include internal details such as instance version and offsets.

## Import

Imports a single item from a local JSON file.

## Query API

Sends a request to given API endpoint with the current item data and displays results in a table with provided column details.

This action has the following special properties in addition to [API action properties](./):

* **Columns:** List of table columns to display.
* **Style:** CSS style of table to display.
* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Item As:** Field to send full item data on in request payload ($ sends item as the root payload, if empty, only ID field is sent).&#x20;

## Export API

Sends a request to given API endpoint with the current item data and exports results in a file with provided column details.

This action has the following special properties in addition to [API action properties](./):

* **Type:** Type of file to export.
* **Sheets:** Configuration of sheets & columns to export.&#x20;

## Call API

Sends a request to given API endpoint with the current item data with option to update current record.

This action has the following special properties in addition to [API action properties](./):

* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Item As:** Field to send full item data on in request payload ($ sends item as the root payload, if empty, only ID field is sent).&#x20;
* **Replace With:** Data element in response to use for replacing current value with ($ for root).
* **Replace Path:** Json path to update value on (e.g. child.value).
* **Refresh:** Whether editor should refresh itself upon receiving API response or not.
* **Input Contents:** Contents array for getting additional inputs to be sent along with item data.
* **Execute Condition:**  [Condition configuration](../extended-scope/conditional-display.md) to allow execution of API call (i.e. for validation of inputs before request).
* **Output Contents:** Contents array for displaying API response details.
* **Invalidate Tags:** List of state tags to invalidate (i.e. trigger refresh) upon API call completion (e.g. productList, productQuery), to automatically refresh listers and lookups if a state contents is changing.

## Call Process API

Sends a request to given API endpoint with the current item data with option to display process results.

This action has the following special properties in addition to [API action properties](./):

* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Type:** Action type (process, list) indicating how to process response data.&#x20;
* **List URL:** URL path of the API endpoint for getting process history for current record.
* **Check URL:** URL path of the API endpoint for getting current run status for current record.
* **Columns:** List of table columns to display.

## DQ API

Executes data quality check and displays results as a dialog. Detailed configuration of the data quality control parameters are set at the backend API flow.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call for data quality check on current record.

## Render Html

Renders static HTML content produced by the backend API and using currently displayed item's data. API endpoint typically uses handlebars templates related to the current item.

This action expects HTML to be displayed as "html" element in API response.

This action has the following special properties in addition to [API action properties](./):

* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Item As:** Field to send full item data on in request payload ($ sends item as the root payload, if empty, only ID field is sent).&#x20;
* **Filters:** List of filters to use for enriching requests sent for html retrieval.

## Render Template

Renders dynamic HTML content based on the template provided using currently displayed item's data.

This action has the following special properties in addition to [API action properties](./):

* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Item As:** Field to send full item data on in request payload ($ sends item as the root payload, if empty, only ID field is sent).&#x20;
* **Template Path:** JSON path to the item's data field which includes full template text.
* **Template:** Full template text to use for rendering data.
* **Filters:** List of filters to use for enriching requests sent for template data retrieval.
* **Data Type:** Format of content list received from API response to be rendered:
  * contents: List of {section, component, template, content}, adding template for each entry to base template and passing {component, content} as "\[section]-data" to handlebars.
  * data: No formatting, passing received list as is
  * templates: List of {id, data: {template\}}, adding template for each entry to base template&#x20;
* **Data Template Form:** Whether templates received from API response is already encapsulated as partial templates ("inline") or should be encapsulated automatically ("body")
* **Data Template Path:** For "templates" data type, the path for getting template from list of entries. Defaults to "data.template".

## Render Plasmic

Renders dynamic HTML content using a page built using Plasmic app and currently displayed item's data.

This action has the following special properties in addition to [API action properties](./):

* **Require ID:** Whether API call should require an item with ID already defined.
* **ID:** Field to send item ID on in request payload.
* **Item As:** Field to send full item data on in request payload ($ sends item as the root payload, if empty, only ID field is sent).&#x20;
* **Component Path:** JSON path to the item's data field which includes plasmic component name.
* **Component:** Plasmic component name to use for rendering data.
* **Filters:** List of filters to use for enriching requests sent for template data retrieval.

## Toggle Action

Toggles a specific action on the current item using a specific function its editor supports.

This action has the following special properties:

* **Function(fn):** Function to call on current item.&#x20;
* **Arguments(args):** Parameters to pass on to called function.&#x20;

## Run Code

Runs a custom JavaScript code with access to specific editor functions and data as props (e.g. props.item, props.editorEvents.onDelete).

This action has the following special properties:

* **Code:** JS code to execute.
