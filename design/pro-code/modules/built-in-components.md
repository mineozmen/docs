---
description: Producers and registries provide access to built-in editors and lists
---

# Built-in components

These let your custom component render the **platform's own** widgets instead of rebuilding them: a standard field editor inside your custom form, or a standard data table inside your custom page.

***

## EditorProducer

```jsx
import EditorProducer from "components/editors/EditorProducer";
const Producer = EditorProducer.default;
const { RenderContent } = EditorProducer;
```

Give it a schema fragment, a UI fragment and a value and it renders the right editor:

```jsx
<Producer
  path="data.title"
  schema={props.schema?.properties?.data?.properties?.title}
  ui={{ widget: "TextEditor", props: { description: "The public title" } }}
  data={props.data?.data?.title}
  onChange={(value, path) => props.onChange(value, path || "data.title")}
  sendMessage={props.sendMessage}
/>
```

This is the fastest way to build a half-custom editor: your own layout and sections, with the platform's editors for the fields themselves, so validation, localisation, tooltips and value menus all keep working.

Rierino ships a large library of field editors: text, numbers, dates, switches, selects and autocompletes, media and file pickers, arrays and tables, rich text and markdown, code and JSON, maps, charts and diagrams and more. Any of them can be used as `ui.widget` for a field, or as an `editorType`.

#### What it does for you

*   **Picks a widget** when you don't name one, based on the field's schema type:

    | Schema                    | Widget                                          |
    | ------------------------- | ----------------------------------------------- |
    | array of strings/numbers  | `ChipArrayEditor`                               |
    | array (other)             | `StepArrayEditor`                               |
    | object                    | `ObjectEditor`                                  |
    | string / number / integer | `TextEditor` (or `ValueDisplay` when read-only) |
    | boolean                   | `SwitchEditor`                                  |
    | date-time                 | `DateTimeEditor`                                |
* **Supplies the label** from the schema title. Never set `label` yourself.
* **Passes your settings through.** Every key under `ui.props` reaches the widget as a prop.
* **Validates** when the field is configured as validated and shows the errors in the field's tooltip.
* **Applies text direction** for right-to-left languages.
* **Resolves indirect configuration.** A field whose schema and UI come from another record type, from a source, or from an option list, resolved by a value in the data.
* **Renders companion editors** side by side when the field configuration defines them.

#### `RenderContent`

Takes a path (or a UI object containing one), walks both the schema and the data down to it and renders the right editor. This is the convenient form when you are iterating over a layout definition:

```jsx
<RenderContent
  entry={{ path: "data.description", widget: "MarkdownEditor" }}
  entryData={props.data}
  schema={props.schema}
  onChange={props.onChange}
  sendMessage={props.sendMessage}
/>
```

`path: "$"` targets the whole record.

***

## EditorRegistry

```jsx
import EditorRegistry from "components/editors/EditorRegistry";
const Registry = EditorRegistry.default;
const getEditors = EditorRegistry.getEditors;
```

Renders whatever editor `editorType` names, either a built-in editor or one of your own stored components. Unknown names render _"Missing Editor: …"_ rather than crashing.

`getEditors()` returns the available built-in editors, which is useful if you are building a picker.

> `EditorRegistry.registerEditor(name, Component)` also exists. Be careful with it: registrations are global and last for the rest of the session, so a name clash can shadow a built-in editor everywhere. Prefer a stored component.

***

## ListerRegistry

```jsx
import ListerRegistry from "components/listers/ListerRegistry";
```

Renders a built-in list type. It needs the data props a list screen normally gets, so on a custom page pair it with `ConnectionWrapper`:

```jsx
<ConnectionWrapper source="order">
  <ListerRegistry
    listerType="QueryTableLister"
    schema={props.schema}
    ui={{ title: "Orders", columns: [{ path: "id" }, { path: "data.status" }] }}
  />
</ConnectionWrapper>
```

Common list types include `QueryTableLister` (the standard data table), `MenuLister`, `BoardLister`, `CalendarLister` and `GanttLister`, plus the `Dependent…` variants used for child collections inside a record editor.

You cannot register a new list type from custom code; use `CustomCodeLister`.
