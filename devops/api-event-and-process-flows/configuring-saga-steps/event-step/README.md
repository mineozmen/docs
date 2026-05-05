---
description: >-
  Event steps pass event payload to an event handler, to execute selected action
  with given parameters.
---

# Event Step

<figure><img src="../../../../.gitbook/assets/image (115).png" alt=""><figcaption><p>Event Step Definition</p></figcaption></figure>

Dragging and dropping an event step from the icon bar of a saga adds a new step. It is possible to configure each step clicking on the edit icon displayed when hovering its node. The following fields are used for overall settings of the step:<br>

* **Name:** Descriptive name of the step, which will be displayed on the saga flow.
* **Auto Fail:** Whether failure of this step should automatically fail the whole saga flow. If not set to true (or the saga itself has auto fail setting), error status should be handled separately.
* **Description:** Verbal description of the step, for reference and documentation.
* **Stroke:** Color to use for drawing borders of the step, for visual purposes.
* **Fill:** Color to use for filling the step node, for visual purposes.

<figure><img src="../../../../.gitbook/assets/image (116).png" alt=""><figcaption><p>Event Step Metadata</p></figcaption></figure>

The actual functionality of an event step is defined using event metadata settings listed below:<br>

* **Event Handler:** Alias of the event handler that should be used for calling a function on the runner receiving this event (e.g. read). This is an optional setting in some cases, as runners can match handlers to events based on streams the requests are received from. However, if the event is being processed locally (e.g. on "local" stream), this setting should be explicitly defined.
* **Event Version\*:** Numerical version of the action implementation to be used. Handlers allow versioning of their functions (i.e. actions) with new releases, so this setting defines which version should be used for a specific request. This is a mandatory setting, which should be set to 0 for unversioned actions.
* **Event Action\*:** Name of the function to be called on target event handler (e.g. Get) or alias of the preconfigured action on target runner (e.g. GetProduct). This is a critical setting, as it specifies actual action to execute for the step.
* **Event Domain:** Alias of a state, query or a system, which is used as the main target for event action (e.g. "Get" from "product" state). The meaning of this setting depends on the event handler configuration itself.
* **Input Element:** Json path for the data input from event payload (e.g. parameters). This parameter allows using events with different formats as input to the same event handler. If no input element is provided, full payload is used as the input.
* **Output Element:** Json path for the data output on event payload (e.g. list). This parameter allows outputing to different paths based on output format required by the API endpoint or the next saga step. If an input element is given, output element is relative to that path. If the output element starts with '$.', it is relative to the root of event payload, ignoring input element setting. If the output element is '-', it is ignored and no data is added to event payload as the output.
* **Deployments:** List of runner deployment ids which are allowed to execute this step. This is required when more than one runner deployment exists (e.g. v1, v2). If the deployment id is not specified in such scenario, both runners would process the same event, resulting in duplicate actions and responses. This is a rare case, typically used when deploying a new runner version while the old version is still running to avoid downtime (e.g. changing deployment id only after new version is up and running).
* **Inject:** Whether these settings (e.g. input element, domain, etc.) should allow injection of key-value parameters and secrets or not.
* [**Follow Ack**](#user-content-fn-1)[^1]**:** Whether event status ACK should continue saga flows [instead of immediately returning](#user-content-fn-2)[^2]
* **Response System:** Alias of the system for the receiving runner to reply on. This setting is used when communication is through an event streaming platform (e.g. Kafka) and the response should be sent through a system different than the default.
* **Response Stream:** Alias of the stream for the receiving runner to reply on. This setting is used when communication is through an event streaming platform (e.g. Kafka) and the response should be sent through a stream different than the default.
* **Parameter Map:** Handler and action specific list of parameters to use for the event. For a full list of these parameters, please refer to [Handlers](../../../microservices/building-blocks/execution-handlers/).

These settings work similar to a remote function call, where the handler and action define target function and other settings define its parameters for execution.

{% hint style="info" %}
An event step can be executed locally or sent out to other runners for async and more scalable execution, orchestrating multiple microservices.
{% endhint %}

## Event Step Data Schema

{% code expandable="true" %}
```json
{
		"eventMeta": {
			"type": "object",
			"description": "Event metadata defining details on the action call",
			"properties": {
				"handler": {
					"type": "string",
					"description": "Alias of the handler to be used for processing event step",
					"examples": ["read", "write", "query", "rest"]
				},
				"action": {
					"type": "string",
					"description": "Name of the action/function to call, which is specific to the handler selected",
					"examples": ["Get", "Create", "GetQuery", "CallRest"]
				},
				"domain": {
					"type": "string",
					"description": "Domain for which the action will be taken, which has a different meaning based on the action type (such as database table for Create action)"
				},
				"inputElement": {
					"type": "string",
					"description": "JSON path of the data field in event payload, which should be used as input for the action"
				},
				"outputElement": {
					"type": "string",
					"description": "JSON path of the data field where outputs of the action should be writted on. Output element uses input element as the root and creates children below that root, unless output element starts with $, which refers to the event payload making it the root."
				},
				"parameters": {
					"type": "object",
					"description": "Key-value pair parameters used by the specific action type (such as inputPattern for Create action)"
				}
			}
		}
	}
```
{% endcode %}

## Using Preconfigured Actions

While it is possible to define all settings for each event step inside a saga, especially for high volume and low latency requirements, it may be preferrable to define default values once and reuse them for each call.

There are 3 alternative ways of achieving this, which can be also combined together based on the use case:

1. Defining predefined "Action" records and using [Broken link](/broken/pages/vMEjRSndZzV2Kzyiepjy "mention") handler to call them.
2. Defining structural [actions.md](../../../microservices/building-blocks/additional-elements/actions.md "mention") runner elements with the default values and using them as event actions for requests to runners with these elements.
3. Defining metadata overrides to streams through which these action requests are sent to the target event runners.

This approach has 2 main benefits:

1. Not sending the same list of settings with each request decreases the size of data transferred and stored.
2. It becomes possible to update the list of parameters used for same type of request across all sagas at once.

[^1]: Since 0.5.0

[^2]: E.g. for TaskEventHandler, whether to return immediately, or allow custom logic in saga flow design using ACK event status with a condition step
