---
description: >-
  Action elements are used for specialized function calls to handlers, using
  already existing handler actions with predefined parameter values.
---

# Actions

Actions are optional, yet useful elements to add to a runner. Each action is a specialized function call on an event handler, with predefined set of parameters such as event metadata.

The use of action elements allows creation of reusable functions, which can simply be requested by name, rather than sending a full set of parameter values. This has the following benefits:

* Decreased data size in transit and store, as the same parameter values do not need to be sent with each request.
* Central management of frequently used action specifications, which simplifies change management.
* Ability to enforce standard values for specific parameters across all saga flows for functional governance.

Since actions are simply mappings of predefined parameters, they can be added with "mapping" element type to runners, and simply updated using the remap commands on runners.

A number of settings are shared across all actions:

| Setting                | Definition                                                                        | Example        | Default |
| ---------------------- | --------------------------------------------------------------------------------- | -------------- | ------- |
| handler                | Name of the handler to use for this action                                        | kafka\_default | -       |
| meta.\[field].default  | Default event metadata field values for requests received for this action element | action=Get     | -       |
| meta.\[field].override | Final event metadata field values for requests received for this action element   | domain=product | -       |

{% file src="../../../.gitbook/assets/action-CallRestUSA-0001.json" %}
Example Action Definition (Can be Imported on Element Screen)
{% endfile %}
