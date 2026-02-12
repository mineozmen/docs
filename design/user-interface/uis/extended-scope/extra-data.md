---
description: >-
  Extra data configurations allow providing additional data contents to editors
  and widgets beyond what API mapping provides.
---

# Extra Data

While in most cases it is possible to receive all required data through API mapping, in certain scenarios, it is useful to receive additional data for display purposes (such as retrieving data based on a currently selected value).

[Extra data configurations](#user-content-fn-1)[^1] can be performed at both editor and widget levels, using an array (allowing multiple extra data feeds) of the following properties for each entry:

* **URL:** API url to pull extra data from, required unless extra data is calculation from input data only.
* **Method:** Request method to pull data from the URL (defaults to GET).
* **Parameters:** "body", "url" or "query", defining where to send the parameters for the action.
* **Extra Body:** Extra json body to include in request (e.g. a filter value to pass on). Extra body is sent in body or query based on the parameters configuration.
* **Extra Headers:** Extra headers to include in request.
* **Input Pattern:** Json path or expression to apply on current data to create request body.
* **Output Pattern:** Json path or expression to apply on URL response.
* **Allow Blank:** Whether null data values should be allowed for URL requests (defaults to false).
* **Merge Path:** Json path to merge calculated data on full results (e.g. product).

An additional property is used above the array entries:

* **Extra Path:** Json path to merge all extra data on current record (e.g. extra).

[^1]: since 0.5.2
