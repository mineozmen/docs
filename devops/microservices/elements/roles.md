---
description: >-
  Role elements are used for describing data feed from different streams with
  specific use cases.
---

# Roles

A role does not require any parameters. The following roles are predefined for typical use cases:

## Event

Most data streams assume Event role inside the platform, which is used throughout requests, saga steps and responses. If a stream is not assigned any specific role, it is treated as an Event stream. Event data has the following structure:

### Request Metadata

This section of event data (requestMeta) contains information about the origin of a request (typically provided by the front-end systems) as well as its destination. It is only updated by the gateway.&#x20;

### Saga Metadata

This section of event data (sagaMeta) contains information required for orchestrating saga flows. Saga event handlers edit and use this section for recording the current step being processed during a saga, while allowing tracking of nested calls between saga flows. It is only updated by the runner orchestrating saga flow for an event.

### Event Metadata&#x20;

This section of event data (eventMeta) contains parameters for executing an action on a target event runner (such as inputElement, outputElement). Event runners rely on this information, along with the source stream an event is received from to perform required operations. It can be provided by a saga orchestrator, using event step details, and enriched by the runner receiving the event based on default metdata defined using stream, action or handler configurations.

### Payload

This section of event data (payload) contains the actual body with user inputs which shall be processed and manipulated by runners. The input provided by end users / front-end systems are stored in "parameters" element of the payload, whereas runners receiving events are allowed to add or edit any part of this section.

### Result

This section of event data (result) contains information about the results of last event runner's process (such as status and error details), which is typically used by the saga orchestrator in deciding whether to continue a saga flow or not.

<details>

<summary>Event Data Schema</summary>

```json
{
	"title": "Event data",
	"type": "object",
	"properties": {
		"requestMeta":{
			"title": "Request metadata",
			"type": "object",
			"properties": {
				"id": {
					"title": "Globally unique request id",
					"type": "string",
					"example": "21997272430159",
				},
				"channel": {
					"title": "Gateway channel receiving request",
					"type": "string",
					"example": "rpc",
				},
				"path": {
					"title": "Url path called on channel",
					"type": "string",
					"example": "/Hello",
				},
				"method": {
					"title": "HTTP method called on channel",
					"type": "string",
					"example": "POST",
				},
				"origin": {
					"title": "Request origin details",
					"type": "object",
					"properties": {
						"system": {
							"title": "System calling the API",
							"type": "string",
							"example": "web-store"
						},
						"type": {
							"title": "Type of user calling the API",
							"type": "string",
							"example": "customer"
						},
						"id": {
							"title": "Id of user calling the API",
							"type": "string",
							"example": "123"
						},
						"ip": {
							"title": "IP of user calling the API",
							"type": "string",
							"example": "127.0.0.1"
						},
						"agent": {
							"title": "User agent of user calling the API",
							"type": "string",
							"example": "Chrome"
						},
						"offset": {
							"title": "Time offset of user calling the API",
							"type": "integer",
							"example": -60
						},
						"lat": {
							"title": "Latitude of user calling the API",
							"type": "integer",
							"example": 0.0
						},
						"lon": {
							"title": "Longitude of user calling the API",
							"type": "integer",
							"example": 0.0
						},
						"parameters": {
							"title": "Custom parameters about the origin",
							"type": "object"
						}
					}
				},
				"auth":{
					"title": "Authentication details",
					"description": "Removed by API gateways",
					"type": "object"
				},
				"sessionId": {
					"title": "User's session id",
					"type": "string",
					"example": "123"
				},
				"startDT": {
					"title": "Request start date/time",
					"type": "number",
					"example": 1678114017948
				},
				"endDT": {
					"title": "Request end date/time",
					"type": "number",
					"example": 1678114017948
				},
				"status": {
					"title": "Request status",
					"type": "string",
					"example": "SUCCESS"
				}
			}
		},
		"sagaMeta": {
			"title": "Saga metadata",
			"type": "array",
			"items": {
				"type": "object",
				"properties": {
					"sagaId": {
						"title": "Id of the saga flow",
						"type": "string",
						"example": "123"
					},
					"stepId": {
						"title": "Current step of the saga flow",
						"type": "string",
						"example": "123"
					},
					"startDT": {
						"title": "Saga start time",
						"type": "integer",
						"example": 1692097879248
					},
					"endDT": {
						"title": "Saga end time",
						"type": "integer",
						"example": 1692097879248
					}
				}
			}
		},
		"eventMeta": {
			"requestTime": {
				"title": "Event request time",
				"type": "integer",
				"example": 1692097879248
			},
			"handler": {
				"title": "Event handler to use",
				"type": "string",
				"example": "read"
			},
			"action": {
				"title": "Handler action to call",
				"type": "string",
				"example": "Get"
			},
			"version": {
				"title": "Action version to use",
				"type": "integer",
				"example": 0
			},
			"domain": {
				"title": "Action domain",
				"type": "string",
				"example": "product"
			},
			"inputElement": {
				"title": "Payload element to input",
				"type": "string",
				"example": "parameters"
			},
			"outputElement": {
				"title": "Payload element to output",
				"type": "string",
				"example": "$.product"
			},
			"parameters": {
				"title": "Action parameters",
				"type": "object",
				"example": {
					"idPath": "productid"
				}
			}
		},
		"payload": {
			"title": "Payload",
			"type": "object",
			"example": {
				"parameters": {
					"productid": "123"
				}
			}
		},
		"result": {
			"title": "Result",
			"type": "object",
			"properties": {
				"receiveTime": {
					"title": "Event receive time",
					"type": "integer",
					"example": 1692097879248
				},
				"endTime": {
					"title": "Event completion time",
					"type": "integer",
					"example": 1692097879248
				},
				"status": {
					"title": "Event step status",
					"type": "string",
					"example": "SUCCESS"
				},
				"error": {
					"title": "Event step error details",
					"type": "object",
					"example": {
						"code": 123,
						"message": "Handler missing"
					}
				}				
			}
		}
	}
}
```

