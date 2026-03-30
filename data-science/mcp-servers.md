---
description: >-
  In addition to full scale GenAI models, Rierino provides facility for creating
  MCP servers utilizing already existing functionality
icon: network-wired
---

# MCP Servers

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>MCP Server UI</p></figcaption></figure>

MCP server definitions allow configuration of services that are available through model context protocol through simple mapping of sagas as tools.&#x20;

Definition of an MCP server includes:

### Definition

* **Name:** A descriptive name
* **Description:** Detailed description of the server
* **Tags:** Descriptive tags for the server
* **Status:** Whether this server should be deployed or not
* **Version:** Current version of the server
* **Domain:** Categorization of the server based on business domain
* **Allowed For Runners:** List of runners which can service the MCP server

### Toolkit

* **Tool Sagas:** Mapping of saga flows as tools, automatically enabled in tools/list and tools/call
* **Tool States:** Mapping of state managers as read & write enabled tools
* **Resources:** Mapping of state managers as static resources, automatically enabled in resources/list and resources/read
* **Resource Templates:** Mapping of state managers as dynamic templated resources, automatically enabled in resources/templates/list and resources/read

### Prompts

List of prompts returned from prompts/list and prompts/get methods

### Comments

Historical list of comments, which typically includes information on server releases
