---
description: Custom listers allow customizing UI screens listing records from a source
---

# Custom Lister

Set the screen's lister to `CustomCodeLister` and put your code in the list configuration. Your component replaces the table; the platform keeps handling the data source, the filters and the record editor.

## Customer lister types

A `structure` setting on the list configuration controls this:

| **Structure**          | **What is rendered**                                                                                                                                           |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _(unset, the default)_ | Full standard screen: title, create button, action menus, favourites, filters and selection, with your component in place of the table, plus the record editor |
| `complete_list`        | Your component plus the record editor. No title bar, no filters                                                                                                |
| `list_editor`          | Your component only. You own the whole screen, editing included                                                                                                |

Use the default when you want a different _presentation_ of a normal list (cards, a board, a calendar) but still want the standard controls. Use `list_editor` when your component is the entire experience.

**Pagination**

A custom list receives the **whole result set** the query returns. The standard page-size handling is switched off, because a custom layout usually wants to group or lay out everything at once.

On a large data source you must limit the query yourself, by pushing `limit` / `skip` (or your own filters) through the `onFilter` prop. See Recipes §3.

**Nested lists**

A list shown _inside_ a record editor (a child collection) uses `DependentCustomCodeLister` instead. Same props as a custom list, minus the `structure` options and minus `onFilter`.

## Customer lister properties

### Data

| Prop          | Shape                                        | Notes                                                          |
| ------------- | -------------------------------------------- | -------------------------------------------------------------- |
| `itemList`    | `{ list, totalCount, _loading, _versionId }` | `list` is always an array                                      |
| `item`        | `{ id, data, …, _loading, _new }`            | the currently selected record                                  |
| `source`      | `string`                                     | the data source id                                             |
| `schema`      | JSON Schema                                  | the schema for this record type                                |
| `ui`          | object                                       | your list configuration; anything you added to it arrives here |
| `editorUi`    | object                                       | the editor configuration for the same screen                   |
| `selection`   | array                                        | ids currently selected                                         |
| `nested`      | boolean                                      | `true` inside a nested (child collection) list                 |
| `routerQuery` | object                                       | the current query string                                       |

Records look like this:

```js
props.itemList = {
  list: [
    { id: 123, data: { there: { could: "be" }, any: 1, data: ["2"], inHere: true } }
  ]
}
```

Saved records also carry `createTime`, `updateTime`, `updaterId` and `instanceVersion`.

### Actions

| Prop                 | Signature                                        | Notes                                                                                                                  |
| -------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `onSelect`           | `(id, row = null, openModal = true)`             | opens the editor for `id` and updates the URL. `onSelect(null, null, false)` closes it                                 |
| `onNew`              | `(data?)`                                        | starts a new record with the given initial data; with no argument, falls back to the configured default item           |
| `onSave`             | `(item, changes, force = null, callback = null)` | `force` may be `"set"` or `"patch"`                                                                                    |
| `onDelete`           | `(record, callback = null)`                      | pass the **whole record object**, not just the id                                                                      |
| `onReload`           | `()`                                             | re-fetch the current record                                                                                            |
| `onFilter`           | `(filters)`                                      | merge filters into the query and re-run it; your paging and filtering hook                                             |
| `onQuery`            | `({ path, method, params, append })`             | run a raw list query                                                                                                   |
| `onList`             | `()`                                             | plain unfiltered listing                                                                                               |
| `setSelection`       | `(idsArray)`                                     |                                                                                                                        |
| `onRoute`            | `(query)`                                        | replace the page query string                                                                                          |
| `onCopy` / `onPaste` | `(value)` / `()`                                 | in-memory clipboard shared with the rest of the screen                                                                 |
| `sendMessage`        | `(text, severity, detail = null)`                | severity is `"success"`, `"error"`, `"warning"` or `"info"`; `text` may be a translation key like `"{{common.saved}}"` |
| `dispatch`           | Redux `dispatch`                                 |                                                                                                                        |

> `onSave`, `onNew` and `onDelete` are **`null`** when the screen is configured read-only (`noSave` / `noCreate` / `noDelete`). Null-check before wiring a button to them.

**Closing the editor when the URL changes**

The standard pattern watches the query string and clears the selection when the `id` disappears:

```jsx
import React, { useEffect } from "react";
import nextrouter from "next/router";
const useRouter = nextrouter.useRouter;

function ExampleLister(props) {
  const router = useRouter();
  const [, currentQueryString = ""] = (router.asPath || "").split("?");

  useEffect(() => {
    const params = new URLSearchParams(currentQueryString);
    if (!params.get("id")) props.onSelect(null, null, false);
  }, [currentQueryString]);

  // ...
}
```
