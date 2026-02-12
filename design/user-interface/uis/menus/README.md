---
description: >-
  Lists, editors and widgets can utilize different menu actions, such as delete,
  import, export, which are configured through UI screens.
---

# Menus

A menu action can trigger data updates, queries or API calls and can be attached to a listing screen, editor screen or an individual value widget.

While it is possible to implement new menu action types, Rierino already includes a number of alternatives for different use cases.

## API Actions

Certain menu actions perform calls to Rierino backend APIs. All these menu actions share the following properties:

* **URL:** URL path of the API endpoint to call.
* **Method:** REST method to use for calling API endpoint.
* **Extra Body:** Extra payload to add to the API call.
* **Extra Headers:** Extra headers to add to the API call.
* **Content Type:** Type of content to send ("none" for empty body).
* **As Text:** Whether response should be returned as text (or json).
