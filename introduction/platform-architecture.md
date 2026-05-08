---
description: >-
  Headless, multi-tier microservices architecture with full control over
  security, scalability, integrations and deployment model
icon: sitemap
---

# Platform Architecture

{% hint style="info" icon="magnifying-glass" %}
**In brief:** Rierino is a headless, micro-composable platform. The architecture has three layers:&#x20;

1. A flexible front-end with a customizable Next.js Admin UI and headless APIs for any customer channel
2. A scalable back-end with API gateways, composable Java service runners, and Python batch tasks
3. Plug-and-play storage supporting cache, NoSQL/SQL, search engines, real-time analytics stores, and data lakes
{% endhint %}

Key components included in a full deployment are:

<figure><img src="../.gitbook/assets/image (111).png" alt="External channels → API Gateway → Service Runners → Data Stores (Cache, Operational, Search, Real-Time, Data Lake). Admin UI connects to runners via admin APIs. Batch tasks (Python) connect to data stores and external systems."><figcaption><p>Platform Architecture</p></figcaption></figure>

{% embed url="https://rierino.com/platform/core" %}

## Hyper-Flexible Front-end

### External UIs & Systems

Rierino is headless. It integrates with any customer-facing channel through APIs. This includes web, apps, social, video, and virtual channels.

This decoupling gives you freedom on UX and tech choices. It also keeps backend teams focused on performance and cost.

### Admin UI

Rierino ships with a highly customizable Next.js front-end for internal users. This is the **Admin UI**.

You can still integrate any custom admin UI through admin APIs. The built-in Admin UI is the fastest path to a production-ready setup.

Customize the Admin UI using Apps, UIs, Sources, and Options. See the [user interface](../design/user-interface/) section.

The Admin UI also includes embedded [data visualization](../data-science/data-visualizations.md). Use it for BI dashboards, ML outputs, and data-quality insights. Keep insights close to the screens where users work.

## Hyper-Scalable Back-end

### API Gateways

Rierino deployments include a gateway layer. It covers request routing, authentication, and session management.

You can use an external gateway (for example, Kong). Rierino’s built-in gateway layer is often simpler to deploy and customize.

This layer typically uses a dedicated Spring Webflux deployment for Gateway Server. It communicates with runner(s) for authentication and session management.

Authentication and session management are implemented as API flows. This keeps them fully customizable. It also makes it easier to integrate existing security systems and policies.

Customize the gateway layer using Systems, Channels, Services, and Tokens. See [Gateway & Security](../devops/api-gateway-and-security/).

{% hint style="info" %}
While it is possible to deploy a single gateway layer for all types of users and requests, it is recommended to have dedicated instances for different user types (e.g. internal users, sellers, buyers) to allow customized network security and performance management configurations for each.
{% endhint %}

### Micro-Composable Services

Micro-composable service runners sit at the heart of Rierino. You can design, deploy, and control them through the Admin UI.

Each runner is composed from a set of [elements](../devops/microservices/building-blocks/). Elements define:

* Which data sources a runner can access
* Which functions it can execute
* Which message channels it can use

This makes it easy to build specialized micro-services.

While it is possible to develop and deploy service runners using any language or platform, Rierino is shipped with 3 main types of Java based runners:

* **Spring Runners:** Use Spring Webflux to deploy reactive web runners.
* **Samza Runners:** Use Apache Samza to deploy message-based runners.
* **Camel Runners:** Use Apache Camel to deploy runners for custom integrations.

### Batch Tasks

Rierino ships with Python libraries for common analytics and batch tasks. Use cases include data integration, segmentation, recommendation models, and data quality. It also supports import and export workloads.

Batch tasks are usually deployed and triggered by a scheduler (for example, Airflow). This improves traceability and operational resilience.

Rierino also includes DevOps features for runner deployment and maintenance. You can integrate these with external CI/CD platforms.

### 3rd Party Platforms

Rierino integrates with 3rd party solutions like payments, OMS, and WMS. It supports both REST API integration and data-based integration approaches.

## Storage and Data Systems

### Data Stores

Rierino supports multiple data systems with different latency and proximity. Each system is replaceable with minimal effort. In some cases you may only need a new adapter.

* **Cache:** Local and shared (In-Memory, Redis, Couchbase, etc.) caching options for lowest latency operations
* **Operational:** NoSQL & SQL (MongoDB, MySQL, PostgreSQL, Oracle, MS SQL, etc.) data stores used as master or read/write stores
* **Search:** Specialized search engines (Elasticsearch, Algolia, etc.) for use cases such as product search
* **Real-Time:** Columnar or similar real-time analytics data stores for insights
* **Data Lake:** Big data store for high latency and analytics use cases

### File Systems

In addition to structured and semi-structured data stores, Rierino supports file systems. Common options include FTP, HDFS, and S3. Use them for assets like trained ML models, digital assets, and documents.
