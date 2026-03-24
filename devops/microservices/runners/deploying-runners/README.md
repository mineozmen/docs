---
description: >-
  Overview of how runner configurations are deployed, the core deployment
  parameters, and the runtime commands available after rollout.
---

# Deploying Runners

Runner configurations do not mandate deployment of these capabilities on a specific platform. It is possible to define any number and type of new runners using different programming languages or frameworks. Runners shipped with Rierino for different use cases are listed in this section.

Typical deployment requires only 3 parameters in a properties file or via [Deployments](../../deployments/) screen:

| Parameter                                  | Definition                                                                           | Example                                   |
| ------------------------------------------ | ------------------------------------------------------------------------------------ | ----------------------------------------- |
| rierino.runner.\[runner].class             | Fully qualified class name for the runner                                            | com.rierino.runner.spring.CRUDEventRunner |
| rierino.runner.\[runner].element           | Id of the runner configuration                                                       | crud-0001                                 |
| rierino.runner.\[runner].deploymentVersion | Version of the runner deployment (allows multiple versions running at the same time) | 1                                         |

Once designed and deployed, it is possible to send certain commands to a runner's instances through the user interface menu (for more commands, [Command Center](../../../administration/sending-commands.md) can be used):

* **Ping:** Sends a ping command to all deployments and partitions of a runner and waits for 5 seconds to check if all the runners are healthy.
* **Rebuild:** Sends are build command to update all runner deployments in real-time, which can be used after operational elements of a runner are edited.
* **Deployments:** Lists deployments including the runner.
* **Dependents:** Lists other runners which use a specific runner as a base.
