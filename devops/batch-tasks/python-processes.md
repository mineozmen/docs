---
description: Python processes are typically triggered using rierino_util.Runner
---

# Python Processes

{% hint style="info" %}
Processes available out of box for execution are listed on this page. It is also possible to create new processes using rierino\_util.Process as the base class.
{% endhint %}

## rierino\_util.NoopProcess

This is a test process that doesn't perform any specific action.

## rierino\_util.PrintProcess

This is a test process that simply prints the request details on console.

## rierino\_runner.IterateProcess

This is the most commonly used process, which allows looping through an [Iterator](python-iterators.md) and performing tasks using a [Processor](python-processors.md) (such as uploading records in an input file to a REST endpoint), using the following parameters passed in args.parameters and payload submitted:

| Args Parameter              | Definition                                                                                                         | Example                                  | Default                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------- | ------------------------------------- |
| processCycle                | How often the processor actions should be triggered (each, buffer or all)                                          | all                                      | each                                  |
| processBuffer               | Buffer size to use if processCycle is set as buffer                                                                | 10                                       | 100                                   |
| bufferMode                  | How the buffer should be consumed (parallel, sequence or function)                                                 | parallel                                 | function (calls Processor.processAll) |
| bufferWorkers               | Size of worker pool to use if buffer is consumed in parallel                                                       | 5                                        | 3                                     |
| bufferTimeout               | Timeout to wait for workers when buffer is consumed in parallel                                                    | 60                                       | 3                                     |
| maxIterations               | Maximum iterations allowed (to limit iterator loop)                                                                | 100                                      | -                                     |
| sourceLoop                  | Allows creating multiple iterations from a single entry from source, using json path                               | data.list                                | -                                     |
| whilePattern                | Jmespath pattern to decide whether iterations should continue or not (uses {summary, raw, entry, iteration} input) | {continue: raw.hasNext}                  | -                                     |
| processPattern              | Jmespath pattern to feed data into Processor (uses {summary, entry, iteration} input)                              | {list: entry.records}                    | -                                     |
| finishPattern               | Jmespath pattern to feed data into finisher (uses {summary, entry, iteration} input)                               | {pages: iteration}                       | -                                     |
| mapPattern                  | Jmespath pattern to calculate summary after each Process (uses {summary, entry, iteration} input)                  | {list: \[]}                              | -                                     |
| mapGroup                    | Field to group process results (or mapPattern results if available) by for summary                                 | \["type"]                                | -                                     |
| mapAgg                      | Data frame aggregation specification to calculate for summary                                                      | {"id": "count"}                          | -                                     |
| starter.module              | Processor module to use as starter to enrich parameters                                                            | -                                        | -                                     |
| starter.\[module\_params]   | Parameters to pass on to starter processor                                                                         | -                                        | -                                     |
| iterator.module             | Iterator module to use for generating loop data                                                                    | rierino\_runner.iterator.DataIterator    | -                                     |
| iterator.\[module\_params]  | Parameters to pass on to iterator                                                                                  | element=list                             | -                                     |
| processor.module            | Processor module to use for each iteration                                                                         | rierino\_runner.processor.RestProcessor  | -                                     |
| processor.\[module\_params] | Parameters to pass on to processor                                                                                 | <p>url=example.com</p><p>method=POST</p> | -                                     |
| finisher.module             | Processor module to use as finisher at the end of iterations                                                       | -                                        | -                                     |
| finisher.\[module\_params]  | Parameters to pass on to finisher processor                                                                        | -                                        | -                                     |

## rierino\_media.MediaProcess

Media proces allows manipulation of image, html and video files using the following arguments passed as args:

| Argument                    | Definition                                          | Example                                                 | Default |
| --------------------------- | --------------------------------------------------- | ------------------------------------------------------- | ------- |
| source                      | Connection & path for the source file               | {connection: "fs\_main", path: "/images/1.png"}         | -       |
| target                      | Connection & path for the target file to be created | {connection: "fs\_main", path: "/images/1\_result.png"} | -       |
| parameters.type             | File type (image, video, html)                      | image                                                   | -       |
| parameters.\[media\_params] | File type and action specific parameters            | {steps: \[], optimize: true}                            | -       |

## rierino\_media.MediaStatsProcess

Media stats proces allows analysis of an image file, including details such as shape, dpi, top colors and background using the following arguments passed as args:

| Argument | Definition                            | Example                                         | Default |
| -------- | ------------------------------------- | ----------------------------------------------- | ------- |
| source   | Connection & path for the source file | {connection: "fs\_main", path: "/images/1.png"} | -       |

## rierino\_tensor.TFModelProcess

TF model proces allows training of a Tensorflow model using the following arguments passed as args:

| Argument | Definition                                                                                  | Example | Default |
| -------- | ------------------------------------------------------------------------------------------- | ------- | ------- |
| model    | Model object, using [ML model](../../data-science/ml-models/) structure in data science app | -       | -       |

## rierino\_spark.SparkModelProcess

Spark model proces allows training Spark based models using the following arguments passed as args:

| Argument    | Definition                                                                                  | Example | Default |
| ----------- | ------------------------------------------------------------------------------------------- | ------- | ------- |
| model       | Model object, using [ML model](../../data-science/ml-models/) structure in data science app | -       | -       |
| sparkConfig | Configuration to pass on to Spark                                                           | -       | -       |
