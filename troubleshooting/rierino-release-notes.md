---
description: This section explains the new features and updates applied with each release.
icon: code-branch
---

# Rierino Release Notes

## 2.5.0 \[05/2026]

### Built-in GenAI Guardrails

Adding guardrails to GenAI interactions is both simplified and enriched with built-in, highly configurable guardrails that can be added to validate & mask inputs, tool responses and outputs for safe LLM use. Regular expressions, LLM judges and even custom rules, flows & logic can now be added as guardrails to any AI agent configuration.

### Built-in AI Chat Summarization & Rating

AI interactions can now be summarized manually or automatically through simple configurations, and users are able to rate all AI responses from within the agent screen for custom model training & fine-tuning as well as reporting.

### Thread & Parallelization Optimizations

Thread performance is further optimized for parallel saga flows, fire-forget use cases as well as for each loop calls, improving memory use and speed.

### Dynamic Header Mapping

API aliases now allow configuration of header maps, allowing automated transformation of custom headers required by third parties to Rierino standard headers and format.

## 2.4.0 \[04/2026]

### GenAI OpenTelemetry

In addition to existing insights within the chat memory, GenAI token and tool usage is now available as metrics and logs through OpenTelemetry, making them natively available for APM platforms.

### Adhoc Fire Forget Calls

All API calls can now be made asynchronous using X-Async header, allowing front-end applications to fire & forget requests.

### Simplified Dynamic Scheduling

Sagas can now be configured to have dynamically scheduled iterations (deciding on how long to wait for next iteration based on current run), without requiring a separate CDC stream configuration.

### New RAI Event Handler

A new RAI event handler replaces Langchain, providing more control over metadata, memory contents, tool governance and ability to summarize chat messages automatically.

## 2.3.0 \[03/2026]

### Dynamic AI Agent Instructions

AI agent instructions now allow using Handlebars templates with pre-processing using saga flows, to generate dynamic systems instructions specific to user and the use case.

### Automated MCP Server State Tools

MCP server configurations now allow selecting states for read and/or write activities, which automatically generate MCP tools with simple configurations.

### AI Agent Guidance Details

AI agent definitions now allow configuration of welcome messages, capability lists and dialog starters to support users better understand how they can benefit from each agent and what actions they can take on their behalf.

### New UI Widgets

3 new widgets are added to UI builder, providing ability to generate rich visual components - 3D Editor, Whiteboard Editor, PDF Editor.

## 2.2.0 \[02/2026]

### Server Sent Events

SSE is now supported out-of-box without any custom development, allowing use of any API endpoint to be delivered as continuous events, with granular delay/stop control, enabling use cases such as MCP events, user notifications.

### Home Page Enhancements

All home pages (global, app specific and agent listing) are now completely customizable using Handlebars templates. Default home pages are also enriched, providing easy access to apps, screens, agents and more.

### New UI Widgets

3 new widgets are added to UI builder, providing alternative methods for selecting values - Sliders, Ratings and Radio Groups.

### Extended Editors

It is now possible to define pre-configured editors and use them across screens, decreasing repetition of configuration and data entry.

## 2.1.0 \[01/2026]

### Enriched AI Agent Governance

AI agent configurations now allow global and granular control over repeated calls to tools, increasing usability of smaller LLM models for more complex use cases without running the risk of tool looping.

It is also now possible to customize response format for & from AI agents within saga flows, and separate their user & AI agent descriptions.

### Granular Response Header Control

API gateways can now be configured to return special headers in response, for each gateway channel, URL path and call method, allowing granular configuration of control headers for caching, security, rate limiting as well as metadata headers.

### New UI Widgets

3 new widgets are added to UI builder, providing capabilities for:

* Incorporating visual diagrams based on graph markdown contents
* Utilizing advanced data grid features for manipulating array-type data
* Using MDX for creating rich and interactive templates

### More Interactive Handlebars Templates

Handlebars widgets now support data-default attributes for automatically populating data on new records, as well as enriched data-drag & data-drop features for DnD interaction on more components.

### Navigation Enhancements

Navigation bar is enriched to simplify switching between apps, also making AI agents accessible across apps and bringing fully customizable global and app specific landing pages.

## 2.0.0 \[12/2025]

### Simplified Branch Management

A much simpler and flexible branching approach is introduced, allowing development and use of multiple branches at the same time, without requiring separate deployments or versioned copies of assets such as sagas, queries, UIs and AI models.

