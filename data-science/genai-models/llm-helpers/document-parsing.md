---
description: >-
  Rierino turns documents — PDFs, spreadsheets, Office files, images, HTML, and
  audio — into clean RAG content
---

# Document Parsing

Three engines are available, each tuned for a different input:

| Engine        | Best for                                | Output                                                  |
| ------------- | --------------------------------------- | ------------------------------------------------------- |
| **XLSX**      | Excel spreadsheets                      | One Markdown table per sheet                            |
| **PDF (OCR)** | PDFs — born-digital or scanned          | Markdown text, with table detection                     |
| **Docling**   | PDFs, Office files, images, HTML, audio | Markdown / HTML / JSON / text, plus layout-aware chunks |

{% hint style="info" %}
All engines accept the same two source styles: **`base64Data`** (inline base64-encoded file) or a **`path` + `connection`** pair (a remote file downloaded through a named connection). Every engine returns `null` content when no source is supplied.
{% endhint %}

***

## Source

Common to all three engines. Passed under `source` in the process config.

| Field                 | Description                                                                  |
| --------------------- | ---------------------------------------------------------------------------- |
| `base64Data`          | Base64-encoded file content (with or without a `data:...;base64,` prefix)    |
| `path`                | Remote file path, downloaded via the named `connection`                      |
| `connection`          | Name of the connection to use when `path` is set                             |
| `filename` / `format` | Docling only — optional file-type hint for `base64Data` (defaults to `.pdf`) |

***

## XLSX

Extracts every sheet of an Excel file and returns them as Markdown tables, one per sheet, separated by horizontal rules. Sheet names become level-2 headings.

#### Parameters

Passed under `parameters`.

| Parameter  | Type | Default | Description                            |
| ---------- | ---- | ------- | -------------------------------------- |
| `excel`    | dict | `{}`    | Excel read options (see below)         |
| `markdown` | dict | `{}`    | Markdown rendering options (see below) |

**Excel options (`parameters.excel`)**

| Parameter    | Type                       | Default             | Description                                            |
| ------------ | -------------------------- | ------------------- | ------------------------------------------------------ |
| `sheet_name` | string / int / list / null | `null` (all sheets) | Sheet(s) to read                                       |
| `header`     | int / list / null          | `0`                 | Row to use as column headers (`null` for none)         |
| `skiprows`   | int / list                 | `0`                 | Rows to skip at the start                              |
| `usecols`    | string / list              | `null` (all)        | Columns to include, e.g. `"A:D"` or `["Name", "Date"]` |
| `dtype`      | dict                       | `null`              | Column dtype overrides, e.g. `{"id": "str"}`           |
| `na_values`  | list                       | standard            | Additional values to treat as empty                    |
| `engine`     | string                     | auto                | Parser engine: `openpyxl`, `xlrd`, `odf`               |

**Markdown options (`parameters.markdown`)**

| Parameter  | Type   | Default   | Description                                        |
| ---------- | ------ | --------- | -------------------------------------------------- |
| `index`    | bool   | `false`   | Include the row index column                       |
| `tablefmt` | string | `"pipe"`  | Table format: `pipe` (GitHub), `grid`, `simple`, … |
| `floatfmt` | string | `"g"`     | Float formatting, e.g. `".2f"`                     |
| `numalign` | string | `"right"` | Number alignment                                   |
| `stralign` | string | `"left"`  | String alignment                                   |

#### Output

| Field | Type          | Description                                                              |
| ----- | ------------- | ------------------------------------------------------------------------ |
| `md`  | string / null | All sheets rendered as Markdown tables. `null` if no source was provided |

***

## PDF (OCR)

Extracts text from PDFs. It always tries **pdfplumber** first (fast, no model required); if the PDF is scanned, rotated, or yields too little text, it falls back to an OCR engine. Tables are detected and rendered as Markdown.

```
PDF input
   │
   ▼
pdfplumber ──── enough good text? ──► return markdown
   │
   │ no (scanned / rotated / low yield)
   ▼
OCR engine (tesseract | paddle | auto)
   │
   ▼
return text
```

#### Top-level parameters

Passed under `parameters`.

