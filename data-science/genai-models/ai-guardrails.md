---
description: >-
  Configure multi-stage safety checks for AI agents using risk policies, regex
  rules, judge models, saga processors, and reusable presets
---

# AI Guardrails

AI guardrails let you inspect, modify, or block content inside the agent loop. You can apply them to user prompts, model responses, and tool results. This gives you a central governance layer without rebuilding [AI Agent APIs](ai-agent-apis.md) or the broader [GenAI Models](./) setup.

If you need fully custom governance, you can still embed checks directly in your `CallAIAgent` flow. The built-in pipeline covers the most common safety controls with less effort and better reuse.

<figure><img src="../../.gitbook/assets/image (180).png" alt=""><figcaption><p>GenAI Guardrails Configuration</p></figcaption></figure>

## Risk policy

The risk policy decides how findings from multiple guardrails are combined.

Each finding carries one of four risk levels:

* **LOW** for weak or informational signals
* **MEDIUM** for meaningful but recoverable concerns
* **HIGH** for strong policy violations
* **CRITICAL** for severe violations that should usually stop execution

After all checks run for a stage, Rierino evaluates the overall result. The policy can combine findings in four ways:

* **Weighted score:** assign a numeric weight to each risk level
* **Block threshold:** stop processing when the total score exceeds a limit
* **Critical override:** block immediately when any critical finding appears
* **Count-based rules:** block when a level appears too many times

The weighted score follows a sum-product model:

$$
\text{score} = \sum (\text{risk weight} \times \text{number of findings})
$$

This lets you tune strictness without rewriting each individual guardrail.

## Guardrail pipeline

An agent loop runs once for each request. It can read memory, call tools, generate responses, and repeat until the task completes. Guardrails can run at three independent points in that loop.

### Pipeline stages

* **Inputs:** validate user prompts before the model or tools see them
* **Outputs:** review model responses before they reach the user
* **Tool Responses:** inspect data returned from RAG, APIs, or tool calls before it goes back into the model context

Each stage has its own configuration. You can reuse the same rules everywhere or apply stricter controls only where they matter.

{% hint style="info" %}
Input, output, and tool-response guardrails use the same configuration model. Only the stage and risk policy differ.
{% endhint %}

### Guardrail actions

Each guardrail returns one of four actions:

* **ALLOW:** pass the content unchanged
* **MODIFY:** mask or rewrite part of the content
* **REPROMPT:** ask the model to produce a safer answer
* **BLOCK:** stop the current loop for that stage

`REPROMPT` is most useful on output checks. It lets the model recover instead of failing the whole request immediately.

### Guardrail types

Rierino supports deterministic, model-based, and flow-based guardrails. You can combine them in the same stage.

#### Regex matcher

Use a regex matcher when you need fast, deterministic detection. Typical cases include prompt injection phrases, script tags, SQL fragments, or identifiers that must never pass through unchanged.

The matcher checks content against explicit patterns, named presets, or preset groups.

**Configuration schema**

