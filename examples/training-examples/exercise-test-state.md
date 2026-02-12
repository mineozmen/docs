---
description: >-
  This example describes activation of CRUD endpoints for a new state manager
  using existing train_crud event runner.
---

# Exercise: Test State

## Open Train CRUD Runner

Open the [Runner](../../devops/microservices/runners/) screen from your [Devops ](/broken/pages/PWyjQCLF01E9OngBbsr8)app. Unless you've modified your implementation, this screen is located at https://\[YOUR\_ADMIN\_UI\_DOMAIN]/app/devops/common/runner.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption><p>Runner UI</p></figcaption></figure>

From the runner list on left hand side, filter and select Train CRUD runner.

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption><p>Train CRUD Runner</p></figcaption></figure>

## Add a New State

From the stencil, drag and drop a new State element (with table icon) onto the runner diagram. Select the newly added element and click on edit action displayed on it (with pencil icon).

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>New State Manager</p></figcaption></figure>

On the displayed screen, select 'Generic Master' element as shown above. This element can be used to add generic state managers for creating & accessing collections on MongoDB master database.

Enter 'test' as the 'alias' for your state, which will be used as the CRUD url path and also used as the collection name on the database. This is the simplest configuration for a CRUD endpoint, full list of configuration options can be viewed in the Devops section.

## Test Your CRUD Operations

The CRUD runner for training examples has a rebuild configuration of 30 seconds, meaning any changes done on the runner, such as adding a new state like this example will be automatically applied within 30 seconds. You can start using full list of CRUD endpoints found [here](../../devops/microservices/runners/deploying-runners/spring-runners.md#crud-event-runner) afterwards.&#x20;

As an example, you can make a call to https://\[YOUR\_ADMIN\_API\_DOMAIN]/api/request/train\_crud/test endpoint.&#x20;

You should receive a response with "200 OK" status and {list: \[]} in its body for your GET request, as the MongoDB collection will be initially empty.
