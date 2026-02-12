---
description: >-
  Lister menu actions represent actions on a business domain, available through
  drop-down menu in listers
---

# Lister Menu Actions

## Refresh List

Refreshes current list of displayed records.

## Local Export List

Exports current filtered list of records directly through the UI as a JSON file with a single listing API call. This action is not used for exporting large list of records, since backend APIs do not return whole data set at once for such cases.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call for listing records.
* **Method:** REST method to use for calling API endpoint.
* **Extra Body:** Extra payload to add to the API call.
* **Close on Success:** Whether action dialog should close on success.&#x20;

## Local Import List

Imports list of records from a local JSON file line by line with backend API calls for each line. This action is not used for importing large list of records, since it would not be utilizing bulk import capabilities of the database.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call for saving each record.
* **Method:** REST method to use for calling API endpoint.
* **Extra Body:** Extra payload to add to the API call.
* **Close on Success:** Whether action dialog should close on success.&#x20;
* **As New:** Whether records should all be treated as new or could be used to update existing.
* **With ID:** Whether record IDs should be retained or automatically replaced with new IDs.

## Export List

Exports current filtered list of records calling a batch export process from backend. This action generates a process entry and produces a CSV or JSON file stored on server to be downloaded once the export operation is completed. This action is only used for exporting large list of records.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call for triggering export process.

## Import List

Imports list of records from a CSV or JSON file uploaded to the server. This action initiates a process entry which is finalized asynchronously once the upload is completed. This action is only used for importing large list of records.

This action has the following special properties:

* **URL:** URL path of the API endpoint to call for triggering import process.

## Query API List

Sends a request to given API endpoint and displays results in a table with provided column details.

This action has the following special properties in addition to [API action properties](./):

* **Columns:** List of table columns to display.
* **Style:** CSS style of table to display.

## Render Html List

Renders static HTML content produced by the backend API and using currently displayed list. API endpoint typically uses handlebars templates related to the current item.

This action expects HTML to be displayed as "html" element in API response.

This action has the following special properties in addition to [API action properties](./):

* **Filters:** List of filters to use for enriching requests sent for html retrieval.

## Render Template List

Renders dynamic HTML content based on the template provided using currently displayed list.

This action has the following special properties in addition to [API action properties](./):

* **Template:** Full template text to use for rendering data.
* **Filters:** List of filters to use for enriching requests sent for template data retrieval.
* **Data Type:** Format of content list received from API response to be rendered:
  * contents: List of {section, component, template, content}, adding template for each entry to base template and passing {component, content} as "\[section]-data" to handlebars.
  * data: No formatting, passing received list as is
  * templates: List of {id, data: {template\}}, adding template for each entry to base template&#x20;
* **Data Template Form:** Whether templates received from API response is already encapsulated as partial templates ("inline") or should be encapsulated automatically ("body")
* **Data Template Path:** For "templates" data type, the path for getting template from list of entries. Defaults to "data.template".

## Toggle Action List

Toggles a specific action on the current lister using a specific function its editor supports.

This action has the following special properties:

* **Function(fn):** Function to call on current lister (e.g. setSelectionMode on a selectable QueryTableLister).&#x20;
* **Arguments(args):** Parameters to pass on to called function (e.g. \["text"]).&#x20;

## Run Code List

Runs a custom JavaScript code with access to specific widget functions and data as props (e.g. props.appliedFilters).

This action has the following special properties:

* **Code:** JS code to execute.

## Set Router List

Sets the query parameters of current page, typically used for applying preset filters on listers.

This action has the following special properties:

* **Params:** Json object with parameter values (null values used for removing query parameters).
* **Reset:** Whether action should completely reset current query parameters or only update given params.
