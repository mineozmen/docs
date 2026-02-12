---
description: >-
  In addition to full scale GenAI models, Rierino provides facility for creating
  MCP servers utilizing already existing functionality
icon: network-wired
---

# MCP Servers

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption><p>MCP Server UI</p></figcaption></figure>

MCP server definitions allow configuration of services that are available through model context protocol through simple mapping of sagas as tools.&#x20;

Initial definition of a server includes:

* **Name:** A descriptive name
* **Description:** Detailed description of the server
* **Tags:** Descriptive tags for the server
* **Status:** Whether this server should be deployed or not
* **Version:** Current version of the server
* **Domain:** Categorization of the server based on business domain
* **Comments:** Historical list of comments, which typically includes information on server releases
* **Parameters:** Server asset configurations including:
  * **Tool Sagas:** Mapping of saga flows as tools
  * **Prompts:** List of prompts to provide
  * **Resources:** Mapping of state managers as static resources
  * **Resource Templates:** Mapping of state managers as dynamic templated resources
