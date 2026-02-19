---
description: >-
  To make your to-do list more manageable, next you can make add query
  functionality.
---

# To-do List Query

## Open the Query Screen

Open the [Query ](../../configuration/queries/)screen from your [Configuration ](/broken/pages/NhIhtgvnmxDul9vF2Bww)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/configuration/common/query.

<figure><img src="../../.gitbook/assets/Todo_Query_App.png" alt=""><figcaption><p>Query Screen</p></figcaption></figure>

## Create a Query

Next, you will create a new Query, which will run on your already existing MongoDB system.

Click on "CREATE NEW" button on top of the menu lister and enter "todo" to ID field. We will reference this id when making requests for its execution.

Enter the following details on "DEFINITION" tab:

<figure><img src="../../.gitbook/assets/Todo_Query_Define.png" alt=""><figcaption><p>Query Definition Screen</p></figcaption></figure>

* **Query Name:** Todo
* **Query Status:** ACTIVE
* **Query Description (optional):** Example query for searching todo list by "project" field.
* **Query Type:** SIMPLE
* **Query Platform:** MONGODB
* **Source Table:** todo

Next, switch to the "WHERE" tab, hover on the plus icon and select "Simple" option to add a simple where condition. You will see a new entry with "Enter condition details" label. Hover on this entry and click on the edit icon to fill in condition details as follows:

<figure><img src="../../.gitbook/assets/Todo_Query_Where.png" alt=""><figcaption><p>Query Condition Dialog</p></figcaption></figure>

* **Use a Function:** false
* **Expression:** data.project
* **Operator:** EQUALS
* **Value:** %%project%%

Save your query.

## Create a Saga

In order to expose this query to end users, we'll follow steps similar to [Hello World Saga](../training-examples/api-flow-examples/exercise-hello-world-api.md) example.

{% hint style="info" %}
We are not utilizing the new runner created in previous sections, since we defined it as a CRUD type runner, which is allowed to process only standard CRUD operations. As an alternative, you could also create a new runner for RPC type requests.
{% endhint %}

Start by opening the Saga screen from your Devops app and creating a new Saga flow as follows:

<figure><img src="../../.gitbook/assets/Todo_Query_Saga_Define.png" alt=""><figcaption><p>Search Todos Saga Definition Screen</p></figcaption></figure>

* **Saga Name:** Search Todos
* **Status:** ACTIVE
* **Saga Path:** /SearchTodos
* **Version:** 0
* **Allowed On:** saga
* **Saga Domain:** util

Next, add Start, Event and Success steps from the Saga stencil and link them to create the flow.

Click on the link connecting Start and Event steps and click on the edit icon displayed to modify this link as follows:

<figure><img src="../../.gitbook/assets/Todo_Query_Saga_Link.png" alt=""><figcaption><p>Saga Link Definition Dialog</p></figcaption></figure>

* **Stream:** local

Setting stream to "local" means that we will be executing this Event step locally, instead of assigning it to a remote runner. When this value is set to anything other than "local", it is passed on to a remote runner through REST calls or Kafka topics.

Next, click on the Event step and click on the edit icon displayed to define this step as follows:

<figure><img src="../../.gitbook/assets/Todo_Query_Saga_Event.png" alt=""><figcaption><p>Saga Event Definition Dialog</p></figcaption></figure>

* **DEFINITION**
  * **Step Name:** Get Project Todos
* **EVENT META**
  * **Event Version:** 0
  * **Event Action:** GetQuery
  * **Event Domain:** master
  * **Input Element:** parameters
  * **Output Element:** $
  * **Parameters Map:**
    * **Key:** queryId
    * **Value:** todo

These settings mean that this Event step will execute [GetQuery](../../devops/microservices/elements/handlers/core-handlers/query-data.md) action, on the "master" system (which is the alias of "system-mongo\_master-0001" in the predefined runner), using query with identifier "todo" (which we just created). Query parameters (i.e. %%project%% in our query) will be search in "parameters" of the request payload and query results will be returned in the root of response payload ($ referring to the root).&#x20;

After you apply these changes click on SAVE button to activate this Saga.

## Test your Query

Now, you can access your query functionality using your favorite API client on the following URL:

<pre><code><strong>GET https://[YOUR_ADMIN_API_DOMAIN]/api/request/rpc/SearchTodos?project=Personal
</strong></code></pre>

You should receive the task you've created earlier, which had "data.project" set to "Personal".

## Update your UI

Now that you can query your todo list, you can update your todo UI to allow users search todos belonging to a specific project.

First, open the Todo Source screen from your Design app (i.e. https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/source?id=todo). Add the following to "Urls" table:

* **Action:** query
* **URL:** request/rpc/SearchTodos

Save and switch to the Todo UI screen from your Design app (i.e. https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/design/common/ui?id=todo). Select the "LISTER" tab and update lister details with the following:

<figure><img src="../../.gitbook/assets/Todo_Query_Lister.png" alt=""><figcaption><p>UI Lister Screen</p></figcaption></figure>

* **Lister:** Query Table
* **List Columns:**
  * **Title:** Name, **Path:** data.name
  * **Title:** Project, **Path:** data.project
* **Main Filters:**
  * **Path:** project, **Widget:** TextFilter, **Properties:** { "label": "Project" }

Save your updated UI design. If you switch back to your Todo screen in your Example app, you'll notice that now you can query todo list using project field:

<figure><img src="../../.gitbook/assets/Todo_Query_UI.png" alt=""><figcaption><p>Updated Todo Screen</p></figcaption></figure>