{% code overflow="wrap" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RegexMatcherGuardrail",
  "type": "object",
  "properties": {
    "patterns": {
      "type": "array",
      "description": "Pattern rules applied in order.",
      "items": {
        "oneOf": [
          {
            "title": "ExplicitPattern",
            "type": "object",
            "properties": {
              "id": {
                "type": "string",
                "description": "Unique rule identifier."
              },
              "regex": {
                "type": "string",
                "description": "Regular expression used for matching."
              },
              "action": {
                "type": "string",
                "enum": ["ALLOW", "MODIFY", "REPROMPT", "BLOCK"]
              },
              "riskLevel": {
                "type": "string",
                "enum": ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
              },
              "failureMessage": {
                "type": "string",
                "description": "Optional message returned when the rule fails."
              }
            },
            "required": ["id", "regex"]
          },
          {
            "title": "PresetReference",
            "type": "object",
            "properties": {
              "preset": {
                "type": "string",
                "description": "Registered preset identifier."
              },
              "action": {
                "type": "string",
                "enum": ["ALLOW", "MODIFY", "REPROMPT", "BLOCK"]
              },
              "riskLevel": {
                "type": "string",
                "enum": ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
              },
              "failureMessage": {
                "type": "string"
              }
            },
            "required": ["preset"]
          }
        ]
      }
    },
    "groups": {
      "type": "array",
      "description": "Preset groups expanded into their member presets.",
      "items": {
        "type": "string"
      }
    }
  },
  "examples": [
    {
      "patterns": [
        {
          "id": "sql-injection-vector",
          "regex": "(?i)(SELECT|INSERT|UPDATE|DELETE|DROP|UNION)\\s+.*",
          "action": "BLOCK",
          "riskLevel": "HIGH",
          "failureMessage": "Security violation detected."
        },
        {
          "preset": "sql-injection"
        },
        {
          "preset": "us-ssn",
          "action": "BLOCK",
          "riskLevel": "CRITICAL",
          "failureMessage": "Sensitive personal data is not allowed."
        }
      ],
      "groups": ["jailbreak-basic"]
    }
  ]
}
```
{% endcode %}

#### Regex masker

Use a regex masker when the content is still useful after redaction. This is common for PII, account numbers, or contact details.

The masker keeps the request moving while removing sensitive values.

**Configuration schema**

{% code overflow="wrap" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RegexMaskerGuardrail",
  "type": "object",
  "properties": {
    "patterns": {
      "type": "array",
      "description": "Masking rules applied in order.",
      "items": {
        "oneOf": [
          {
            "title": "ExplicitMaskPattern",
            "type": "object",
            "properties": {
              "id": {
                "type": "string"
              },
              "regex": {
                "type": "string"
              },
              "maskCharacter": {
                "type": "string",
                "minLength": 1,
                "maxLength": 1,
                "description": "Single character used for masking."
              },
              "preserveLength": {
                "type": "boolean",
                "description": "When true, keeps the original matched length."
              },
              "riskLevel": {
                "type": "string",
                "enum": ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
              }
            },
            "required": ["id", "regex"]
          },
          {
            "title": "PresetMaskReference",
            "type": "object",
            "properties": {
              "preset": {
                "type": "string"
              },
              "maskCharacter": {
                "type": "string",
                "minLength": 1,
                "maxLength": 1
              },
              "preserveLength": {
                "type": "boolean"
              },
              "riskLevel": {
                "type": "string",
                "enum": ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
              }
            },
            "required": ["preset"]
          }
        ]
      }
    },
    "groups": {
      "type": "array",
      "items": {
        "type": "string"
      }
    }
  },
  "examples": [
    {
      "patterns": [
        {
          "id": "us-ssn",
          "regex": "\\b\\d{3}-\\d{2}-\\d{4}\\b",
          "maskCharacter": "*",
          "preserveLength": true,
          "riskLevel": "HIGH"
        },
        {
          "preset": "email"
        },
        {
          "preset": "us-phone",
          "maskCharacter": "X",
          "preserveLength": false,
          "riskLevel": "MEDIUM"
        }
      ],
      "groups": ["pii-basic"]
    }
  ]
}
```
{% endcode %}

#### LLM as Judge

Use an LLM judge when the policy depends on meaning, intent, tone, or context. This works well for brand violations, unsafe advice, policy classification, or nuanced output review.

The judge runs as a separate GenAI model and returns a decision marker. Rierino converts that decision into a guardrail action.

**Configuration schema**

{% code overflow="wrap" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LlmJudgeGuardrail",
  "type": "object",
  "properties": {
    "judgeInvoker": {
      "type": "string",
      "const": "rai",
      "description": "Invoker used for the judge model."
    },
    "systemPrompt": {
      "type": "string",
      "description": "System instruction sent to the judge."
    },
    "userPromptTemplate": {
      "type": "string",
      "description": "Template populated with the evaluated payload."
    },
    "violationMarker": {
      "type": "string",
      "description": "Text marker that indicates a policy violation."
    },
    "failureAction": {
      "type": "string",
      "enum": ["ALLOW", "MODIFY", "REPROMPT", "BLOCK"]
    },
    "repromptMessage": {
      "type": "string",
      "description": "Prompt used when the action is REPROMPT."
    },
    "invoke": {
      "type": "object",
      "properties": {
        "model": {
          "type": "string",
          "description": "GenAI model ID used as the judge."
        }
      },
      "required": ["model"]
    }
  },
  "required": [
    "judgeInvoker",
    "systemPrompt",
    "userPromptTemplate",
    "violationMarker",
    "failureAction",
    "invoke"
  ],
  "examples": [
    {
      "judgeInvoker": "rai",
      "systemPrompt": "You are a policy verification model. Answer SAFE or VIOLATION.",
      "userPromptTemplate": "BOT OUTPUT: {{payload}}",
      "violationMarker": "VIOLATION",
      "failureAction": "REPROMPT",
      "repromptMessage": "Rewrite the response without naming competitors or exposing restricted details.",
      "invoke": {
        "model": "content_assistant"
      }
    }
  ]
}
```
{% endcode %}

#### Saga processor

Use a saga processor when you need full platform logic. This is the most flexible option. It can call states, queries, systems, rules, or other sagas before returning a decision.

This is the right choice for domain-specific validation, content enrichment, or policy logic that depends on business data.

**Configuration schema**

{% code overflow="wrap" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SagaProcessorGuardrail",
  "type": "object",
  "properties": {
    "sagaPath": {
      "type": "string",
      "description": "Saga API path used to execute the guardrail."
    },
    "sagaId": {
      "type": "string",
      "description": "Direct saga identifier."
    }
  },
  "oneOf": [
    {
      "required": ["sagaPath"]
    },
    {
      "required": ["sagaId"]
    }
  ],
  "examples": [
    {
      "sagaPath": "/CheckAIRequest"
    },
    {
      "sagaId": "check-ai-0001"
    }
  ]
}
```
{% endcode %}

