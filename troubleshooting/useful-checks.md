---
description: >-
  This section explains most common errors and the checks to perform for
  troubleshooting
icon: square-check
---

# Useful Checks

## "No active saga for path" Error

* [ ] Check if the URL path contains the right gateway channel and the gateway channel has the right "Target Path/Stream" matching target saga runner
* [ ] Check if the saga "Status" is set to "ACTIVE"
* [ ] Check if the saga "Saga Path" is defined and matching expected URL path
* [ ] Check if the saga "Allowed On" includes the stream where the runner will be receiving its requests from (i.e. default stream for RPC event runners and request stream for Samza event runners)
* [ ] Check if the saga's runner has the right saga state configuration (e.g. if you've created a new saga and receiving this error make sure your saga state manager is using a loading strategy)

## Timeout from Saga Paths

* [ ] Check if your target saga runner is deployed and ready to receive requests (can be observed from runner logs)
* [ ] Check if your target saga runner is deployed using the correct runner type and has the necessary base runners or settings (e.g. Samza Base)
* [ ] If you are using Kafka based saga runners:
  * [ ] Check if the saga runner has its request stream (e.g. request\_train) configured correctly:
    * [ ] Set as an input stream for the runner
    * [ ] Enabled on the saga event handler as an event input
    * [ ] Having "meta.action.override" set as "StartSaga"
  * [ ] Check if the gateway channel "Target Path/Stream" is matching the saga runner's request stream alias (i.e. Kafka topic name)
  * [ ] Check if the saga runner has its saga stream (e.g. saga\_train) configured correctly:
    * [ ] Set as an input stream for the runner
    * [ ] Enabled on the saga event handler as an event input
    * [ ] Having "meta.action.override" set as "StepSaga"
    * [ ] With saga event handler having "parameter.saga.stream" set as stream's alias (e.g. saga\_train)
  * [ ] Check if the saga runner has its response stream (e.g. response\_train) configured correctly:
    * [ ] Set as an output stream for the runner
    * [ ] With saga event handler having "parameter.response.stream" set as stream's alias (e.g. response\_train)

## Any Error on a Saga Path

* [ ] Check if each event step has "Event Action" and "Event Version" defined as a minimum
* [ ] Check if each condition / transform step has "Class" defined as a minimum
* [ ] Check [error codes](error-codes.md) for understanding the root cause of a specific error message
