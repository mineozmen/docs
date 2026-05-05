---
description: >-
  How to define a deployment, choose runner versions, pass task parameters, and
  include extra deployment files.
---

# Defining a Deployment

Opening the **Deployment** screen from **Devops** app menu or navigation bar, you will come across an editor, allowing definition of deployments.

Definition of a deployment includes:

* **Name:** Logical name for the deployment
* **Status:** Whether the deployment should be used or not
* **Description:** Detailed description of the deployment
* **Deployment Task:** Deployment service endpoint to use for deployment (e.g. Jenkins job)&#x20;
* **Runners:** List of runners and their versions to include in this deployment, with:
  * **Name:** Logical name for runner instance, (which is used in generating service URLs for REST based runners)
  * **Class:** [Runner](../service-runners/) class to use for deployment
  * **Runner:** Configured runner to deploy
  * **Deployment Version:** Unique identifier, starting from 1, if multiple, different versions of the same runner will exist together (which is rarely required in cases such as Kafka migration)
  * **Disabled:** Whether the runner should be used in the deployment or not (allows temporarily disabling runners without removing them)
* **Parameters:** Deployment task specific parameters to use for configuring this deployment (e.g. Docker container or K8s details based on task selected). For typical deployment parameters on K8s (such as namespace, pool, rierinoVersion), please refer Helm Charts section of the Installation documentation.
* **Contents:** Optional extra file contents to pass on to the deployment task, where typical entries in K8s deployments include:
* "build.gradle:dependencies" for adding extra dependencies to build.gradle file for Java based runners, such as:&#x20;

<pre class="language-gradle"><code class="lang-gradle">//For Siddhi event handler
implementation (group:'com.rierino.custom', name: 'siddhi', version:"${<a data-footnote-ref href="#user-content-fn-1">rierinoVersion</a>}")
//For Vault KV Loader
implementation (group:'com.rierino.processors', name: 'vault', version:"${rierinoVersion}")
</code></pre>

* "application.properties" for adding extra properties to application.properties file for Java based runners (e.g. custom configurations for spring based runners)
* "preinit.sh" for adding extra shell commands before executing init container
* "postinit.sh" for adding extra shell commands after executing init container
* "prerun.sh" for adding extra shell commands before executing main container
* "extra.sh" for creating shell commands in /app/config/extraContainer.sh file of extra container (typically used by py-runner docker image for installing additional libraries before execution)&#x20;

{% file src="../../../.gitbook/assets/train_samza.json" %}
Example Deployment Definition (Can be Imported on Deployment Screen)
{% endfile %}

[^1]: Variable for current Rierino version defined for the deployment&#x20;
