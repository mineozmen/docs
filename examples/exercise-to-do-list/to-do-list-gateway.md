---
description: >-
  To provide public access to your new deployment, you need to configure one of
  the existing API gateways or deploy a new one.
---

# To-do List Gateway

## Open the Gateway Channel Screen

Open the [Gateway > Channel](../../devops/gateway-and-security/gateway-servers/gateway-channels.md) screen from your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/gateway\_channel.

<figure><img src="../../.gitbook/assets/Todo_Gateway_Channel.png" alt=""><figcaption><p>Gateway Channel Screen</p></figcaption></figure>

## Create a Gateway Channel

Next, you will create a new API channel on this new Gateway System to assign a specific URL path to your deployment.

Click on "CREATE NEW" button, assign "todo\_crud" as the id, enter the following details and save:



<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption><p>Gateway Channel Definition Screen</p></figcaption></figure>

* **DEFINITION**
  * **Channel Name:** Todo
  * **Channel Status:** ACTIVE
  * **Description:** CRUD path to todo list runner
  * **Applicable Gateways:** admin-prod
  * **System Executor:** CRUD
  * **System Parameters:**
    * **Key:** server.baseUrl, **Value:** http://runner-spring-example-0001-service.admin-backend.svc.cluster.local:1235
  * **Target:** /api/todo
* **AUTHENTICATION**
  * **Path Authentications:** { "\*": { "isPublic": true } }

These settings will map "todo\_crud" endpoint on "admin-prod" gateway to "/api/todo" endpoint of your "example-0001" deployment. The authentication settings provide public access to all paths on this endpoint.&#x20;

After saving the new channel, your settings will be automatically applied to your gateway (after a few seconds as defined in configuration). You can also refresh gateway settings manually, by making calls to the following endpoints using your favorite API client:

```
POST https://[YOUR_ADMIN_API_DOMAIN]/api/control/system/example-admin
POST https://[YOUR_ADMIN_API_DOMAIN]/api/control/channel/todo_crud
```

## Test your Gateway

Now, you can access your todo list APIs publicly through your gateway. Test your access by listing the record you've created in last section by:

```
GET https://[YOUR_ADMIN_API_DOMAIN]/api/request/todo_crud/todo
```

{% hint style="info" %}
Please note that you only need to create new gateway mappings when you create microservices that should be directly accessible by end users.&#x20;
{% endhint %}
