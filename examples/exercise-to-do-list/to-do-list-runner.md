---
description: >-
  First step in building this application will be development of the runner,
  which will facilitate read/write operations to a MongoDB collection.
---

# To-do List Runner

## Open the Element Screen

Open the [Element ](../../devops/microservices/elements/)screen from your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/element.

<figure><img src="../../.gitbook/assets/Todo_Element_Open.png" alt=""><figcaption><p>Element Screen</p></figcaption></figure>

## Create a State Element

Next, you will create the configuration for a new data store which will maintain to-do list records in a MongoDB collection.

Click on "CREATE NEW" button on top of the menu lister of Element screen. This will clear the contents of Element design page, and allow you to create a new Element from scratch.

<figure><img src="../../.gitbook/assets/Create_Button.png" alt=""><figcaption><p>Create New Button</p></figcaption></figure>

Provide a unique identifier to your new element from top left corner marked with "ID: " tag and a pencil icon. For this example, we are using "state-todo-0001" as the id.

Enter the following details from "DEFINITION" and "STATE DEFINITION" tabs and save:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>State Definition Tab</p></figcaption></figure>

* **DEFINITION**
  * **Element Name:** Todo
  * **Element Type:** STATE
  * **Element Status:** ACTIVE
  * **Element Description (optional):** Example todo list data store
* **STATE DEFINITION**
  * **Manager:** Mongo State Manager
  * **System:** mongo\_master
  * **Collection:** todo

These settings will mean that this state will be managed by a [MongoStateManager](../../devops/microservices/elements/state-managers/shared-states/mongodb-collection.md) using connection settings provided by the already existing "mongo\_master" system element and records will be stored in a collection named "todo".

{% hint style="info" %}
**Why do you need Elements?**

Elements provide reusable building blocks for runners. Without such an element structure, you would need to define all configurations, settings and parameters within the runners, which would mean replications and maintenance issues for systems, states or handlers shared between different runners.
{% endhint %}

## Create a Runner

Next, you will create a new runner, which will be responsible for executing read/write operations on the new todo state you defined.

Click on the Runner icon on navigation bar to switch to the [Runner ](../../devops/microservices/runners/)screen on your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/runner.

<figure><img src="../../.gitbook/assets/Todo_Runner_Open.png" alt=""><figcaption><p>Runner Screen</p></figcaption></figure>

Click on "CREATE NEW" button on top of the menu lister of Runner screen and provide the unique identifier "todo-0001".

### Define the Runner

Click on the circled edit icon for displaying Runner definition screen.

<figure><img src="../../.gitbook/assets/Definition_Button.png" alt=""><figcaption><p>Definition Button</p></figcaption></figure>

Enter the following details on displayed screen for describing your new Runner graph:

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption><p>Runner Definition Screen</p></figcaption></figure>

* **Runner Name:** Todo
* **Runner Status:**  ACTIVE
* **Runner Domain (optional):** admin
* **Base Runners:** Base CRUD
* **Runner Description (optional):** This is an example runner for todo list management.

These settings will mean that the new runner will inherit features from the "Base CRUD" runner, making it possible to perform standard CRUD operations with default read, write handlers.

For this example, you do not need to set I/O or Settings details. Click on the close icon on top right corner to apply changes.

### Add Runner Elements

From the stencil, drag and drop a System element to the runner graph.

<figure><img src="../../.gitbook/assets/Todo_Runner_Stencil.png" alt=""><figcaption><p>Runner Stencil</p></figcaption></figure>

Similarly, drag and drop , a State element from the stencil. Your graph should look like the following after these actions:

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption><p>Runner Elements</p></figcaption></figure>

#### Select System Element

Click on the System element and you will see an edit icon displayed on its top left corner, click on it to edit element details as follows:

<figure><img src="../../.gitbook/assets/Todo_Runner_System.png" alt=""><figcaption><p>System Selection Screen</p></figcaption></figure>

* **Element:** system-mongo\_master-0001
* **Member Type:** OPERATION
* **Alias:** mongo\_master
* **Description(optional):** MongoDB system for master collections

These settings will add the predefined "system-mongo\_master-0001" system element to the runner with "mongo\_master" alias. This assigned alias automatically links the system to related other elements (such as the State element).

{% hint style="info" %}
**Why do you need Aliases?**

We use aliases in addition to element ids when assigning elements to runners. This allows us to replace core elements such as Systems when needed without having to update all other elements dependent on them. It also allows reusing same elements multiple times within a runner, which can be preferred for organization or configuration purposes (e.g. using 2 copies of the same Handler with different parameter overrides).
{% endhint %}

If you click on the "ELEMENT" tab, you can see parameters assigned to this elements during its initial definition.

<figure><img src="../../.gitbook/assets/Todo_Runner_System_Element.png" alt=""><figcaption><p>System Element Screen</p></figcaption></figure>

