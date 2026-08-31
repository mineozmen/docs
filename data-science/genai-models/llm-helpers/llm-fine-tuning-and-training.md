---
description: Rierino's tuner runs LLM post-training as a configurable, multi-step pipeline
---

# LLM Fine-tuning & Training

You describe a model and an ordered list of training steps in a JSON configuration, and the platform executes each step in turn — loading a base model, resolving a dataset, running the trainer, evaluating it against acceptance thresholds, and saving the result. No Python is written by hand: every knob below is a parameter you pass through the API.

Built-in training methods cover the full post-training spectrum:

| Method     | Purpose                                                                        |
| ---------- | ------------------------------------------------------------------------------ |
| **CPT**    | Continued pre-training — adapt a base model to a new domain/corpus             |
| **SFT**    | Supervised fine-tuning on prompt/completion (or chat) data                     |
| **DPO**    | Direct Preference Optimization on chosen/rejected pairs                        |
| **KTO**    | Kahneman-Tversky Optimization on pointwise (good/bad) feedback                 |
| **PPO**    | Proximal Policy Optimization against a reward model                            |
| **GRPO**   | Group Relative Policy Optimization with reward functions and/or a reward model |
| **Reward** | Train a reward model from preference pairs (feeds PPO/GRPO)                    |

{% hint style="info" %}
Steps chain automatically. Each step's base model defaults to the **previous step's output**, so common pipelines like **SFT → DPO** or **Reward → PPO** need no manual wiring between stages.
{% endhint %}

***

## How it works

The configuration carries a model definition with an ordered `steps[]` list. One tuner runs per step:

```
model.data.steps[]
        │
        ▼
for each step:
    load base model + tokenizer   (explicit, or previous step's output)
    optional PEFT / LoRA wrap
    load dataset from inputs
    build trainer (per-method)
    train → evaluate → acceptance check
        │
        ▼
    save model → archive → copy to remote step path
        │
        ▼
    next step's base model defaults to this output
```

PEFT/LoRA adapters are merged into the base weights at save time (by default when LoRA is used), so each downstream step loads a normal merged model.

***

## Model definition

The top-level model object provided to the process.

| Field            | Description                                                          |
| ---------------- | -------------------------------------------------------------------- |
| `id`             | Model id                                                             |
| `data.name`      | Display name                                                         |
| `data.version`   | Version (used for working-directory naming and the remote step path) |
| `data.status`    | `"A"` to run; anything else is skipped                               |
| `data.root`      | Remote root used by the step path                                    |
| `data.directory` | Remote subdirectory under `root` (defaults to `{id}_{version}`)      |
| `data.steps`     | Ordered list of step configs (see below)                             |

#### Step

| Field                     | Description                                                                |
| ------------------------- | -------------------------------------------------------------------------- |
| `id`                      | Step id                                                                    |
| `identifier`              | Optional human-friendly id, used as the archive filename. Defaults to `id` |
| `root`                    | Optional override for the remote step path of this step only               |
| `parameters.training`     | Training configuration (see Training parameters)                           |
| `parameters.optimization` | Post-training options (see Optimization)                                   |

***

## Training parameters

Passed under `parameters.training` on each step.

| Parameter         | Type   | Default                | Description                                                                              |
| ----------------- | ------ | ---------------------- | ---------------------------------------------------------------------------------------- |
| `method`          | string | `"sft"`                | Training method: `cpt`, `sft`, `dpo`, `kto`, `ppo`, `grpo`, `reward`                     |
| `class`           | string | from `method`          | Explicit tuner class name (advanced — overrides `method`)                                |
| `package`         | string | `rierino_llm.tuner`    | Module path for a custom tuner class                                                     |
| `model`           | string | previous step's output | Base model id / path. The first step must set this                                       |
| `modelParams`     | dict   | `{}`                   | Model-load overrides (e.g. `{"torch_dtype": "bfloat16"}`)                                |
| `tokenizerParams` | dict   | `{}`                   | Tokenizer-load overrides                                                                 |
| `inputs`          | dict   | —                      | Dataset source (see Inputs)                                                              |
| `dataset`         | dict   | `{}`                   | Dataset parsing (see Dataset)                                                            |
| `batchSize`       | int    | `1`                    | Per-device train batch size                                                              |
| `epochs`          | int    | `1`                    | Number of training epochs                                                                |
| `learningRate`    | float  | trainer default        | Learning rate                                                                            |
| `gradAccum`       | int    | `1`                    | Gradient accumulation steps                                                              |
| `maxSteps`        | int    | —                      | Max training steps (overrides `epochs` when set)                                         |
| `validationRatio` | float  | `0`                    | Fraction of training data held out for evaluation when no explicit eval set is given     |
| `methodParams`    | dict   | `{}`                   | Method-specific extras (e.g. `{"beta": 0.1}` for DPO, `{"num_generations": 4}` for GRPO) |
| `peft`            | dict   | —                      | LoRA configuration. When set, the model is wrapped with PEFT/LoRA                        |
| `refModel`        | string | —                      | Reference model id (DPO / KTO / PPO)                                                     |
| `rewardModel`     | string | —                      | Reward model id (required for PPO, optional for GRPO)                                    |
| `rewardFuncs`     | list   | —                      | Import paths to GRPO reward functions                                                    |
| `checkpoint`      | bool   | `false`                | Resume from the latest checkpoint in the working directory                               |
| `acceptance`      | dict   | `{}`                   | Per-metric `{min, max}` thresholds — a step failing any is rejected and not saved        |
| `samples`         | list   | —                      | Optional prompts to test-generate from after saving (sanity check)                       |
| `args`            | dict   | `{}`                   | Catch-all for any additional training argument not exposed above                         |

