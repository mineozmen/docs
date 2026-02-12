---
description: Admin UI has custom functions added to standard Handlebars library
icon: code
---

# Handlebars

The following functions can be used in addition to specification provided by Handlebars language, to support common use cases:

## Implementation Domain

### Server Side

These functions are available for server side rendering using Handlebars template event handler.

### Client Side

These functions are available for Handlebars widgets and menus.

## Functions

### not(boolean)

Returns opposite of the given boolean value.

### eq(any, any)

Returns true if the provided two values (e.g. number, string) are equal to each other.

### neq(any, any)

Returns true if the provided two values (e.g. number, string) are not equal to each other.

### gt(any, any)

Returns true if the first provided value is greater than the second.

### gte(any, any)

Returns true if the first provided value is greater than or equal to the second.

### lt(any, any)

Returns true if the first provided value is less than the second.

### lte(any, any)

Returns true if the first provided value is less than or equal to the second.

### and(...boolean)

Returns true if all of the provided values are true.

### or(...boolean)

Returns true if any of the provided values is true.

### cond(boolean, any, any)

If a given condition value is true, returns the first given parameter, otherwise returns the second given parameter.

### max(array)

Returns the maximum value in provided array.

### min(array)

Returns the minimum value in provided array.

### range(number)

Creates an array with elements ranging from 0 to provided value-1.

### slice(array, number=0, number=array.length)

Returns array elements from start to end index.

### size(array)

Returns the number of elements in an array.

### len(string)

Returns the length of a given string.

### arr(...any)

Converts provided list of values to an array.

### coalesce(...any)

[Returns ](#user-content-fn-1)[^1]the first non-null element in an array.

### lines(string)

[Converts ](#user-content-fn-2)[^2]lines of string into an array of string.

### math(number, string, number)

Calculates given operation (+,-,/,\*,%) between two given numbers.

[Converts ](#user-content-fn-3)[^3]a given json object into string.

### toJson(string)

Converts a given string into json object.

### md(string)

[Converts ](#user-content-fn-4)[^4]provided markdown text into HTML string.

### eval(object, string)

[Evaluates ](#user-content-fn-5)[^5]a JMESPath expression string on a given object.

### dateToString(number, string="en-US", string="MM-dd-yyyy")

[Converts ](#user-content-fn-6)[^6]given epoch milliseconds into date with given locale and pattern strings.

### stringToDate(string, string="en-US", string="MM-dd-yyyy")

Converts given date string in given locale and pattern into epoch milliseconds.

### dataLookup(string, string | string\[])

[Looks up](#user-content-fn-7)[^7] and returns record(s) from a given source, using a given id or ids. If "\*" is given as the id, all records in the source are returned. If a string array is provided as the ids, their list is returned as an array. Otherwise, a single record for the provided id is returned.&#x20;

For client side, the source given must match a source record from design application.

For server side, the source given must match a state name on current runner.

{% hint style="warning" %}
As this helper gives access to all states on a runner without checking access rights, it is recommended to limit its use to runners that do not have access to sensitive states such as customer data for server-side use cases.
{% endhint %}

### setVar(string, any)

Creates a contectual variable with given name and value, typically used for storing and reusing calculated values without "with" expressions.

## Client-Side Actions

In addition to custom functions, client-side Handlebars displays support custom data-\* attributes, which help utilize built-in data editing and action triggering features.

### Data Editing

Mainly used in HandlebarsDisplay [widget](../design/user-interface/uis/widgets/object-widgets.md), following data tags allow editing record data and local state variables:

* **data-change-path**: Json path for the data change to apply (e.g. price.USD)
* **data-change-type** (optional): When set to "local", it sets a variable that is a local state for the handlebars element only. Otherwise, the value is set with an onChange request, effectively acting as a regular value editor

Value of the input DOM element which has "data-change-path" attribute is used for reflecting these changes.

### Click Events

Used across different widget and lister types, following data tags allow triggering built-in events from handlebars templates:

* **data-event**: Name of the event to trigger (e.g. "new", "delete")
* **data-params**: Json formatted parameters to pass to the event (e.g. '{"value": 123}')&#x20;
* **data-id** (optional): Identifier which allows passing additional values along with data-params (e.g. "ABC"). When defined, additional DOM elements are tagged with the same data-id value as well as the following attributes:
  * **data-path**: Json path to add additional data to
  * **data-type** (or type): Type of value to pass from the DOM element ("number", "json" options in addition to default text)

List of events that can be used with "data-event" depends on the type of component rendering handlebars template, with the following shared and component specific events available:&#x20;

#### Shared Events

* **set**: Triggers onChange event, with "{path, value}" format for data-params.
* **setLocal**: Sets a local state variable value, which can be used by the template, with "{path, value}" format for data-params.
* **sendMessage**: Triggers an information/error message with "{message, type, detail}" format for data-params.
* **api**: Triggers an API call with "{body, config}" format for data-params.
* **menu**: Triggers a menu action call, using "{action}" format for data-params using an existing menu action's ID.

#### HandlebarsLister Events

* **select**: Selects and displays details of a given item using an object editor dialog, with "{id}" format for data-params.
* **new**: Displays an empty object editor dialog to create a new record.
* **delete**: Deletes a given record with "{id}" format for data-params.
* **refresh**: Refreshes current list.
* **filter**: Applies given filter to the listing with data-params matching filter fields.

#### DependentHandlebarsLister Events

Inherits most of HandlebarsLister events, with the following additional events:

* **up**: Moves the current row up, updating its sort priority, if lister is configured to be sortable, with "{id}" format for data-params.
* **down**: Moves the current row down, updating its sort priority, if lister is configured to be sortable, with "{id}" format for data-params.
* **dnd**: Executes completion of a drag & drop event, with "id" and "beforeId" fields to be provided using data-id & data-path attributes.
* **api**: Triggers an API call, with similar structure as shared events, with the potion to send {format: "dnd"} in data-params, which allows sending sorted ids, mainly used for applying a drag & drop sort activity.

### Drag & Drop Events

Handlebars components support [draggable](https://html.spec.whatwg.org/multipage/dnd.html#the-draggable-attribute) attribute, which is enriched with following custom data attributes for built-in dnd interactions:

* **data-drag-value**: Defines the Json value to drag to a drop target (e.g. '{"id": "123"}')&#x20;
* **data-drop-event**: Defines the event to trigger when an item is dropped in the zone (e.g. "dnd")
* **data-drop-value**: Defines the the Json value to pass to data-drop-event (e.g. '{"beforeId": "111}')
* **data-drop-params**: Defines the main event parameters to pass to data-drop-event

An example minimal template to use drag & drop behavior on a DependentHandlebarsLister:

```handlebars
<button {{#if (not dirtySort)}}disabled{{/if}} data-event="api" data-params='{ "format": "dnd", "config": {"url": "request/rpc/SortMyRecords"} }'>APPLY SORT</button>
{{#each sortedData}}
  <div data-drop-event="dnd"  data-drop-value='{"beforeId": "{{id}}"}'>DROP HERE</div>
  <div draggable data-drag-value='{"id": "{{id}}"}'>DRAG THIS</div>
{{/each}}
```

[^1]: Client-side naming as "rierinoCoalesce" before version 0.5.0.

[^2]: Client-side naming as "rierinoLines" before version 0.5.0.

[^3]: Client-side naming as "rierinoJson" before version 0.5.0.

[^4]: Client-side naming as "rierinoMarkdown" before version 0.5.0.

[^5]: Client-side naming as "rierinoEval" before version 0.5.0.

[^6]: Client-side naming as "rierinoDate" before version 0.5.0.

[^7]: since 0.5.2
