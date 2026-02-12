---
description: >-
  Extra event configurations allow triggering custom JavaScript code on specific
  editor and widget actions.
---

# Extra Events

Various actions such as "On Change", "On Blur" for widgets and "On Save" for item editors can use extra event configurations to trigger custom logic after these actions are performed. Extra event configurations have the following settings:

* **Action:** Type of action to trigger after
* **Prevent Default:** Whether the original action should be performed or ignored (e.g. if true, On Change does not perform the actual data change)
* **Code:** JavaScript code to execute when triggered

The custom code has access to a "props" variable, which includes:

* **data:** Current data in the widget
* **ui:** UI configuration of the widget
* **editorEvents:** Events allowed by the item editor (e.g. onNew, onChange)
* **sendMessage:** Function to trigger snack displays
* **defaultFn:** Original action, which is typically useful if preventDefault is set to true and the original function needs to be triggered after a special logic

Additional values are also available based on the action type (e.g. changes for On Save, event, updated for On Blur, previous, updated for On Change). &#x20;