### Simplified Rollback Management

A new rollback screen now allows reverting changes on all or selected assets to pre-migration versions or historical snapshots, without the need for building custom backup flows. Merges on main branch also allow automated versioning, which can be used as rollback snapshots.

### Built-in User Rate Limiting

User and IP based global and granular rate limits are now available, removing the need for incorporating third party services or building custom counters inside saga flows.

### Table Enhancements

Tables now support more user friendly sorting as well as inline actions for simplified and faster user experience.

## 1.11.0 \[11/2025]

### More Dynamic mTLS Support

Both SSL and mTLS can now be configured with dynamic key and certificate managers, allowing use of local files, auto generated certificates, trust state managers as well as API based access for rotating server and client certificates.

### Custom Embedding Providers

In addition to existing ONNX embedding model files, now it is possible to use different embedding providers (such as OpenAI) simplifying use of a wide range of public and private models for vector search and similar operations.

### API Gateway Error Enrichments

API gateway channels now allow definition of retry configurations per path and also allow returning 'safe' error details without information disclosure risks, which can be specified using error safe pattern in error transformation steps.&#x20;

### Inline Visualization Widgets

Data visualization components, such as charts and metric displays can now be used as widgets inside UI tabs, in addition to already existing embedded dashboard screens.

### Cross-tab Cache Invalidation

Changes in any record as well as API calls can now be configured to trigger cache invalidations and content refresh on other browser tabs as well, without the need to refresh page contents.&#x20;

### Handlebars Data Tags

Handlebars templates now support data tags such as "data-event", "data-path" allowing automated interactivity for triggering built-in lister, editor and value widget actions and reflecting data entered in these custom displays on rest of the contents.

## 1.10.0 \[10/2025]

### Enriched Validation Messages

Json schemas can now include advanced validation parameters (such as x-soup and x-jmespath), and custom error messages written in Markdown can be displayed on admin UI for providing detailed feedback to users.

### More Interactive AI Agent UIs

Now, GenAI agent UIs can automatically trigger API calls, or automatically generate and submit messages to AI agents, making agent interactions more efficient and responsive.

### New UI Features

Various new features are added across different UI widgets, such as:

* Ability to embed imported data into a single record using FileUpload widget
* Read-only data pattern for simplified display of restructured data in editors
* Create-only snapshot option locking inputs after a record is created
* Drag & drop enabled handlebars lister for custom dependent lists
* SVG API for servicing platform's icons to external systems
* Additional pagination parameters on listers

### New File Features

For event handlers such as PDF and FS handlers, new features are added, including:

* Ability to convert and store base64 images as files
* Support for using local font files inside generated PDFs&#x20;

## 1.9.0 \[09/2025]

### AI Agent UIs

GenAI agents can now interact with users using UIs (such as displaying products as a product card or asking for inputs with editors for approvals). All existing UI widgets can be provided to AI agents for these interactions for representing and requesting data in a well structured  manner during agent communications.

### Batch Record Sorting

Batch sorting feature is added for updating multiple records' order all at once through drag & drop, for use cases such as changing order of blog posts inside query table lister.&#x20;

### Batch File Mapping

It is now possible to have a dedicated file upload page which automatically performs pre and post upload actions, such as grouping and renaming uploaded files for mapping them to existing content records.

### Fire-Forget Sagas

In addition to already existing fire & forget actions, now it is possible to configure individual saga flows as fire & forget, returning immediately with a reference ID for long running API calls without keeping threads open on client and API gateway layers for long periods of time.

### Environment Services

Admin UI can now utilize a predefined environment API to pull project & user specific configurations to customize all UI properties dynamically on the fly.

### Apache Rest Connector Option

In addition to existing standard connector, now the rest event handler supports using Apache connector through a simple configuration as an alternative.

## 1.8.0 \[08/2025]

### Simplified Error Handling

In addition to existing auto-fail and conditional error handling flows, now it is possible to:

* Define "On Fail" saga for each saga to automatically trigger recovery or error auditing flows
* Add step results (e.g. "SUCCESS", "FAIL") on each event step link for conditional routing without extra nodes
* Define "Continue On Fail" option on each step to allow continuing saga execution even when on "Auto Fail" mode

### Simplified Parallelization

In addition to existing "For Event Handler", now it is possible to:

* Have multiple starting steps for a saga flow to automatically run parallel flows
* Configure for each parameters on any event step to process array entries in parallel/sequence without additional configurations

