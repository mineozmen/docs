---
description: >-
  Rierino platform covers various components, used in front-end, microservices,
  data storage and integration as well as analytics
icon: sitemap
---

# Architecture

Key components included in a full deployment are as follows:

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption><p>Platform Architecture</p></figcaption></figure>

## Hyper-Flexible Front-end

### External UIs & Systems

As a headless platform, Rierino allows integration with any customer facing channel (e.g. web, app, social, video, virtual) and technology through its set of APIs. Removing dependency between the platform and the front-end gives creative freedom for branded and customized user experiences, while allowing microservice developers to focus on performance and cost optimization.

### Admin UI

Rierino is shipped with a highly customizable Next.js front-end for the internal users, referred to as the Admin UI. While, similar to the customer facing channels, it is possible to integrate Rierino back-end with any UI for administration through its set of admin APIs, Rierino already provides a flexible Admin UI for easier and faster deployment.

The Admin UI can be customized by configuring Apps, UIs, Sources and Options as defined in [user interface](../design/user-interface/) section. It also has an embedded [data visualization](../data-science/data-visualizations.md) component, which can service BI dashboards, ML models and data quality insights directly within the interface where users manage products, prices, etc.

## Hyper-Scalable Back-end

### API Gateways

Rierino deployments include a gateway layer, which includes request routing, authentication and session management capabilities. While it is possible to use an external API gateway such as Kong, Rierino already provides a flexible layer for easier and faster deployment.

This layer consists of a dedicated Spring Webflux deployment for Gateway Server which communicates with runner(s) responsible for authentication and session management activities.

All authentication and session management actions are managed as API flows, making them 100% customizable, allowing integration of existing security systems and policies.

The gateway layer can be customized by configuring Systems, Channels, Services and Tokens as defined in [Gateway & Security](../devops/gateway-and-security/) section.&#x20;

{% hint style="info" %}
While it is possible to deploy a single gateway layer for all types of users and requests, it is recommended to have dedicated instances for different user types (e.g. internal users, sellers, buyers) to allow customized network security and performance management configurations for each.
{% endhint %}

### Micro-Composable Services

Micro-composable service runners sit at the heart of Rierino platform, and can be designed, deployed and controlled through the admin UI. Each service runner consists of a specific set of [elements ](../devops/microservices/elements/)which define the data sources it can access, list of functions it can execute and message channels it can use, providing ability to build specialized micro-services.

While it is possible to develop and deploy service runners using any language or platform, Rierino is shipped with 3 main types of Java based runners:

* **Spring Runners:** Using Spring Webflux framework to deploy reactive web runners
* **Samza Runners:** Using Apache Samza library to deploy message based runners&#x20;
* **Camel Runners:** Using Apache Camel library to deploy runners capable of custom integrations

### Batch Tasks

Rierino is shipped with a set of Python libraries for most common analytics & batch tasks, such as data integration, customer segmentation, recommendation models, data quality assessment, data import or export.

These batch tasks are typically deployed and triggered by a scheduler (e.g. Airflow), ensuring traceability and resilience of each task.

In addition to these tasks, Rierino is shipped with a set of devops features, to simplify deployment and maintenance of runners, which can be also managed through external CI/CD platforms.&#x20;

### 3rd Party Platforms

Rierino provides ability to integrate with 3rd party solutions (such as payments, OMS, WMS) through both REST API and data based integration approaches.

## Plug & Play Storage

### Data Stores

Rierino supports use of various data systems with varying latency and proximity. Each of these systems are replaceable with minimal effort (such as developing an adapter if necessary).&#x20;

* **Cache:** Local and shared caching options for lowest latency operations
* **Operational:** NoSQL & SQL data stores used as master or read/write stores
* **Search:** Specialized search engines for use cases such as product search
* **Real-Time:** Columnar or similar real-time analytics data stores for insights
* **Data Lake:** Big data store for high latency and analytics use cases

### File Systems

In addition to structured/semi-structured data systems, Rierino supports use of file systems such as FTP, HDFS, S3 for storing assets such as trained ML models, digital assets and documents.
