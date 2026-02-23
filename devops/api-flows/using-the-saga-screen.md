# Using the Saga Screen

## At a glance

* Left panel: list of sagas, grouped by **domain** and **path**.
* Center canvas: drag-and-drop **step graph**.
* Top bar: edit saga details, add steps, and tidy layout.
* Save is “live”: changes propagate to runners in real time.

### Open the Saga screen

Open **Devops → Saga**. You’ll land on a visual graph editor for building flows step by step.

The screen behaves like other Rierino screens. You get save, delete, duplicate, and import/export menus.

### Pick the right saga (domain/path/stream)

Sagas are listed on the left, grouped by domain and path. The system can store multiple saga records for the same URL path.

<details>

<summary>How multiple sagas on the same path are selected</summary>

* If only one saga is **active**, it is used.
* If more than one saga is active, but each is allowed on different **streams**, the runner picks the one matching the incoming stream.
* If more than one saga is active on the **same** stream, only the first record is used.

</details>

### Use a meaningful saga ID

The saga ID field is editable. Use a meaningful unique ID for easier log searches.

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption><p>Icon Bar</p></figcaption></figure>

### Icon bar: edit, build, layout

{% columns %}
{% column %}
#### Left: edit saga details

Click the edit icon to open the saga definition popup. This is where you set path, allowed runners/streams, schema, caching, and more.
{% endcolumn %}

{% column %}
#### Middle: add steps

Drag step types onto the canvas. Then connect them with links to define the execution order.
{% endcolumn %}

{% column %}
#### Right: layout tools

Use alignment and grid controls to keep the flow readable. You can also change the grid size and visibility.
{% endcolumn %}
{% endcolumns %}

### Build a flow (recommended shape)

Each saga needs at least one **Start** step. Use **Success** and **Fail** steps as exit points. Add Event / Transform / Condition steps in between.

<details>

<summary>Parallel starts (multiple Start steps)</summary>

If a saga has more than one Start step, they run in parallel threads. This is available starting from version 1.8.0.

Parallel branches do not auto-merge payloads. Use the Merge event handler if you need a combined payload.

Keep parallel branches in the same runner when possible. Prefer synchronous links if distributing across runners.

</details>

### Save & deploy behavior (real-time)

Creating or updating a saga updates the runtime in real time. This depends on change data capture not being disabled.

<details>

<summary>How saves affect in-flight requests</summary>

* Requests already running on a different saga record (different ID) continue on the old flow. This is true even if you mark that other saga inactive.
* Requests running on the currently active saga may pick up your new graph mid-flight. Big changes (like removing steps) can break these requests.

If you need zero disruption, use a versioned or backup saga briefly. Switch traffic once you’re confident no requests are in-flight.

</details>

{% hint style="info" %}
To copy multiple steps at once, keep pressing Ctrl while clicking **Copy** on a selected step. This builds a multi-step clipboard and works across sagas.
{% endhint %}

## Saga Data Schema (reference)

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
