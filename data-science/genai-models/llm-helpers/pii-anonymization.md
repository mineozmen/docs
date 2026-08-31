---
description: >-
  Rierino detects and removes PII (Personally Identifiable Information) from
  text before it reaches an LLM, then restores it in the model's output
---

# PII Anonymization

Two processes work as a pair:

* **Anonymize** — detect PII spans and replace them with placeholders, returning a mapping.
* **Deanonymize** — use that mapping to put the original values back into the LLM's response.

Both are called over the API: you pass text and parameters, and receive anonymized text (plus a mapping) or restored content. No Python is written by hand.

```
text ──► Anonymize ──► anonymized text + mapping ──► LLM
                              │                        │
                              └──────────┐   ┌─────────┘
                                         ▼   ▼
                              mapping + LLM output ──► Deanonymize ──► restored result
```

{% hint style="success" %}
**Why round-trip through placeholders?** The LLM never sees real names, emails, or account numbers — only opaque tokens like `<PERSON_a1b2c3d4>`. It reasons over the structure of the request and returns tokens in its answer, which Deanonymize swaps back to the real values. Sensitive data stays inside your platform.
{% endhint %}

***

## Anonymize

#### Parameters

Passed under `parameters` in the process config.

| Parameter    | Type   | Default | Description                                                                                         |
| ------------ | ------ | ------- | --------------------------------------------------------------------------------------------------- |
| `text`       | string | —       | Text to anonymize                                                                                   |
| `uuid`       | bool   | `false` | UUID mode — replace PII with reversible `<ENTITY_hex>` placeholders (recommended for LLM pipelines) |
| `analyzer`   | dict   | `{}`    | Detection options, forwarded to Presidio's analyzer (see below)                                     |
| `anonymizer` | dict   | `{}`    | Replacement operators — only used when `uuid` is `false` (see below)                                |

#### Detection options (`parameters.analyzer`)

| Parameter                 | Type   | Default  | Description                                                                     |
| ------------------------- | ------ | -------- | ------------------------------------------------------------------------------- |
| `supported_languages`     | list   | `["en"]` | Languages the engine supports                                                   |
| `default_score_threshold` | float  | `0.35`   | Minimum confidence for a detection to be kept                                   |
| `language`                | string | `"en"`   | Language of the input text                                                      |
| `entities`                | list   | all      | Restrict detection to specific entity types, e.g. `["PERSON", "EMAIL_ADDRESS"]` |
| `score_threshold`         | float  | —        | Per-call override of `default_score_threshold`                                  |
| `allow_list`              | list   | —        | Values that should never be anonymized, even if detected                        |
| `context`                 | list   | —        | Context words that boost detection confidence                                   |
| `log_decision_process`    | bool   | `false`  | Log which recognizer fired for each detection                                   |

#### Common entity types

| Entity          | Description                                |
| --------------- | ------------------------------------------ |
| `PERSON`        | Full names                                 |
| `EMAIL_ADDRESS` | Email addresses                            |
| `PHONE_NUMBER`  | Phone numbers                              |
| `LOCATION`      | Places, addresses                          |
| `ORGANIZATION`  | Company and institution names              |
| `DATE_TIME`     | Dates and times                            |
| `NRP`           | Nationalities, religions, political groups |
| `IBAN_CODE`     | Bank account numbers                       |
| `CREDIT_CARD`   | Credit card numbers                        |
| `CRYPTO`        | Cryptocurrency wallet addresses            |
| `IP_ADDRESS`    | IPv4 and IPv6 addresses                    |
| `URL`           | Web URLs                                   |
| `US_SSN`        | US Social Security Numbers                 |

