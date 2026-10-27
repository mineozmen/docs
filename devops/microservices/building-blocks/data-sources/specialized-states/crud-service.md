---
description: >-
  This state manager (com.rierino.state.manager.CRUDStateManager) uses a remote
  REST service as a data store and source, converting state changes and requests
  to CRUD calls.
---

# CRUD Service

## Manager Parameters

| Parameter         | Definition                                                                                                             | Example                        | Default |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------ | ------- |
| system            | Name of the [system ](../../systems-integrations/#rest)which defines remote service details (uri, timeout, allowPatch) | {uri:"http://wms.example.com"} | -       |
| resource          | Name/path of the resource on target system                                                                             | inventory                      | -       |
| suffix            | Path added after the resource and id in REST calls                                                                     | details                        | -       |
| pageSize          | Number of records requested per page                                                                                   | 100                            | -       |
| pageParameter     | Query parameter name for page number                                                                                   | page                           | -       |
| pageSizeParameter | Query parameter name for page size                                                                                     | size                           | -       |
