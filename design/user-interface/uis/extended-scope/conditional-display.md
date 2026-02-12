---
description: >-
  It is possible to display / hide widgets, menus or tabs using conditions based
  on current record's data.
---

# Conditional Display

Both UI tabs and widgets can be set to display themselves only when certain conditions are met, using one the following condition setting alternatives:

## Value Match

Used for simple use cases where contents are displayed based on value of a different widget.

| Field  | Description                      | Example     |
| ------ | -------------------------------- | ----------- |
| path   | JSON path of current record data | data.status |
| values | List of values allowing display  | \["A", "D"] |

## Pattern Calculation

Used for sophisticated conditions where calculations are required based on multiple elements.

| Field   | Description                               | Example                         |
| ------- | ----------------------------------------- | ------------------------------- |
| pattern | Evaluation pattern on current record data | =(data.price-data.salesPrice)>0 |

Evaluation pattern can be one of the following:

| Type          | Description                                | Example                         |
| ------------- | ------------------------------------------ | ------------------------------- |
| ==\[CONSTANT] | Constant value                             | ==true                          |
| =\[JMES]      | JMESPath expression on current record data | =(data.price-data.salesPrice)>0 |
| Other         | JSON path of current record data           | data.isActive                   |

## Complex Condition

Used for applying AND / OR logic on multiple conditions.

| Field      | Description                                     | Example                                      |
| ---------- | ----------------------------------------------- | -------------------------------------------- |
| operator   | Condition type                                  | AND                                          |
| statements | List of statements to combine with the operator | \[{"path": "data.status", "values": \["A"]}] |

{% hint style="info" %}
For the widget menus, widget's own data is passed as the current record.
{% endhint %}
