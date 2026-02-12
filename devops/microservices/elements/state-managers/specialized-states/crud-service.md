---
description: >-
  This state manager (com.rierino.state.manager.CRUDStateManager) uses a remote
  REST service as a data store and source, converting state changes and requests
  to CRUD calls.
---

# CRUD Service

## Manager Parameters

| Parameter | Definition                                                                                                  | Example                        | Default |
| --------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------ | ------- |
| system    | Name of the [system ](../../systems.md#rest)which defines remote service details (uri, timeout, allowPatch) | {uri:"http://wms.example.com"} | -       |
| resource  | Name/path of the resource on target system                                                                  | inventory                      | -       |
