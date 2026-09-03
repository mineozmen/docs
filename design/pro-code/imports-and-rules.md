---
description: Custom elements have access to Rierino libraries through imports
---

# Imports & rules

## Import mechanism

Every platform module **except `react`** is exposed as a single default export. Named imports from a platform module are a **compile error**, not a runtime surprise.

```jsx
// ❌ will not compile
import { useSelector, useDispatch } from "react-redux";
import { useRouter } from "next/router";
import { LangContext } from "operations/providers/LangProvider";
import { Button, Grid } from "@rierino-open/core-ui";
```

```jsx
// ✅ default-import the module, then destructure or use property access
import ReactRedux from "react-redux";
import nextrouter from "next/router";
import LangProvider from "operations/providers/LangProvider";
import CoreUI from "@rierino-open/core-ui";

const { useSelector, useDispatch } = ReactRedux;
const useRouter = nextrouter.useRouter;
const LangContext = LangProvider.LangContext;
const { Button, Grid } = CoreUI;
```

## Default import contents

Two shapes, depending on the module. Some give you a **value** (a component, an object) directly; others give you the **whole module**, in which case the module's own main export sits at `.default` and everything else is a property.

### Modules that give you a value directly

| Import                              | You get                           |
| ----------------------------------- | --------------------------------- |
| `operations/objectutils`            | the `objectutils` helper object   |
| `operations/global`                 | the `global` helper object        |
| `operations/auth`                   | the `auth` helper object          |
| `operations/condition`              | the `condition` helper object     |
| `operations/store`                  | the data-access entry point       |
| `operations/connectionwrapper`      | the `ConnectionWrapper` component |
| `operations/changewrapper`          | the `ChangeWrapper` component     |
| `components/listers/ListerRegistry` | the `ListerRegistry` component    |
| `components/editors/LabeledEditor`  | the `LabeledEditor` component     |
| `components/displays/DynamicIcon`   | the `DynamicIcon` component       |
| `components/displays/DynamicText`   | the `DynamicText` component       |
| `components/boxes/CustomTooltip`    | the `CustomTooltip` component     |
| `components/Conditional`            | the `Conditional` component       |

### Modules that give you the whole module

| Import                                    | You get                                                        | Main export at                    |
| ----------------------------------------- | -------------------------------------------------------------- | --------------------------------- |
| `react-redux`                             | `.useSelector`, `.useDispatch`, `.Provider`, …                 | n/a                               |
| `next/router`                             | `.useRouter`, `.withRouter`, …                                 | `.default` (the router singleton) |
| `@rierino-open/core-ui`                   | `.Button`, `.Grid`, `.Modal`, …                                | n/a                               |
| `components/editors/EditorRegistry`       | `.registerEditor`, `.getEditors`                               | `.default` (the component)        |
| `components/editors/EditorProducer`       | `.SingleEditorProducer`, `.RenderContent`, `.ValidatedContent` | `.default` (the component)        |
| `operations/providers/LangProvider`       | `.LangContext`, `.isLangRtl`, `.getAllTranslations`            | `.default` (the provider)         |
| `operations/providers/StyleProvider`      | `.StyleContext`, `.getAllStyles`                               | `.default`                        |
| `operations/providers/PreferenceProvider` | `.PreferenceContext`                                           | `.default`                        |
| `operations/providers/BranchProvider`     | `.BranchContext`                                               | `.default`                        |
| `operations/providers/EvalProvider`       | `.EvalContext`                                                 | `.default`                        |

Rule of thumb: if the module is a **provider or a registry**, the component you want is at `.default` and the context/helpers are properties.

**`react` is the exception**

```jsx
import React, { useState, useEffect, useMemo, useRef, useContext } from "react";
```

Both default and named imports work. You get the application's own React instance, which is what lets your component sit inside the platform's contexts and use hooks without conflicts.

