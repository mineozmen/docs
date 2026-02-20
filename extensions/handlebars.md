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

All helpers below are available on both server-side and client-side rendering.

### Logic & comparison

#### Negate boolean

**Signature:** `not(value: boolean) -> boolean`\
**Parameters:**

* `value` (boolean, required): Input boolean.

Returns the opposite of `value`.

#### Equals

**Signature:** `eq(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left` equals `right`.

#### Not equals

**Signature:** `neq(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left` does not equal `right`.

#### Greater than

**Signature:** `gt(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left > right`.

#### Greater than or equal

**Signature:** `gte(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left >= right`.

#### Less than

**Signature:** `lt(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left < right`.

#### Less than or equal

**Signature:** `lte(left: any, right: any) -> boolean`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `true` when `left <= right`.

#### Logical AND

**Signature:** `and(...values: boolean) -> boolean`\
**Parameters:**

* `values` (boolean, required): One or more boolean values.

Returns `true` when all `values` are `true`.

#### Logical OR

**Signature:** `or(...values: boolean) -> boolean`\
**Parameters:**

* `values` (boolean, required): One or more boolean values.

Returns `true` when any value in `values` is `true`.

#### Conditional value

**Signature:** `cond(test: boolean, ifTrue: any, ifFalse: any) -> any`\
**Parameters:**

* `test` (boolean, required): Condition to evaluate.
* `ifTrue` (any, required): Value to return when `test` is `true`.
* `ifFalse` (any, required): Value to return when `test` is `false`.

Returns `ifTrue` or `ifFalse` based on `test`.

### Arrays

#### Max value

**Signature:** `max(values: array) -> any`\
**Parameters:**

* `values` (array, required): Input array.

Returns the maximum value in `values`.

#### Min value

**Signature:** `min(values: array) -> any`\
**Parameters:**

* `values` (array, required): Input array.

Returns the minimum value in `values`.

#### Range

**Signature:** `range(end: number) -> array`\
**Parameters:**

* `end` (number, required): Exclusive end.

Creates an array with elements ranging from `0` to `end - 1`.

#### Slice array

**Signature:** `slice(list: array, start: number=0, end: number=null) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `start` (number, default: `0`): Start index (0-based).
* `end` (number, default: `null`): End index (exclusive). When `null`, slices to end of array.

Returns a sub-array from `start` to `end`.

#### Array size

**Signature:** `size(list: array) -> number`\
**Parameters:**

* `list` (array, required): Input array.

Returns the number of elements in `list`.

#### Build array

**Signature:** `arr(...values: any) -> array`\
**Parameters:**

* `values` (any, required): One or more values.

Converts `values` into an array.

#### First non-null value

**Signature:** `coalesce(...values: any) -> any`\
**Parameters:**

* `values` (any, required): One or more values.

Returns the first non-null value in `values`.

### Strings & text

#### String length

**Signature:** `len(value: string) -> number`\
**Parameters:**

* `value` (string, required): Input string.

Returns the length of `value`.

#### Split lines

**Signature:** `lines(text: string) -> array`\
**Parameters:**

* `text` (string, required): Input string.

Splits `text` by line breaks and returns an array of strings.

### Math

#### Math operation

**Signature:** `math(left: number, op: string, right: number) -> number`\
**Parameters:**

* `left` (number, required): Left operand.
* `op` (string, required): Operation (`+`, `-`, `/`, `*`, `%`).
* `right` (number, required): Right operand.

Applies `op` between `left` and `right`.

### JSON

#### JSON stringify

**Signature:** `fromJson(value: any) -> string`\
**Parameters:**

* `value` (any, required): Object/array/value to stringify.

Converts `value` into a JSON string.

#### JSON parse

**Signature:** `toJson(value: string) -> any`\
**Parameters:**

* `value` (string, required): JSON string.

Parses `value` into a JSON object/array/value.

### Markdown

#### Render markdown

**Signature:** `md(text: string) -> string`\
**Parameters:**

* `text` (string, required): Markdown input.

Converts markdown to an HTML string.

### JMESPath

#### Evaluate expression

**Signature:** `eval(input: object, expr: string) -> any`\
**Parameters:**

* `input` (object, required): Input object to evaluate against.
* `expr` (string, required): JMESPath expression.

Evaluates `expr` against `input`.

### Date & time

#### Format epoch milliseconds

**Signature:** `dateToString(epochMs: number, locale: string="en-US", pattern: string="MM-dd-yyyy") -> string`\
**Parameters:**

* `epochMs` (number, required): Epoch milliseconds.
* `locale` (string, default: `"en-US"`): Locale string.
* `pattern` (string, default: `"MM-dd-yyyy"`): Date format pattern.

Formats `epochMs` using `locale` and `pattern`.

#### Parse date string

**Signature:** `stringToDate(value: string, locale: string="en-US", pattern: string="MM-dd-yyyy") -> number`\
**Parameters:**

* `value` (string, required): Date string.
* `locale` (string, default: `"en-US"`): Locale string.
* `pattern` (string, default: `"MM-dd-yyyy"`): Date format pattern.

Parses `value` into epoch milliseconds.

### Data access

#### Lookup records by id

**Signature:** `dataLookup(source: string, ids: string | string[]) -> any`\
**Parameters:**

* `source` (string, required): Lookup source.
* `ids` (string | string\[], required): ID, list of IDs, or `"*"` for all.

Looks up and returns record(s) from `source` using `ids`.

* When `ids` is `"*"`, returns all records.
* When `ids` is an array, returns an array of records.
* Otherwise, returns a single record.

For client-side use, `source` must match a source record from the design application.

For server-side use, `source` must match a state name on the current runner.

{% hint style="warning" %}
This helper can read all runner states without checking access rights.

Limit usage to runners without sensitive states (e.g. customer data).
{% endhint %}

### Variables

#### Set contextual variable

**Signature:** `setVar(name: string, value: any) -> any`\
**Parameters:**

* `name` (string, required): Variable name.
* `value` (any, required): Value to assign.

Creates a contextual variable with the given name and value.

Use it to reuse calculated values without `with` expressions.

{% hint style="info" %}
Legacy naming and version notes:

* Before `0.5.0`, some client-side helpers were exposed with `rierino*` names:
  * `coalesce` → `rierinoCoalesce`
  * `lines` → `rierinoLines`
  * `fromJson` → `rierinoJson`
  * `md` → `rierinoMarkdown`
  * `eval` → `rierinoEval`
  * `dateToString` → `rierinoDate`
* `dataLookup` is available since `0.5.2`.
{% endhint %}

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
* **data-params**: Json formatted parameters to pass to the event (e.g. '{"value": 123}')
* **data-id** (optional): Identifier which allows passing additional values along with data-params (e.g. "ABC"). When defined, additional DOM elements are tagged with the same data-id value as well as the following attributes:
  * **data-path**: Json path to add additional data to
  * **data-type** (or type): Type of value to pass from the DOM element ("number", "json" options in addition to default text)

List of events that can be used with "data-event" depends on the type of component rendering handlebars template, with the following shared and component specific events available:

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

* **data-drag-value**: Defines the Json value to drag to a drop target (e.g. '{"id": "123"}')
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
