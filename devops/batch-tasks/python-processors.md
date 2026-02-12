---
description: Processors are used by IterateProcess and ProcessorRunner for taking actions
---

# Python Processors

{% hint style="info" %}
Processors available out of box for execution are listed on this page. It is also possible to create new processors using rierino\_runner.Processor as the base class.
{% endhint %}

## rierino\_runner.processor.NoopProcessor

This is a test processpr that doesn't perform any specific action.

## rierino\_runner.processor.PrintProcessor

This is a test processor that simply prints the request details on console.

## rierino\_runner.processor.FSProcessor

This processor performs file operations with a file system.

| Parameter | Definition                                                | Example      | Default |
| --------- | --------------------------------------------------------- | ------------ | ------- |
| system    | Name of system in connections to connect to               | fs\_media    | -       |
| action    | File action to perform (can be passed in payload as well) | writeToLocal | -       |

Following payload fields are used by the actions, based on the action type:

* path
* tarName
* content
* localPath
* targetPath
* fileName
* targetFile
* prefix

Applicable file actions are as follows:

* removeLocalFile
* removeLocalDir
* makeLocalDir
* writeToLocal
* readFromLocal
* tarLocal
* untarLocal
* removeFile
* removeDir
* makeDir
* copyFromLocal
* copyFromLocalDir
* syncFromLocalDir
* copyToLocal

## rierino\_runner.processor.IOProcessor

This processor performs record operations on databases or files.

| Parameter | Definition                                      | Example                                     | Default |
| --------- | ----------------------------------------------- | ------------------------------------------- | ------- |
| system    | Name of system in connections to connect to     | mongo\_master                               | -       |
| output    | Output parameters for writing records to system | {database: "master", collection: "product"} | -       |

## rierino\_runner.processor.RestProcessor

This processor makes calls to a REST API. For more complex REST API integrations, Java sagas should be called to perform the request instead.

| Parameter   | Definition                                                                                            | Example               | Default |
| ----------- | ----------------------------------------------------------------------------------------------------- | --------------------- | ------- |
| system      | Name of system in connections to connect to                                                           | erp                   | -       |
| method      | Request method                                                                                        | POST                  | -       |
| url         | Request URL (if there is a targetPath in payload, it is appended to this url)                         | /CreateProduct        | -       |
| headers     | Headers to pass on in request                                                                         | {}                    | -       |
| token       | Authorization token to send for request                                                               | {API\_KEY} (from env) | -       |
| tokenPrefix | Authorization token prefix to use                                                                     | gateway\_token        | Bearer  |
| username    | Username to send for basic authentication                                                             | {USERNAME} (from env) | -       |
| password    | Password to send for basic authentication                                                             | {PASSWORD} (from env) | -       |
| download    | Whether response should be downloaded as file (with localPath & filePath from payload) (binary, true) | true                  | -       |
| internal    | Whether request should be formatted in Event data format as an internal microservice call             | true                  | -       |
| retries     | Number of retries if the REST call fails                                                              | 3                     | 0       |

## rierino\_runner.processor.JavaProcessor

This processor is only applicable when called through Java Py4JEventHandler, allowing hook calls to the Java runner without requiring explicit REST API calls.

| Parameter | Definition                               | Example | Default |
| --------- | ---------------------------------------- | ------- | ------- |
| meta      | Event metadata to pass on to Java runner | -       | -       |
| retries   | Number of retries if the Java call fails | 3       | 0       |

## rierino\_runner.processor.KafkaProcessor

This processor sends messages to a Kafka topic.

| Parameter | Definition                                  | Example        | Default |
| --------- | ------------------------------------------- | -------------- | ------- |
| system    | Name of system in connections to connect to | kafka\_default | -       |
| topic     | Name of topic to send messages to           | email\_trigger | -       |

## rierino\_runner.processor.ProcessProcessor

This processor allows calling any [Python Process](python-processes.md), passing payload as well as parameters enriched with payload.

| Parameter      | Definition                 | Example        | Default |
| -------------- | -------------------------- | -------------- | ------- |
| processPackage | Python package for Process | rierino\_media | -       |
| processModule  | Python module for Process  | MediaProcess   | -       |

## rierino\_runner.processor.MultiStepProcessor

This processor performs a sequence of calls to different processors.

| Parameter | Definition               | Example                          | Default |
| --------- | ------------------------ | -------------------------------- | ------- |
| steps     | List of steps to perform | \[{module: "", stepPattern: ""}] | -       |
