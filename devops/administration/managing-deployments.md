---
description: >-
  Manage deployed runner packages with deploy, undeploy, restart, health check,
  pod inspection, and log retrieval actions.
---

# Managing Deployments

Once defined, it is possible to perform the following actions on a [deployment](../microservices/deployment-packages/) through the editor menu (for more commands, [Command Center](sending-commands.md) can be used):

* **Deploy:** Makes a call to "/Deploy" API endpoint with current deployment's id to trigger the CD task responsible for runner deployments.
* **Undeploy:** Makes a call to "/Undeploy" API endpoint with current deployment's id to trigger the CD task responsible for runner removals.
* **Restart:** Makes a call to "/Restart" API endpoint with current deployment's id to trigger the CD task responsible for runner restarts.
* **Deployments:** Makes a call to "/GetDeployments" API endpoint with current deployment's id to get status of current deployment on K8s.
* **Pods:** Makes a call to "/GetPods" API endpoint with current deployment's id to get a list of pods running current deployment on K8s.
* **Logs:** Makes a call to "/GetLogs" API endpoint with current deployment's id to get last log lines from the current deployment on K8s.
* **Ping:** Sends a ping command to all runners of a deployment and waits for 5 seconds to check if all are healthy.

It is possible to get the full list of K8s deployments from "Deployments" lister menu as well.

{% hint style="info" %}
Actions such as "Deploy" and "Undeploy" are executed with Rest API calls to the kubernetes cluster (e.g. kubectl) or automation tool (e.g. Jenkins) depending on the deployment platform and their status can be monitored from its own interface.
{% endhint %}