| Parameter            | Type   | Default       | Description                                                                                    |
| -------------------- | ------ | ------------- | ---------------------------------------------------------------------------------------------- |
| `ocr_engine`         | string | `"tesseract"` | OCR backend: `tesseract`, `paddle`, or `auto`                                                  |
| `min_chars`          | int    | `100`         | Minimum characters pdfplumber must return before its output is accepted                        |
| `min_avg_word_len`   | float  | `3.0`         | Minimum average word length of pdfplumber output — catches rotated/garbled PDFs                |
| `ocr_page_min_chars` | int    | `50`          | `auto` only — per-page threshold below which Tesseract output is discarded and Paddle is tried |
| `dpi_retry`          | int    | `300`         | `auto` only — DPI for the high-resolution Paddle retry (`0` disables)                          |
| `extract`            | dict   | `{}`          | pdfplumber open options (e.g. `{"password": "secret"}`)                                        |
| `page`               | dict   | `{}`          | Page range and rendering options (see below)                                                   |
| `ocr`                | dict   | `{}`          | Shared OCR options (see below)                                                                 |
| `ocr_tesseract`      | dict   | `{}`          | Tesseract-specific overrides, merged on top of `ocr`                                           |
| `ocr_paddle`         | dict   | `{}`          | Paddle-specific overrides, merged on top of `ocr`                                              |
| `chunk`              | dict   | `{}`          | Optional post-extraction chunking (see Chunking)                                               |

**Engine choice**

| Value       | Description                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------- |
| `tesseract` | CPU-only, fast, good for clean single-column text. Does not handle rotated pages                         |
| `paddle`    | Better for complex layouts, tables, and non-Latin scripts. Handles rotation. CPU (CNN) or GPU (VL model) |
| `auto`      | Per-page: Tesseract → Paddle → Paddle @ high DPI. Best quality, highest cost                             |

**Page options (`parameters.page`)**

| Parameter           | Type | Default      | Description                                                          |
| ------------------- | ---- | ------------ | -------------------------------------------------------------------- |
| `first_page`        | int  | `null` (all) | First page to process, 1-indexed                                     |
| `last_page`         | int  | `null` (all) | Last page to process, inclusive                                      |
| `concatenate_pages` | bool | `true`       | Join all page outputs. When false, only the first page is returned   |
| `dpi`               | int  | `200`        | Render resolution for OCR. Higher = better quality, slower           |
| `extract_tables`    | bool | `true`       | pdfplumber only — detect and render tables as Markdown               |
| `char_threshold`    | int  | `10`         | pdfplumber only — minimum characters a page must have to be included |

**OCR options (`parameters.ocr`)**

The shared `lang` sets the document language and accepts either code style (`"eng"` or `"en"`); each engine normalises automatically. Supported languages include English, French, German, Spanish, Italian, Portuguese, Dutch, Russian, Turkish, Arabic, Chinese (Simplified/Traditional), Japanese, Korean, and Hindi.

Paddle's most useful overrides (`parameters.ocr_paddle`):

| Parameter                      | Type   | Default                       | Description                                                                 |
| ------------------------------ | ------ | ----------------------------- | --------------------------------------------------------------------------- |
| `device`                       | string | `"cpu"`                       | `cpu` or `gpu`. Determines the default backend                              |
| `use_vl_model`                 | bool   | `false` on CPU, `true` on GPU | `false` = standard PP-OCRv5 (CNN, fast on CPU); `true` = PaddleOCR-VL (GPU) |
| `use_doc_orientation_classify` | bool   | —                             | VL only — detect and correct page rotation                                  |
| `vl_rec_backend`               | string | `"local"`                     | `local` or `openai` — offload VLM inference to a remote server              |
| `vl_rec_server_url`            | string | —                             | OpenAI-compatible base URL for remote VL inference                          |

#### Engine comparison

