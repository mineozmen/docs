---
description: Recipes are copy-pasteable starting points as examples
---

# Recipes

Each one is a complete, single-file component.

> The examples below keep their CSS in an inline `<style>` block so each one can be pasted and run on its own. In real screens, define your styles in the platform's styling feature instead and keep only the class names in your component.

***

## A simple editor widget

Wraps `LabeledEditor`, edits one value, keeps no local state for `data`.

```jsx
import React from "react";
import LabeledEditor from "components/editors/LabeledEditor";

function TextEditor(props) {
  const { data, onChange, valueType } = props;
  const isNumeric = valueType === "integer" || valueType === "number";

  const handleChange = (e) => {
    let value = e.target.value;
    if (value === "") value = undefined;
    else if (valueType === "integer") {
      const parsed = parseInt(value, 10);
      value = isNaN(parsed) ? undefined : parsed;
    } else if (valueType === "number") {
      const parsed = Number(value);
      value = isNaN(parsed) ? undefined : parsed;
    }
    onChange(value);
  };

  return (
    <LabeledEditor {...props}>
      <input
        className="Rie-input Rie-input-text"
        type={isNumeric ? "number" : "text"}
        value={data === undefined ? "" : data}
        onChange={handleChange}
        {...(valueType === "number" && { step: "any" })}
        {...(valueType === "integer" && { step: "1" })}
      />
    </LabeledEditor>
  );
}

export default TextEditor;
```

Three rules this follows:

* no `useState` for `data`, since the platform owns the value,
* `label` is never defined; it comes from the field's schema,
* `undefined` clears the value, which removes the field from the record.

#### When you _do_ need local state

Only for high-frequency input. Keep a local mirror and throttle the update:

```jsx
import React, { useState, useEffect, useRef } from "react";
import LabeledEditor from "components/editors/LabeledEditor";

export default function ColorEditor(props) {
  const [local, setLocal] = useState(props.data);
  const timer = useRef(null);

  useEffect(() => { setLocal(props.data); }, [props.data]);

  const push = (value) => {
    setLocal(value);
    if (timer.current) clearTimeout(timer.current);
    timer.current = setTimeout(() => props.onChange(value), 150);
  };

  return (
    <LabeledEditor {...props}>
      <input type="color" value={local || "#000000"} onChange={(e) => push(e.target.value)} />
    </LabeledEditor>
  );
}
```

## A custom list with grouped cards

