---
description: >-
  Custom components allow creating reusable widgets for editing value, array or
  objects inside UIs
---

# Custom Component

## Custom component properties

| Prop                       | Notes                                                                                                         |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `data`                     | the **field's** value, not the whole record                                                                   |
| `onChange`                 | `(newValue)`, the path is filled in for you                                                                   |
| `label`                    | injected from the field's schema title. **Never define it yourself**                                          |
| `path`                     | the field path this widget edits                                                                              |
| `valueType`                | `"string"`, `"number"`, `"integer"`, `"boolean"`, `"object"` or `"array"`; for arrays this is the _item_ type |
| `valueProps`               | from the field configuration                                                                                  |
| `schema`, `ui`, `snapshot` |                                                                                                               |
| `editorEvents`             | the parent editor's action bundle                                                                             |
| `sendMessage`              |                                                                                                               |
| …your settings             | **every key under the field's `ui.props` arrives as a top-level prop**                                        |

That last row is how you configure a reusable widget: anything you put under `props` in the field's UI configuration shows up as a prop on your component.
