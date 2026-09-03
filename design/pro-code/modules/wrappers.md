---
description: >-
  Connection and change wrappers provide built-in mechanisms for managing
  records
---

# Wrappers

```jsx
import ConnectionWrapper from "operations/connectionwrapper";
import ChangeWrapper from "operations/changewrapper";
```

These are the two pieces that give a standard screen its data contract. On a **custom page**, where nothing is set up for you, wrapping your own component in them is the shortest route to the same behaviour a normal list or editor screen gets.

> Both take **exactly one child element**. They inject props into that child, so a fragment or multiple children will not work.

***

## ConnectionWrapper

Handles everything about talking to a data source: listing, loading a record, create, update, delete, success and error notifications, keeping `?id=` in the URL and resolving version conflicts.

#### Properties

| Prop                               | Notes                                                                  |
| ---------------------------------- | ---------------------------------------------------------------------- |
| `source`                           | **required**, the data source id                                       |
| `noCreate` / `noSave` / `noDelete` | pass `true` to withhold that action (the child receives `null` for it) |
| `namePath`                         | path or expression used to title the record in page history            |
| `noRoute`                          | don't sync the selected record into the URL                            |

#### What your child receives

The full list prop set from Props reference: `itemList`, `item`, `onList`, `onQuery`, `onSelect`, `onNew`, `onSave`, `onDelete`, `onReload`, `onCopy`, `onPaste`, `dispatch`, `sendMessage`.

```jsx
import React from "react";
import ConnectionWrapper from "operations/connectionwrapper";

function Inner(props) {
  React.useEffect(() => {
    props.onQuery({ params: { limit: 50 } });
  }, []);

  return (
    <ul>
      {(props.itemList?.list || []).map((i) => (
        <li key={i.id} onClick={() => props.onSelect(i.id)}>{i.data?.title || i.id}</li>
      ))}
    </ul>
  );
}

export default function Page() {
  return (
    <ConnectionWrapper source="product">
      <Inner />
    </ConnectionWrapper>
  );
}
```

#### Worth knowing

* **A load error replaces your component.** If the list or record request fails, the wrapper renders an error screen instead of your child. You will not get a chance to render a custom empty state for that case.
* **It owns the URL.** The selected record id is written into `?id=` (debounced). If your component also navigates, the two will fight. Set `noRoute` if you want to drive the URL yourself.
* **It shows its own notifications.** Success and failure messages for every save and delete are raised for you; don't duplicate them.
* **Version conflicts are handled for you.** A conflict opens a dialog offering to force the write or merge the changed fields.
* `onCopy` / `onPaste` share an in-memory clipboard across the screen.
* `item._new` tells you whether the current record has ever been saved.

***

## ChangeWrapper

Holds an editable working copy of a record and tracks which fields are dirty. This is the piece that sits between a list and its editor. Use it if you build your own editing flow.

#### Properties

| Prop                      | Notes                                                                                                                                                    |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `item`                    | the record to edit                                                                                                                                       |
| `defaults`                | array of `{ path, value }` applied to new copies. `value` follows the `=expression` convention and can read the current user and preferences via `_meta` |
| `skips`                   | top-level fields to strip from the copy                                                                                                                  |
| `root`                    | provide record context to the subtree (used by nested editors)                                                                                           |
| `extraData` / `extraPath` | enrichment data to fetch and attach to the working copy                                                                                                  |
| `onChange`                | called with the whole updated record on every change                                                                                                     |

#### What your child receives

| Prop            | Notes                                       |
| --------------- | ------------------------------------------- |
| `data`          | the working copy                            |
| `onChange`      | `(value, path)`                             |
| `changes`       | `{ [path]: { value, counter } }`            |
| `changeCounter` | `0` when pristine, handy for disabling Save |

#### Change semantics

* `path === "$"` replaces the whole record.
* Any other path writes into the copy at that path.
* Writing `undefined` or `null` **removes** the field rather than storing an empty value.
* The working copy is rebuilt and `changes` reset, whenever a newly loaded record arrives.
