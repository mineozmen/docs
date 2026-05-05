---
description: >-
  This step performs a condition check and directs flow of saga based on its
  outcome.
---

# Condition Step

<figure><img src="../../../../.gitbook/assets/image (118).png" alt=""><figcaption><p>Condition Definition</p></figcaption></figure>

Condition steps allow branching out alternative flows based on input parameters, data received or calculated between saga steps. Output of a condition step is a condition value calculated by this step, which is then used by the links to define which step should be followed after this condition step.

Dragging and dropping a condition step from the icon bar of a saga adds a new condition, which can be configured by clicking on the edit icon displayed when hovering its node. The following fields are used for overall settings of the condition:

* **Name:** Descriptive name of the step, which will be displayed on the saga flow.
* **Auto Fail:** Whether failure of this step should automatically fail the whole saga flow. If not set to true (or the saga itself has auto fail setting), error status should be handled separately.
* **Description:** Verbal description of the step, for reference and documentation.
* **Stroke:** Color to use for drawing borders of the step, for visual purposes.
* **Fill:** Color to use for filling the step node, for visual purposes.

The actual functionality of a condition step is defined using settings listed on the parameters tab:

* **Condition Class:** Type of the condition handler to use (such as Element Value or Error Code).
* **Condition Parameters:** Condition class specific list of parameters to use as listed below.&#x20;

{% hint style="info" %}
A condition is always executed locally by the saga orchestrator runner.
{% endhint %}

## Condition Step Data Schema

```json
	{
		"conditionClass": {
			"type": "string",
			"description": "The Java class condition logic (e.g., `com.rierino.handler.saga.condition.ElementValueCondition`)"
		},
		"conditionParameters": {
			"type": "object",
			"description": "Key-value pair parameters used by the specific condition class (such as pattern for JMESPath condition class)"
		}
	}
```