### Stored Action Steps

A new event handler allows using predefined event steps across sagas without the need to copy their contents - all automatically updating whenthe stored step is reconfigured. This simplifies use of actions, which were previously available as runner elements.

### Built-in Favorites & Preferences

Admin UI now has built-in features allowing users to add/remove any record from their favorites list, and have preferences for applying default filters on any listing.

### Interactive Handlebars Editing

A new drag & drop UI builder allows creating custom Handlebars templates with a WYSIWYG editor and the rendered handlebars editors allow clicking on contents to edit them for building visual content editors.

### Bulk Media Editing

A new media editor allows quick upload and mapping of images for different channels and resolutions with a bulk upload feature.

### SQL State Enhancements

In addition to existing XA distributed transactions, a more simplified, local transaction model is added, as well as a new write action to call stored procedures on SQL state managers for non value returning calls.

## 1.7.0 \[07/2025]

### Traceability Enrichments

API gateway logs and spans now have additional structured attributes and allow passing trace id back to client for end to end traceability.

### WoT Produce

A new event handler now allows producing a WoT "Thing" definition for allowing access to runner APIs as actions to invoke.

### AI Agent Action Auditing

AI agents can now return information on their token usage, metadata and tool execution details as part of their response for detailed understanding of their actions and cost.

### Admin UI Connection Reset Recovery

In cases where the load balancer above admin UI causes connection resets due to misconfigured keep alive settings, admin UI can automatically retry calls to recover connections. &#x20;

## 1.6.0 \[06/2025]

### WoT Auto Discovery for AI Agents

AI agents can now discover properties and actions they can take on web of things (e.g. IoT devices), allowing them control over physical world with simple configurations.

### General Purpose Locking

A dedicated lock event handler is added, allowing use of distributed locking mechanism across transactions even when external systems or non-atomic transactions are involved.&#x20;

### Simplified Pro-Code Custom Editors

While extending UI widgets has been possible using WebComponents, this new feature allows entering React code directly into Rierino web interface for creating custom components which can be used in any UI screen. Component creation is also supported by RAI for building AI generated components.

### Scroll Helper

Optional scroll helper buttons are added to UI screens, allowing quick access to top & bottom of the page when its length is relatively long for navigating using scrollbar.

## 1.5.0 \[05/2025]

### AI Agent Enrichments

AI agent model configurations now support access to all state managers for read & write operations, API calls to 3rd party systems simply by configuring an OpenAPI document URL and custom script execution capabilities.

### Saga Auditing

Calls to saga flows now allow using "rierino-audit-path" header to return full history of steps executed for real-time detailed trace and debugging.

### Table Lister Ordering

Query table listers now allow automated priority field updates by shifting records with a click of a button.

### Log Enrichments

Default logging contents now include additional tags for easy debugging, without a full OpenTelemetry set-up.

## 1.4.0 \[04/2025]

### MCP Server Development

A new event handlers allows rapid development of MCP servers with support for:

* **Tools:** Through simple mapping of existing or new sagas
* **Resources:** Through simple mapping of existing or new state managers
* **Prompts:** Through configurations

### A2A Server Development

A new event handler allows rapid development of A2A servers which utilize existing LLM saga flows for task handling, making them usable for agent to agent interactions.

### AI Agent Instructions

AI agent model configurations now support including instructions, which are passed as system messages to the underlying LLM models.

### Web of Things Consumer Development

A new event handler allows development of WoT integration capabilities for communicating with IoT and similar devices.

### Web Parser Features

A new event handler is created to allow parsing of HTML documents and page URLs.

### Fire-Forget Actions

While asynchronous actions had been supported through various means in the past (such as calls through Kafka topics, task event handlers, Py4J calls), a new feature allows any saga step to be processed as a fire & forget action through simple configuration on all runner types - typically used with long-running LLM tasks in A2A protocol.&#x20;

## 1.3.0 \[03/2025]

### New OpenTelemetry Features

Additional OpenTelemetry log, tracer and metric features are added to provide in-depth insights into individual elements within microservices, including:

* New span attributes for runners, handlers, state and query managers
* Automated span linking from front-end to all backend function calls
* Granular counters and histograms with key request and operation dimensions &#x20;

### Json Schema Validation Extension

A custom "x-validation" keyword is now supported in Saga input schemas, allowing automated validation of requests with custom build Validator classes, including a "JsoupValidator", which allows easy detection of non-safe HTML tags in API inputs.

