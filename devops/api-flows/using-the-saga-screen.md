# Using the Saga Screen

Opening the **Saga** screen from **Devops** app menu or navigation bar, you will come across a visual graph editor, allowing design of flows step by step.

Sagas are listed on the left side of this screen, grouped by their domain and path information. It is possible to assign multiple saga flows to the same path (i.e. URL endpoint), under the following conditions:

* Either only 1 saga has active status - typically used when older or alternate versions are kept for reference or disaster recovery
* More than 1 sagas are active, but they are allowed on different streams - typically used for allowing same endpoint with different flows between development, test and production environments
* If more than 1 saga is set to active on the same stream, only the first record is used

The saga screen has similar functionalities as any other UI screen, such as save, delete, duplicate, item import and export actions and menus. Saga flow designed on this screen is stored in JSON format, the same as any other data source.

On top of this screen, you will notice that the saga ID is editable, this allows you to assign a meaningful unique ID to each saga for your reference. Since these IDs will be recorded within event logs, this approach makes them more manageable than assigning random or sequential IDs.

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption><p>Icon Bar</p></figcaption></figure>

Below the ID field, you can find the icon bar, which allows construction of a saga flow as well as changing its layout.

If you click on the edit icon on the left of this bar, you will see the pop-up editor for defining saga details.

At the center of the icon bar, you can see different types of saga steps, which you can and drop on to the grid to add new steps to the saga flow.

Each saga must have at least one **Start** step, and can have multiple **Fail** or **Success** steps if that makes your flow easier to follow. Between these steps, you can add any number of steps or conditional logic to build your API flow.

If a saga has more than one Start steps, these steps would be triggered and followed in parallel using multiple threads. This feature provides a simplified approach for parallelization without the need for asynchronous flows or event streaming.

{% hint style="info" %}
Multiple start steps are allowed starting from version 1.8.0 and they would be omitted in previous releases.

Each parallel flow is executed independently, and would not merge their event payloads automatically even if they converge on same steps. If payloads have to be merged, Merge event handler should be used.

If a saga is parallelized, it is recommended to keep all following steps executing within the same runner or distributed through synchronous links/communications.
{% endhint %}

At the right side, you can see the icons for changing the layout of saga flow after you've added its steps, such as automatically aligning steps, hiding / displaying the grid as well as changing its size.

Once you create a new saga or update an existing one, it is automatically updated on the respective runners - as long as the change data capture is not disabled. This update changes API flow for the saga's URL endpoint in real-time. This change can affect still ongoing requests to this URL endpoint (e.g. requests started just milliseconds before you save your saga) as follows:

* If they are running on another saga record (i.e. with a different id), they continue and finish using that previous saga flow, regardless of whether their saga is set to inactive status or not, effectively having no impact on these requests
* If you've updated the design of the currently active saga, they start using this new saga flow for completion. If you've done drastic changes on the saga flow, such as removing some steps, this can cause some of these requests to fail. In such cases, we recommend creating a new version of the saga or switching to a back-up saga flow for a few seconds to make sure there are no active requests on your saga.

## Saga Data Schema

{% code expandable="true" %}
```json
{
	"type": "object",
	"properties": {
		"id":{
			"type": "string"
		},
		"data":{
			"type": "object",
			"properties": {
				"path":{
					"type": "string",
					"description": "URL path from which the saga is called, in a single /Path format"
				},
				"name":{
					"type": "string",
					"description": "Logical name of the saga"
				},
				"description":{
					"type": "string",
					"description": "Logical description of what the saga does"
				},
				"allowedFor":{
					"type": "array",
					"description": "List of runners on which the saga is allowed to run",
					"items": {
						"type": "string",
						"description": "ID of the runner"
					}
				},
				"schema":{
					"type": "object",
					"properties": {
						"input": {
							"type": "object",
							"description": "JSON schema representing input data model and validation rules"
						},
						"output": {
							"type": "object",
							"description": "JSON schema representing output data model and example values for mocking APIs"
						}
					}
				},
				"steps":{
					"type": "array",
					"description": "List of connected steps in saga, which create a sequential execution logic for the flow",
					"items": {
						"type": "object",
						"properties": {
							"id": {
								"type": "integer",
								"description": "Unique ID of the step inside the saga, typically assigned in a sequential manner"
							},
							"name":{
								"type": "string",
								"description": "Logical name of the step"
							},
							"description": {
								"type": "string",
								"description": "Logical description of the step"
							},
							"type": {
								"type": "string",
								"description": "Type of the step",
								"enum": ["START", "FINISH", "FAIL", "EVENT", "CONDITION", "TRANSFORM"]
							},
							"position": {
								"type": "object",
								"description": "Visual positioning of the step in flow diagram. Try to use a logical layout, where the steps have spacing between them. Each step has 100 width and 50 height.",
								"properties":{
									"x":{
										"type": "number",
										"description": "X position on the diagram"
									},
									"y":{
										"type": "number",
										"description": "Y position on the diagram"
									}
								}
							},
							"links": {
								"type": "array",
								"description": "Links from this step to others defining next step to execute after this step".
								"items":{
									"type": "object",
									"properties": {
										"id": {
											"type": "integer",
											"description": "Unique ID of the condition inside the saga, typically assigned in a sequential manner"
										},
										"toStepID": {
											"type": "integer",
											"description": "Next step to execute"
										},
										"conditionValues":{
											"type": "array",
											"description": "Value for which this link should be traversed. This value depends on the output produced by conditionClass of the originating step. * can be used as a wildcard for a default 'else' branch. If the previous step is an event type step, SUCCESS and FAIL values can be used to route failed events to a different next step, but only if the user explicitly mentions following a different flow on failure. You do not need to provide conditionValues if there is a single next step and it will always be executed."
										}										
									}
								}
							}
						}
					}
				},
			}
		}
	}
}
```
{% endcode %}