</details>

## Payload

Payload role type can be considered as a stripped down version of the Event roles, only including the payload section of event data. This role is typically used for consuming data feeds such as from Kafka topics, which are not in Event data format, but should be treated as such.&#x20;

{% hint style="info" %}
Since these records are missing metadata, their metadata should be created using override/default settings of their streams.  &#x20;
{% endhint %}

## Pulse

Pulses are used for change data capture and include only identification details of a change (such as updated product's id).

<details>

<summary>Pulse Data Schema</summary>

```json
{
	"title": "Pulse data",
	"type": "object",
	"properties": {
		"id": {
			"title": "Id of aggregate changed",
			"type": "string",
			"example": "123"
		},
		"partitionId": {
			"title": "Partition id of aggregate changed",
			"type": "string",
			"example": "0"			
		},
		"offset": {
			"title": "Offset of event causing change",
			"type": "integer",
			"example": 123
		},
		"offsetPartition": {
			"title": "Partition of event causing change",
			"type": "string",
			"example": "dev-0"
		},
		"journalOffset": {
			"title": "Offset of journal causing change",
			"type": "integer",
			"example": 123
		},
		"journalPartition": {
			"title": "Partition of journal causing change",
			"type": "string",
			"example": "jrn-0"
		}
	}
}
```

</details>

## Journal

Journals are also used for change data capture and include complete contents of a change (such as all updated fields of a product).

<details>

<summary>Journal Data Schema</summary>

```json
{
	"title": "Journal data",
	"type": "object",
	"properties": {
		"id": {
			"title": "Id of aggregate changed",
			"type": "string",
			"example": "123"
		},
		"action": {
			"title": "Type of change",
			"type": "string",
			"example": "CREATE"
		},
		"requestId": {
			"title": "Id of request causing change",
			"type": "string",
			"example": "123"
		},
		"content": {
			"title": "Contents of change in aggregate form",
			"type": "object",
			"example": {
				"id": "123",
				"data": {
					"name": "Test record"
				} 
			}
		},
		"parameters": {
			"title": "Extra parameters about change",
			"type": "object"
		}
	}
}
```

</details>

## Debug

Debug role is used for directly logging data received from a stream. To display these logs, runner log level must be set to at least DEBUG.

{% hint style="info" %}
A new role can be defined simply by creating a role alias inside a runner.
{% endhint %}
