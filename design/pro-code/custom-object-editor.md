---
description: >-
  Custom object editors allow customizing form screens used for viewing and
  editing single item records
---

# Custom Object Editor

Set the screen's editor to `ObjectEditorCustomCode` and put your code in the editor configuration. Your component replaces the whole record form: header, fields, save/delete buttons.

The platform still gives you the working copy of the record, change tracking, and a ready-made set of actions.

> **What your component receives:** the working copy of the record, the dirty fields and the editor actions. See Props reference → Custom editor props for the full list.

**Use `props.events()` for saving**

`events` is a **function** that returns the action bundle:

```jsx
const { onSave, onNew, onDelete, onClose, onReload } = props.events();
```

Prefer `events().onSave()` over calling `props.onSave` directly. It takes no arguments, runs schema validation when the editor is configured as validated, reports validation errors to the user and honours the "close after save" setting. Calling `props.onSave` yourself skips all of that.

**Locale switching**

If the editor configuration lists `locales`, a locale switcher is provided around your component and localised fields resolve against the selected locale. Without it, that layer is disabled.

**Component key**

The fallback key for a custom editor is derived from the data source, but the source is not passed down to editors, so it commonly resolves to the same value for **every** custom editor in the application. That doesn't break anything (the cache verifies your source before reusing it), but it does mean your editors recompile whenever you switch between them.

Set the component key (`editorType`) explicitly in the editor configuration.

## Custom object editor properties

### Data

| Prop            | Shape                            | Notes                                                                        |
| --------------- | -------------------------------- | ---------------------------------------------------------------------------- |
| `data`          | the working copy of the record   | already merged with configured defaults                                      |
| `changes`       | `{ [path]: { value, counter } }` | the paths edited since load                                                  |
| `changeCounter` | number                           | `0` means pristine, which is useful for disabling Save                       |
| `schema`        | JSON Schema                      |                                                                              |
| `ui`            | object                           | your editor configuration                                                    |
| `open`          | boolean                          | for dialog-style editors                                                     |
| `readOnly`      | boolean                          |                                                                              |
| `snapshot`      | `string \| null`                 | set when viewing a historic version                                          |
| `id`            | `string`                         | a **path expression** for where the id lives (e.g. `"id"`), not the id value |
| `name`          | `string`                         | a path or expression for the display name                                    |

> `source` is **not** passed to editors. Don't rely on `props.source` here.

### Actions

| Prop          | Signature                                                                         |
| ------------- | --------------------------------------------------------------------------------- |
| `events`      | `() => ({ onSave, onNew, onDelete, onChange, onClose, onReload, specialEvents })` |
| `onChange`    | `(value, path)`                                                                   |
| `onSave`      | `(item, changes, force, callback)`; raw, prefer `events().onSave()`               |
| `onDelete`    | `(record, callback)`                                                              |
| `onNew`       | `(data?)`                                                                         |
| `onClose`     | `()`                                                                              |
| `onReload`    | `()`                                                                              |
| `sendMessage` | `(text, severity, detail)`                                                        |
| `dispatch`    | Redux `dispatch`                                                                  |

`onChange` paths are dotted, with square brackets around any segment that itself contains dots:

```js
props.onChange("New title", "data.title");
props.onChange(9.99, "data.prices[tr-TR].amount");
props.onChange(wholeRecord, "$");        // "$" replaces the entire record
```

Passing `undefined` or `null` **removes** the key rather than storing a null.