Full list: [Presidio supported entities](https://microsoft.github.io/presidio/supported_entities/).

#### Output

| Field     | Type          | Description                                                      |
| --------- | ------------- | ---------------------------------------------------------------- |
| `result`  | string / null | Anonymized text. `null` if no input text was provided            |
| `mapping` | dict / null   | `{ placeholder: entry }` — each entry is `{ text, label?, id? }` |

Each mapping entry always carries `text` (the original span). Entries that matched a custom list record also carry `label` (canonical name) and, when the record had one, `id`.

***

### Replacement modes

#### UUID mode (`uuid: true`) — recommended for LLM pipelines

Each detected span is replaced with a unique `<ENTITY_TYPE_hex8>` placeholder. The same original value always maps to the same placeholder within a single run, and the mapping is fully reversible.

```
Input:  "Call John Doe or Jane Doe at 212-555-0100"
Output: "Call <PERSON_a1b2c3d4> or <PERSON_e5f6a7b8> at <PHONE_NUMBER_c9d0e1f2>"
Mapping: {
  "<PERSON_a1b2c3d4>": {"text": "John Doe"},
  "<PERSON_e5f6a7b8>": {"text": "Jane Doe"},
  "<PHONE_NUMBER_c9d0e1f2>": {"text": "212-555-0100"}
}
```

#### Operator mode (`uuid: false`)

Uses Presidio's built-in operators instead of reversible placeholders. Set `parameters.anonymizer.operators` as a map of entity type (or `"DEFAULT"`) to an operator config.

| Operator  | Parameters                                  | Description                                         |
| --------- | ------------------------------------------- | --------------------------------------------------- |
| `replace` | `new_value`                                 | Replace with a fixed string, e.g. `"<PERSON>"`      |
| `redact`  | —                                           | Remove the PII span entirely                        |
| `mask`    | `masking_char`, `chars_to_mask`, `from_end` | Mask N characters with a character                  |
| `hash`    | `hash_type` (`sha256`, `sha512`, `md5`)     | Replace with a hash of the original value           |
| `encrypt` | `key`                                       | AES-CBC encrypt the value (reversible with the key) |

{% hint style="info" %}
Operator mode is for **one-way** redaction (masking, hashing, fixed labels). For a reversible round trip through an LLM, use **UUID mode** — only it produces a mapping you can fully deanonymize afterwards.
{% endhint %}

***

### Custom entity lists

Beyond Presidio's built-in detectors, you can supply your own lists of domain-specific entities — company names, product codes, internal IDs — under `lists` (parallel to `parameters`). Each entry defines a recognizer with exact and/or fuzzy matching.

| Field                   | Type   | Default           | Description                                                                                                               |
| ----------------------- | ------ | ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `entity`                | string | `"CUSTOM_ORG"`    | Entity type label assigned to matches                                                                                     |
| `name`                  | string | `"custom_dict"`   | Recognizer name                                                                                                           |
| `language`              | string | `"en"`            | Language this recognizer applies to                                                                                       |
| `context`               | list   | company/org words | Context words that boost detection confidence                                                                             |
| `pattern`               | bool   | `false`           | Enable exact / deny-list matching                                                                                         |
| `fuzzy`                 | bool   | `false`           | Enable fuzzy matching                                                                                                     |
| `pattern_score`         | float  | `0.85`            | Confidence assigned to exact matches                                                                                      |
| `fuzzy_threshold`       | float  | `0.90`            | Similarity ratio (0–1) required for a fuzzy match                                                                         |
| `fuzzy_min_alpha_chars` | int    | `6`               | Minimum alphabetic characters a span must have before fuzzy matching is attempted — filters short fragments like `"Ltd."` |
| `fuzzy_prefix_only`     | bool   | `false`           | Compare candidates only against the _start_ of each name — prevents mid-string fragments from matching                    |
| `records`               | list   | —                 | Inline list of strings or `{id, label}` objects to match                                                                  |
| `path`                  | string | —                 | Path to a CSV file with a `label` column (used when `records` is not set)                                                 |

Both `pattern` and `fuzzy` can be enabled together — two recognizers are created and their results merged.

#### Records format

Records can be plain strings or objects carrying an `id` and `label`. Both forms can be mixed. The `id` and `label` flow through to the mapping and the deanonymizer output.

```json
"records": ["Acme Corp", "Globex"]
```

```json
"records": [
  {"id": "42", "label": "Acme Corp"},
  {"id": "43", "label": "Globex Corporation"}
]
```

{% hint style="warning" %}
**Fuzzy matching false positives.** Substring scoring can make a short token like `"Ltd."` match any organisation name containing those letters. Two guards help: `fuzzy_min_alpha_chars` skips spans too short to be meaningful, and `fuzzy_prefix_only` accepts only tokens that match the _beginning_ of a name (e.g. `"Abbott"` → `"ABBOTT LABORATUARLARI…"`). When using `fuzzy_prefix_only`, lower `fuzzy_threshold` slightly (e.g. `0.85`) to account for the stricter scoring. Where candidates overlap, the longest non-overlapping match wins.
{% endhint %}

***

## Deanonymize

Reverses anonymization using the mapping returned by Anonymize. Accepts either a plain string (`text`) or a JSON structure (`body`).

#### Parameters

| Parameter      | Type        | Default | Description                                                                                      |
| -------------- | ----------- | ------- | ------------------------------------------------------------------------------------------------ |
| `mapping`      | dict        | `{}`    | The `mapping` dict returned by Anonymize                                                         |
| `text`         | string      | —       | Anonymized string to restore. Mutually exclusive with `body`                                     |
| `body`         | dict / list | —       | JSON structure to restore — all string values are traversed recursively                          |
| `inject_label` | bool        | `false` | Replace placeholders with the record's canonical `label` instead of the original detected `text` |

#### Output

**Text mode** returns `result` — the restored string.

**Body mode** returns `result` (the restored structure) plus `records` — a `{ field_path: {id?, label, text} }` map, one entry per body field that contained a custom-list placeholder. Nested fields use dot notation (e.g. `"company.seller"`). Fields whose placeholder was a built-in entity (`PERSON`, `EMAIL_ADDRESS`, …) are not listed in `records`.

#### `inject_label` behaviour

| `inject_label`    | Placeholder replaced with                                       |
| ----------------- | --------------------------------------------------------------- |
| `false` (default) | `text` — the original span from the input (`"Acme Corp."`)      |
| `true`            | `label` — the canonical record name (`"Acme Corporation Ltd."`) |

A placeholder with no `label` (built-in entity) always falls back to `text`, regardless of this flag.
