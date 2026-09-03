---
description: Core UI is Rierino's built-in design system with atomic components
---

# Core UI

```jsx
import CoreUI from "@rierino-open/core-ui";
const { Grid, Button, TextField, Modal } = CoreUI;
```

Default-import the module and destructure; named imports do not work.

These controls are already styled by the active theme, so using them instead of raw `<input>` / `<button>` is what makes a custom component look native.

### Available components

`AutocompleteBase`, `AutocompleteInput`, `Button`, `Calendar`, `Checkbox`, `CheckboxGroup`, `ClickAwayListener`, `Grid`, `Input`, `Menu`, `Modal`, `Popover`, `Radio`, `RadioGroup`, `Rating`, `Slider`, `Switch`, `Tab`, `TabList`, `TabPanel`, `Tabs`, `TextField`, `TreeView`.

***

## Grid

The layout primitive used throughout Rierino, with 12 columns by default.

```jsx
<Grid columns={12} gap={2} flow="row">
  <Grid.Item xs={12} sm={4}>label</Grid.Item>
  <Grid.Item xs={12} sm={8}>input</Grid.Item>
</Grid>
```

| Component   | Props                                                                      |
| ----------- | -------------------------------------------------------------------------- |
| `Grid`      | `columns`, `gap`, `flow` (`"row"` / `"col"`), `flex`, `className`, `style` |
| `Grid.Item` | `xs`, `sm`, `className`, `style`                                           |

`<Grid flex>` with plain `<Grid.Item>` children is the idiom for toolbars and button rows.

***

## Button

**Caveat: the handler is `onPress`, not `onClick`.** These controls are built on React Aria, which uses press events so they work with touch, keyboard and pointer alike. Same for `isDisabled` rather than `disabled`.

```jsx
<Button
  intent="g-button"
  className="Rie-base-button uppercase"
  onPress={() => props.onNew()}
>
  <DynamicIcon name="AddCircle" /> <DynamicText text="{{common.create-new}}" />
</Button>
```

| Prop         | Notes                                                             |
| ------------ | ----------------------------------------------------------------- |
| `onPress`    | the click handler                                                 |
| `intent`     | `"g-button"` is the primary gradient button used for Create/Apply |
| `appearance` | `"plain"` for icon-only and secondary buttons                     |
| `isDisabled` |                                                                   |

***

## TextField

Unlike `Button`, this one uses a familiar DOM-style `onChange(event)`.

```jsx
<TextField
  type="text"
  variant="standard"
  className="Rie-text-single"
  placeholder="Search"
  value={value || ""}
  onChange={(e) => setValue(e.target.value)}
  onKeyDown={(ev) => { if (ev.key === "Enter") { ev.preventDefault(); apply(); } }}
/>
```

***

## Modal

A compound component:

```jsx
<Modal open={open} onClose={close}>
  <Modal.Content>
    <Modal.Header className="Rie-dialog-title">Title</Modal.Header>
    <Modal.Body style={{ padding: "0 20px" }}>…</Modal.Body>
  </Modal.Content>
</Modal>
```

***

## Tabs

```jsx
<Tabs>
  <TabList>
    <Tab>First</Tab>
    <Tab>Second</Tab>
  </TabList>
  <TabPanel>…</TabPanel>
  <TabPanel>…</TabPanel>
</Tabs>
```

***

**When not to use Core UI**

For a bespoke layout (a Kanban board, a gallery, a dashboard) plain `div`s with `Rie-`-prefixed classes are the normal approach, styled through the platform's styling feature. Reach for Core UI when you need a _control_ that should match the rest of the application: buttons, inputs, dialogs, tabs.
