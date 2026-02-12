---
icon: brain-circuit
---

# Built with ML & AI

Rierino supports extensive use of ML\&AI capabilities, out of box, including:

## **AI-Powered Productivity with RAI**

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

At the heart of Rierino’s AI-supported development capabilities is the AI assistant, [RAI](https://rierino.com/platform/rai). Fully embedded into the Rierino UI, RAI empowers users with tools to automate a variety of tasks, from generating content and creating user interfaces to managing workflows and analyzing data. By integrating RAI into the platform, Rierino bridges the gap between technical and non-technical users, providing an intuitive, conversational interface that enhances productivity and fosters innovation across the organization.

For more details [click here](../design/user-interface/uis/menus/rai-menu-actions.md).

## **'Empowered' AI Agent Development**

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

[Rierino AI Agent Builder](https://rierino.com/platform/agent) provides ability to rapidly create AI agents that can utilize internal data sources as well as enterprise systems and APIs for triggering automated actions, ranging from retrieving product details for ecommerce clients to triggering purchase orders for internal employees. All these tools are automatically governed by security policies and user access rights, providing the foundations for building well structured assistants for all stakeholders.

{% embed url="https://www.youtube.com/watch?ab_channel=Rierino&v=jgKj4_019Ps" %}

For more details [click here](../data-science/genai-models/).

## **MCP Server & Middleware**

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption><p>MCP Server</p></figcaption></figure>

Rierino provides ability to service all of its functionality over Model Context Protocol (MCP), with a simple configuration that allows selection of flows to be serviced as tools and states to be serviced as resources over MCP, while maintaining already existing security policies and users roles. Any API, event processing or CDC flow can be included as MCP server tools and triggered using any protocol already supported by Rierino (e.g. REST, WebSocket, Event Streaming), going beyond the standard MCP implementations.

MCP servers can be built from scratch, using functionality such as database access, business rule evaluation, ML model prediction, external REST API calls, or could access 3rd party MCP servers acting as a middleware based on request details.&#x20;

For more detils [click here](../devops/microservices/elements/handlers/ml-and-ai-handlers/service-mcp-requests.md).

## A2A Server & Middleware

Similar to MCP protocol, Rierino provides ability to service its functionality over Agent2Agent (A2A) protocol, with a simple configuration that allows selection of LLM flows for servicing tasks for other AI Agents.&#x20;

For more detils [click here](../devops/microservices/elements/handlers/ml-and-ai-handlers/service-a2a-requests.md).

## **Real-Time Inference with Specialized Handlers**

Rierino enables seamless real-time machine learning by providing specialized handlers that work with a variety of prebuilt ML models. These handlers ensure that your system can process and analyze data in milliseconds, empowering businesses to make instant, data-driven decisions. Whether it’s dynamic pricing adjustments, personalized recommendations, or fraud detection, Rierino’s real-time inference capabilities integrate into your APIs and workflows, offering both speed and accuracy without the need for complex configurations.

For more details [click here](../devops/microservices/elements/handlers/ml-and-ai-handlers/use-ml-models.md).

## **Seamless Integration with REST API-Based AI Services**

Rierino's platform integrates seamlessly with leading REST API-based AI services, including OpenAI and others, to unlock cutting-edge generative AI and predictive analytics functionalities. This connectivity allows businesses to leverage external AI capabilities, such as natural language processing, content generation, or sentiment analysis, without the need for complex middleware. The result is a unified system where AI-driven innovation becomes a natural extension of your workflows.

For more details [click here](../devops/microservices/elements/handlers/core-handlers/call-rest-api.md).

## **Custom Python ML Model Training and Scoring**

Harness the power of custom machine learning models directly within Rierino. Using Python-based capabilities, users can orchestrate the training and scoring of models tailored to their unique business needs. This flexibility allows organizations to design, refine, and deploy bespoke ML solutions—whether for predictive analytics, customer segmentation, or operational optimization. By providing tools that integrate custom scripts with the broader platform, Rierino ensures your ML strategies align perfectly with your goals.

For more details [click here](../devops/microservices/elements/handlers/custom-code-handlers/run-python-procedure.md).
