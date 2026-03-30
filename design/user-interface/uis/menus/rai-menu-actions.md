---
description: >-
  RAI menu actions are AI assistance actions that facilitate various automation
  use cases.
---

# RAI Menu Actions

<figure><img src="../../../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Sample RAI Action</p></figcaption></figure>

RAI allows using text prompts, as well as images, which can be copied and pasted into the prompt space to include them in the conversation (e.g. create description based on this product image).

When RAI is activated on a screen, it starts a conversation thread, and uses memory of that thread in consequtive actions and answers. To clear its memory and restart, RAI dialog can be closed and reopened.&#x20;

## RAI Parameters

RAI can be empowered using 2 different approaches:

### Frontend Agent (F)

These agents are embedded into Admin UI and make direct calls to OpenAI, utilizing assistant APIs, where tool calls are automatically translated into changes in UI. They are optimized for specific development support activities only and do not have access to regular backend sagas or states.

Since they do not need to go through API gateway and backend microservices, they are better suited for interactions involving large sized contents such as images.

These agents support the following special properties:

* **Prompts:** List of label, content pairs providing a predefined list of prompts that users can start with
* **Path:** Special path on api/ai to call AI actions (default is specific to action type)
* **Params:** List of special parameters to pass on to AI service, such as:
  * **assistantId:** ID of the AI assistant to utilize

### Backend Agent (B)

These agents utilize the [GenAI models](../../../../data-science/genai-models/) configured on backend microservices, giving them access to all capabilities of Rierino through sagas, states and systems. They can even write and execute dynamic scripts on the fly.

These agents support the following special properties:

* **Prompts:** List of label, content pairs providing a predefined list of prompts that users can start with
* **Params:** List of special parameters to pass on to AI service, such as:
  * **Agent:** ID of the GenAI model to utilize
  * **API Path:** API path to communicate with the agent (e.g. request/train\_rpc/CallAIAgent)
  * **Predefined Prompt:** ID of the GenAI model prompt, if request will be sent as a structured data using predefined prompt template

## General Purpose Actions

### Text AI (F,B)

Allows manipulation of value widgets (including text, code, template type inputs). This action can be used for a variety of use case, such as:

* Generating, rewording or summarizing text content such as product descriptions
* Generating or revising Handlebars templates used in CMS modules
* Generating or revising custom scripts or JMESPath patterns used in Saga flows

When used in frontend agent mode, by default, GPT\_ASSISTANT\_TEXT environment variable is used to identify the assistant ID to use for AI interactions.

### Translate AI (F,B)

Allows translation of localized text and objects. This action is used with Localized Editors, and allows selection of source and target languages for translation, based on editor's configuration.&#x20;

This action has the following special properties:

* **To JSON:** Whether response should be converted to JSON object
* **Data Path:** Item data path to inject received response into in current record (e.g. data.content)
* **Separate Answer:** Whether agent should also send its comments in addition to data updates

When used in frontend agent mode, by default, GPT\_ASSISTANT\_TRANSLATE environment variable is used to identify the assistant ID to use for AI interactions.

### Generic AI (F,B)

Allows population of a data model (e.g. creating a product entry) using a predefined schema.&#x20;

This action has the following special properties:

* **To JSON:** Whether response should be converted to JSON object
* **Data Path:** Item data path to inject received response into in current record (e.g. data.content)
* **Separate Answer:** Whether agent should also send its comments in addition to data updates
* **Schema:** JSON schema to utilize for populating data (F)

When used in frontend agent mode, by default, GPT\_ASSISTANT\_ANY environment variable is used to identify the assistant ID to use for AI interactions.

## Development Actions

### Saga AI (F)

Allows creation of new Saga flows.

By default, GPT\_ASSISTANT\_SAGA environment variable is used to identify the assistant ID to use for AI interactions.

### Runner AI (F)

Allows creation of new Runner entries.

By default, GPT\_ASSISTANT\_RUNNER environment variable is used to identify the assistant ID to use for AI interactions.

### Query AI (F)

Allows creation of new Query entries.

By default, GPT\_ASSISTANT\_QUERY environment variable is used to identify the assistant ID to use for AI interactions.

### UI AI (F)

Allows creation of new UI entries.

By default, GPT\_ASSISTANT\_UI environment variable is used to identify the assistant ID to use for AI interactions.

### App AI (F)

Allows creation of new App entries.

By default, GPT\_ASSISTANT\_APP environment variable is used to identify the assistant ID to use for AI interactions.

## Using RAI as Frontend Agents

While backend agents are automatically accessible if they are built into runners, to enable frontend agents, following configurations are required:

* [ ] "OPENAI\_API\_KEY" environment variable must be present in admin UI deployments.
* [ ] RAI assistants need to be created before the first use, which can be performed by using POST /api/\[type]/assistant endpoints if the "OPENAI\_API\_KEY" is configured.
* [ ] Once the assistants are created, either assistantIds can be configured for each menu action separately on admin UI, or the "GPT\_ASSISTANT\_%" environment variables mentioned in this section can be configured for the admin UI deployments.

After the first initialization, it is possible to manipulate prompts and tools of these assistants directly from the OpenAI dashboard and can be reset using PATCH /api/\[type]/assistant endpoint.

All assistants are also accessible through POST /api/\[type] endpoints, passing commands as "content" input.
