---
description: A number of building blocks are also available as shared components
---

# Building blocks

Five ready-made components. All are plain default imports.

```jsx
import LabeledEditor from "components/editors/LabeledEditor";
import DynamicIcon from "components/displays/DynamicIcon";
import DynamicText from "components/displays/DynamicText";
import CustomTooltip from "components/boxes/CustomTooltip";
import Conditional from "components/Conditional";
```

***

## LabeledEditor

The standard label-and-input row. **Wrap most custom editor widgets in it.** That is what makes your widget line up with the built-in editors and inherit the description tooltip, the value menu and the validation styling.

```jsx
import React from "react";
import LabeledEditor from "components/editors/LabeledEditor";

export default function TextEditor(props) {
  return (
    <LabeledEditor {...props}>
      <input
        className="Rie-input Rie-input-text"
        value={props.data ?? ""}
        onChange={(e) => props.onChange(e.target.value)}
      />
    </LabeledEditor>
  );
}
```

**Always spread `{...props}` onto it.** It reads the label, configuration, value, errors and handlers from there.

| Prop                  | Notes                                                                                                                                                                   |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `label`               | injected for you from the field's schema title, so **never define it yourself**. A `null` label hides the label cell entirely                                           |
| `collapse`            | `"collapsed"` to start folded, `"expanded"` to start open. Setting either one adds the expand/collapse control and a bordered container; leave it unset for a plain row |
| `labelFn`             | `() => ReactNode`, replaces the whole label cell                                                                                                                        |
| `renderAfterLabel`    | `() => ReactNode`, appended after the label                                                                                                                             |
| `containerProperties` | extra layout props                                                                                                                                                      |

#### The info tooltip

An info icon appears automatically when the field configuration provides a description, an example or a default value, or when there are validation errors, so documenting a field is a configuration change, not a code change.

#### Layout

A 12-column grid: label on the left (4 columns from `sm` up), input on the right (8). Everything is wrapped in an error boundary, so a crash inside your widget does not take down the surrounding form.

***

## DynamicText

Renders translated and localised text. Use it instead of hardcoding strings.

```jsx
<DynamicText text="{{common.save}}" />
<DynamicText text={{ enUS: "Save", trTR: "Kaydet" }} />
<DynamicText text="Plain string" />
```

Only one prop, `text`. Renders nothing when the resolved text is empty.

#### Where the translations come from

Translations are managed in the dashboard, not in your code. They are grouped, and each entry has a key and a value per locale.

`{{group.key}}` looks an entry up in that group; a bare `{{key}}` looks in the `common` group. A missing translation falls back to rendering the key itself, so an untranslated string shows up visibly rather than as a blank.

That means the normal workflow is: **add your labels as translations in the dashboard, then reference them by key from custom code.** Your component then picks up new languages and wording changes without being edited or recompiled.

For a one-off string that will never be reused, the inline localised-object form (`{ enUS: "Save", trTR: "Kaydet" }`) is acceptable, but anything a user reads more than once belongs in the translation list.

***

## DynamicIcon

Renders an icon by name from the platform's icon set, so your component picks up icon and theme changes automatically.

```jsx
<DynamicIcon name="AddCircle" />
<DynamicIcon name="Check" style={{ width: "12px", height: "12px" }} />
<DynamicIcon names={["Custom_thing", "Nav_help"]} />
```

| Prop              | Notes                                                         |
| ----------------- | ------------------------------------------------------------- |
| `name`            | the icon key                                                  |
| `names`           | a list of candidates; the first one that exists wins          |
| _(anything else)_ | passed to the icon, such as `style`, `className` or `onClick` |

An unknown name renders a fallback icon rather than breaking the layout, so a typo or a not-yet-registered icon will not break your component.

#### Where the icons come from

Icons are registered in the dashboard and `DynamicIcon` resolves names against that set, both the built-in icons and any you add yourself. So to use your own artwork, **register it as an icon in the dashboard and reference it by name**; you do not embed SVGs or image URLs in your component.

Two things this buys you: the icon follows the active theme and it can be changed centrally without touching your code.

The `names` prop exists for exactly this: list your own icon first and a built-in as the fallback, so the component still renders correctly in an environment where your icon has not been registered yet.

```jsx
<DynamicIcon names={["Acme_invoice", "Nav_help"]} />
```

Frequently used built-in names: `AddCircle`, `Check`, `Edit`, `Info`, `Star`, `EmptyStar`, `Collapse`, `Expand`, `Nav_help`, `Exclamation`.

***

## CustomTooltip

```jsx
<CustomTooltip title={<div>Details…</div>} placement="left" arrow interactive>
  <div>hover me</div>
</CustomTooltip>
```

| Prop                                                       | Notes                                   |
| ---------------------------------------------------------- | --------------------------------------- |
| `title`                                                    | the tooltip content, a string or a node |
| `placement`, `offset`, `arrow`, `interactive`, `className` | presentation                            |

When `title` is empty it renders the child untouched, so it is safe to use unconditionally. The child must be an element that can hold a ref, so wrap bare text in a `<span>`.

***

## Conditional

Show or hide content using the same condition format as the rest of the platform's configuration.

```jsx
<Conditional
  data={props.data}
  condition={{ path: "data.status", values: ["active"] }}
>
  <button onClick={publish}>Publish</button>
</Conditional>
```

| Prop        | Notes                                   |
| ----------- | --------------------------------------- |
| `data`      | what the condition is evaluated against |
| `condition` | see condition                           |

A missing condition renders the children. Evaluation is asynchronous and starts optimistic, so children can flash briefly before a failing condition hides them. Avoid it for anything that must never be seen and use it for progressive disclosure instead.
