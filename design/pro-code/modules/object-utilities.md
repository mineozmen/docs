---
description: A built-in module allows easy manipulation of JSON records
---

# Object utilities

```jsx
import objectutils from "operations/objectutils";
```

Path access, object shaping and expression evaluation. This is the helper you will reach for most often when working with record data.

> Call every method **on the object**, as in `objectutils.eval(...)`, not `const { eval } = objectutils`. These methods depend on their own context and will not work when destructured.

***

## Path syntax

Paths are dotted. Wrap a segment in square brackets when the segment itself contains a dot:

```
data.title
data.prices[tr-TR].amount
list.0.name
```

`splitPath(path, useIndex = false)` returns the segments. Pass `useIndex: true` when numeric segments should be real array indexes rather than string keys.

***

## Expressions

Rierino uses JMESPath for expressions, extended with its own set of functions, summarised under Expression functions below.

#### `eval(obj, expression, throwError = false)`

Runs an expression **synchronously**.

```js
objectutils.eval(item, "data.variants[?stock > `0`].sku");
```

Because it is synchronous, the async-only functions (notably `lookup`) cannot resolve here and return `null`. For those, use `evalExp` from EvalProvider.

#### `evalOrValue(obj, q, defaultValue = null)`

If `q` is a string starting with `=`, the rest is evaluated as an expression. Otherwise `q` is returned as a literal.

```js
objectutils.evalOrValue(ctx, "=_meta.user.name");  // evaluated
objectutils.evalOrValue(ctx, "Acme");              // literal
objectutils.evalOrValue(ctx, null);                // defaultValue
```

This is the convention behind configuration fields named `value`, such as filter defaults and new-record defaults.

#### `evalOrPath(obj, q, defaultValue = null)`

Three-way dispatch on the prefix:

| Prefix   | Behaviour                               |
| -------- | --------------------------------------- |
| `==`     | return the rest as a **literal string** |
| `=`      | evaluate the rest as an expression      |
| _(none)_ | treat `q` as a plain **path**           |

This is the convention behind configuration fields that accept either a path or an expression, such as `id`, `name` and `icon`.

***

## Reading and writing values

| Method               | Signature                                                 | Notes                                                                                                                                                                           |
| -------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `getPathElement`     | `(obj, path, defaultValue = null, useIndex = false)`      | safe deep read                                                                                                                                                                  |
| `setPathElement`     | `(obj, path, value, deleteNull = true, useIndex = false)` | deep write, creating intermediate objects along the way                                                                                                                         |
| `splitPath`          | `(path, useIndex = false)`                                |                                                                                                                                                                                 |
| `getArrayAndIndex`   | `(root, itemPath)`                                        | `{ arr, index }` for a path ending in an array index; throws if the parent is not an array or the index is out of range                                                         |
| `moveRelativeToPath` | `(root, dragPath, dropPath, position = "after")`          | reorder an item within or across arrays; the primitive behind drag-and-drop lists. `position` is `"before"` or `"after"`; dropping into a descendant of the dragged item throws |

Two caveats on `setPathElement`:

* It **mutates** the object you pass and returns the _parent_ node, not the root.
* With the default `deleteNull`, writing `null` or `undefined` **deletes** the key instead of storing an empty value. This is intentional and it is what makes clearing a field remove it from the record, but it will surprise you if you expect a null.

***

## Shaping objects

| Method          | Signature                         | Notes                                                                                                        |
| --------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `deepMerge`     | `(obj1, obj2, copyFirst = false)` | `obj2` wins. Arrays are replaced, not merged                                                                 |
| `flattenObj`    | `(obj, parent = "", res = {})`    | `{a:{b:1}}` → `{"a.b": 1}` plus type markers so it can be rebuilt                                            |
| `simpleFlatten` | `(obj, parent = "", res = {})`    | the same, without the markers                                                                                |
| `unflattenObj`  | `(obj, depth = 1, res = {})`      | inverse of `flattenObj`. Needs the markers to restore arrays; skips `_`-prefixed keys and drops empty values |
| `upperCase`     | `(name)`                          | null-safe                                                                                                    |

`deepMerge` mutates its first argument unless you pass `copyFirst: true`. **Always pass `copyFirst: true` when merging into something you did not create.** Configuration objects handed to you in props are shared.

`objectDiff` still exists but is deprecated and logs a warning on every call. Don't use it in new code.
