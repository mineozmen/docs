---
description: >-
  Rierino JavaScript SDK provides ability to build highly customized UI
  components and use them as part of the low-code development environment.
icon: display-code
---

# Pro-Code

A custom component is a **single JSX code** that:

* is written in plain JavaScript (no TypeScript),
* exports a React component as its **default export**,
* may import from a fixed set of platform modules and from `esm.sh`,
* runs as part of the application, not in a sandbox.

The smallest working component:

```jsx
import React from "react";

export default function Hello(props) {
  return <div>Hello {props.data?.name}</div>;
}
```

Everything else in this documentation is about two questions: **what can I import** and **what props do I get**.

## Custom coded sections

There are five places you can put custom code. Each one decides how much of the surrounding screen the platform still builds for you and which props your component receives.

Pick the one that matches how much control you want:

| You want to…                                | Use                      |
| ------------------------------------------- | ------------------------ |
| replace record listings                     | **Custom lister**        |
| replace single record editors               | **Custom object editor** |
| reuse one custom widget across many screens | **Custom component**     |
| build a whole page from scratch             | **Custom page**          |
| replace the login & landing pages           | **Special template**     |

## Custom code formats

Whatever you are building a list, an editor or a page it goes through one compiler. You supply the code in one of two ways and the dashboard decides which based on how you configured the screen:

* **Inline code**: the JSX you type into the code editor. It is compiled in the browser as you edit, after a short debounce, so you get a live preview.
* **A stored component**: code saved as a reusable component record and referenced by id. It is compiled once on the server and delivered ready-to-run.

Inline code is the fast path while you are designing. A stored component is the right choice for anything reused across screens and it is lighter for the browser, since no compiler needs to load. Both accept exactly the same code.

## Main requirements

* **Default export**: The component is loaded from the module's default export. It must be a function component, a class component, or a `React.forwardRef(...)` result. A `React.memo(...)` result is **not** accepted directly, so wrap it like:

```jsx
const Inner = React.memo(MyComponent);
export default (props) => <Inner {...props} />;
```

* **Single file**: There is no file system behind your code. No relative imports (`./utils`), no splitting a component across files. If you need shared logic across screens, save it as a stored component and reference it.
* **JavaScript, not TypeScript**: Type annotations are a compile error.
* **Import React**: Always write `import React from "react"` even if you never reference `React` by name.

## Component caching

Compiled output is cached, keyed by the **component key** you configure (`editorType` in a screen configuration, or the record id for a stored component). The cache always verifies your source before reusing an entry, so editing code always recompiles, so you will never see a stale build.

The caveat is _collisions_: two different components sharing one key will keep evicting each other and recompile every time you switch between them. Give each custom component its own key.

Most screen types fall back to a key derived from the data source when you don't set one. That default is convenient but not always unique, for custom object editors in particular it can resolve to the same value for every editor in the app. **Set the component key explicitly.** If you leave it empty entirely, caching is skipped and your code recompiles on every mount.

## Custom code access

Because it runs in the page rather than in a sandbox, your component has access to the browser (`window`, `document`, `localStorage`), the session and the platform modules documented here. Treat custom code with the same care as application code: review it and don't paste in code you don't trust.
