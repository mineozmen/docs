---
description: >-
  This example describes development of a simple locally executed saga flow for
  train_rpc runner, which returns "Hello World" message in a detailed,
  step-by-step manner.
---

# Exercise: Hello World API

## Open the Saga Screen

Open the [Saga ](../../devops/api-flows/)screen from your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/saga.

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Open.png" alt=""><figcaption><p>Saga Screen</p></figcaption></figure>

## Create a new Saga

Click on "CREATE NEW" button on top of the menu lister of Saga screen. This will clear the contents of Saga design page, and allow you to create a new Saga from scratch.

<figure><img src="../../.gitbook/assets/Create_Button.png" alt=""><figcaption><p>Create New Button</p></figcaption></figure>

## Assign an ID

Provide a unique identifier to your new saga flow from top left corner marked with "ID: " tag and a pencil icon. For this example, we are using "hello\_world-0001" as the id.

<figure><img src="../../.gitbook/assets/Hello_World_Saga_ID.png" alt=""><figcaption><p>Saga ID Assignment</p></figcaption></figure>

It is optional to enter an ID for assets created. If no ID is given, backend services can automatically assign an ID if an ID generator is configured for the related state manager. However, giving a recognizable ID for core assets such as sagas, queries, models simply facilitates easier communication and auditing.

{% hint style="info" %}
If you will be giving your own IDs, it is recommended to use alphanumerical characters, hyphen and underscore followed by a 4 digit partition identifier (i.e. 0001 for assigning assets to partition 1). For assets which do not require partitioning (i.e. will not be created in very high volumes), the partition identifier can be ignored.
{% endhint %}

## Define the Saga

Click on the circled edit icon for displaying Saga definition screen.

<figure><img src="../../.gitbook/assets/Definition_Button.png" alt=""><figcaption><p>Definition Button</p></figcaption></figure>

Enter the following details on displayed screen for describing your new Saga flow:

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Hello World Saga</p></figcaption></figure>

* **Saga Name:** Hello World
* **Saga Domain (optional):** util
* **Status:** ACTIVE
* **Saga Description (optional):** Returns "Hello World"
* **Saga Path**: /HelloWorld
* **Version:** 0
* **Allowed For:** Train RPC
* **Auto Fail:** true

These settings will mean that requests received on /HelloWorld URL path for train\_rpc runner will trigger execution of this Saga flow as the first version of its kind.&#x20;

{% hint style="info" %}
"Allowed For" setting enables this Saga flow on a specific runner only, making sure unintended runners do not respond to this request. When left empty, all runners are allowed to execute the saga.
{% endhint %}

For this example, you do not need to set Resilience parameters. Click on the close icon on top right corner to apply changes.

## Add Saga Steps

From the stencil, drag and drop a Start step to saga flow.

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Start_Step.png" alt=""><figcaption><p>Saga Stencil</p></figcaption></figure>

Similarly, drag and drop a Transform and Success step from the stencil. Your graph should look like the following after these actions:

<figure><img src="../../.gitbook/assets/Hello_World_Saga_All_Steps.png" alt=""><figcaption><p>Saga Steps</p></figcaption></figure>

Click on the Start step and you will see a link icon displayed on its right bottom corner, click on the link icon and then the Transform step to connect these two steps together.

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Link.png" alt=""><figcaption><p>Saga Link</p></figcaption></figure>

Similarly, link Transform step to the Success step. Your graph should look like the following after these connections:

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Links.png" alt=""><figcaption><p>Saga Links</p></figcaption></figure>

## Define the Transform Step

Now that our steps are connected, it is possible to receive events, execute an action on them and return them with success code. However, we still need to change the event payload for returning "Hello World" message. To do this, click on the [Transform ](../../devops/api-flows/configuring-saga-steps/transform-step/)step and open its editor by clicking on the edit icon displayed on its top left corner.

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Transform_Icon.png" alt=""><figcaption><p>Transform Step Icons</p></figcaption></figure>

Enter the following details on displayed screen for configuring your data transformation:

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Transform_Define.png" alt=""><figcaption><p>Transform Definition Screen</p></figcaption></figure>

* **Step Name:** Say Hello World
* **Step Description(optional):** This step adds a static "Hello World" message.
* **Transform Class:** Add Value to Payload
* **Transform Parameters:**
  * **Key:** value, **Value:** {message: "Hello World"}

Click on "Apply" button on top right corner to apply changes close this dialog.

{% hint style="info" %}
**Why is there a Transform "Class"?**

As a design principle, instead of crowding the stencil for possible activities (such as different transformation types), we chose to provide different options using "Class" or "Action" selection in our Saga steps.&#x20;

This allows users to change process type for any given step simply selecting/entering a new value, instead of replacing it with a different one which could break ongoing runs and create discontinuity in tracing performance of steps.
{% endhint %}

Your Transform step should now have the name you entered as follows:

<figure><img src="../../.gitbook/assets/Hello_World_Saga_Transform_Set.png" alt=""><figcaption><p>Defined Transform Step</p></figcaption></figure>

## Save the Saga

Click on the "SAVE" button displayed on top right corner of the Saga screen.

<figure><img src="../../.gitbook/assets/Save_Button.png" alt=""><figcaption><p>Save Button</p></figcaption></figure>

You should receive a notification saying Saga was successfully saved.

## Test Your Saga

Use your favorite API client or an internet browser to test your new API endpoint. Unless you've modified your implementation, this endpoint is available at https://\[YOUR\_ADMIN\_API\_DOMAIN]/api/request/train\_rpc/HelloWorld.&#x20;

You should receive a response with "200 OK" status and {message: "Hello World"} in its body for your GET requests.
