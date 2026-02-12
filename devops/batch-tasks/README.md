---
description: >-
  Python processes and processors allow execution of batch tasks such as import,
  export and ML training.
icon: stopwatch
---

# Batch Tasks

Python runners are used for executing batch processes, which are typically deployed as one-time jobs or scheduled cron-jobs. While it is possible to use Java runners and sagas for executing and scheduling any flow, these batch runners can be utilized for long running processes, as well as leveraging Python specific ML libraries without blocking real-time microservice resources.&#x20;

Two main types of runners are available out-of-box, using [Rierino Python Packages](https://app.gitbook.com/s/pV7u8nn9fFM9XMp0tNic/artifacts):

## rierino\_util.Runner

Triggers "[Process](python-processes.md)" type Python tasks, using a "base64" command line argument to pass base64 encoded contents of the Json object for process parameters. These parameters should include:

| Parameter | Definition                                | Example                                                                                                | Default |
| --------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------- |
| package   | Python package of the module to execute   | rierino\_runner                                                                                        | -       |
| module    | Process module to execute                 | IterateProcess                                                                                         | -       |
| args      | Process module specific arguments to pass | {"parameters": {"iterator":{ "module": "rierino\_runner.iterator.DataIterator", "element": "list" \}}} | -       |

## rierino\_runner.ProcessorRunner

Triggers "[Processor](python-processors.md)" type Python tasks, for single step, relatively simpler executions, using the following command line arguments:

| Argument            | Definition                                                                                                     | Example              | Default |
| ------------------- | -------------------------------------------------------------------------------------------------------------- | -------------------- | ------- |
| base64              | Base64 encoded contents of the Json object for full parameters (can include dict, package, module, parameters) | -                    | -       |
| dict                | Json string for passing input payload to processor                                                             | {list: \[]}          | -       |
| package             | Python package of the module to execute                                                                        | rierino\_runner      | -       |
| module              | Processor module to execute                                                                                    | RestProcessor        | -       |
| \[processor\_param] | Key=value pair for passing individual parameters to the processor                                              | {url: "example.com"} | -       |