The guardrail saga receives the content to inspect in the `content` field. It should return a payload plus decision metadata.

**Saga response schema**

{% code overflow="wrap" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "GuardrailSagaResponse",
  "type": "object",
  "properties": {
    "payload": {
      "description": "Replacement or reprompt content returned by the saga."
    },
    "meta": {
      "type": "object",
      "properties": {
        "action": {
          "type": "string",
          "enum": ["ALLOW", "MODIFY", "REPROMPT", "BLOCK"]
        },
        "message": {
          "type": "string",
          "description": "Human-readable reason for the decision."
        },
        "riskLevel": {
          "type": "string",
          "enum": ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
        },
        "sourceId": {
          "type": "string",
          "description": "Identifier of the rule or subsystem that produced the result."
        }
      },
      "required": ["action"]
    }
  },
  "required": ["meta"],
  "examples": [
    {
      "payload": "Please rephrase your request without account numbers or personal identifiers.",
      "meta": {
        "action": "BLOCK",
        "message": "Blocked because the content matched a spam and PII policy.",
        "riskLevel": "HIGH",
        "sourceId": "spam-check"
      }
    },
    {
      "payload": "Customer email: [EMAIL]",
      "meta": {
        "action": "MODIFY",
        "message": "Sensitive contact detail was masked before continuing.",
        "riskLevel": "MEDIUM",
        "sourceId": "pii-masker"
      }
    }
  ]
}
```
{% endcode %}

### Guardrail presets

Regex guardrails include a built-in preset registry. Presets save time and keep common rules consistent across models.

You can reference a preset directly or include a preset group. You can also override the default action, risk level, or masking behavior inside a specific guardrail.

#### Preset patterns

| Preset                 | Purpose                                                     | Default risk | Default action | Default masking                    |
| ---------------------- | ----------------------------------------------------------- | ------------ | -------------- | ---------------------------------- |
| `email`                | Detect email addresses                                      | `MEDIUM`     | `MODIFY`       | `[EMAIL]`                          |
| `us-ssn`               | Detect US Social Security numbers                           | `HIGH`       | `MODIFY`       | `*` with original length preserved |
| `us-phone`             | Detect US and Canada phone numbers                          | `MEDIUM`     | `MODIFY`       | `[PHONE]`                          |
| `credit-card`          | Detect 13 to 19 digit card numbers                          | `HIGH`       | `MODIFY`       | `[CARD]`                           |
| `iban`                 | Detect IBAN account numbers                                 | `HIGH`       | `MODIFY`       | `[IBAN]`                           |
| `ipv4`                 | Detect IPv4 addresses                                       | `LOW`        | `MODIFY`       | `[IP]`                             |
| `ipv6`                 | Detect full-form IPv6 addresses                             | `LOW`        | `MODIFY`       | `[IPV6]`                           |
| `dob-iso`              | Detect dates of birth in `YYYY-MM-DD` format                | `MEDIUM`     | `MODIFY`       | `[DOB]`                            |
| `dob-us`               | Detect dates of birth in `MM/DD/YYYY` format                | `MEDIUM`     | `MODIFY`       | `[DOB]`                            |
| `sql-injection`        | Detect common SQL injection phrasing                        | `HIGH`       | `BLOCK`        | Not used                           |
| `javascript-injection` | Detect script tags, `javascript:` URIs, and inline handlers | `HIGH`       | `BLOCK`        | Not used                           |
| `forced-instruction`   | Detect common jailbreak overrides of system instructions    | `HIGH`       | `BLOCK`        | Not used                           |
| `prompt-leak`          | Detect attempts to reveal hidden prompts or instructions    | `MEDIUM`     | `BLOCK`        | Not used                           |
| `command-injection`    | Detect dangerous shell command chaining                     | `CRITICAL`   | `BLOCK`        | Not used                           |
| `path-traversal`       | Detect repeated directory traversal sequences               | `MEDIUM`     | `BLOCK`        | Not used                           |

#### Preset groups

| Group                | Included presets                                                             | Typical use                              |
| -------------------- | ---------------------------------------------------------------------------- | ---------------------------------------- |
| `pii-basic`          | `email`, `us-ssn`, `us-phone`, `credit-card`                                 | Basic redaction for common personal data |
| `pii-extended`       | `pii-basic` plus `iban`, `ipv4`, `ipv6`, `dob-iso`, `dob-us`                 | Broader privacy filtering                |
| `jailbreak-basic`    | `sql-injection`, `javascript-injection`, `forced-instruction`, `prompt-leak` | Baseline prompt defense                  |
| `jailbreak-extended` | `jailbreak-basic` plus `command-injection`, `path-traversal`                 | Stronger defense for tool-enabled agents |

{% hint style="success" %}
Preset defaults are only a starting point. You can override risk level, action, and masking behavior inside each guardrail configuration.
{% endhint %}