Click on APPLY button to apply changes and close the dialog.

#### Select State Element

Next, edit the State element in a similar way, with the following details:

<figure><img src="../../.gitbook/assets/Todo_Runner_State_Element.png" alt=""><figcaption><p>State Selection Screen</p></figcaption></figure>

* **Element:** state-todo-0001 (as created in earlier step)
* **Member Type:** OPERATION
* **Alias:** todo
* **Description (optional):** State for todo list records

Click on APPLY button to apply changes and close the dialog.

## Deploy the Runner

Once your runner is defined, next step is to deploy it to get it up and running. It is possible to deploy multiple runners within a single deployment package, but for this example, you will be deploying the runner as a stand-alone package.

{% hint style="danger" %}
Deployment from within Rierino UI is not enabled for Sandbox deployments. For deploying new runners in sandboxes, they can be added to the existing adminrunner deployment and the Docker container can be restarted to enable the runner. This section focuses on regular K8s deployments.
{% endhint %}

First, open the [Deployment ](../../devops/microservices/deployments/)screen from your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/deployment.

### Create a Deployment

Click on "CREATE NEW" button on top of the menu lister of Deployment screen and provide the unique identifier "example-0001".

Fill in the "DEFINITION" tab with following details:

<figure><img src="../../.gitbook/assets/Todo_Deployment_Define.png" alt=""><figcaption><p>Deployment Definition Screen</p></figcaption></figure>

* **Name:** Example Runners
* **Status:** ACTIVE
* **Description (optional):** Deployment package for execution of example runners
* **Runners:**
  * **Name:** todo
  * **Class:** CRUD Runner
  * **Element:** todo-0001
  * **Deployment Version:** 1

These settings assign 1 runner with "todo-0001" identifier, which is created in previous steps to this deployment. Runner name has various uses, such as log labeling, targeting runners for commands, forming URL paths, as well as application / consumer naming. Runner class defines the type of [Runner ](../../devops/microservices/runners/)to be used for execution (such as CRUD, RPC, Socket or Samza). Deployment version is useful when multiple versions of a runner needs to be running at the same time (e.g. during a systems migration).

{% hint style="info" %}
**Why do you need a Deployment?**

Runners define "how" and "what" of microservices to be used, providing an abstraction over "where" these services are deployed. This approach allows separation of configuration and infrastructure responsibilities, while allowing deployment of same runners in different servers (e.g. for multi-national deployments, replication). It also allows coupling of multiple runners into single deployment, for microservices that do not require heavy resources.
{% endhint %}

Next, switch to the "PARAMETERS" tab and enter the following parameter key-value pairs:

<figure><img src="../../.gitbook/assets/Todo_Deployment_Parameters.png" alt=""><figcaption><p>Deployment Parameters Screen</p></figcaption></figure>

* **rierinoVersion:** 0.1.1
* **template:** spring
* **namespace:** admin-backend
* **pool:** admin-node-pool

These settings specify the Rierino core libraries and settings to deploy, as well as the Kubernetes namespace and node pool to utilize for creating this runner.

{% hint style="info" %}
You can increase scalability of your runner by setting replicaCount parameter to 2 or more, or increase its log level by setting logLevel to "DEBUG".
{% endhint %}

Click on SAVE button to finalize creation of the deployment.

### Trigger the Deployment

Next, from the dropdown menu on top right side of the Deployment screen, click on "DEPLOY" button.

<figure><img src="../../.gitbook/assets/Todo_Deployment_Deploy.png" alt=""><figcaption><p>Deployment Action Menu</p></figcaption></figure>

This will automatically trigger a deployment request and you will receive a notification of successful delivery of the request.

## Test Your Runner

In case you have access rights to your infrastructure, you can check deployment status of your Runner through the Jenkins interface as well as Kubernetes management portal of your cloud service provider.

Once the runner is up and running, you can use to test its status using your new runner's endpoints for read / write operations on your todo list records. By default, all runners in K8s are deployed in a secure cluster without exposing a public endpoint and are only accessible from within that cluster or through the [Gateway Servers. ](../../devops/gateway-and-security/gateway-servers/)

If you don't wish to wait for setting up the gateway access as described in next section, you can use kubectl commands for this test as follows:

```shell
kubectl exec -ti --namespace=admin-backend deployment/runner-example-0001-deployment -- /bin/bash
```

This will establish connection to your deployed runner, from which you can do REST calls using curl:

```shell
#Following should return status:"UP"
curl localhost:1235/actuator/health

#Following should return details of the new record created
curl -X POST -H "Content-Type: application/json" -d '{"data": {"name": "Take out trash", "project": "Personal"}}' localhost:1235/api/todo/todo/1

#Following should return a list including the new record created
curl localhost:1235/api/todo/todo
```
