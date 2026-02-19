---
description: >-
  Build a complete to-do list feature end-to-end on Rierino, covering a CRUD
  microservice, MongoDB-backed storage, gateway exposure, an admin UI, and a
  simple search flow using queries and sagas.
icon: graduation-cap
---

# In-depth Exercise

This exercise walks through building a small, realistic app. It starts with the backend. It ends with a working admin screen.

You will create a `todo` data model, expose it over an API, and build a UI. You will also add a basic query endpoint for search.

{% hint style="info" %}
This exercise assumes you already understand the core Rierino concepts (Elements, Runners, Deployments, Sources, Schemas, UIs, and Sagas).

If you are new to the platform, start with the simpler [Training Examples](../training-examples/) first. They build the same mental model with smaller steps.
{% endhint %}

### Exercise sections

* [**To-do List Runner**](to-do-list-runner.md)
  * Build a CRUD runner backed by a MongoDB collection named `todo`.
  * Define the required Elements (System + State) and attach them to the runner.
  * Deploy the runner and verify basic reads and writes with `curl`.
* [**To-do List Gateway**](to-do-list-gateway.md)
  * Create a gateway channel that forwards external requests to your runner service.
  * Map a clean public path (for example `/api/todo`) to the runner’s internal base URL.
  * Test the gateway path and confirm you can list the records you created earlier.
* [**To-do List UI**](to-do-list-ui.md)
  * Create a dedicated Admin app entry and menu route for the to-do feature.
  * Define a Source so the UI knows where to send list and edit requests.
  * Define a JSON Schema and a UI layout so users can create and edit items.
* [**To-do List Query**](to-do-list-query.md)
  * Add search by creating a MongoDB Query (filtering by a field like `data.project`).
  * Expose that query through a Saga endpoint, so it is callable like an API.
  * Update the Source and UI lister to use the query endpoint and show filtered results.
