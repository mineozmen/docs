---
description: >-
  Certain widgets and menus have access to data elements outside their scope,
  using data context
---

# Data Context

As Rierino allows use of highly sophisticated and nested data models with arrays and objects, it is sometimes necessary to refer to data elements not directly edited by a widget (for example an item editor filtering its list of values based on the value of another editor).&#x20;

Example use cases include Item Editor, Button Action, Handlebars Display, Attribute Array Editor widgets as well as Call Value API and Set Value actions.

Such widgets and menus use path expressions to access data with the following conventions:

| Path Expression | Description                                                                       | Example     |
| --------------- | --------------------------------------------------------------------------------- | ----------- |
| $               | Get current list entry data                                                       | $           |
| \_              | Get current record data                                                           | \_          |
| #               | <p>Get current locale data</p><p>(of closest localized editor)</p>                | #           |
| $\[EXPRESSION]  | Get a specific data element using JSON path from list entry                       | $label      |
| \_\[EXPRESSION] | Get a specific data element using JSON path from record                           | \_data.name |
| #\[EXPRESSION]  | Get a specific data element using JSON path from locale data                      | #text       |
| \[EXPRESSION]   | Get a specific data element using JSON path (widget selects list entry or record) | data.name   |

Where "list entry" refers to the current element within an array (e.g. specific row inside a table editor), "record" refers to the root aggregate being currently edited (e.g. current product record) and "locale data" refers to the contents of a parent localized editor for the currently selected locale. &#x20;

{% hint style="info" %}
It is also possible to use JMESPath expressions as \[EXPRESSION] in the form of =\[JMES].
{% endhint %}
