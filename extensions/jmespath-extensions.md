---
description: >-
  Both admin UI and runners have custom functions added to standard JMESPath
  library
icon: brackets-curly
---

# JMESPath Extensions

The following functions can be used in addition to specification provided by JMESPath language, to support common use cases:

## Implementation Domain

### Server Side

These functions are available wherever JMESPath expressions are used by runners (e.g. transform steps, input/output patterns).

### Client Side

These functions are available in [conditional display](https://claude.ai/design/user-interface/uis/extended-scope/conditional-display.md), eval value widget, [table editor](https://claude.ai/design/user-interface/uis/widgets/array-widgets.md#table-editor) pattern cells as well as name & icon field definitions of [listers](https://claude.ai/design/user-interface/uis/listers.md). They are not supported in other JMESPath use cases.

### Batch

These functions are available wherever JMESPath expressions are used by Python runners and libraries (e.g. ML training steps, python event handlers).

{% hint style="info" %}
Customization on the client side includes async functions, which are not applicable for certain use cases, which is why these functions are not used in all UI elements.
{% endhint %}

Some functions are only available on batch, client or server side, which are marked as follows:

**\[C]:** Available on Client-Side.

**\[S]:** Available on Server-Side.

**\[B]:** Available on Batch.

**Unmarked:** Available on all.

{% hint style="warning" %}
Server-side JMESPath implementation returns null when the input node is null, regardless of the expression provided (including constant value expressions), due to Java JMESPath library implementation logic.

This applies to all areas where JMESPath is used (i.e. transform steps, event input and output patterns).
{% endhint %}

{% hint style="info" %}
**Reading the examples:** Each function has an example showing the input record, the expression, and the result. Literal values in expressions use standard JMESPath syntax: `'text'` is a string, and backticks hold JSON values such as numbers, booleans, arrays and objects (for example `` `2` ``, `` `true` ``, `` `["a","b"]` ``). Example timestamps use UTC unless a time zone is given.
{% endhint %}

## Functions

### Arrays

#### Merge arrays

**Signature:** `add_all(left: array, right: array) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.

Merges `left` and `right` and returns a new array.

<details>

<summary>Example</summary>

Input:

```json
{
  "defaultTags": ["new", "featured"],
  "customTags": ["summer", "sale"]
}
```

Expression:

```
add_all(defaultTags, customTags)
```

Output:

```json
["new", "featured", "summer", "sale"]
```

</details>

#### Cartesian product

**Signature:** `cartesian(lists: array) -> array`\
**Parameters:**

* `lists` (array, required): Array of arrays.

Creates the cartesian product of `lists`.

<details>

<summary>Example</summary>

Generate all size and color combinations for product variants.

Input:

```json
{
  "sizes": ["S", "M"],
  "colors": ["red", "blue"]
}
```

Expression:

```
cartesian([sizes, colors])
```

Output:

```json
[
  ["S", "red"],
  ["S", "blue"],
  ["M", "red"],
  ["M", "blue"]
]
```

</details>

#### Distinct values

**Availability:** \[S, C]\
**Signature:** `distinct(values: array) -> array`\
**Parameters:**

* `values` (array, required): Input array.

Returns distinct values in `values`.

<details>

<summary>Example</summary>

Input:

```json
{
  "orders": [
    { "id": "o1", "country": "PT" },
    { "id": "o2", "country": "TR" },
    { "id": "o3", "country": "PT" }
  ]
}
```

Expression:

```
distinct(orders[].country)
```

Output:

```json
["PT", "TR"]
```

</details>

#### Read array element by index

**Availability:** \[S, C]\
**Signature:** `element_at(list: array, index: number) -> any`\
**Parameters:**

* `list` (array, required): Input array.
* `index` (number, required): Index to read.

Returns the element at `index`. An index outside the array raises an error rather than returning null.

<details>

<summary>Example</summary>

Unlike `items[2]`, the index can come from the input record.

Input:

```json
{
  "steps": ["draft", "review", "approved", "published"],
  "currentStep": 2
}
```

Expression:

```
element_at(steps, currentStep)
```

Output:

```json
"approved"
```

</details>

#### Group array

**Availability:** \[S, C]\
**Signature:** `group_by(list: array, field: string) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `field` (string, required): Field to group by.

Groups entries and returns `[{ id: "", list: [] }]`. The order of the groups is not guaranteed.

<details>

<summary>Example</summary>

Input:

```json
{
  "orders": [
    { "id": "o1", "status": "paid" },
    { "id": "o2", "status": "pending" },
    { "id": "o3", "status": "paid" }
  ]
}
```

Expression:

```
group_by(orders, 'status')
```

Output:

```json
[
  {
    "id": "paid",
    "list": [
      { "id": "o1", "status": "paid" },
      { "id": "o3", "status": "paid" }
    ]
  },
  {
    "id": "pending",
    "list": [
      { "id": "o2", "status": "pending" }
    ]
  }
]
```

</details>

#### Add index to array entries

**Signature:** `index_array(list: array, field: string="index", start: number=0) -> array`\
**Parameters:**

* `list` (array, required): Array of objects.
* `field` (string, default: `"index"`): Field name to add.
* `start` (number, default: `0`): Start index.

Adds an index field to each entry in `list`.

<details>

<summary>Example</summary>

Add a 1-based line number to order lines.

Input:

```json
{
  "lines": [
    { "sku": "p1", "qty": 2 },
    { "sku": "p2", "qty": 1 }
  ]
}
```

Expression:

```
index_array(lines, 'lineNo', `1`)
```

Output:

```json
[
  { "sku": "p1", "qty": 2, "lineNo": 1 },
  { "sku": "p2", "qty": 1, "lineNo": 2 }
]
```

</details>

#### Array intersection

**Availability:** \[S, C]\
**Signature:** `intersect(left: array, right: array) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.

Returns the intersection of `left` and `right`.

<details>

<summary>Example</summary>

Find which of the user's roles are allowed for an action.

Input:

```json
{
  "userRoles": ["editor", "viewer", "finance"],
  "allowedRoles": ["admin", "editor", "finance"]
}
```

Expression:

```
intersect(userRoles, allowedRoles)
```

Output:

```json
["editor", "finance"]
```

\{% hint style="info" %\} Combine with `length()` to check access: `` length(intersect(userRoles, allowedRoles)) > `0` `` returns `true`. \{% endhint %\}

</details>

#### Remove array entries

**Signature:** `remove_all(list: array, toRemove: array) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `toRemove` (array, required): Values to remove.

Removes all entries of `toRemove` from `list`.

<details>

<summary>Example</summary>

Input:

```json
{
  "channels": ["web", "mobile", "pos", "marketplace"],
  "disabledChannels": ["pos", "marketplace"]
}
```

Expression:

```
remove_all(channels, disabledChannels)
```

Output:

```json
["web", "mobile"]
```

</details>

#### Repeat object template

**Signature:** `repeat(template: object, list: array, keyField: string) -> array`\
**Parameters:**

* `template` (object, required): Base object to repeat.
* `list` (array, required): Array of values to add.
* `keyField` (string, required): Field name each value is written to.

Repeats `template` for each entry of `list`, writing the entry to `keyField`. Object entries are nested under `keyField`, not merged into the template.

<details>

<summary>Example</summary>

Build one update record per product ID.

Input:

```json
{
  "update": { "status": "archived", "updatedBy": "system" },
  "productIds": ["p1", "p2", "p3"]
}
```

Expression:

```
repeat(update, productIds, 'id')
```

Output:

```json
[
  { "status": "archived", "updatedBy": "system", "id": "p1" },
  { "status": "archived", "updatedBy": "system", "id": "p2" },
  { "status": "archived", "updatedBy": "system", "id": "p3" }
]
```

</details>

#### Chunk array

**Availability:** \[S]\
**Signature:** `split_array(list: array, size: number) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `size` (number, required): Max chunk size.

Splits `list` into a list of arrays, each up to `size` entries.

<details>

<summary>Example</summary>

Split IDs into batches of 2 for bulk API calls.

Input:

```json
{
  "ids": ["p1", "p2", "p3", "p4", "p5"]
}
```

Expression:

```
split_array(ids, `2`)
```

Output:

```json
[
  ["p1", "p2"],
  ["p3", "p4"],
  ["p5"]
]
```

</details>

#### Merge arrays by index

**Signature:** `merge_index(left: array, right: array, leftField: string=null, rightField: string=null) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.
* `leftField` (string, default: `null`): Field to nest `left` entry into.
* `rightField` (string, default: `null`): Field to nest `right` entry into.

Merges two arrays by index.

If `leftField` and `rightField` are set, entries are nested under those fields. Otherwise, fields are merged (spread) into a single entry. The result has one entry per `left` entry, so `right` must be at least as long as `left`.

<details>

<summary>Example</summary>

Input:

```json
{
  "products": [
    { "sku": "p1", "name": "Shirt" },
    { "sku": "p2", "name": "Hat" }
  ],
  "prices": [
    { "price": 25 },
    { "price": 10 }
  ]
}
```

Expression (spread fields):

```
merge_index(products, prices)
```

Output:

```json
[
  { "sku": "p1", "name": "Shirt", "price": 25 },
  { "sku": "p2", "name": "Hat", "price": 10 }
]
```

Expression (nested fields):

```
merge_index(products, prices, 'product', 'pricing')
```

Output:

```json
[
  { "product": { "sku": "p1", "name": "Shirt" }, "pricing": { "price": 25 } },
  { "product": { "sku": "p2", "name": "Hat" }, "pricing": { "price": 10 } }
]
```

</details>

### Objects & paths

#### Deep merge objects

**Signature:** `deep_merge(...objects: object) -> object`\
**Parameters:**

* `objects` (object, required): One or more objects.

Deep merges nested objects. Arrays are replaced, not concatenated. Use `merge()` for shallow merges.

<details>

<summary>Example</summary>

Apply user settings on top of defaults.

Input:

```json
{
  "defaults": { "settings": { "theme": "light", "language": "en" }, "pageSize": 20 },
  "user": { "settings": { "theme": "dark" } }
}
```

Expression:

```
deep_merge(defaults, user)
```

Output:

```json
{
  "settings": { "theme": "dark", "language": "en" },
  "pageSize": 20
}
```

\{% hint style="info" %\} With the shallow `merge(defaults, user)`, `settings` would be replaced entirely, giving `{"settings": {"theme": "dark"}, "pageSize": 20}`. \{% endhint %\}

</details>

#### Deep diff

**Availability:** \[S]\
**Signature:** `deep_diff(oldValue: any, newValue: any) -> array`\
**Parameters:**

* `oldValue` (any, required): Old record.
* `newValue` (any, required): New record.

Returns a nested list of differences with `"old"` vs `"new"` values.

<details>

<summary>Example</summary>

Detect what changed in a product update.

Input:

```json
{
  "before": { "name": "Shirt", "price": 25, "stock": { "warehouse": 10 } },
  "after": { "name": "Shirt", "price": 22, "stock": { "warehouse": 8 } }
}
```

Expression:

```
deep_diff(before, after)
```

Output:

```json
{
  "price": { "old": 25, "new": 22 },
  "stock": {
    "warehouse": { "old": 10, "new": 8 }
  }
}
```

</details>

#### Key/value to object

**Availability:** \[S, C]\
**Signature:** `k_vtoo(key: string, value: any) -> object`\
**Parameters:**

* `key` (string, required): Key to use.
* `value` (any, required): Value to assign.

Converts `(key, value)` to an object.

<details>

<summary>Example</summary>

Build an object whose field name comes from the data.

Input:

```json
{
  "locale": "pt",
  "title": "Camisa azul"
}
```

Expression:

```
k_vtoo(locale, title)
```

Output:

```json
{ "pt": "Camisa azul" }
```

</details>

#### List to object

**Signature:** `ltoo(pairs: array, keyField: string, valueFieldOrDropKey: string|boolean=null) -> object`\
**Parameters:**

* `pairs` (array, required): Array of objects.
* `keyField` (string, required): Field name holding the key.
* `valueFieldOrDropKey` (string or boolean, optional): If a string, the field whose value is used as the value. If `true`, the whole entry is used without `keyField`. When omitted, the whole entry is used as the value.

Converts a list of key-value pairs into an object.

<details>

<summary>Example</summary>

Index a product list by SKU for quick access.

Input:

```json
{
  "products": [
    { "sku": "p1", "name": "Shirt" },
    { "sku": "p2", "name": "Hat" }
  ]
}
```

Expression:

```
ltoo(products, 'sku')
```

Output:

```json
{
  "p1": { "sku": "p1", "name": "Shirt" },
  "p2": { "sku": "p2", "name": "Hat" }
}
```

Expression (value field):

```
ltoo(products, 'sku', 'name')
```

Output:

```json
{ "p1": "Shirt", "p2": "Hat" }
```

Expression (drop the key field):

```
ltoo(products, 'sku', `true`)
```

Output:

```json
{
  "p1": { "name": "Shirt" },
  "p2": { "name": "Hat" }
}
```

</details>

#### Object to list

**Signature:** `otol(obj: object, keyField: string, valueField: string) -> array`\
**Parameters:**

* `obj` (object, required): Input object.
* `keyField` (string, required): Field name to use for keys.
* `valueField` (string, required): Field name to use for values.

Converts an object to a list of key-value pairs.

<details>

<summary>Example</summary>

Turn a stock map into a list for a table.

Input:

```json
{
  "stock": { "p1": 12, "p2": 0, "p3": 5 }
}
```

Expression:

```
otol(stock, 'sku', 'quantity')
```

Output:

```json
[
  { "sku": "p1", "quantity": 12 },
  { "sku": "p2", "quantity": 0 },
  { "sku": "p3", "quantity": 5 }
]
```

</details>

#### Read object element by path

**Availability:** \[S, C]\
**Signature:** `get_element(obj: object, path: string) -> any`\
**Parameters:**

* `obj` (object, required): Object to read from.
* `path` (string, required): JSON path string.

Returns element at `path`.

<details>

<summary>Example</summary>

Read a field whose path is given in the input (e.g. a configurable mapping).

Input:

```json
{
  "fieldPath": "address.city",
  "customer": {
    "name": "Ana",
    "address": { "city": "Lisbon", "country": "PT" }
  }
}
```

Expression:

```
get_element(customer, fieldPath)
```

Output:

```json
"Lisbon"
```

</details>

#### Set object element by path

**Signature:** `set_element(obj: object, path: string, value: any) -> object`\
**Parameters:**

* `obj` (object, required): Object to update.
* `path` (string, required): JSON path to write.
* `value` (any, required): Value to set.

Adds/sets `value` at `path`.

<details>

<summary>Example</summary>

Input:

```json
{
  "product": { "sku": "p1", "pricing": { "list": 25 } },
  "salePrice": 20
}
```

Expression:

```
set_element(product, 'pricing.sale', salePrice)
```

Output:

```json
{
  "sku": "p1",
  "pricing": { "list": 25, "sale": 20 }
}
```

</details>

#### Remove object fields

**Availability:** \[S, C]\
**Signature:** `remove_elements(obj: object, fields: array) -> object`\
**Parameters:**

* `obj` (object, required): Object to modify.
* `fields` (array, required): Array of field names or paths (strings) to remove.

Removes all listed fields from `obj`. The original object is not modified.

<details>

<summary>Example</summary>

Strip sensitive fields before returning a user record.

Input:

```json
{
  "id": "u1",
  "email": "ana@example.com",
  "password": "********",
  "resetToken": "abc123"
}
```

Expression:

```
remove_elements(@, `["password", "resetToken"]`)
```

Output:

```json
{
  "id": "u1",
  "email": "ana@example.com"
}
```

</details>

#### Remove null fields

**Availability:** \[S, C]\
**Signature:** `remove_nulls(obj: object|array, removeEmptyStrings: boolean=false) -> object|array`\
**Parameters:**

* `obj` (object or array, required): Object or array to clean.
* `removeEmptyStrings` (boolean, default: `false`): Also remove empty string values.

Removes null fields (or null entries of an array) from `obj`. Optionally removes empty strings too. Returns null for other input types.

<details>

<summary>Example</summary>

Input:

```json
{
  "name": "Shirt",
  "color": null,
  "description": "",
  "price": 25
}
```

Expression:

```
remove_nulls(@)
```

Output:

```json
{ "name": "Shirt", "description": "", "price": 25 }
```

Expression (also removing empty strings):

```
remove_nulls(@, `true`)
```

Output:

```json
{ "name": "Shirt", "price": 25 }
```

</details>

#### Find JSON paths by regex

**Availability:** \[S]\
**Signature:** `find_paths(input: any, regex: string) -> array`\
**Parameters:**

* `input` (any, required): Record to search.
* `regex` (string, required): Regex to match against JSON paths.

Searches all JSON paths in `input` and returns matching paths.

<details>

<summary>Example</summary>

Find every email field in a record, wherever it is nested.

Input:

```json
{
  "name": "Ana",
  "billing": { "email": "billing@example.com" },
  "shipping": { "email": "ana@example.com", "city": "Lisbon" }
}
```

Expression:

```
find_paths(@, '.*email$')
```

Output:

```json
["billing.email", "shipping.email"]
```

</details>

#### Value type

**Availability:** \[S, C]\
**Signature:** `type(value: any) -> string`\
**Parameters:**

* `value` (any, required): Input value.

Returns type of `value`: `OBJECT`, `ARRAY`, `NUMBER`, `STRING`, `BOOLEAN`, `NULL`.

<details>

<summary>Example</summary>

Input:

```json
{
  "price": 25,
  "tags": ["sale"],
  "active": true
}
```

Expression:

```
{price: type(price), tags: type(tags), active: type(active), missing: type(discount)}
```

Output:

```json
{
  "price": "NUMBER",
  "tags": "ARRAY",
  "active": "BOOLEAN",
  "missing": "NULL"
}
```

</details>

### Dynamic expressions

#### Evaluate dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic(input: any, expr: string) -> any`\
**Parameters:**

* `input` (any, required): Input object passed to the expression.
* `expr` (string, required): JMESPath expression as a string.

Evaluates `expr` against `input`.

<details>

<summary>Example</summary>

Evaluate a rule that is stored as data (e.g. in a configuration record).

Input:

```json
{
  "rule": "order.total > `100` && order.country == 'PT'",
  "context": {
    "order": { "total": 150, "country": "PT" }
  }
}
```

Expression:

```
dynamic(context, rule)
```

Output:

```json
true
```

</details>

#### Map with dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic_repeat(input: any, list: array, expr: string) -> array`\
**Parameters:**

* `input` (any, required): Passed as `input`.
* `list` (array, required): Iteration list. Each entry is passed as `entry`.
* `expr` (string, required): JMESPath expression string.

Evaluates `expr` for each `{input, entry}` and returns the results as an array.

<details>

<summary>Example</summary>

Combine each list entry with a value from the parent record, which standard projections cannot reach.

Input:

```json
{
  "currency": "EUR",
  "prices": [
    { "sku": "p1", "amount": 25 },
    { "sku": "p2", "amount": 10 }
  ]
}
```

Expression:

```
dynamic_repeat(@, prices, '{sku: entry.sku, amount: entry.amount, currency: input.currency}')
```

Output:

```json
[
  { "sku": "p1", "amount": 25, "currency": "EUR" },
  { "sku": "p2", "amount": 10, "currency": "EUR" }
]
```

</details>

#### Filter with dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic_filter(input: any, list: array, expr: string) -> array`\
**Parameters:**

* `input` (any, required): Passed as `input`.
* `list` (array, required): Array to filter. Each entry is passed as `entry`.
* `expr` (string, required): Expression returning a boolean.

Filters `list` using the boolean result of `expr` for each `{input, entry}`

<details>

<summary>Example</summary>

Filter by a threshold that comes from the parent record.

Input:

```json
{
  "minStock": 5,
  "products": [
    { "sku": "p1", "stock": 2 },
    { "sku": "p2", "stock": 8 },
    { "sku": "p3", "stock": 5 }
  ]
}
```

Expression:

```
dynamic_filter(@, products, 'entry.stock >= input.minStock')
```

Output:

```json
[
  { "sku": "p2", "stock": 8 },
  { "sku": "p3", "stock": 5 }
]
```

</details>

### Strings & text

#### Replace substring

**Signature:** `replace(text: string, search: string, replacement: string) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `search` (string, required): Substring to replace.
* `replacement` (string, required): Replacement substring.

Replaces occurrences of `search` with `replacement` in `text`.

<details>

<summary>Example</summary>

Input:

```json
{ "sku": "TSHIRT-BLUE-XL" }
```

Expression:

```
replace(sku, '-', '_')
```

Output:

```json
"TSHIRT_BLUE_XL"
```

</details>

#### Split string

**Signature:** `split(text: string, delimiter: string) -> array`\
**Parameters:**

* `text` (string, required): Input string.
* `delimiter` (string, required): Delimiter, as a regular expression.

Splits `text` by `delimiter`. Trailing empty strings are dropped.

\{% hint style="warning" %\} The delimiter is treated as a regular expression, so special characters must be escaped: use `split(path, '\.')` or `split(value, '\|')`. Unescaped, `split('a.b', '.')` returns an empty array. \{% endhint %\}

<details>

<summary>Example</summary>

Input:

```json
{ "tags": "sale,new,summer" }
```

Expression:

```
split(tags, ',')
```

Output:

```json
["sale", "new", "summer"]
```

</details>

#### Substring

**Signature:** `substr(text: string, start: number, end: number=null) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `start` (number, required): Start index (0-based).
* `end` (number, default: `null`): End index (exclusive). When `null`, returns to end.

Returns a substring of `text`.

<details>

<summary>Example</summary>

Input:

```json
{ "orderNo": "2026-PT-000042" }
```

Expression:

```
{year: substr(orderNo, `0`, `4`), sequence: substr(orderNo, `8`)}
```

Output:

```json
{ "year": "2026", "sequence": "000042" }
```

</details>

#### Trim whitespace

**Availability:** \[S, C]\
**Signature:** `trim(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Returns trimmed `value`.

<details>

<summary>Example</summary>

Input:

```json
{ "name": "   Blue Shirt  " }
```

Expression:

```
trim(name)
```

Output:

```json
"Blue Shirt"
```

</details>

#### Lowercase

**Availability:** \[S, C]\
**Signature:** `to_lower(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Lowercases `value`.

<details>

<summary>Example</summary>

Input:

```json
{ "email": "Ana.Silva@Example.COM" }
```

Expression:

```
to_lower(email)
```

Output:

```json
"ana.silva@example.com"
```

</details>

#### Uppercase

**Availability:** \[S, C]\
**Signature:** `to_upper(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Uppercases `value`.

<details>

<summary>Example</summary>

Input:

```json
{ "countryCode": "pt" }
```

Expression:

```
to_upper(countryCode)
```

Output:

```json
"PT"
```

</details>

#### Slugify

**Signature:** `slug(text: string, transliterate: boolean=false) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `transliterate` (boolean, default: `false`): Use transliteration when true.

Converts `text` to a slug. It trims, lowercases, normalizes/transliterates, removes non-ASCII letters, replaces non-alphanumerics with `-`, and trims `-`.

<details>

<summary>Example</summary>

Input:

```json
{ "title": "  Crème Brûlée: Summer Edition! " }
```

Expression:

```
slug(title)
```

Output:

```json
"creme-brulee-summer-edition"
```

Example with transliteration (non-Latin text):

Input:

```json
{ "title": "Москва Tour" }
```

Expression:

```
{plain: slug(title), transliterated: slug(title, `true`)}
```

Output:

```json
{ "plain": "tour", "transliterated": "moskva-tour" }
```

</details>

#### URL encode

**Availability:** \[S, C]\
**Signature:** `encode_url(value: string) -> string`\
**Parameters:**

* `value` (string, required): Plain text.

URL-encodes `value`. On the server side, spaces are encoded as `+` (form encoding).

<details>

<summary>Example</summary>

Input:

```json
{ "filter": "size=M&color=red" }
```

Expression:

```
encode_url(filter)
```

Output:

```json
"size%3DM%26color%3Dred"
```

</details>

#### URL decode

**Availability:** \[S, C]\
**Signature:** `decode_url(value: string) -> string`\
**Parameters:**

* `value` (string, required): URL-encoded text.

Decodes URL-encoded text.

<details>

<summary>Example</summary>

Input:

```json
{ "filter": "size%3DM%26color%3Dred" }
```

Expression:

```
decode_url(filter)
```

Output:

```json
"size=M&color=red"
```

</details>

#### Base64 encode

**Signature:** `btoa(value: any) -> string`\
**Parameters:**

* `value` (any, required): Value to encode.

Encodes `value` into a base64 string.

<details>

<summary>Example</summary>

Build an HTTP Basic authorization header.

Input:

```json
{ "username": "user", "password": "pass" }
```

Expression:

```
join('', ['Basic ', btoa(join(':', [username, password]))])
```

Output:

```json
"Basic dXNlcjpwYXNz"
```

</details>

#### Base64 decode

**Signature:** `atob(value: string) -> string`\
**Parameters:**

* `value` (string, required): Base64-encoded string.

Decodes `value` from base64 and returns UTF-8 text.

<details>

<summary>Example</summary>

Input:

```json
{ "data": "aGVsbG8gd29ybGQ=" }
```

Expression:

```
atob(data)
```

Output:

```json
"hello world"
```

</details>

#### Regex match

**Availability:** \[C, B]\
**Signature:** `regex(value: string, pattern: string) -> any`\
**Parameters:**

* `value` (string, required): Input string.
* `pattern` (string, required): Regex pattern.

Returns regex match result for `value`.

<details>

<summary>Example</summary>

Extract the year and sequence from an order number.

Input:

```json
{ "orderNo": "ORD-2026-0042" }
```

Expression:

```
regex(orderNo, 'ORD-(\d{4})-(\d+)')
```

Output:

```json
["ORD-2026-0042", "2026", "0042"]
```

</details>

#### Base64 encode with options

**Availability:** \[S]\
**Signature:** `base64_encode(value: string, urlSafe: boolean=false, withoutPadding: boolean=false) -> string`\
**Parameters:**

* `value` (string, required): Text to encode (as UTF-8).
* `urlSafe` (boolean, default: `false`): Use the URL-safe alphabet (`-` and `_` instead of `+` and `/`).
* `withoutPadding` (boolean, default: `false`): Omit trailing `=` padding.

Encodes `value` into base64, with options for URL-safe output such as JWT segments.

<details>

<summary>Example</summary>

Input:

```json
{ "header": "{\"alg\":\"HS256\",\"typ\":\"JWT\"}" }
```

Expression:

```
base64_encode(header, `true`, `true`)
```

Output:

```json
"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9"
```

</details>

#### Base64 decode with options

**Availability:** \[S]\
**Signature:** `base64_decode(value: string, urlSafe: boolean=false) -> string`\
**Parameters:**

* `value` (string, required): Base64 text.
* `urlSafe` (boolean, default: `false`): `value` uses the URL-safe alphabet.

Decodes base64 `value` and returns UTF-8 text.

<details>

<summary>Example</summary>

Input:

```json
{ "segment": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" }
```

Expression:

```
to_json(base64_decode(segment, `true`)).alg
```

Output:

```json
"HS256"
```

</details>

#### Character from code point

**Availability:** \[S]\
**Signature:** `unicode(code: string) -> string`\
**Parameters:**

* `code` (string, required): Unicode code point in hexadecimal, without `U+`.

Returns the character for `code`.

<details>

<summary>Example</summary>

Input:

```json
{ "price": "25" }
```

Expression:

```
join('', [price, ' ', unicode('20AC')])
```

Output:

```json
"25 €"
```

</details>

### JSON & CSV

#### JSON parse

**Availability:** \[S, C]\
**Signature:** `to_json(value: string) -> any`\
**Parameters:**

* `value` (string, required): JSON text.

Parses JSON text into an object/array.

<details>

<summary>Example</summary>

Parse a JSON payload received as a string (e.g. a message body).

Input:

```json
{ "body": "{\"id\":\"p1\",\"qty\":2}" }
```

Expression:

```
to_json(body).qty
```

Output:

```json
2
```

</details>

#### JSON stringify

**Availability:** \[S, C]\
**Signature:** `from_json(value: object) -> string`\
**Parameters:**

* `value` (object, required): JSON object.

Converts a JSON object into a string. Server side accepts objects only; to stringify an array, wrap it first (e.g. `from_json({items: items})`).

<details>

<summary>Example</summary>

Input:

```json
{ "item": { "id": "p1", "qty": 2 } }
```

Expression:

```
from_json(item)
```

Output:

```json
"{\"id\":\"p1\",\"qty\":2}"
```

</details>

#### Parse CSV

**Availability:** \[S]\
**Signature:** `csv_to_json(csv: string, options: object=null) -> array`\
**Parameters:**

* `csv` (string, required): CSV input text.
* `options` (object, default: `null`): CSV parsing options. Uses [Apache Commons CSV `CSVFormat`](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html).

Parses CSV text into an array of records.

<details>

<summary>Example</summary>

Input:

```json
{ "file": "sku,qty\np1,5\np2,3" }
```

Expression:

```
csv_to_json(file)
```

Output:

```json
[
  { "sku": "p1", "qty": "5" },
  { "sku": "p2", "qty": "3" }
]
```

</details>

#### Print CSV

**Availability:** \[S]\
**Signature:** `json_to_csv(records: array, options: object=null) -> string`\
**Parameters:**

* `records` (array, required): Array to print.
* `options` (object, default: `null`): Printing options. Uses [Apache Commons CSV `CSVFormat`](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html).

Prints `records` as CSV text.

<details>

<summary>Example</summary>

Input:

```json
{
  "rows": [
    { "sku": "p1", "qty": 5 },
    { "sku": "p2", "qty": 3 }
  ]
}
```

Expression:

```
json_to_csv(rows)
```

Output:

```json
"sku,qty\r\np1,5\r\np2,3\r\n"
```

</details>

#### JSON to XML

**Availability:** \[S]\
**Signature:** `json_to_xml(value: object) -> string`\
**Parameters:**

* `value` (object, required): Object to convert.

Converts `value` into XML text. Returns null on error.

#### XML to JSON

**Availability:** \[S]\
**Signature:** `xml_to_json(value: string) -> any`\
**Parameters:**

* `value` (string, required): XML text.

Parses XML text into JSON. Returns null on error.

### Date & time

#### Current time

**Signature:** `now() -> number`

Returns current time in epoch milliseconds.

<details>

<summary>Example</summary>

Set an expiry time 24 hours from now (the output depends on when it runs).

Input:

```json
{ "token": "abc123" }
```

Expression:

```
{token: token, createdAt: now(), expiresAt: sum([now(), `86400000`])}
```

Output:

```json
{
  "token": "abc123",
  "createdAt": 1790332200000,
  "expiresAt": 1790418600000
}
```

\{% hint style="info" %\} On the server side, the input must not be null for `now()` to return a value (see the warning above). Pass any object, such as `{}`. \{% endhint %\}

</details>

#### Format date

**Signature:** `date_to_string(epochMs: number, pattern: string="MM-dd-yyyy") -> string`\
**Parameters:**

* `epochMs` (number, required): Epoch time in milliseconds.
* `pattern` (string, default: `"MM-dd-yyyy"`): Java date format pattern.

Formats `epochMs` using `pattern`, in the server's time zone. The example below assumes a server running in UTC.

<details>

<summary>Example</summary>

Input:

```json
{ "createdAt": 1790332200000 }
```

Expression:

```
{default: date_to_string(createdAt), custom: date_to_string(createdAt, 'yyyy-MM-dd HH:mm')}
```

Output:

```json
{ "default": "09-25-2026", "custom": "2026-09-25 10:30" }
```

</details>

#### Parse date

**Signature:** `string_to_date(value: string, pattern: string="MM-dd-yyyy") -> number`\
**Parameters:**

* `value` (string, required): Date text.
* `pattern` (string, default: `"MM-dd-yyyy"`): Java date format pattern.

Parses `value` into epoch milliseconds, in the server's time zone. Returns null if `value` does not match `pattern`. The example below assumes a server running in UTC.

<details>

<summary>Example</summary>

Input:

```json
{ "deliveryDate": "2026-09-25" }
```

Expression:

```
string_to_date(deliveryDate, 'yyyy-MM-dd')
```

Output:

```json
1790294400000
```

</details>

#### Cron time helper

**Availability:** \[S]\
**Signature:** `cron(pattern: string, action: string, timeZone: string, epochMs: number) -> number | boolean`\
**Parameters:**

* `pattern` (string, required): Cron pattern in 5-field UNIX format (`minute hour day-of-month month day-of-week`).
* `action` (string, required): One of `last`, `next`, `fromLast`, `toNext`, `match`.
* `timeZone` (string, required): Time zone ID. Use `"SYSTEM"` for server time zone.
* `epochMs` (number, required): Epoch time in milliseconds.

Processes `pattern` relative to `epochMs` using `timeZone`.

* `last`: epoch ms for last match before `epochMs`.
* `next`: epoch ms for next match after `epochMs`.
* `fromLast`: milliseconds since last match.
* `toNext`: milliseconds until next match.
* `match`: whether the pattern matches the given time.

<details>

<summary>Example</summary>

Work with a daily 09:00 (Lisbon) schedule, evaluated at 2026-09-25 10:30 UTC (11:30 in Lisbon).

Input:

```json
{
  "schedule": "0 9 * * *",
  "at": 1790332200000
}
```

Expression:

```
{
  last: cron(schedule, 'last', 'Europe/Lisbon', at),
  next: cron(schedule, 'next', 'Europe/Lisbon', at),
  fromLast: cron(schedule, 'fromLast', 'Europe/Lisbon', at),
  toNext: cron(schedule, 'toNext', 'Europe/Lisbon', at),
  match: cron(schedule, 'match', 'Europe/Lisbon', at)
}
```

Output:

```json
{
  "last": 1790323200000,
  "next": 1790409600000,
  "fromLast": 9000000,
  "toNext": 77400000,
  "match": false
}
```

Here `last` is 2026-09-25 09:00 Lisbon, `next` is 2026-09-26 09:00 Lisbon, `fromLast` is 2.5 hours and `toNext` is 21.5 hours. Quartz-style 6-field patterns (with seconds or `?`) are not supported.

</details>

### Crypto & hashing

#### Encrypt text

**Availability:** \[S]\
**Signature:** `encrypt(plainText: string, key: string, algorithm: string="AES/ECB/PKCS5Padding", keyAlgorithm: string="AES", iv: string=null, urlSafe: boolean=false, withoutPadding: boolean=false) -> string`\
**Parameters:**

* `plainText` (string, required): Plain text.
* `key` (string, required): Encryption key, used as UTF-8 bytes (16, 24 or 32 characters for AES).
* `algorithm` (string, default: `"AES/ECB/PKCS5Padding"`): Cipher algorithm.
* `keyAlgorithm` (string, default: `"AES"`): Key algorithm.
* `iv` (string, default: `null`): Initialization vector for CBC/GCM modes. When omitted in these modes, a random IV is generated and prepended to the output.
* `urlSafe` (boolean, default: `false`): Use URL-safe base64.
* `withoutPadding` (boolean, default: `false`): Omit base64 `=` padding.

Encrypts `plainText` using `key` and returns base64 text. Returns null on error.

\{% hint style="warning" %\} `AES/ECB/PKCS5Padding` is not recommended for high-security use cases. Use a stronger mode, such as `AES/GCM/NoPadding`. \{% endhint %\}

\{% hint style="info" %\} For structured payloads, pair this with `to_json()` / `from_json()`. \{% endhint %\}

<details>

<summary>Example</summary>

Input:

```json
{ "cardNo": "4111-1111-1111-1111" }
```

Expression:

```
encrypt(cardNo, '0123456789abcdef')
```

Output:

```json
"mNgzGbAEcN9vw9hiNQudMbJ5iTaPCcJ0ZW3DUzS7y1Y="
```

Example with a structured payload:

```
encrypt(from_json(@), '0123456789abcdef')
```

\{% hint style="info" %\} Keep keys out of expressions in real use, for example by reading them with `get_env()`. \{% endhint %\}

</details>

#### Decrypt text

**Availability:** \[S]\
**Signature:** `decrypt(cipherText: string, key: string, algorithm: string="AES/ECB/PKCS5Padding", keyAlgorithm: string="AES", iv: string=null, urlSafe: boolean=false, withoutPadding: boolean=false) -> string`\
**Parameters:**

* `cipherText` (string, required): Encrypted base64 text.
* `key` (string, required): Encryption key.
* `algorithm` (string, default: `"AES/ECB/PKCS5Padding"`): Cipher algorithm.
* `keyAlgorithm` (string, default: `"AES"`): Key algorithm.
* `iv` (string, default: `null`): Initialization vector for CBC/GCM modes. When omitted in these modes, the IV is read from the start of `cipherText` (as produced by `encrypt()`).
* `urlSafe` (boolean, default: `false`): `cipherText` uses URL-safe base64.
* `withoutPadding` (boolean, default: `false`): Kept for symmetry with `encrypt()`.

Decrypts `cipherText` using `key`. Returns null on error (e.g. wrong key).

\{% hint style="warning" %\} `AES/ECB/PKCS5Padding` is not recommended for high-security use cases. Use a stronger mode. \{% endhint %\}

<details>

<summary>Example</summary>

Input:

```json
{ "encrypted": "mNgzGbAEcN9vw9hiNQudMbJ5iTaPCcJ0ZW3DUzS7y1Y=" }
```

Expression:

```
decrypt(encrypted, '0123456789abcdef')
```

Output:

```json
"4111-1111-1111-1111"
```

</details>

#### Hash text

**Availability:** \[S]\
**Signature:** `hash(plainText: string, key: string, algorithm: string="SHA-256", iterations: number=1) -> string`\
**Parameters:**

* `plainText` (string, required): Plain text to hash.
* `key` (string, default: `plainText`): Key/salt string. When omitted, `plainText` itself is used.
* `algorithm` (string, default: `"SHA-256"`): Hash algorithm.
* `iterations` (number, default: `1`): Iteration count.

Hashes `plainText` using `key` and returns a hex string.

<details>

<summary>Example</summary>

Hash a password with a per-user salt before storing it.

Input:

```json
{ "password": "S3cret!", "salt": "k9Xq2LmP" }
```

Expression:

```
{salt: salt, passwordHash: hash(password, salt, 'SHA-256', `1000`)}
```

Output:

```json
{
  "salt": "k9Xq2LmP",
  "passwordHash": "ffa400764f2585fab38dc70e284661bbfcafd1351e3bb6d54a7a4a09d4e316d0"
}
```

</details>

#### Validate hash

**Availability:** \[S]\
**Signature:** `validate_hash(plainText: string, hashedText: string, key: string, algorithm: string="SHA-256", iterations: number=1) -> boolean`\
**Parameters:**

* `plainText` (string, required): Plain text to verify.
* `hashedText` (string, required): Hashed string to compare against.
* `key` (string, default: `plainText`): Key/salt string. When omitted, `plainText` itself is used.
* `algorithm` (string, default: `"SHA-256"`): Hash algorithm.
* `iterations` (number, default: `1`): Iteration count.

Validates `plainText` against `hashedText`.

<details>

<summary>Example</summary>

Check a login attempt against the stored hash. Use the same algorithm and iterations as when hashing.

Input:

```json
{
  "attempt": "S3cret!",
  "user": { "salt": "k9Xq2LmP", "passwordHash": "ffa400764f2585fab38dc70e284661bbfcafd1351e3bb6d54a7a4a09d4e316d0" }
}
```

Expression:

```
validate_hash(attempt, user.passwordHash, user.salt, 'SHA-256', `1000`)
```

Output:

```json
true
```

</details>

#### Generate random key

**Availability:** \[S, C]\
**Signature:** `salt_key(length: number) -> string`\
**Parameters:**

* `length` (number, required): Desired length.

Creates a secure random key string of `length` characters.

<details>

<summary>Example</summary>

Output is random.

Input:

```json
{ "userId": "u1" }
```

Expression:

```
{userId: userId, salt: salt_key(`16`)}
```

Output:

```json
{ "userId": "u1", "salt": "q8ZfK2mW0xLr7TbN" }
```

</details>

### Lookup & joins

#### Lookup / join

**Signatures:**

* **Availability:** \[S, B] `lookup(left: array, right: array, key: string, full: boolean=false) -> array`
* **Availability:** \[C] `lookup(source: string, id: string) -> any`

**Join arrays** (\[S, B])\
Joins `left` and `right` on field `key`.

* `full=false` performs an inner join: `left` entries with no match are dropped.
* `full=true` performs a left join: `left` entries with no match are kept as they are.

Matching entries are merged into one object (on conflicting fields, `right` wins). A `left` entry that matches several `right` entries produces one result per match. Unmatched `right` entries are never included.

**Client-side source lookup** (\[C])\
Looks up a record by `id` from a configured [source](https://claude.ai/design/api-mapping.md).

\{% hint style="info" %\} Client-side `lookup()` is only available in evaluation providers (not in Handlebars templates).

It is not implemented server-side to avoid unintended access to sensitive states. \{% endhint %\}

<details>

<summary>Example: join arrays</summary>

\[S, B]

Input:

```json
{
  "orders": [
    { "orderId": "o1", "customerId": "c1" },
    { "orderId": "o2", "customerId": "c9" }
  ],
  "customers": [
    { "customerId": "c1", "name": "Ana" },
    { "customerId": "c2", "name": "Mehmet" }
  ]
}
```

Expression (inner join):

```
lookup(orders, customers, 'customerId')
```

Output:

```json
[
  { "orderId": "o1", "customerId": "c1", "name": "Ana" }
]
```

Expression (keep unmatched orders):

```
lookup(orders, customers, 'customerId', `true`)
```

Output:

```json
[
  { "orderId": "o1", "customerId": "c1", "name": "Ana" },
  { "orderId": "o2", "customerId": "c9" }
]
```

</details>

<details>

<summary>Example: source lookup</summary>

\[C]

Input (current form record):

```json
{ "categoryId": "cat-shoes" }
```

Expression:

```
lookup('categories', categoryId).name
```

Output:

```json
"Shoes"
```

</details>

### Math & logic

\{% hint style="info" %\} On the server side, `divide`, `multiply`, `mod`, `power` and `minus` return decimal numbers, so whole results appear as `56.0` rather than `56`. Wrap them with `to_int()` when an integer is needed. \{% endhint %\}

#### Compare values

**Availability:** \[S, C]\
**Signature:** `compare(left: any, right: any) -> number`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `0` if equal, `-1` if `left < right`, `1` if `left > right`.

<details>

<summary>Example</summary>

Input:

```json
{ "stock": 3, "reorderLevel": 5 }
```

Expression:

```
compare(stock, reorderLevel)
```

Output:

```json
-1
```

</details>

#### Conditional value

**Availability:** \[S, C]\
**Signature:** `cond(test: boolean, ifTrue: any, ifFalse: any) -> any`\
**Parameters:**

* `test` (boolean, required): Condition to evaluate.
* `ifTrue` (any, required): Value to return if `test` is true.
* `ifFalse` (any, required): Value to return if `test` is false.

Returns `ifTrue` or `ifFalse` based on `test`.

<details>

<summary>Example</summary>

Input:

```json
{ "sku": "p1", "stock": 0 }
```

Expression:

```
{sku: sku, availability: cond(stock > `0`, 'In stock', 'Out of stock')}
```

Output:

```json
{ "sku": "p1", "availability": "Out of stock" }
```

</details>

#### Divide numbers

**Signature:** `divide(dividend: number, divisor: number) -> number | null`\
**Parameters:**

* `dividend` (number, required): Numerator.
* `divisor` (number, required): Denominator.

Divides `dividend` by `divisor`. Returns `null` if `divisor` is `0`.

<details>

<summary>Example</summary>

Average order value.

Input:

```json
{ "revenue": 90, "orderCount": 4 }
```

Expression:

```
divide(revenue, orderCount)
```

Output:

```json
22.5
```

With `"orderCount": 0`, the same expression returns `null` instead of failing.

</details>

#### Multiply numbers

**Signature:** `multiply(...values: number) -> number`\
**Parameters:**

* `values` (number, required): One or more factors.

Returns the product of all `values` (e.g. `left * right`).

<details>

<summary>Example</summary>

Line totals, then the order total.

Input:

```json
{
  "lines": [
    { "sku": "p1", "price": 12.5, "qty": 4 },
    { "sku": "p2", "price": 3, "qty": 2 }
  ]
}
```

Expression:

```
sum(lines[].multiply(price, qty))
```

Output:

```json
56.0
```

</details>

#### Modulo

**Signature:** `mod(value: number, base: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.
* `base` (number, required): Base.

Returns `value % base`. Returns `null` if `base` is `0`.

<details>

<summary>Example</summary>

How many items are left over after filling boxes of 6.

Input:

```json
{ "quantity": 17, "boxSize": 6 }
```

Expression:

```
mod(quantity, boxSize)
```

Output:

```json
5.0
```

</details>

#### Power

**Signature:** `power(value: number, exponent: number) -> number`\
**Parameters:**

* `value` (number, required): Base.
* `exponent` (number, required): Exponent.

Returns `value ^ exponent`.

<details>

<summary>Example</summary>

Exponential retry backoff (1000 ms × 2^attempt).

Input:

```json
{ "attempt": 3 }
```

Expression:

```
multiply(`1000`, power(`2`, attempt))
```

Output:

```json
8000.0
```

</details>

#### Negate number

**Signature:** `minus(value: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.

Returns `-value`.

<details>

<summary>Example</summary>

Record a refund as a negative amount.

Input:

```json
{ "refundAmount": 25 }
```

Expression:

```
{type: 'refund', amount: minus(refundAmount)}
```

Output:

```json
{ "type": "refund", "amount": -25.0 }
```

</details>

#### To integer

**Availability:** \[S, C]\
**Signature:** `to_int(value: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.

Returns the integer part of `value`, dropping the decimals (e.g. `4.7` → `4`, `-4.7` → `-4`).

<details>

<summary>Example</summary>

Input:

```json
{ "rating": 4.7 }
```

Expression:

```
to_int(rating)
```

Output:

```json
4
```

</details>

### IDs & randomness

#### Generate GUID

**Availability:** \[S, C]\
**Signature:** `guid() -> string`

Creates and returns a globally unique ID string.

<details>

<summary>Example</summary>

Output is random.

Input:

```json
{ "name": "New campaign" }
```

Expression:

```
{id: guid(), name: name}
```

Output:

```json
{ "id": "6512bd43d9caa6e02c990b0a", "name": "New campaign" }
```

</details>

#### Generate UUID

**Availability:** \[S, C]\
**Signature:** `uuid(name: string=null) -> string`\
**Parameters:**

* `name` (string, optional): When given, returns a name-based (version 3) UUID that is always the same for the same `name`.

Creates a universally unique ID value. Without `name`, a random (version 4) UUID is returned.

<details>

<summary>Example</summary>

Output is random.

Input:

```json
{ "event": "order_created" }
```

Expression:

```
{eventId: uuid(), event: event}
```

Output:

```json
{ "eventId": "3f2b8c1e-7a4d-4e9b-9c61-2d5f0a8e4b17", "event": "order_created" }
```

Example: Derive a stable ID from a business key, so re-processing the same record gives the same ID.

Input:

```json
{ "orderNo": "order-1001" }
```

Expression:

```
uuid(orderNo)
```

Output:

```json
"929211f1-2c9f-3014-b7a8-1243838a056c"
```

</details>

#### Random integer

**Availability:** \[S, C]\
**Signature:** `rand(min: number, max: number) -> number`\
**Parameters:**

* `min` (number, required): Minimum integer.
* `max` (number, required): Maximum integer.

Returns a random integer between `min` and `max`.

<details>

<summary>Example</summary>

Assign a user to one of 4 A/B test buckets. Output is random.

Input:

```json
{ "userId": "u1" }
```

Expression:

```
{userId: userId, bucket: rand(`1`, `4`)}
```

Output:

```json
{ "userId": "u1", "bucket": 3 }
```

</details>

### Environment

#### Read environment variable

**Availability:** \[S]\
**Signature:** `get_env(name: string) -> string | null`\
**Parameters:**

* `name` (string, required): Environment variable name.

Returns the environment variable value. Intended for server-side use only.

\{% hint style="warning" %\} Only allow-listed variables can be read; others return null. By default only `RIERINO_ENV` is allowed. Set the `RIERINO_SAFE_VARS` environment variable on the runner to a comma-separated list to allow others (this replaces the default list). \{% endhint %\}

<details>

<summary>Example</summary>

Input:

```json
{ "path": "/orders" }
```

Expression:

```
join('', [get_env('API_BASE_URL'), path])
```

Output (with `API_BASE_URL=https://api.example.com` and `RIERINO_SAFE_VARS=RIERINO_ENV,API_BASE_URL`):

```json
"https://api.example.com/orders"
```

</details>
