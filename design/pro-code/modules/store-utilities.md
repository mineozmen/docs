---
description: >-
  Store utilities provide access to RTK queries for interacting with configured
  data sources
---

# Store utilities

```jsx
import store from "operations/store";
```

This is how custom code reads and writes data without a surrounding list screen. It is most useful on custom pages, which have no data plumbing of their own.

```jsx
const {
  useGetListQuery,
  useLazyGetListQuery,
  useGetItemQuery,
  useLazyGetItemQuery,
  useNewItemMutation,
  useUpdateItemMutation,
  useDeleteItemMutation,
} = store.getApi();
```

> Call `store.getApi()` **inside your component body**, not at module top level.

These are RTK Query hooks: queries return `{ data, currentData, error, isLoading, isFetching, isSuccess, refetch }`, mutations return `[trigger, { data, isLoading, isSuccess, error }]` and lazy queries return `[trigger, result]` where `trigger(arg).unwrap()` gives a promise.

Results are cached and shared across the whole application, so asking for the same data twice does not cause two requests.

***

## Listing records

```jsx
const { data, isFetching } = useGetListQuery(
  { source: "product", type: "list" },
  { refetchOnMountOrArgChange: true }
);
// data → { list: [{ id, data, … }], totalCount }
```

| Field       | Notes                                                                                  |
| ----------- | -------------------------------------------------------------------------------------- |
| `source`    | **required**, the data source id                                                       |
| `type`      | `"list"` for a plain listing, `"query"` for a filtered/paginated one. Default `"list"` |
| `params`    | becomes the query string. Array values are sent as array parameters                    |
| `payload`   | request body, for sources listed via POST                                              |
| `method`    | override the source's configured method                                                |
| `path`      | appended to the listing URL                                                            |
| `versionId` | see the caveat below                                                                   |

Pass `{ skip: !ready }` to defer a query until its input is available.

### Filtered query versioning

A plain `type: "list"` query refreshes automatically after any create, update or delete on that source.

A `type: "query"` query only does so **if you give it a `versionId`**. Without one it will happily serve stale results after a save. Generate one per query:

```jsx
import global from "operations/global";

const versionId = React.useMemo(() => global.generateUUID(), []);
const { data } = useGetListQuery({
  source: "order",
  type: "query",
  versionId,
  params: { limit: 50, status: "new" },
});
```

***

## Reading one record

```jsx
const { currentData: item } = useGetItemQuery(
  id ? { id, source: "product" } : undefined,
  { skip: !id, refetchOnMountOrArgChange: true }
);
```

> Use `currentData`, not `data`, whenever you use `skip`. `data` keeps the previous argument's result while the new one loads.

***

## Creating, updating, deleting

```jsx
const [createItem] = useNewItemMutation();
createItem({ source: "product", item: { data: { title: "New" } } });

const [updateItem] = useUpdateItemMutation();
updateItem({ source: "product", item, changes });

const [deleteItem] = useDeleteItemMutation();
deleteItem({ source: "product", id });
```

All three refresh the affected lists and record automatically, including in other browser tabs. Pass `skipInvalidate: true` if you deliberately want to suppress that.

### Full update vs partial update

`updateItem` chooses between sending the whole record and sending only the changed fields. It sends the whole record when there are many changes, when a changed value is itself an object (a partial update cannot express removals), or when the source does not support partial updates.

You can force it: `force: "set"` always sends the full record, `force: "patch"` always sends only the changes.

`changes` uses the same shape the platform's change tracking produces:

```js
{ "data.title": { value: "New title", counter: 0 } }
```

### Version conflicts

On sources with version validation, saving a record someone else has changed in the meantime fails with `error === "VERSION_CONFLICT"` rather than a normal error.

A standard editor handles this for you by showing a conflict dialog with _force_ and _merge_ options. **If you write your own save path, you have to handle it yourself**. At minimum, tell the user and reload the record.

Other errors come back as `{ status, message, code, data }`.
