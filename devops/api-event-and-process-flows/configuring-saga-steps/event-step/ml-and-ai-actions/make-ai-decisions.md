---
description: >-
  These actions answer structured questions about a given state, for use cases
  such as routing, triage, intent detection and scoring. They return typed
  answers with probabilities instead of free text.
---

# Make AI Decisions

## Make AI Decisions Actions

### **Decide**

Answers one or more typed questions about a state and, optionally, a conversation history. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in request event payload   | input   | -       |
| Output Element | Json path for the output in response event payload | output  | -       |
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
          "examples": ["input"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
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
| Parameter     | Definition                                                                          | Example                                   | Default |
| ------------- | ----------------------------------------------------------------------------------- | ----------------------------------------- | ------- |
| Input Pattern | JMESpath pattern to apply on input element for getting state, questions and history | {state: ticket, questions: q, history: h} | -       |
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
              "description": "JMESpath pattern to apply on input element for getting state, questions and history",
              "examples": ["{state: ticket, questions: q, history: h}"],
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

The input element (after the input pattern is applied) must be in `{state, questions, history}` format:

* `state`: the content to decide on. It can be a text value or an object. Object fields are passed to the model as named state values.
* `questions` (required): an object keyed by question name. Each question has a `type`, `instructions` and, depending on the type, `criteria`.
* `history` (optional): an array of `{role, content}` messages, where `role` is `user` or `assistant`.

Either `state` or `history` must be provided.

| Question Type | Definition                                     | Criteria                                                                                                       |
| ------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `choice`      | Selects one option from a set (classification) | Object of `{option: description}` (description can be `null`), or an array of option names. At least 2 options |
| `score`       | Rates on an ordered scale                      | Array of levels from lowest to highest                                                                         |
| `noul`        | Answers as yes, no or unknown                  | Not used                                                                                                       |

`instructions` is usually text. Structured instructions are also accepted and passed to the model as JSON text.

```json
{
  "state": "I was charged twice for my order and want my money back today.",
  "questions": {
    "department": {"type": "choice", "instructions": "Which team should handle this?", "criteria": {"billing": "payments and invoices", "support": null, "sales": null}},
    "urgency":    {"type": "score",  "instructions": "How urgent is this request?", "criteria": ["low", "medium", "high"]},
    "refund":     {"type": "noul",   "instructions": "Is the customer asking for a refund?"}
  }
}
```

Answers are written to `answers` in the output element, keyed by question name. Model details are written to `usage` as `{model, input_tokens}`.

```json
{
  "answers": {
    "department": {"type": "choice", "choice": "billing", "probabilities": {"billing": 0.91, "support": 0.07, "sales": 0.02}, "confidence": 0.91},
    "urgency":    {"type": "score", "score": 0.86, "level": "high", "probabilities": {"low": 0.03, "medium": 0.18, "high": 0.79}},
    "refund":     {"type": "noul", "noul": true}
  },
  "usage": {"model": "deberta-v3-base-mnli-fever-anli", "input_tokens": 142}
}
```

`probabilities` and `confidence` are returned only when the model provides them. For `score` questions, `level` is the most likely level.