### Admin UI Style Customization

Admin UI received a complete revamp on its widget structure, replacing previous library with a completely new library and Tailwind based styling. It also support multiple themes, selectable by end users.&#x20;

### Admin UI Stylized Export / Import

New "Import XLSX" and "Export XLSX" Admin UI menu actions allow rich formatting of Excel files for downloading contents from the front-end layer.

## 1.2.0 \[02/2025]

### AI Agent Development

New features are added to facilitate development and utilization of AI agents powered with all existing Rierino capabilities (such as access to enterprise data sources, integration with other systems, execution of business rules), including:

* GenAI Model configurations under Data Science application
* AI Agent user interface that can be incorporated into any application, selecting allowed list of agents
* Ability to select from a wide range of LLM providers for AI agents using LangChain model event handler

### Concurrency Conflict Handling

Ability to handle version conflicts when performing updates through admin UI is added, allowing users to "overwrite" or "merge into" updates from other users.

### Gantt Lister

A new lister with gantt-chart style functionality is added for easy management of date based activity management.

### New JMESPath Extensions

The following extensions are added to JMESPath patterns with this release:

* **encrypt:** Encrypts given data input
* **remove\_nulls:** Removes null values from object inputs
* **uuid:** Generates a universal unique ID

## 1.1.0 \[01/2025]

### Board Lister

A new lister with kanban-style board functionality is added for easy management of tasks and software requirements.

### Map Editor Preset Option

A new configuration "preset" is added to map editors, allowing a predefined list of keys to be displayed for easy and structured data entry.

### GitBook Help Integration

GitBook search functionality is added to help button, to allow in-app searching of relevant content on GitBook.&#x20;

### Request Metadata Claims

A claims map is added to request metadata, allowing passing of user access information in metadata (e.g. roles, seller id) as an alternate to passing them in event payload. This also provides CRUD event runners ability to utilize user access information in their processes (as CRUD runners only receive "parameters" otherwise).

### Advanced CRUD Operations

Additional parameters are added to state managers, allowing use of queries to check complex access rights (e.g. user can only access records of own organization) on records, which are utilized by Read, Condition and Write event handlers. These parameters allow using basic CRUD event runners with automated access control, without requiring definition of custom Sagas each time.

### LangChain4J Handler

A new ML handler is added, which allows use of chat and image processing models based on a large list of LLM providers (such as OpenAI, Bedrock, Anthropic, Gemini, Mistral, Dall-E, etc.).

## 1.0.0 \[12/2024]

### RAI Vision Features

RAI the AI assistant now supports copying and pasting images to generate content, such as:

* Generating Handlebars template from web page screenshots
* Generating detailed descriptions from product photos
* Generating JSON schema from ER diagrams

### Advanced Admin-UI Login Configurations

Additional configuration options are enabled on Admin-UI, to enable/disable reCAPTCHA, OTP, external apps in authentication flows easily.

### Saga Scheduler

Sagas can be scheduled directly on the runners now, eliminating the need for cron-job deployments for  recurring single Saga call activities.

### StatfulSet Deployment Option

Deployments can be now configured to deploy as StatefulSet on kubernetes clusters, with pod index assigned as their partition.

### Couchbase State Manager

Built-in support for Couchbase is added as a state manager, which can be used via Core+ package.

### Read-Only State Manager

State managers can now be configured to be read-only, to safeguard them against accidental use in write action steps.

### New JMESPath Extensions

The following extensions are added to JMESPath patterns with this release:

* **salt\_key:** Generates a secure random key with given length for encryption
* **merge\_index:** Merges two arrays by their index
* **group\_by:** Groups entries of an array by a given field
* **element\_at:** Returns entry of an array at given index
* **cron:** Evaluates a cron expression and compares against a given time

### Admin UI Landing Path

LANDING\_PATH environment variable to redirect users to a specific app or page on domain root URL.

### Document Export Handlers

Excel and PDF export event handlers are now available for creating formatted files using event payloads.&#x20;

### Gateway System & Channel Integration

Gateway system configurations are now possible from within channel configuration screen to simplify gateway - runner API mappings.

### Python Processor Runner

A direct Process runner is added to Python library to allow running a processor directly from command line without wrapping with an EventHandler for simple job & cron-job use cases.

### Rest Response Headers & Cookies

Rest event handlers can now include response headers and cookies in returned results, in addition to response bodies.

### Direct RPC Runner Calling

