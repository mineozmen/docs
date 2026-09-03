---
description: Custom page allows creating full pages with custom code
---

# Custom Page

A hosted page is a menu entry whose content is entirely your code. Nothing is fetched or wrapped for you: no data source, no list, no editor. You get a blank canvas inside the application shell.

> **What your component receives:** the application context, the menu entry's own settings and (when configured) the schema and UI design. See Props reference → Custom page props for the full list.

Two things to know:

* **The page must be in the app menu.** A hosted page URL that isn't reachable from the app's menu configuration is rejected with a _"ui not in app"_ redirect. Add the entry to the menu.
* **You fetch your own data.** Either use the query hooks from `store`, or wrap yourself in `ConnectionWrapper` to get the same data contract a standard list screen has. Both are shown in Recipes §6.

If the menu entry is configured to use a UI definition, the schema and UI design for that type are handed to your page as props, so you can render a standard list or standard editors inside your custom layout.

There is also a **full-structure page**: a normal list screen whose structure is set to `full`. It behaves the same way: the standard list machinery is skipped and your code owns the screen. The difference is that every key of the list configuration arrives as a prop, which is a convenient way to pass settings into your page.

## Custom page properties

| Prop                       | Notes                                                                                            |
| -------------------------- | ------------------------------------------------------------------------------------------------ |
| `app`, `apps`, `appConfig` | the current application context                                                                  |
| `type`                     | the page type from the URL                                                                       |
| `screenConfig`             | the menu entry for this page; your own settings on that entry are here                           |
| `schema`, `uiDesign`       | populated only when the menu entry is configured to use a UI definition; empty objects otherwise |
| `router`                   | the Next.js router                                                                               |

For a **full-structure page**, every key of the list configuration is spread as a prop instead.
