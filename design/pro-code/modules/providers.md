---
description: >-
  Five providers wrap custom components, giving them access to built-in data and
  functions
---

# Providers

Five providers are available. You are already inside all of them, so what you normally want is the **context**, not the provider component.

Remember the import shape: the provider is at `.default`, the context is a named property.

```jsx
import LangProvider from "operations/providers/LangProvider";
const LangContext = LangProvider.LangContext;
```

***

## LangProvider

_Translations and locale._

```jsx
import React from "react";
import LangProvider from "operations/providers/LangProvider";
const LangContext = LangProvider.LangContext;

function Title() {
  const { getText, getLocale, updateLocale } = React.useContext(LangContext);
  return <h4>{getText("{{common.create-new}}")}</h4>;
}
```

| Context value                    | Notes                                                                     |
| -------------------------------- | ------------------------------------------------------------------------- |
| `getText(text, safeStr = false)` | resolve a translation key, a localised object, or a plain string          |
| `getTranslation(key)`            | look up `"group.key"` directly (a bare key falls into the `common` group) |
| `getLocale()`                    | the current locale                                                        |
| `updateLocale(locale)`           | switch locale and persist the choice                                      |
| `safeText(text)`                 | force a value to a string                                                 |
| `state`                          | `{ selectedLocale, translations }`                                        |

Also on the module: `isLangRtl(locale)`, true for right-to-left languages, so you can flip your own layout.

#### `getText` handles three shapes

```jsx
getText("{{common.save}}")                       // translation key
getText({ enUS: "Save", trTR: "Kaydet" })        // localised object → current locale
getText("Save")                                  // plain string, returned as-is
```

A missing translation falls back to the key rather than rendering blank.

Locale codes are in the compact form (`enUS`, `trTR`).

The entries themselves are managed in the dashboard. See Where the translations come from.

> For plain rendering, prefer the `DynamicText` component; it is this context in component form and keeps your JSX cleaner.

***

## StyleProvider

_Theming._

```jsx
const { getTheme, setTheme, themes } = React.useContext(StyleContext);
```

`setTheme(id)` switches the active theme and remembers the choice. `themes` is the list of available themes.

You rarely need this: your component inherits the active theme through CSS variables. Use it only if you are building a theme switcher.

***

## PreferenceProvider

_Per-user UI state._

```jsx
const { preferences, setPreference } = React.useContext(PreferenceContext);

const key = "myLister.sidebarCollapsed";
const collapsed = !!preferences?.[key];
setPreference({ id: key, preference: !collapsed });
```

The right place to remember things like a collapsed sidebar, a chosen view mode or a column layout, per user and across sessions.

Use the context rather than `global.getPreferences()`: the context re-renders your component when a preference changes.

Namespace your keys with your component's name to avoid collisions.

***

## BranchProvider

_The active branch._

```jsx
const { branch, setBranch, getAllBranches } = React.useContext(BranchContext);
```

* `branch` is `null` on main, otherwise the branch id.
* `setBranch("main")` returns to main.
* `getAllBranches` loads the available branches.

Branched record ids carry the branch as a prefix. See the branch helpers in `global`, especially `pickBranchList`, which you need if your custom list runs over a branch-enabled source.

***

## EvalProvider

_Async expressions._

The asynchronous counterpart to `objectutils`. Use it when an expression needs to fetch another record.

```jsx
const { evalExp, pathExp, evalOrPath } = React.useContext(EvalContext);
```

| Function                      | Notes                                                               |
| ----------------------------- | ------------------------------------------------------------------- |
| `evalExp(obj, expression)`    | async; the full expression language, including `lookup(source, id)` |
| `pathExp(obj, path, default)` | synchronous plain path lookup                                       |
| `evalOrPath(obj, q, default)` | async; `==` literal / `=` expression / plain path                   |

```jsx
const [name, setName] = React.useState("");

React.useEffect(() => {
  evalExp(props.data, "lookup('category', data.categoryId).data.title").then(setName);
}, [props.data?.data?.categoryId]);
```

**When to use which:** `objectutils.eval` for pure expressions you want synchronously; `evalExp` when the expression may contain `lookup`, or when it comes from user configuration and could contain anything.