|                   | pdfplumber | Tesseract          | Paddle (CPU) | Paddle (GPU/VL)                 |
| ----------------- | ---------- | ------------------ | ------------ | ------------------------------- |
| Digital PDFs      | Best       | —                  | —            | —                               |
| Scanned PDFs      | —          | Good               | Good         | Best                            |
| Tables            | Markdown   | Plain text         | Plain text   | Markdown                        |
| Rotated pages     | Garbled    | Fails              | Handles      | Handles                         |
| Non-Latin scripts | —          | With language pack | Yes          | Yes                             |
| Speed             | Fast       | Fast               | Medium       | Slow (local) / Network (remote) |
| GPU required      | No         | No                 | No           | Yes                             |

#### Output

| Field    | Type          | Description                                                                                  |
| -------- | ------------- | -------------------------------------------------------------------------------------------- |
| `md`     | string / null | Extracted text. `null` if no source was provided; empty string if extraction yielded nothing |
| `chunks` | list          | Only when `chunk.enabled` is true (see Chunking)                                             |

***

## Docling

Converts documents into `md` / `html` / `json` / `text` (and more) using [docling](https://github.com/docling-project/docling), with an optional layout-aware chunking step. It handles PDFs, Office files (including legacy `.doc` / `.ppt` / `.xls` via LibreOffice), images, HTML/markdown, and audio (Whisper transcription).

Two entry points share one engine:

* **DoclingProcess** — convert a document into a string, with optional post-conversion chunking.
* **ChunkerProcess** — convert and chunk in one call, using docling's own layout-aware chunker.

#### DoclingProcess parameters

Passed under `parameters`.

| Parameter   | Type   | Default           | Description                                                                                               |
| ----------- | ------ | ----------------- | --------------------------------------------------------------------------------------------------------- |
| `format`    | string | `"md"`            | Output format: `md`, `html`, `json`, `text`, `doctags`, `vtt`, `doclang`                                  |
| `asr_model` | string | `"WHISPER_TURBO"` | Whisper model for audio transcription                                                                     |
| `convert`   | dict   | `{}`              | Conversion bounds (see below)                                                                             |
| `pipeline`  | dict   | `{}`              | PDF/image pipeline overrides (see below)                                                                  |
| `vlm`       | dict   | `{}`              | Convert with a vision-language model over an OpenAI-compatible endpoint. Takes precedence over `pipeline` |
| `chunk`     | dict   | `{}`              | Optional post-conversion chunking. Requires `enabled: true`                                               |

**Convert bounds (`parameters.convert`)**

Use these to bound cost and memory on large documents.

| Parameter       | Type           | Description                                           |
| --------------- | -------------- | ----------------------------------------------------- |
| `page_range`    | `[start, end]` | 1-based page window to process                        |
| `max_num_pages` | int            | Abort cleanly if the document exceeds this many pages |
| `max_file_size` | int            | Abort cleanly if the file exceeds this many bytes     |
| `headers`       | dict           | HTTP headers for URL sources                          |

**Pipeline overrides (`parameters.pipeline`)**

Applies to PDF and image inputs.

| Parameter                 | Type  | Description                                                  |
| ------------------------- | ----- | ------------------------------------------------------------ |
| `do_ocr`                  | bool  | Run OCR (turn off for born-digital PDFs to save time/memory) |
| `do_table_structure`      | bool  | Recover table structure                                      |
| `images_scale`            | float | Render scale for page images (lower = less memory)           |
| `generate_page_images`    | bool  | Retain full-page images (memory-heavy — off by default)      |
| `table_structure_options` | dict  | e.g. `{ "mode": "accurate", "do_cell_matching": true }`      |
| `accelerator_options`     | dict  | e.g. `{ "device": "cuda", "num_threads": 8 }`                |

**VLM conversion (`parameters.vlm`)**

When present, docling converts pages with a **vision-language model** — running locally or on any OpenAI-compatible `/v1/chat/completions` endpoint (LocalAI, Ollama, vLLM) — instead of the standard layout+OCR pipeline.

| Parameter         | Type   | Default                          | Description                                                         |
| ----------------- | ------ | -------------------------------- | ------------------------------------------------------------------- |
| `url`             | string | Ollama localhost                 | Endpoint URL                                                        |
| `model`           | string | —                                | Model name                                                          |
| `headers`         | dict   | `{}`                             | Request headers, e.g. `{ "Authorization": "Bearer ..." }`           |
| `prompt`          | string | "Convert this page to markdown." | Instruction sent with each page image                               |
| `response_format` | string | `"markdown"`                     | Expected output: `markdown`, `doctags`, `html`, `otsl`, `plaintext` |
| `timeout`         | float  | `60`                             | Per-request timeout (seconds)                                       |
| `concurrency`     | int    | `1`                              | Concurrent page requests                                            |

{% hint style="warning" %}
`response_format` must match what the model actually returns. Keep `markdown` for a general OCR/markdown VLM; use `doctags` only for docling-tags-native models (e.g. Granite-Docling / SmolDocling).
{% endhint %}

#### DoclingProcess output

| Field     | Type          | Description                                                               |
| --------- | ------------- | ------------------------------------------------------------------------- |
| `format`  | string        | The format the content was exported to                                    |
| `content` | string / null | Converted document. `null` if no source was provided or conversion failed |
| `chunks`  | list          | Only when `chunk.enabled` — docling-native chunks                         |

***

## Chunking

Both the PDF (OCR) engine and Docling can split their output into retrieval-friendly **chunks** for RAG. The chunker is structure-aware: it keeps Markdown tables intact, breaks at headers (attaching a heading breadcrumb to each chunk), and preserves real page numbers.

* For **PDF (OCR)**, chunking runs on the extracted Markdown — pass options under `parameters.chunk` with `enabled: true`.
* For **Docling**, chunking uses docling's layout-aware chunker over the structured document — pass options under `parameters.chunk` (DoclingProcess), or use **ChunkerProcess** to convert and chunk in one call.

#### Markdown chunk options (PDF/OCR)

| Parameter         | Type   | Default       | Description                                        |
| ----------------- | ------ | ------------- | -------------------------------------------------- |
| `enabled`         | bool   | `false`       | Master toggle                                      |
| `strategy`        | string | `"recursive"` | `recursive`, `markdown`, `fixed`, or `page`        |
| `chunk_size`      | int    | `1000`        | Target maximum size per chunk, in `size_unit`      |
| `size_unit`       | string | `"chars"`     | `chars`, `words`, or `tokens`                      |
| `chunk_overlap`   | int    | `150`         | Overlap between consecutive chunks                 |
| `respect_headers` | bool   | `true`        | Break at headers and attach the heading breadcrumb |
| `respect_tables`  | bool   | `true`        | Never split inside a Markdown table                |
| `respect_pages`   | bool   | `false`       | Never let a chunk span more than one page          |

#### Docling chunk options

| Parameter          | Type   | Default         | Description                                                                                        |
| ------------------ | ------ | --------------- | -------------------------------------------------------------------------------------------------- |
| `enabled`          | bool   | `false`         | DoclingProcess only — master toggle (ignored by ChunkerProcess)                                    |
| `type`             | string | `"hybrid"`      | `hybrid` (token-budgeted, merges undersized peers) or `hierarchical` (one chunk per document leaf) |
| `max_tokens`       | int    | chunker default | Hybrid only — target maximum tokens per chunk                                                      |
| `merge_peers`      | bool   | `true`          | Hybrid only — merge adjacent undersized chunks sharing a heading path                              |
| `contextualize`    | bool   | `true`          | Prefix each chunk with its heading breadcrumb                                                      |
| `include_metadata` | bool   | `true`          | Emit `{text, metadata}` objects; when false, emit bare strings                                     |

#### Chunk metadata

When metadata is included, each chunk is `{ "text": …, "metadata": {…} }` with fields such as `index` (position), `headings` / `heading_path` (breadcrumb), `pages` / `page_start` / `page_end` (real source pages), `char_count`, `token_count`, and `is_table`.

***

## Choosing an engine

| Your input                                               | Use                                   |
| -------------------------------------------------------- | ------------------------------------- |
| Excel spreadsheet                                        | **XLSX**                              |
| Born-digital or scanned PDF, need fast text/tables       | **PDF (OCR)**                         |
| Office docs, images, HTML, or audio                      | **Docling**                           |
| PDF where you want layout-aware, structure-native chunks | **Docling (ChunkerProcess)**          |
| Any of the above, then split for RAG                     | Add **chunking** to the chosen engine |
