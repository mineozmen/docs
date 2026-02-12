---
description: >-
  Rierino is a low-code microservices and AI agent development platform with an
  architecture designed for scale and flexibility
icon: circle-info
---

# Overview

Designed to match the speed, scale & versatility of any business, Rierino is the highly-adaptive and hyperintelligent next step where growth is a strong ambition. Inspired by one of the world’s most unique gentle giants, Rierino is determined to support digitally mature businesses in establishing new territories and facing competition head on.

The following table compares [Rierino Core](https://rierino.com/platform/core) against different categories of traditional low code development platforms, along with the core capabilities provided out of box with Rierino:

<figure><img src=".gitbook/assets/image (144).png" alt=""><figcaption><p>Rierino vs. Traditional Low Code Development Platforms</p></figcaption></figure>

{% hint style="info" %}
Fastest way to start testing out and developing on Rierino is using our 'Developer Lite' edition on AWS Marketplace, which is deployed as a multi-service VM within the region and instance type of your choosing with 100% control.&#x20;

<mark style="background-color:yellow;">**Click**</mark> [<mark style="background-color:yellow;">**here**</mark>](https://aws.amazon.com/marketplace/pp/prodview-up2fcxku3k742) <mark style="background-color:yellow;">**to start with AWS now.**</mark>&#x20;

You can also check all alternative ways to start using Rierino [here](htps://rierino.com/start).
{% endhint %}

## Front-End UI

### Internal User Experience

Rierino provides a low-code UI builder for building web forms and data lists, typically used by internal users and partners in managing products, contents and other business assets. It is also used for user interactions during workflow tasks and triggering automated actions.&#x20;

Rierino UI comes with embedded BI capabilities, ensuring business users have access to the data required for their decision making directly within the platform. It also supports AI assisted operations, such as summarizing, translating or rewording content.

Rierino also allows extension of its UI capabilities with incorporation of 3rd party webcomponents, as well as Handlebars templates for customization requirements.&#x20;

You can find related capabilities under [User Interface](design/user-interface/) and [Data Visualization](data-science/data-visualizations.md) sections of this document.

## Services & Business Logic

### Microservices Development

A key value proposition and differentiator of Rierino is the ability to build complex microservices with a low-code, drag\&drop interface. These microservices can range from simple database operations to execution of complex business logic and ML models and can be developed without writing single line of code.

Rierino also provides the capabilities for deploying these microservices in a variety of infrastructure, ranging from public and private cloud providers to on-prem and bare metal deployments.&#x20;

You can find related capabilities under [Microservices](devops/microservices/) section of this document.

### Microservices Orchestration

In addition to creating microservices, Rierino provides ability to orchestrate these microservices, as well as any 3rd party microservice, converting them to real-time API flows, asynchronous trigger flows as well as workflows.

Orchestration allows users to extend the platform using any language, or integrate already existing microservices into the platform seamlessly.

You can find related capabilities under [API Flows](devops/api-flows/) and [Gateway & Security](devops/gateway-and-security/) sections of this document.

### Workflow Management

In addition to creating real-time APIs, Rierino can be used to design workflows, assign tasks to users, let them review and provide process information from customizable UI screens and escalate them when tasks are not completed in time.

It is also possible to incorporate business rule and ML capabilities into workflows for decision and process automation.&#x20;

You can find related capabilities under [Orchestrate User Task](devops/microservices/elements/handlers/core-handlers/orchestrate-user-task.md) section of this document.

### Rule Management

Rierino platform contains a flexible and customizable business rule engine, which can be used to configure real-time decisioning mechanisms for any business domain (such as pricing decisions, fraud detection).

Depending on the modules deployed, the solution already includes a number of rule domains configured, which can be further customized based on business requirements.

You can find related capabilities under [Business Rules](configuration/business-rules/) section of this document.

### ML Automation

Rierino also provides real-time ML scoring and MLOps automation capabilities, which can be used to automatically train ML models and deploy them for real-time scoring.

Once configured on the platform, ML models developed by data scientists in R or Python are automatically converted and uploaded to model repository, which then can be used as a step in any API flow.

You can find related capabilities under [ML Models](data-science/ml-models/) and [Complex Event Processing](data-science/complex-event-processing/) sections of this document.

## Systems & Data Integration

### Database Systems

Rierino has an open architecture, allowing seamless integration with any type of data source, ranging from more traditional SQL databases to no-SQL databases, search engines and cache systems.

The platform also provides an abstraction layer for database operations and queries, making it possible to switch between different data systems without having to migrate all queries and procedures.

You can find related capabilities under [State Managers](devops/microservices/elements/state-managers/) and [Query Managers](devops/microservices/elements/query-managers/) sections of this document.

### External APIs

With Rierino, it is possible to integrate with public and private 3rd party APIs using different authentication mechanisms and communication formats (e.g. JSON, XML, SOAP).

You can find related capabilities under [Call Rest API](devops/microservices/elements/handlers/core-handlers/call-rest-api.md), [Call SOAP API](devops/microservices/elements/handlers/specialized-handlers/call-soap-api.md) and [Integrate with Camel](devops/microservices/elements/handlers/specialized-handlers/integrate-with-camel.md) sections of this document.

### Streaming Events

Another key value proposition and differentiator of Rierino is its ability to consume and produce real-time event streams (e.g. on Kafka), enabling a range of capabilities not possible using more traditional means (such as async tasks, CDC feeds, some analytics use cases).

You can find related capabilities under [Streams](devops/microservices/elements/streams/) section of this document.