RPC runners now support /api\_direct endpoints which allow calling them in similar form as the API gateways (as opposed to /api endpoints which require full Event data format), to simplify in-cluster requests.&#x20;

## 0.8.3 \[11/2024]

### GraphQL Query Support

CRUD event runners now support single-state graphql query operations, with filter, limit, skip and field selection, on /\[channel]/graphql endpoints.

### Embedded Gateway System Configuration

Gateway channel configurations now allow embedded configuration of gateway system parameters, allowing new channel definitions without a matching system definition.

### Runner Element Status

Elements added to runners can now be set to 'Draft' or 'Passive' status, to disable them within runners without removing them from their configuration.

### Maximum Rows Limit on States

State managers now allow configuration of 'maximum rows to read', which restrict the maximum number of rows read and condition handlers can retrieve from them.

### Application.Properties Configuration with Deployment

'rierino.deployment' parameter is enabled in application.properties files (in addition to rierino.runner parameters), to allow dynamic inclusion of runners on application restart.

### Ready-Core Helm Chart

An umbrella helm chart named 'ready-core' is added to installation assets to allow deployment of core platform without using Ansible playbooks.

## 0.8.2 \[10/2024]

### New RAI Features

This release extends our RAI the AI assistant functionalities with the following:

* **Query AI:** Facilitates creating simple, aggregation and pipeline queries, with the ability to incorporate custom database expressions &#x20;

### Admin UI Proxy

Admin UI is extended with /api endpoints, which can be used as a proxy to admin gateway APIs. This feature allows restriction of direct access to the API gateway as well as CORS control when the UI and gateway services are deployed on different domains.

### Simple Rule Event Handler

A new event handler is added, which allows evaluation of rules without using Drools as a comprehensive rule engine, for less sophisticated use cases.&#x20;

### Rest Retry & Fallback

Retry and fallback parameters are added to REST system configurations for granular and automated control over access issues in external systems. &#x20;

### State Loader Update On Parameter

A new parameter is added to state loader configurations, allowing users to disable triggering state listeners (e.g. recompiling templates) when a received record is not a real update on existing local copy.

### Admin UI Custom Code Actions

Custom code menu actions and events are added to admin UI, to allow extension of client-side flows and triggers without the need for using remote components.

### Admin UI Conditional Menus

Conditional display of editor and widget menus is enabled, with the same parameters as conditional tab/editor displays.

### Output Element Ignore Option

'-' option is added for event step output element configuration, which allows ignoring produced output from the step.

### New JMESPath Extensions

The following extensions are added to JMESPath patterns with this release:

* **get\_element:** Retrieves specific element inside an array
* **group\_by:** Groups elements of an array by a given key field
* **decode\_url:** Decodes provided URL string
* **encode\_url:** Encodes provided URL string
* **trim:** Trims provided string
* **type:** Returns type of provided input

### Flattened Map Editor

The following parameter is added to MapEditor to allow editing nested objects:

* **flatten:** Flag, which allows editing nested objects if set to true.

### Delimited Text Editor

A new editor is added allowing easy management of delimited text contents.

### Collapsable Editors

A new parameter "collapse" is added to all editors, which allows grouping child editors for collapsed view, to simplify using UIs with high number of widgets.

## 0.8.1 \[09/2024]

### RAI Launch

This release includes our launch of RAI the AI assistant, which embeds GenAI capabilities into Rierino UI and APIs. The following assistants are included with this release:

* **Text AI:** Facilitates creating and rewording textual contents, including templates and codes, which can be added to value editors
* **Translate AI:** Facilitates translation of textual and complex contents, which can be added to localized editors
* **Generic AI:** Facilitates creation and revision of business records such as products, which can be added to item menus
* **UI AI:** Facilitates creation and enrichment of UI designs, which is already incuded in UI design menus
* **Schema AI:** Facilitates creation and revision of data schemas, which is already incuded in schema design menus

### Validate Handlebars Menu Action

A new menu action is added for validating Handlebars templates from within the admin UI directly, without the need for trying to render contents. This menu action can be added to any Handlebars value editor.

### Migration Lister

A new lister type is added to simplify migration from development to test and production environments, eliminating the need for migrating different assets (e.g. sagas, elements, queries) from different screens.

This screen is accessible through /app/devops/common/migration path.

## Earlier Releases

If you are using an older version and need to learn about updates between releases 0.0.0 and 0.8.0, please contact our technical team.
