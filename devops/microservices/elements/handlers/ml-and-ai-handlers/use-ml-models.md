---
description: >-
  This set of handlers provide ability to run real-time inference using various
  machine learning libraries, which share the same set of parameters and
  actions.
---

# Use ML Models

## Handler Parameters

This handler is [cacheable](../#handler-caching).

| Parameter          | Definition                                          | Example        | Default        |
| ------------------ | --------------------------------------------------- | -------------- | -------------- |
| model.state        | Name of the state manager with model configurations | model          | -              |
| model.id           | Id of the model to run on this instance             | recommendation | -              |
| model.store.system | Name of the system to store model assets            | hdfs\_default  | -              |
| model.local.dir    | Local directory to store model assets for execution | /tmp           | java.io.tmpdir |

{% file src="../../../../../.gitbook/assets/handler-tensor-0001.json" %}
Example Tensorflow Model Handler Definition (Can be Imported on Element Screen)
{% endfile %}

## Actions

### Score / ScoreML

Scores input data using pretrained model managed by this handler. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example    | Default |
| -------------- | -------------------------------------------------- | ---------- | ------- |
| Input Element  | Json path for the input in request event payload   | parameters | -       |
| Output Element | Json path for the output in response event payload | $.score    | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "examples": [
            "parameters"
          ],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": [
            "$.score"
          ],
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

With event metadata parameters as:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter      | Definition                                                          | Example                        | Default |
| -------------- | ------------------------------------------------------------------- | ------------------------------ | ------- |
| Input Pattern  | JMESPath pattern to apply on data input                             | \[ \[text] ]                   | -       |
| Output Pattern | JMESPath pattern to apply on data output, before returning response | {totalPrice:basket.totalPrice} | -       |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "inputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data input",
              "examples": [
                "[ [text] ]"
              ],
              "default": null
            },
            "outputPattern": {
              "type": "string",
              "description": "JMESPath pattern to apply on data output, before returning response",
              "examples": [
                "{totalPrice:basket.totalPrice}"
              ],
              "default": null
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Versions

### TensorScoreEventHandler

This handler (_com.rierino.handler.tensor.TensorScoreEventHandler_) uses [Tensorflow](https://www.tensorflow.org) library for inference using saved models.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'tensor', version:"${rierinoVersion}")
```

### OnnxScoreEventHandler

This handler (_com.rierino.handler.onnx.OnnxScoreEventHandler_) uses [ONNX Runtime](https://onnxruntime.ai) library for inference using models saved using this common format.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'onnx', version:"${rierinoVersion}")
```

### PMMLScoreEventHandler

This handler (_com.rierino.handler.pmml.PMMLScoreEventHandler_) uses [PMML](https://dmg.org) formatted models for inference.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'pmml', version:"${rierinoVersion}")
```

### OpenNLPScoreEventHandler

This handler (_com.rierino.handler.opennlp.OpenNLPScoreEventHandler_) uses [OpenNLP](https://opennlp.apache.org) library for inference using common NLP tools, saved dictionaries and models.

This handler requires the following dependency added to [deployment contents](../../../deployments/defining-a-deployment.md):

```gradle
implementation (group:'com.rierino.custom', name: 'opennlp', version:"${rierinoVersion}")
```
