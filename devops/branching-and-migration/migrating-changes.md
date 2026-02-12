---
description: >-
  Rierino provides built-in migration capabilities, allowing  micro-releases as
  well
---

# Migrating Changes

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption><p>Migration UI</p></figcaption></figure>

All development on Rierino is ultimately stored in its backend data store, and can be accessible through APIs - for both retrieving latest version of microservices, queries, APIs, ML models, etc. as well as pushing them to staging, testing, production or other environments. As a result, migration of releases to such environments can be achieved using a variety of methods:

## Full Migration Through UI

Devops application has a migration screen, which allows listing of all current and updated assets that need to be migrated to a target environment. Using this screen, it is possible to select all or specific assets for a release and take one of the following actions:

* **Migrate:** Automatically pushing the selected assets to target environment
* **Skip This:** Skipping current version of the asset in migration actions
* **Skip Always:** Marking assets to be always ignored in migration actions

{% hint style="info" %}
Migrate action requires configuration of access from development microservices to the target environments' API gateway in admin\_rpc runner definition.
{% endhint %}

## Offline Migration Through UI

In case it is not possible or preferred to establish connection between development and target environments, migration screen can be used to "Export" the list of selected assets to a json file, which can be "Imported" from the migration screen of the target system directly.

## CI/CD Tools

In case manual migration through UI is not preferred, migration can be facilitated through any CI/CD tool, which can simply call admin\_crud API endpoints to query and extract updated assets from the development environment and push them with write API endpoints on the target environment.

## Bulk DB Migration

In case preferred, it is also possible to bulk export configuration assets directly from the "devops" and "configuration" databases of the development environment and import them on the target environment's database. &#x20;

{% hint style="info" %}
DB migration is the least recommended approach as it overrides instance version and offset values on target database as well.
{% endhint %}
