---
description: >-
  All UI components (such as editor widgets) are defined and configured within
  the platform, allowing extensions as well.
---

# Components

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption><p>Component Screen</p></figcaption></figure>

Component definitions allow customization of entry of parameters when adding them into editors, as well as addition of custom components (e.g. implemented with web components).

Components include the following types:

* **Action:** Used by [menus](uis/menus/) and button action widgets
* **Cell:** Used by table editors for cell display configurations
* **DV:** Used by [data visualization](../../data-science/data-visualizations.md) layout editors for dashboard design
* **Editor:** Used by UI editors for configuring [widgets](uis/widgets/) used
* **Filter:** Used by table listers for configuring query filters

## Extending Components

It is possible extend the list of existing components by adding external libraries and using web components.

### Adding External Libraries

External library dependencies can be added as a comma separated list of js file URLs in SCRIPT\_URLS environment variable of the admin UI deployment.

### Web Components

New widgets can be added for use in UI editors by integrating existing web components or creating new ones.

{% embed url="https://www.webcomponents.org/introduction" %}
Web Components Introduction
{% endembed %}

#### Widgets

You can add new widget  components by creating a new record in components screen with the following properties:

| Property             | Description                                                                                     | Example         |
| -------------------- | ----------------------------------------------------------------------------------------------- | --------------- |
| remote               | Set to true to indicate that this is an external component                                      | true            |
| tag                  | Web component's tag name                                                                        |  fast-text-area |
| propMap.data         | Web component's attribute to pass widget data (if set to <> data is provided as inline element) | value           |
| propMap.onChange     | Web component's attribute to pass widget onChange event                                         | onInput         |
| propMap.onChangeEval | Web component's event variable to send to onChange function                                     | target.value    |
| propMap.\*           | Any other property to pass on to component                                                      | color=red       |

It is also possible to pass additional properties using component parameters (i.e. ui.props), which will be passed on to the web component using same naming.

{% file src="../../.gitbook/assets/FastTextArea.json" %}
Example Web Component (can be imported on Component screen)
{% endfile %}

#### Listers

You can add new lister components by creating a new record in components screen with the following properties:

| Property | Description                                                | Example         |
| -------- | ---------------------------------------------------------- | --------------- |
| remote   | Set to true to indicate that this is an external component | true            |
| tag      | Web component's tag name                                   |  fast-text-area |

The web components also receive all properties available to listers (e.g. onSave, onNew events), which can be used with a custom component implementation.

### Functions

You can add new actions by creating a new record in components screen with the following properties:

| Property | Description                                                            | Example |
| -------- | ---------------------------------------------------------------------- | ------- |
| fn       | Function name (external library must register it as a window function) | alert   |
| title    | Title to display for action                                            | Alert   |
| icon     | Icon name to display for action                                        | Alert   |

Registered window function is called with properties of the action itself (e.g. including record details).