{% hint style="info" %}
**`args` and `methodParams` are escape hatches.** Anything the underlying trainer accepts but that isn't surfaced as a named parameter above can be passed through these two dictionaries — `args` for general training arguments, `methodParams` for method-specific config.
{% endhint %}

#### PEFT / LoRA

When `peft` is set, the model is wrapped as a LoRA adapter. Common keys:

| Key               | Description                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| `r`               | LoRA rank                                                                  |
| `lora_alpha`      | LoRA scaling factor                                                        |
| `lora_dropout`    | Dropout applied to LoRA layers                                             |
| `target_modules`  | Modules to adapt, e.g. `"all-linear"`                                      |
| `modules_to_save` | Extra modules trained in full (e.g. `["embed_tokens", "lm_head"]` for CPT) |

***

## Inputs

Passed under `parameters.training.inputs`. The dataset is resolved from the first matching field.

| Field        | Description                                                                                                                |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `hub_id`     | HuggingFace Hub dataset id                                                                                                 |
| `records`    | Inline list of record objects                                                                                              |
| `base64Data` | Base64-encoded dataset file (format taken from `dataset.format`, default `jsonl`)                                          |
| `path`       | Remote or local path. For shards use a comma-separated string, a JSON list, or (locally) a glob such as `corpus/*.parquet` |
| `connection` | Named connection used to download `path` when the source is remote                                                         |

{% hint style="warning" %}
Remote paths must reference **explicit files** — globs are expanded on the local filesystem only. For sharded remote corpora, provide a comma-separated list of concrete files.
{% endhint %}

***

## Dataset

Passed under `parameters.training.dataset`. Controls how the resolved data is parsed.

| Parameter        | Type   | Default             | Description                                                                             |
| ---------------- | ------ | ------------------- | --------------------------------------------------------------------------------------- |
| `format`         | string | auto from extension | `jsonl`, `json`, `csv`, `parquet`, or `hub`                                             |
| `split`          | string | `"train"`           | Hub split name                                                                          |
| `eval_path`      | string | —                   | Optional separate evaluation file                                                       |
| `eval_split`     | string | —                   | Optional Hub evaluation split                                                           |
| `column_mapping` | dict   | `{}`                | Map each expected column to its source column: `{ expectedColumn: sourceColumnInFile }` |
| `load_kwargs`    | dict   | `{}`                | Extra options for Hub dataset loading                                                   |

#### Required columns per method

Columns are checked **after** `column_mapping` is applied.

| Method   | Required columns                            |
| -------- | ------------------------------------------- |
| `cpt`    | `text` (raw corpus)                         |
| `sft`    | `prompt`, `completion` (or `messages`)      |
| `dpo`    | `prompt`, `chosen`, `rejected`              |
| `kto`    | `prompt`, `completion`, `label` (boolean)   |
| `ppo`    | `prompt` (+ `rewardModel`)                  |
| `grpo`   | `prompt` (+ `rewardFuncs` or `rewardModel`) |
| `reward` | `chosen`, `rejected`                        |

{% hint style="info" %}
`column_mapping` is written as **target → source** (`{"prompt": "question", "completion": "answer"}`), so you name the column the trainer expects and point it at whatever column your file actually uses.
{% endhint %}

***

## Optimization

Passed under `parameters.optimization` on each step.

| Parameter    | Type   | Default                   | Description                                            |
| ------------ | ------ | ------------------------- | ------------------------------------------------------ |
| `merge`      | bool   | `true` when `peft` is set | Merge the LoRA adapter into base weights before saving |
| `push`       | bool   | `false`                   | Push the saved model to the HuggingFace Hub            |
| `hubModelId` | string | —                         | Target repository id for the push                      |

***

## Acceptance & sampling

* **`acceptance`** defines per-metric `{min, max}` thresholds evaluated after training. If any metric falls outside its bounds, the step is **rejected**: it is not saved, and the next step falls back to the last accepted step's output.
* **`samples`** is a list of prompts the platform generates from immediately after saving, as a quick qualitative check that the model produces sensible output.

***

## Output

| Field   | Type   | Description                                                                           |
| ------- | ------ | ------------------------------------------------------------------------------------- |
| `model` | string | The model definition's display name                                                   |
| `steps` | list   | Per-step result: `{ step, accepted, output_dir, remote_path, hub_model_id, metrics }` |

Rejected steps appear in the list with `accepted: false` and do not advance the pipeline.

***

## Method comparison

|                        | SFT    | DPO        | KTO            | PPO        | GRPO     | Reward       |
| ---------------------- | ------ | ---------- | -------------- | ---------- | -------- | ------------ |
| Needs preference pairs | No     | Yes        | No (pointwise) | No         | No       | Yes          |
| Needs reward model     | No     | No         | No             | Yes        | Optional | —            |
| Needs reward functions | No     | No         | No             | No         | Optional | No           |
| Needs reference model  | No     | Yes (auto) | Yes (auto)     | Yes (auto) | No       | No           |
| Output                 | Policy | Policy     | Policy         | Policy     | Policy   | Reward model |
