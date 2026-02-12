---
description: Iterators are used by IterateProcess to produce loops of Processor calls
---

# Python Iterators

{% hint style="info" %}
Iterators available out of box for execution are listed on this page. It is also possible to create new iterators using rierino\_runner.Iterator as the base class.
{% endhint %}

## rierino\_runner.iterator.NumberIterator

This iterator produces and iterates through sequential numbers.

## rierino\_runner.iterator.DataIterator

This iterator translates a data array into individual records and loops through them.

| Parameter | Definition                                | Example   | Default |
| --------- | ----------------------------------------- | --------- | ------- |
| element   | Element in received payload to iterate on | data.list | -       |

## rierino\_runner.iterator.FSIterator

This iterator goes through list of files within a file system and returns their details as a loop.

| Parameter   | Definition                                                            | Example     | Default |
| ----------- | --------------------------------------------------------------------- | ----------- | ------- |
| system      | Name of system in connections to connect to                           | fs\_default | -       |
| path        | Path on file system to list                                           | /media      | -       |
| pathElement | Json path for the sub-path in input payload to append to path         | data.folder | -       |
| withPrefix  | Whether path includes a prefix (after last / character)               | true        | -       |
| format      | Type of data to be produced for discovered files (full, name, file)   | path        | file    |
| dedupe      | Whether duplicates - based on prefix - should be removed (order, all) | order       | -       |
| orderRegex  | Regex to extract "order" number from file name                        | -           | -       |
| download    | Whether files should be downloaedd                                    | true        | -       |
| recursive   | Whether sub directories should be also iterated                       | true        | -       |
| withDirs    | Whether directories should be included in results                     | true        | -       |

## rierino\_runner.iterator.IOIterator

This iterator reads records from a database or file and iterates through them as a loop, utilizing rierino\_util.IOUtil.

| Parameter | Definition                                                 | Example                                     | Default |
| --------- | ---------------------------------------------------------- | ------------------------------------------- | ------- |
| system    | Name of system in connections to connect to                | mongo\_master                               | -       |
| input     | Input parameters required for reading data from the system | {database: "master", collection: "product"} | -       |

## rierino\_runner.iterator.RestIterator

This iterator makes calls to a REST API and passes responses after each call as a loop.

| Parameter | Definition                                                      | Example         | Default |
| --------- | --------------------------------------------------------------- | --------------- | ------- |
| system    | Name of system in connections to connect to                     | erp             | -       |
| method    | Request method                                                  | GET             | -       |
| url       | Request URL                                                     | /ListProducts   | -       |
| headers   | Headers to pass on in request                                   | {}              | -       |
| pattern   | Jmespath pattern to apply to input payload for request contents | {product: data} | -       |
| retries   | Number of retries if the REST call fails                        | 3               | 0       |

## rierino\_runner.iterator.JavaIterator

This iterator is only applicable when called through Java Py4JEventHandler, allowing hook calls to the Java runner without requiring explicit REST API calls.

| Parameter | Definition                                                      | Example         | Default |
| --------- | --------------------------------------------------------------- | --------------- | ------- |
| pattern   | Jmespath pattern to apply to input payload for request contents | {product: data} | -       |
| meta      | Event metadata to pass on to Java runner                        | -               | -       |
| retries   | Number of retries if the Java call fails                        | 3               | 0       |