```jsx
import React, { useEffect } from "react";
import nextrouter from "next/router";
const useRouter = nextrouter.useRouter;

function ColorGroupedLister(props) {
  const router = useRouter();
  const [, currentQueryString = ""] = (router.asPath || "").split("?");

  // close the editor when the id leaves the url
  useEffect(() => {
    const params = new URLSearchParams(currentQueryString);
    if (!params.get("id")) props.onSelect(null, null, false);
  }, [currentQueryString]);

  const items = props.itemList?.list || [];
  const colors = ["orange", "red", "blue"];

  return (
    <div className="Rie-ColorLister-wrapper">
      <style>{`
        .Rie-ColorLister-wrapper { display: flex; gap: 20px; padding: 24px; width: 100%; box-sizing: border-box; }
        .Rie-ColorLister-column { flex: 1; background: #fff; border-radius: 8px; padding: 16px;
          display: flex; flex-direction: column; gap: 12px; }
        .Rie-ColorLister-column-header { font-weight: 600; text-transform: capitalize;
          padding-bottom: 12px; border-bottom: 2px solid var(--colors--athens-gray); }
        .Rie-ColorLister-card { padding: 16px; border-radius: 6px; cursor: pointer; color: #fff; }
        .Rie-ColorLister-bg-orange { background: #f97316; }
        .Rie-ColorLister-bg-red { background: #ef4444; }
        .Rie-ColorLister-bg-blue { background: #3b82f6; }
      `}</style>

      {colors.map((color) => (
        <div key={color} className="Rie-ColorLister-column">
          <div className="Rie-ColorLister-column-header">{color}</div>
          {items
            .filter((item) => item.data?.color === color)
            .map((item) => (
              <div
                key={item.id}
                className={"Rie-ColorLister-card Rie-ColorLister-bg-" + color}
                onClick={() => props.onSelect(item.id)}
              >
                {item.data?.name || "Unnamed Item"}
              </div>
            ))}
        </div>
      ))}
    </div>
  );
}

export default ColorGroupedLister;
```

## List with server-side filtering and paging

A custom list receives the whole result set, so drive paging yourself through `onFilter`:

```jsx
import React, { useState } from "react";

export default function PagedLister(props) {
  const [page, setPage] = useState(0);
  const pageSize = 25;

  const go = (next) => {
    setPage(next);
    props.onFilter({ skip: next * pageSize, limit: pageSize });
  };

  const items = props.itemList?.list || [];
  const total = props.itemList?.totalCount;

  return (
    <div className="Rie-PagedLister-root">
      {props.itemList?._loading ? <div>Loading…</div> : null}
      <ul>
        {items.map((i) => (
          <li key={i.id} onClick={() => props.onSelect(i.id)}>{i.data?.title || i.id}</li>
        ))}
      </ul>
      <button disabled={page === 0} onClick={() => go(page - 1)}>Prev</button>
      <span> {page + 1}{total ? " / " + Math.ceil(total / pageSize) : ""} </span>
      <button onClick={() => go(page + 1)}>Next</button>
    </div>
  );
}
```

***

## A custom editor

```jsx
import React from "react";
import DynamicText from "components/displays/DynamicText";
import CoreUI from "@rierino-open/core-ui";
const { Button, Grid } = CoreUI;

export default function ProductEditor(props) {
  const { data, onChange, changeCounter } = props;
  const { onSave, onClose, onDelete } = props.events();

  return (
    <div className="Rie-ProductEditor-root">
      <style>{`.Rie-ProductEditor-root { padding: 16px; }`}</style>

      <Grid flex>
        <Grid.Item>
          <h4><DynamicText text={data?.data?.title} /></h4>
        </Grid.Item>
        <Grid.Item className="Rie-editor-buttons">
          <Button intent="g-button" isDisabled={!changeCounter} onPress={onSave}>
            <DynamicText text="{{common.save}}" />
          </Button>
          <Button appearance="plain" onPress={() => onDelete(data)}>
            <DynamicText text="{{common.delete}}" />
          </Button>
          <Button appearance="plain" onPress={onClose}>
            <DynamicText text="{{common.close}}" />
          </Button>
        </Grid.Item>
      </Grid>

      <input
        className="Rie-input"
        value={data?.data?.title ?? ""}
        onChange={(e) => onChange(e.target.value, "data.title")}
      />
    </div>
  );
}
```

Two things to notice: `props.events()` is a **function call** and the `onSave` it returns takes **no arguments**: it reads the record and the changes itself, and runs validation.

***

## Reusing the standard editors

Your sections and layout, the platform's field editors:

```jsx
import React from "react";
import EditorProducer from "components/editors/EditorProducer";
const Producer = EditorProducer.default;

export default function HalfCustomEditor(props) {
  const field = (name, ui) => (
    <Producer
      key={name}
      path={"data." + name}
      schema={props.schema?.properties?.data?.properties?.[name]}
      ui={ui}
      data={props.data?.data?.[name]}
      onChange={(value, path) => props.onChange(value, path || "data." + name)}
      sendMessage={props.sendMessage}
    />
  );

  return (
    <div className="Rie-HalfCustom-root">
      <h5>Basics</h5>
      {field("title", { widget: "TextEditor" })}
      {field("active", { widget: "SwitchEditor" })}

      <h5>Content</h5>
      {field("description", { widget: "MarkdownEditor" })}
    </div>
  );
}
```

***

## A custom page that fetches its own data

Custom pages get no data by default. Two options.

### Option A: query directly

```jsx
import React from "react";
import store from "operations/store";

export default function Dashboard(props) {
  const { useGetListQuery } = store.getApi();
  const { data, isFetching } = useGetListQuery(
    { source: "order", type: "list" },
    { refetchOnMountOrArgChange: true }
  );

  if (isFetching) return <div>Loading…</div>;

  return (
    <div className="Rie-Dashboard-root">
      <h3>{data?.totalCount ?? (data?.list || []).length} orders</h3>
    </div>
  );
}
```

For a **filtered** query, remember the `versionId` so results refresh after a save:

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

### Option B: get the standard data contract

```jsx
import React from "react";
import ConnectionWrapper from "operations/connectionwrapper";
import ListerRegistry from "components/listers/ListerRegistry";

export default function Page(props) {
  return (
    <ConnectionWrapper source="order">
      <ListerRegistry
        listerType="QueryTableLister"
        schema={props.schema}
        ui={{ title: "Orders", columns: [{ path: "id" }, { path: "data.status" }] }}
      />
    </ConnectionWrapper>
  );
}
```

This needs `props.schema`, so configure the menu entry to use the UI definition for that type.

***

## Calling the backend directly

When the standard record operations aren't enough:

```jsx
import global from "operations/global";

async function exportSelection(ids) {
  const res = await global.fetchWrapper(
    global.getAPIURL("request/crud/order/export"),
    { method: "POST", headers: global.getAPIHeaders(), body: JSON.stringify({ ids }) }
  );
  if (!res.ok) throw new Error("export failed");
  return res.json();
}
```

For a file, use `global.downloadWrapper(url)` instead. It handles the filename and the save dialog.

***

## Translations, icons, tooltips, conditions

```jsx
import React from "react";
import DynamicText from "components/displays/DynamicText";
import DynamicIcon from "components/displays/DynamicIcon";
import CustomTooltip from "components/boxes/CustomTooltip";
import Conditional from "components/Conditional";

export default function Toolbar(props) {
  return (
    <div className="Rie-MyToolbar-root">
      <CustomTooltip title={<DynamicText text="{{common.create-new}}" />}>
        <div onClick={() => props.onNew()}>
          <DynamicIcon name="AddCircle" />
        </div>
      </CustomTooltip>

      <Conditional data={props.item} condition={{ path: "data.status", values: ["draft"] }}>
        <button onClick={() => props.events().onSave()}>
          <DynamicText text="{{common.publish}}" />
        </button>
      </Conditional>
    </div>
  );
}
```

***

## Expressions, including lookups

Synchronous, no context needed:

```jsx
import objectutils from "operations/objectutils";

const skus = objectutils.eval(props.data, "data.variants[?stock > `0`].sku");
```

Asynchronous, when the expression fetches another record:

```jsx
import React from "react";
import EvalProvider from "operations/providers/EvalProvider";
const EvalContext = EvalProvider.EvalContext;

export default function CategoryName(props) {
  const { evalExp } = React.useContext(EvalContext);
  const [name, setName] = React.useState("");

  React.useEffect(() => {
    let alive = true;
    evalExp(props.data, "lookup('category', data.categoryId).data.title")
      .then((v) => { if (alive) setName(v || ""); });
    return () => { alive = false; };
  }, [props.data?.data?.categoryId]);

  return <span>{name}</span>;
}
```

***

## Remembering UI state per user

```jsx
import React from "react";
import PreferenceProvider from "operations/providers/PreferenceProvider";
const PreferenceContext = PreferenceProvider.PreferenceContext;

export default function CollapsibleSidebar(props) {
  const { preferences, setPreference } = React.useContext(PreferenceContext);
  const key = "myLister.sidebarCollapsed";
  const collapsed = !!preferences?.[key];

  return (
    <div className={"Rie-MySidebar-root" + (collapsed ? " Rie-MySidebar-collapsed" : "")}>
      <button onClick={() => setPreference({ id: key, preference: !collapsed })}>
        {collapsed ? "»" : "«"}
      </button>
      {/* ... */}
    </div>
  );
}
```

***

## Using an npm package

```jsx
import React from "react";
import _ from "esm.sh/lodash";
import { format } from "esm.sh/date-fns";

export default function Report(props) {
  const byDay = _.groupBy(props.itemList?.list || [], (i) =>
    format(new Date(i.createTime), "yyyy-MM-dd")
  );

  return (
    <ul>
      {Object.entries(byDay).map(([day, rows]) => (
        <li key={day}>{day}: {rows.length}</li>
      ))}
    </ul>
  );
}
```

`esm.sh/...` with no `https://`. The package is downloaded when your code is compiled and bundled into your component, so prefer a platform module when one will do.
