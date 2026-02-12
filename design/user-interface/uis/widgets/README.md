---
description: >-
  Different widgets allow displaying and editing different data types and
  formats.
---

# Widgets

Widgets define the visual components which build up the editor screens. Each widget has a unique look and feel, and is able to work with certain types of data.

There are 5 categories of widgets:

* [**Value Widgets**](value-widgets.md)**:** Used for editing simple values such as text, numbers and boolean values
* [**Array Widgets**](array-widgets.md)**:** Used for editing arrays of values or objects
* [**Object Widgets**](object-widgets.md)**:** Used for editing Json objects
* [**Indirect Widgets**](indirect-widgets.md)**:** Used for editing data that is not within current element's scope
* [**Atom Widgets**](atom-widgets.md)**:** Used for display of simple data with traditional html elements

While all widgets have their own properties, most also support use of the following:

| Property                   | Definition                                                                                                                                                                              | Example                                              | Default |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | ------- |
| props.label                | Text label to display above the widget                                                                                                                                                  | Name                                                 | -       |
| props.description          | Descriptive text to display on hover of info icon                                                                                                                                       | This is the product name field                       | -       |
| props.example              | Example value to display on hover of info icon                                                                                                                                          | Cool Product                                         | -       |
| props.default              | Default value used if no value is entered to be displayed on hover of info icon (informational only)                                                                                    | Missing Name                                         | -       |
| props.snapshot             | Whether widget should be read-only, as a snapshot (true for simple read-only entries "styled" for formatted read-only entries, "create-styled" for allowing input for new records only) | styled                                               | -       |
| props.validated            | Whether JSON schema of widget data should be validated[^1]                                                                                                                              | true                                                 | -       |
| props.collapse             | Whether the widget should allow expand/collapse ("collapsed" or "expanded" to allow and start with a specific state)                                                                    | collapsed                                            | -       |
| props.extraEditors         | List of additional editors to 'attach' to the editor (instead of creating separate entries)                                                                                             | \[ {"ui": {"widget": "TextEditor", "props": {} } } ] | -       |
| containerProps.style       | CSS inline styling to apply to widget container                                                                                                                                         | {border: "1px solid"}                                | -       |
| containerProps.label.style | CSS inline styling to apply to widget's label wrapper                                                                                                                                   | -                                                    | -       |
| containerProps.input.style | CSS inline styling to apply to widget's input wrapper                                                                                                                                   | -                                                    | -       |
| props.dataPattern          | Jmespath pattern for read-only display of calculated value                                                                                                                              | join(' ', \['Hello', name])                          | -       |

## Sourced Widgets

Certain widgets load data from sources (such as options for a drop-down), and support the following common properties:

| Property              | Definition                                                                                           | Example                               | Default |
| --------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------- | ------- |
| props.source          | Name of data source mapping to use for fetching list of records                                      | attribute                             | -       |
| props.sourceFilters   | Mapping, path (or JMES pattern) pairs of record value based filters to apply on data source requests | \[{mapping: "productid", path: "id"}] | -       |
| props.filterField     | Path of data source records to apply static filter on                                                | status                                | -       |
| props.filterValue     | Local filter to apply on the received list                                                           | 'A'                                   | -       |
| props.filterValues    | List of local filters to apply on the received list                                                  | \['A', 'B']                           | -       |
| props.filterValuePath | Json path for record field to use apply on the received list                                         | data.category                         | -       |
| props.sortFields      | List of received fields to sort results on                                                           | \['data.priority']                    | -       |

[^1]: Does not stop UI from saving, but displays specific message on editor. For restricting save on invalid contents, please refer to validated property of the UI itself.
