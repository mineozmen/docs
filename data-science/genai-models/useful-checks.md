---
description: >-
  This section explains most common errors and the checks to perform for
  troubleshooting
---

# Useful Checks

## "Model not found" error

* [ ] Check if the agent id in request matches the GenAI model record id
* [ ] Check if the GenAI model is configured to be allowed for the specific runner (or all runners)
* [ ] Check if the runner LangChain handler has a "domain" specificiation that doesn't match the model's domain configuration
* [ ] Check if the "channel" in request URL path matches the specific runner

## Agent not remembering chat history

* [ ] Check if the agent has "memory" parameter configured for an existing state manager on the runner
* [ ] Check if the agent memory state manager is a shared state (e.g. not in-memory), if there are multiple replicas running for the runner

{% hint style="info" %}
Only agents with memory utilize instructions & tools, so it is recommended to havethe "memory" parameter defined except for lightweight chat use cases.
{% endhint %}

## Agent not executing tool as expected

* [ ] Ask the agent to provide the request body and the response details (such as errors) it is receiving from calling the tool
* [ ] If the tool is a saga flow:
  * [ ] Try to access saga as an API with the same request and review response
  * [ ] Check to make sure that the saga is allowed to run on the runner of the agent
  * [ ] Check to make sure that the user making the request has rights to execute the saga (if it is restricted)
  * [ ] Check to make sure that the saga returns a "message" field in its response
  * [ ] Check to make sure that the saga input schema is mapped accurately for the flow logic
* [ ] If the tool is a state processor:
  * [ ] Check to make sure that the target state is available on the runner of the agent
  * [ ] Check to make sure that the user making the request has rights to access the state (if it is restricted)
* [ ] If the tool is a system processor:
  * [ ] Check to make sure that the target system is available on the runner of the agent and has access details configured correctly
  * [ ] Create a simple saga to test system accessibility through an API&#x20;
