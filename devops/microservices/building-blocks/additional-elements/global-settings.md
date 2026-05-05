---
description: >-
  Global settings are typically used for runner specific configurations which
  are not specific to an element type, and can be loaded after the runner
  initialization.
---

# Global Settings

Globals allow defining and loading properties which are not tied to a specific system component to runners (e.g. to application.properties files). Typical use cases include:

* **Serde Configurations:** Defining serialization/deserialization settings for different runner types, used across all streams (e.g. task.drop.serialization.errors setting for Samza runners).
* **Runner Specific Configurations:** Core Spring, Samza, etc. properties to apply (e.g. job.coordinator.system for Samza runners).

These settings are typically defined only once at the initial Rierino platform deployment (with predefined values) and do not change over time. The main benefit of these elements is to provide complete control over underlying system library settings.

For more details on available generic settings, please refer to your selected runner's own documentation (e.g. Spring, Samza).

Since they are runner specific, global settings usually have their runner type defined.

{% hint style="info" %}
An example is "task.ignored.exceptions" configuration for [Samza](https://samza.apache.org/learn/documentation/latest/jobs/configuration-table.html), which specifies list of exceptions to ignore in a task's process or window methods.
{% endhint %}
