---
description: >-
  Both admin UI and runners have custom functions added to standard JMESPath
  library
icon: brackets-curly
---

# JMESPath

The following functions can be used in addition to specification provided by JMESPath language, to support common use cases:

## Implementation Domain

### Server Side

These functions are available wherever JMESPath expressions are used by runners (e.g. transform steps, input/output patterns).

### Client Side

These functions are available in [conditional display](../design/user-interface/uis/extended-scope/conditional-display.md), eval value widget, [table editor](../design/user-interface/uis/widgets/array-widgets.md#table-editor) pattern cells as well as name & icon field definitions of [listers](../design/user-interface/uis/listers.md). They are not supported in other JMESPath use cases.

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

## Functions

### Arrays

#### Merge arrays

**Signature:** `add_all(left: array, right: array) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.

Merges `left` and `right` and returns a new array.

#### Cartesian product

**Signature:** `cartesian(lists: array) -> array`\
**Parameters:**

* `lists` (array, required): Array of arrays.

Creates the cartesian product of `lists`.

#### Distinct values

**Availability:** \[S, C]\
**Signature:** `distinct(values: array) -> array`\
**Parameters:**

* `values` (array, required): Input array.

Returns distinct values in `values`.

#### Read array element by index

**Availability:** \[S, C]\
**Signature:** `element_at(list: array, index: number) -> any`\
**Parameters:**

* `list` (array, required): Input array.
* `index` (number, required): Index to read.

Returns the element at `index`.

#### Group array

**Availability:** \[S, C]\
**Signature:** `group_by(list: array, field: string) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `field` (string, required): Field to group by.

Groups entries and returns `[{ id: "", list: [] }]`.

#### Add index to array entries

**Signature:** `index_array(list: array, field: string="index", start: number=0) -> array`\
**Parameters:**

* `list` (array, required): Array of objects.
* `field` (string, default: `"index"`): Field name to add.
* `start` (number, default: `0`): Start index.

Adds an index field to each entry in `list`.

#### Array intersection

**Availability:** \[S, C]\
**Signature:** `intersect(left: array, right: array) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.

Returns the intersection of `left` and `right`.

#### Remove array entries

**Signature:** `remove_all(list: array, toRemove: array) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `toRemove` (array, required): Values to remove.

Removes all entries of `toRemove` from `list`.

#### Repeat object template

**Signature:** `repeat(template: object, list: array, keyField: string) -> array`\
**Parameters:**

* `template` (object, required): Base object to repeat.
* `list` (array, required): Array of values or objects to merge in.
* `keyField` (string, required): Field name to use when merging scalar entries.

Repeats `template` for each entry of `list`.

#### Chunk array

**Availability:** \[S]\
**Signature:** `split_array(list: array, size: number) -> array`\
**Parameters:**

* `list` (array, required): Input array.
* `size` (number, required): Max chunk size.

Splits `list` into a list of arrays, each up to `size` entries.

#### Merge arrays by index

**Signature:** `merge_index(left: array, right: array, leftField: string=null, rightField: string=null) -> array`\
**Parameters:**

* `left` (array, required): First array.
* `right` (array, required): Second array.
* `leftField` (string, default: `null`): Field to nest `left` entry into.
* `rightField` (string, default: `null`): Field to nest `right` entry into.

Merges two arrays by index.

If `leftField` and `rightField` are set, entries are nested under those fields. Otherwise, fields are merged (spread) into a single entry.

### Objects & paths

#### Deep merge objects

**Signature:** `deep_merge(...objects: object) -> object`\
**Parameters:**

* `objects` (object, required): One or more objects.

Deep merges nested objects. Use `merge()` for shallow merges.

#### Deep diff

**Availability:** \[S]\
**Signature:** `deep_diff(oldValue: any, newValue: any) -> array`\
**Parameters:**

* `oldValue` (any, required): Old record.
* `newValue` (any, required): New record.

Returns a nested list of differences with `"old"` vs `"new"` values.

#### Key/value to object

**Availability:** \[S, C]\
**Signature:** `k_vtoo(key: string, value: any) -> object`\
**Parameters:**

* `key` (string, required): Key to use.
* `value` (any, required): Value to assign.

Converts `(key, value)` to an object.

#### List to object

**Signature:** `ltoo(pairs: array, keyField: string) -> object`\
**Parameters:**

* `pairs` (array, required): Array of key-value objects.
* `keyField` (string, required): Field name holding the key.

Converts a list of key-value pairs into an object.

#### Object to list

**Signature:** `otol(obj: object, keyField: string, valueField: string) -> array`\
**Parameters:**

* `obj` (object, required): Input object.
* `keyField` (string, required): Field name to use for keys.
* `valueField` (string, required): Field name to use for values.

Converts an object to a list of key-value pairs.

#### Read object element by path

**Availability:** \[S, C]\
**Signature:** `get_element(obj: object, path: string) -> any`\
**Parameters:**

* `obj` (object, required): Object to read from.
* `path` (string, required): JSON path string.

Returns element at `path`.

#### Set object element by path

**Signature:** `set_element(obj: object, path: string, value: any) -> object`\
**Parameters:**

* `obj` (object, required): Object to update.
* `path` (string, required): JSON path to write.
* `value` (any, required): Value to set.

Adds/sets `value` at `path`.

#### Remove object fields

**Availability:** \[S, C]\
**Signature:** `remove_elements(obj: object, fields: array) -> object`\
**Parameters:**

* `obj` (object, required): Object to modify.
* `fields` (array, required): Array of field names (strings) to remove.

Removes all listed fields from `obj`.

#### Remove null fields

**Availability:** \[S, C]\
**Signature:** `remove_nulls(obj: object, removeEmptyStrings: boolean=false) -> object`\
**Parameters:**

* `obj` (object, required): Object to clean.
* `removeEmptyStrings` (boolean, default: `false`): Also remove empty string values.

Removes null fields from `obj`. Optionally removes empty strings too.

#### Find JSON paths by regex

**Availability:** \[S]\
**Signature:** `find_paths(input: any, regex: string) -> array`\
**Parameters:**

* `input` (any, required): Record to search.
* `regex` (string, required): Regex to match against JSON paths.

Searches all JSON paths in `input` and returns matching paths.

#### Value type

**Availability:** \[S, C]\
**Signature:** `type(value: any) -> string`\
**Parameters:**

* `value` (any, required): Input value.

Returns type of `value`: `OBJECT`, `ARRAY`, `NUMBER`, `STRING`, `BOOLEAN`, `NULL`.

### Dynamic expressions

#### Evaluate dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic(input: any, expr: string) -> any`\
**Parameters:**

* `input` (any, required): Input object passed to the expression.
* `expr` (string, required): JMESPath expression as a string.

Evaluates `expr` against `input`.

#### Map with dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic_repeat(input: any, list: array, expr: string) -> array`\
**Parameters:**

* `input` (any, required): Passed as `input`.
* `list` (array, required): Iteration list. Each entry is passed as `entry`.
* `expr` (string, required): JMESPath expression string.

Evaluates `expr` for each `{input, entry}` and returns the results as an array.

#### Filter with dynamic expression

**Availability:** \[S, C]\
**Signature:** `dynamic_filter(input: any, list: array, expr: string) -> array`\
**Parameters:**

* `input` (any, required): Passed as `input`.
* `list` (array, required): Array to filter. Each entry is passed as `entry`.
* `expr` (string, required): Expression returning a boolean.

Filters `list` using the boolean result of `expr` for each `{input, entry}`&#x20;

### Strings & text

#### Replace substring

**Signature:** `replace(text: string, search: string, replacement: string) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `search` (string, required): Substring to replace.
* `replacement` (string, required): Replacement substring.

Replaces occurrences of `search` with `replacement` in `text`.

#### Split string

**Signature:** `split(text: string, delimiter: string) -> array`\
**Parameters:**

* `text` (string, required): Input string.
* `delimiter` (string, required): Delimiter.

Splits `text` by `delimiter`.

#### Substring

**Signature:** `substr(text: string, start: number, end: number=null) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `start` (number, required): Start index (0-based).
* `end` (number, default: `null`): End index (exclusive). When `null`, returns to end.

Returns a substring of `text`.

#### Trim whitespace

**Availability:** \[S, C]\
**Signature:** `trim(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Returns trimmed `value`.

#### Lowercase

**Availability:** \[S, C]\
**Signature:** `to_lower(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Lowercases `value`.

#### Uppercase

**Availability:** \[S, C]\
**Signature:** `to_upper(value: string) -> string`\
**Parameters:**

* `value` (string, required): Input string.

Uppercases `value`.

#### Slugify

**Signature:** `slug(text: string, transliterate: boolean=false) -> string`\
**Parameters:**

* `text` (string, required): Input string.
* `transliterate` (boolean, default: `false`): Use transliteration when true.

Converts `text` to a slug. It trims, lowercases, normalizes/transliterates, removes non-ASCII letters, replaces non-alphanumerics with `-`, and trims `-`.

#### URL encode

**Availability:** \[S, C]\
**Signature:** `encode_url(value: string) -> string`\
**Parameters:**

* `value` (string, required): Plain text.

URL-encodes `value`.

#### URL decode

**Availability:** \[S, C]\
**Signature:** `decode_url(value: string) -> string`\
**Parameters:**

* `value` (string, required): URL-encoded text.

Decodes URL-encoded text.

#### Base64 encode

**Signature:** `btoa(value: any) -> string`\
**Parameters:**

* `value` (any, required): Value to encode.

Encodes `value` into a base64 string.

#### Base64 decode

**Signature:** `atob(value: string) -> string`\
**Parameters:**

* `value` (string, required): Base64-encoded string.

Decodes `value` from base64 and returns UTF-8 text.

#### Regex match

**Availability:** \[C, B]\
**Signature:** `regex(value: string, pattern: string) -> any`\
**Parameters:**

* `value` (string, required): Input string.
* `pattern` (string, required): Regex pattern.

Returns regex match result for `value`.

### JSON & CSV

#### JSON parse

**Availability:** \[S, C]\
**Signature:** `to_json(value: string) -> any`\
**Parameters:**

* `value` (string, required): JSON text.

Parses JSON text into an object/array.

#### JSON stringify

**Availability:** \[S, C]\
**Signature:** `from_json(value: any) -> string`\
**Parameters:**

* `value` (any, required): JSON object or array.

Converts a JSON object/array into a string.

#### Parse CSV

**Availability:** \[S]\
**Signature:** `csv_to_json(csv: string, options: object=null) -> array`\
**Parameters:**

* `csv` (string, required): CSV input text.
* `options` (object, default: `null`): CSV parsing options. Uses [Apache Commons CSV `CSVFormat`](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html).

Parses CSV text into an array of records.

#### Print CSV

**Availability:** \[S]\
**Signature:** `json_to_csv(records: array, options: object=null) -> string`\
**Parameters:**

* `records` (array, required): Array to print.
* `options` (object, default: `null`): Printing options. Uses [Apache Commons CSV `CSVFormat`](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html).

Prints `records` as CSV text.

### Date & time

#### Current time

**Signature:** `now() -> number`

Returns current time in epoch milliseconds.

#### Format date

**Signature:** `date_to_string(epochMs: number, pattern: string="MM-dd-yyyy") -> string`\
**Parameters:**

* `epochMs` (number, required): Epoch time in milliseconds.
* `pattern` (string, default: `"MM-dd-yyyy"`): Java date format pattern.

Formats `epochMs` using `pattern`.

#### Parse date

**Signature:** `string_to_date(value: string, pattern: string="MM-dd-yyyy") -> number`\
**Parameters:**

* `value` (string, required): Date text.
* `pattern` (string, default: `"MM-dd-yyyy"`): Java date format pattern.

Parses `value` into epoch milliseconds.

#### Cron time helper

**Availability:** \[S]\
**Signature:** `cron(pattern: string, action: string, timeZone: string, epochMs: number) -> number | boolean`\
**Parameters:**

* `pattern` (string, required): Cron pattern.
* `action` (string, required): One of `last`, `next`, `fromLast`, `toNext`, `match`.
* `timeZone` (string, default: `"SYSTEM"`): Time zone ID. Use `"SYSTEM"` for server time zone.
* `epochMs` (number, required): Epoch time in milliseconds.

Processes `pattern` relative to `epochMs` using `timeZone`.

* `last`: epoch ms for last match before `epochMs`.
* `next`: epoch ms for next match after `epochMs`.
* `fromLast`: milliseconds since last match.
* `toNext`: milliseconds until next match.
* `match`: whether the pattern matches the given time.

### Crypto & hashing

#### Encrypt text

**Availability:** \[S]\
**Signature:** `encrypt(plainText: string, key: string, algorithm: string="AES/ECB/PKCS5Padding", keyAlgorithm: string="AES") -> string`\
**Parameters:**

* `plainText` (string, required): Plain text.
* `key` (string, required): Encryption key.
* `algorithm` (string, default: `"AES/ECB/PKCS5Padding"`): Cipher algorithm.
* `keyAlgorithm` (string, default: `"AES"`): Key algorithm.

Encrypts `plainText` using `key`.

{% hint style="warning" %}
`AES/ECB/PKCS5Padding` is not recommended for high-security use cases. Use a stronger mode.
{% endhint %}

{% hint style="info" %}
For structured payloads, pair this with `to_json()` / `from_json()`.
{% endhint %}

#### Decrypt text

**Availability:** \[S]\
**Signature:** `decrypt(cipherText: string, key: string, algorithm: string="AES/ECB/PKCS5Padding", keyAlgorithm: string="AES") -> string`\
**Parameters:**

* `cipherText` (string, required): Encrypted text.
* `key` (string, required): Encryption key.
* `algorithm` (string, default: `"AES/ECB/PKCS5Padding"`): Cipher algorithm.
* `keyAlgorithm` (string, default: `"AES"`): Key algorithm.

Decrypts `cipherText` using `key`.

{% hint style="warning" %}
`AES/ECB/PKCS5Padding` is not recommended for high-security use cases. Use a stronger mode.
{% endhint %}

#### Hash text

**Availability:** \[S]\
**Signature:** `hash(plainText: string, key: string, algorithm: string="SHA-256", iterations: number=1) -> string`\
**Parameters:**

* `plainText` (string, required): Plain text to hash.
* `key` (string, required): Key/salt string.
* `algorithm` (string, default: `"SHA-256"`): Hash algorithm.
* `iterations` (number, default: `1`): Iteration count.

Hashes `plainText` using `key`.

#### Validate hash

**Availability:** \[S]\
**Signature:** `validate_hash(plainText: string, hashedText: string, key: string, algorithm: string="SHA-256", iterations: number=1) -> boolean`\
**Parameters:**

* `plainText` (string, required): Plain text to verify.
* `hashedText` (string, required): Hashed string to compare against.
* `key` (string, required): Key/salt string.
* `algorithm` (string, default: `"SHA-256"`): Hash algorithm.
* `iterations` (number, default: `1`): Iteration count.

Validates `plainText` against `hashedText`.

#### Generate random key

**Availability:** \[S, C]\
**Signature:** `salt_key(length: number) -> string`\
**Parameters:**

* `length` (number, required): Desired length.

Creates a secure random key string of `length` characters.

### Lookup & joins

#### Lookup / join

**Signatures:**

* **Availability:** \[S, B] `lookup(left: array, right: array, key: string, full: boolean=false) -> array`
* **Availability:** \[C] `lookup(source: string, id: string) -> any`

**Join arrays** (\[S, B])\
Joins `left` and `right` on field `key`.

* `full=false` performs a left join.
* `full=true` performs a full join.

**Client-side source lookup** (\[C])\
Looks up a record by `id` from a configured [source](../design/api-mapping/).

{% hint style="info" %}
Client-side `lookup()` is only available in evaluation providers (not in Handlebars templates).

It is not implemented server-side to avoid unintended access to sensitive states.
{% endhint %}

### Math & logic

#### Compare values

**Availability:** \[S, C]\
**Signature:** `compare(left: any, right: any) -> number`\
**Parameters:**

* `left` (any, required): First value.
* `right` (any, required): Second value.

Returns `0` if equal, `-1` if `left < right`, `1` if `left > right`.

#### Conditional value

**Availability:** \[S, C]\
**Signature:** `cond(test: boolean, ifTrue: any, ifFalse: any) -> any`\
**Parameters:**

* `test` (boolean, required): Condition to evaluate.
* `ifTrue` (any, required): Value to return if `test` is true.
* `ifFalse` (any, required): Value to return if `test` is false.

Returns `ifTrue` or `ifFalse` based on `test`.

#### Divide numbers

**Signature:** `divide(dividend: number, divisor: number) -> number | null`\
**Parameters:**

* `dividend` (number, required): Numerator.
* `divisor` (number, required): Denominator.

Divides `dividend` by `divisor`. Returns `null` if `divisor` is `0`.

#### Multiply numbers

**Signature:** `multiply(left: number, right: number) -> number`\
**Parameters:**

* `left` (number, required): First factor.
* `right` (number, required): Second factor.

Returns `left * right`.

#### Modulo

**Signature:** `mod(value: number, base: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.
* `base` (number, required): Base.

Returns `value % base`.

#### Power

**Signature:** `power(value: number, exponent: number) -> number`\
**Parameters:**

* `value` (number, required): Base.
* `exponent` (number, required): Exponent.

Returns `value ^ exponent`.

#### Negate number

**Signature:** `minus(value: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.

Returns `-value`.

#### To integer

**Availability:** \[S, C]\
**Signature:** `to_int(value: number) -> number`\
**Parameters:**

* `value` (number, required): Input number.

Returns the integer part of `value` (floor-like). Returns an int value.

### IDs & randomness

#### Generate GUID

**Availability:** \[S, C]\
**Signature:** `guid() -> string`

Creates and returns a globally unique ID string.

#### Generate UUID

**Availability:** \[S, C]\
**Signature:** `uuid() -> string`

Creates a universally unique ID value.

#### Random integer

**Availability:** \[S, C]\
**Signature:** `rand(min: number, max: number) -> number`\
**Parameters:**

* `min` (number, required): Minimum integer.
* `max` (number, required): Maximum integer.

Returns a random integer between `min` and `max`.

### Environment

#### Read environment variable

**Availability:** \[S]\
**Signature:** `get_env(name: string) -> string | null`\
**Parameters:**

* `name` (string, required): Environment variable name.

Returns the environment variable value. Intended for server-side use only.