Available: `Children`, `Component`, `Fragment`, `Profiler`, `PureComponent`, `StrictMode`, `Suspense`, `act`, `cloneElement`, `createContext`, `createElement`, `createFactory`, `createRef`, `forwardRef`, `isValidElement`, `lazy`, `memo`, `startTransition`, `unstable_act`, `useCallback`, `useContext`, `useDebugValue`, `useDeferredValue`, `useEffect`, `useId`, `useImperativeHandle`, `useInsertionEffect`, `useLayoutEffect`, `useMemo`, `useReducer`, `useRef`, `useState`, `useSyncExternalStore`, `useTransition`, `version`.

`react-dom` is not available.

## External packages: `esm.sh`

Any npm package can be pulled in through `esm.sh`:

```jsx
import _ from "esm.sh/lodash";
import { format } from "esm.sh/date-fns";
import Chart from "esm.sh/chart.js/auto";
import { pick } from "esm.sh/lodash-es@4.17.21";
```

* **Never write the `https://` prefix.** Use the bare `esm.sh/package` form.
* **Named imports work here.** The restriction applies only to platform modules.
* Version pinning works the way esm.sh expects: `esm.sh/lodash@4.17.21`.
* Packages that import their own sub-files resolve correctly; you can import deep paths such as `esm.sh/chart.js/auto`.

The package is downloaded **when your code is compiled** and bundled into your component, so:

* the first compile of a component using a big package is slow and the result is large;
* compilation needs network access to `esm.sh`. In an air-gapped or egress-restricted environment, `esm.sh` imports will not compile.

Prefer a platform module when one does the job.

## Import restrictions

Anything not listed above and not prefixed with `esm.sh/` fails to compile. The common ones:

* `react-dom`
* `lodash`, `date-fns`, `uuid`, … as bare names (use `esm.sh/...` instead)
* `next/image`, `next/link`, `next/head`
* other `operations/*` or `components/*` modules that are not in the tables above, such as internal editors, listers, tables, menus, filters and schema helpers
* relative imports (`./anything`)

The module set is fixed by the platform release. If your use case genuinely needs something that isn't exposed, raise it with your Rierino contact rather than trying to reach it through `window`.

## Styling

Rierino has its own styling feature and that is where your CSS should normally live. Styles defined there are managed centrally, follow the active theme, can be reused across screens and can be edited without touching your component's code.

{% hint style="info" %}
**Define your styles in the platform first.** See [Design → User Interface → Styles](https://docs.rierino.com/design/user-interface/styles) for how to create and apply them.
{% endhint %}

Write your component against the class names you defined there:

```jsx
export default function Card(props) {
  return <div className="Rie-MyCard-root">{props.data?.title}</div>;
}
```

**When the styling feature isn't enough**

Some CSS is so specific to one component's markup that it does not belong in a shared style definition: a bespoke grid, a one-off animation, rules tied to element structure that exists only inside your component. For those, render a `<style>` element inside the component itself:

```jsx
export default function Card(props) {
  return (
    <div className="Rie-MyCard-root">
      <style>{`
        .Rie-MyCard-root {
          padding: 16px;
          border-radius: 8px;
          border: 1px solid var(--colors--athens-gray);
        }
      `}</style>
      {props.data?.title}
    </div>
  );
}
```

Rules for both approaches:

* **Prefix every class with `Rie-`** and make it specific to your component (`Rie-MyKanban-column`, not `Rie-column`). Generic names collide with the platform's own styles and with other custom components on the same screen.
* **Use the theme's CSS variables** rather than hardcoded colours: `var(--colors--persian-blue)`, `var(--colors--athens-gray)`, `var(--colors--pigeon-post)` and the rest of the active theme. Hardcoded colours will not follow a theme change.
* Inline CSS lives in a template literal, so escape any backtick or `${` you need literally.

### Structure conventions

* Functional components. Hooks are fully supported.
* `export default YourComponent;` at the end.
* Don't manage the main `data` prop with `useState`; the platform owns it. The exception is high-frequency input (colour pickers, sliders, rich text), where you keep a local mirror and throttle the call back to `onChange`.
* Never define a `label` prop yourself in an editor widget. It is injected for you from the field's schema.
