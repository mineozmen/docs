---
description: >-
  All Rierino deployments share a preconfigured yet fully customizable data
  model.
---

# Common Data

Common data model stores elements mainly used by [DevOps](/broken/pages/PWyjQCLF01E9OngBbsr8), [Design](/broken/pages/R6CwbBwzD5B7a9dcS18z),[ ](/broken/pages/PWyjQCLF01E9OngBbsr8)[Configuration](/broken/pages/NhIhtgvnmxDul9vF2Bww), and [Data Science](/broken/pages/wWEaREJE3r9cn4bQ9yGk) operations.&#x20;

## Authentication

For increased security and flexible authentication models, Rierino does not store any user credentials within its databases - which is handled by the identity and access management solution such as Keycloak. Authentication database only includes data required for linking user identities with customer records instead.&#x20;

| Table          | Description                                                                                           |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| user\_attempt  | List of login attempts by different user ids for audit logging                                        |
| user\_customer | Mapping of user ids with customer ids, which also allows multiple identification methods per customer |

## Devops

DevOps database includes key data for defining gateways, runners, APIs as well as UI design, typically configured through the admin UI.

| Table            | Description                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| app              | List of configurations used for admin UI apps                                |
| deployment       | List of runner deployments                                                   |
| element          | List of elements used for building runners                                   |
| gateway\_channel | List of channels used by gateway for communicating with runners              |
| gateway\_service | List of services used by gateway for messaging and file systems interactions |
| gateway\_system  | List of systems used by gateway for identifying runners                      |
| gateway\_token   | List of access tokens used by gateway for user authentication                |
| handler\_code    | List of custom codes that can be dynamically updated and deployed            |
| option           | List of options displayed in admin UI drop-down widgets                      |
| runner           | List of runners and their members                                            |
| saga             | List of sagas providing dynamic API endpoints                                |
| schema           | List of data models in Json schema format                                    |
| source           | List of API endpoints used for populating data in admin UI                   |
| ui               | List of admin UI screens and their widget settings                           |
| variable         | List of runtime variables used for parameter injection in queries and events |

## Configuration

DevOps database includes key data for defining queries, business rules, data flows as well as data science models, typically configured through the admin UI.

| Table         | Description                                                                        |
| ------------- | ---------------------------------------------------------------------------------- |
| customization | List of customizations (i.e. customer personas) used for personalization.          |
| dataflow      | List of flows used for real-time data integration.                                 |
| model         | List of data science model configurations.                                         |
| query         | List of database queries used in retrieving custom and enriched data for requests. |
| rule\_\*      | List of business rules for different domains (e.g. basket\_promotion)              |
| ruledomain    | List of rule domain configurations which provide the system for business rules     |

In addition to these common data elements, each Rierino deployment includes business domain and app specific databases and tables (such as product data).
