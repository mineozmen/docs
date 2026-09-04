# Troubleshooting

## Compile errors

A compile failure shows as red text where your component would be, with the message and the line and column in your code.

### No matching export in "…" for import "X"

You used a named import from a platform module. Only `react` and `esm.sh` packages support named imports; platform modules give you a single default export.

```jsx
import ReactRedux from "react-redux";
const { useSelector } = ReactRedux;
```

See Imports & rules.

### Could not resolve "…"

The module is not available. The usual causes:

| You wrote                        | Fix                                                       |
| -------------------------------- | --------------------------------------------------------- |
| `import _ from "lodash"`         | `"esm.sh/lodash"`                                         |
| `import { useX } from "./hooks"` | inline it, since your code is a single file               |
| `import Image from "next/image"` | use a plain `<img>`                                       |
| some other internal module       | not exposed; see the available modules in Imports & rules |

The module set is fixed by the platform release. If you genuinely need something that isn't there, raise it with your Rierino contact.

### Could not load remote package https://esm.sh/…

The package download failed at compile time: wrong package name, or no network access to `esm.sh` from your environment. Note the result is remembered for the session, so reload the page after fixing it.

#### Syntax errors that look wrong

* **TypeScript is not supported.** Type annotations are a syntax error.
* Backticks and `${` inside a `<style>{` … `}</style>` block must be escaped, because the CSS lives in a template literal.

***

## Compiled module does not export a valid React component as default.

The code compiled, but the default export isn't something that can be rendered.

| Cause                            | Fix                                                                              |
| -------------------------------- | -------------------------------------------------------------------------------- |
| no `export default`              | add it                                                                           |
| only named exports               | `export default MyComponent;`                                                    |
| `export default React.memo(fn)`  | wrap it: `const Inner = React.memo(fn); export default (p) => <Inner {...p} />;` |
| `export default { MyComponent }` | export the component itself, not an object                                       |

***

## Something went wrong rendering the component.

Your component threw while rendering. The message is not shown on screen, so open the browser console for the stack trace.

The frequent causes, roughly in order:

* **Reading data that hasn't loaded yet.** `props.data`, `props.item` and `props.itemList` are empty on the first render. Use optional chaining everywhere.
* **Calling an action that is `null`.** `onSave`, `onNew` and `onDelete` are `null` when the screen is configured read-only. Null-check first.
* **Using `props.events` as an object.** It is a function: `props.events()`.
* **Destructuring a context that isn't there.** A context you are not actually inside returns `undefined`; guard before destructuring.
* **Giving `ConnectionWrapper` or `ChangeWrapper` more than one child.** Both take exactly one element.

***

## Nothing renders

| What you see                         | What it means                                                                                                                                             |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Provide code to compile.`           | No code reached the component. With a stored component this also appears _briefly_ while it loads. If it stays, the component record could not be fetched |
| `Custom Lister Code is not defined.` | The list configuration has no code and no component reference                                                                                             |
| `Missing Editor: …`                  | The editor name is neither a built-in editor nor one of your stored components                                                                            |
| `Missing Lister: …`                  | The list type is not a known list type                                                                                                                    |
| Nothing at all, in a custom editor   | A custom editor renders nothing when it has no code configured                                                                                            |
| An empty area that never resolves    | Still compiling, or the compile threw; check the browser console                                                                                          |
| A redirect to _ui not in app_        | The custom page's path is not in the app menu. Add it                                                                                                     |

***

## Stale code

Compiled output is cached, but the cache always verifies your source before reusing it, so **editing your code always recompiles**. If you are nonetheless seeing old behaviour:

1. **Check the component key.** Two components sharing one key evict each other and recompile constantly. Custom object editors are the common case, because the fallback key is often the same for all of them. Set the key explicitly.
2. **For a stored component, confirm you saved the record**, not just the preview.
3. **Clear the browser's stored data** for the site as a last resort, then reload.

***

## Performance

* **A custom list loads the whole result set.** Standard page-size handling is off. On a large source, push `limit` and `skip` through `onFilter`. See [Recipes](recipes.md).
* **`esm.sh` imports dominate compile time.** A large package makes the first compile slow and the component big. Prefer a platform module when one exists.
* **Prefer a stored component in production.** It is compiled once and delivered ready to run, so the browser never loads a compiler for it.
* **Don't fetch during render.** Use `useEffect` with a correct dependency array; lists re-render on every data update.

***

## Debugging tips

* `console.log` from your component goes to the normal browser console and you can set breakpoints in your compiled component in the browser's debugger.
* **Start with `console.log(props)`** at the top of your component. It tells you immediately which integration point mounted you and exactly what you were given, which is faster than reading the props reference.
* When data looks wrong, log `props.itemList?.list?.[0]` or `props.data` rather than guessing at the record shape; it varies by source.
