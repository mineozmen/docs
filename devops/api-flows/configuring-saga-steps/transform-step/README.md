---
description: >-
  This step performs a transformation on an event and continues with its output
  to the next step.
---

# Transform Step

<figure><img src="../../../../.gitbook/assets/image (119).png" alt=""><figcaption><p>Transform Definition</p></figcaption></figure>

{% hint style="info" %}
A transform step is always executed locally by the saga orchestrator runner.
{% endhint %}

Transform steps allow modifying structure of an event payload, which may be required when integrating with 3rd party services or returning data in a format preferred by the front-end applications.

Dragging and dropping a transform step from the icon bar of a saga adds a new transformation, which can be configured by clicking on the edit icon displayed when hovering its node. The following fields are used for overall settings of the transformation:

* **Name:** Descriptive name of the step, which will be displayed on the saga flow.
* **Auto Fail:** Whether failure of this step should automatically fail the whole saga flow. If not set to true (or the saga itself has auto fail setting), error status should be handled separately.
* **Description:** Verbal description of the step, for reference and documentation.
* **Stroke:** Color to use for drawing borders of the step, for visual purposes.
* **Fill:** Color to use for filling the step node, for visual purposes.

The actual functionality of a transform step is defined using settings listed on the parameters tab:

* **Transform Class:** Type of the transform handler to use (such as JMESPath Transformation, Add Value to Payload).
* **Transform Parameters:** Transform class specific list of parameters to use.&#x20;

## Transform Step Data Schema

```json
	{
		"transformClass": {
			"type": "string",
			"description": "The Java class for transformation (e.g., `com.rierino.handler.transform.JMESPayloadTransform`)"
		},
		"transformParameters": {
			"type": "object",
			"description": "Key-value pair parameters used by the specific transform class (such as pattern for JMESPath transform class)"
		}
	}
```
