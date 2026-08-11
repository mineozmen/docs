---
description: >-
  These actions provide ability to produce formatted Word files using templates
  and input payload.
---

# Generate PowerPoint

## Generate PowerPoint Actions

### ExportPPTX

Produces a PowerPoint (`.pptx`) file from given input data. The document can be produced from an existing template (replacing text in named **shapes** or replacing `${...}` **tokens**), by generating **layout-driven slides** against the available slide layouts, or from scratch as a single title/body slide built from **text or Markdown**. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field         | Definition                                       | Example  | Default |
| ------------- | ------------------------------------------------ | -------- | ------- |
| Input Element | Json path for the input in request event payload | customer | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate PowerPoint ExportPPTX action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "inputElement": {
          "type": "string",
          "definition": "Json path for the input in request event payload",
          "example": "customer",
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
{% tab title="Table" %}
| Parameter       | Definition                                                                                                                                                                                                                                                                      | Example             | Default       |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------- |
| Output Path     | Path on the target file system to write the generated presentation to                                                                                                                                                                                                           | output/deck.pptx    | -             |
| Template Path   | Path on the target file system to read the template presentation from. If omitted, the deck is built from the input                                                                                                                                                             | templates/deck.pptx | -             |
| Template Format | How the deck is produced: `shapes` replaces shape text matched by shape name; `slides` builds one slide per spec from the available layouts; any other value replaces `${field}` tokens in slide text (or, with no template, builds a single title/body slide from `text`/`md`) | slides              | tokens / text |
| Input Pattern   | JMESPath pattern to apply on input element                                                                                                                                                                                                                                      | {slides: slides}    | -             |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Generate PowerPoint ExportPPTX action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "outputPath": {
              "type": "string",
              "definition": "Path on the target file system to write the generated presentation to",
              "example": "output/deck.pptx",
              "default": null
            },
            "templatePath": {
              "type": "string",
              "definition": "Path on the target file system to read the template presentation from; if omitted, the deck is built from the input",
              "example": "templates/deck.pptx",
              "default": null
            },
            "templateFormat": {
              "type": "string",
              "definition": "How the deck is produced: 'shapes' replaces shape text matched by shape name, 'slides' builds one slide per spec from the available layouts, any other value replaces ${field} tokens in slide text (or, with no template, builds a single title/body slide from text/md)",
              "example": "slides",
              "default": null
            },
            "inputPattern": {
              "type": "string",
              "definition": "JMESPath pattern to apply on input element",
              "example": "{slides: slides}",
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

## Content Format

The shape of the input payload depends on how the deck is produced. The mode is selected by whether a `templatePath` is given and by `templateFormat`.

#### Simple slide — no template, default format

With no `templatePath` (and `templateFormat` other than `slides`), a single title/body slide is generated. Provide a `text` **or** `md` field, plus an optional `title`. When `md` is present, its value is rendered as editable text (see the Markdown subset below); otherwise `text` is used verbatim, with newlines becoming separate paragraphs.

```json
{
    "title": "Quarterly Review",
    "md": "# Highlights\n\n- Revenue up 12%\n- Two new markets\n\n**Strong** finish to the year."
}
```

#### Shapes — `templateFormat` = `shapes`

Requires a `templatePath`. Each top-level input field maps to a shape whose **name** equals the field name; the shape's text is replaced with the field value. This is the analog of Word content controls, keyed on shape name instead of a tag. Grouped shapes are descended into, and only non-null fields are applied.

```json
{
    "customerName": "Acme Corp",
    "orderDate": "2026-01-15",
    "total": "$1,299.00"
}
```

#### Tokens — default format with a template

The default when a `templatePath` is set (any `templateFormat` other than `shapes` or `slides`). Each top-level field defines a `${field}` token that is replaced everywhere it appears in slide text. Replacement is done per paragraph, so a token split across multiple runs still matches. This is the analog of Word mail merge.

```json
{
    "customerName": "Acme Corp",
    "orderDate": "2026-01-15",
    "total": "$1,299.00"
}
```

#### Slides — `templateFormat` = `slides`

Builds one slide per spec from the available slide layouts (from the template, or from the blank package when no `templatePath` is given). New slides are appended after any existing ones. The input is either an **array** of slide specs, or an object with a `slides` array field.

```json
{
    "slides": [
        {
            "layout": "Title Slide",
            "content": { "title": "Annual Report", "subtitle": "FY 2025" }
        },
        {
            "layout": "Title and Content",
            "content": {
                "title": "Highlights",
                "text": "Revenue up 12%\nTwo new markets"
            },
            "elements": [
                {
                    "type": "image",
                    "src": "https://example.com/chart.png",
                    "x": 5, "y": 1.5, "w": 4, "h": 3
                }
            ]
        }
    ]
}
```

Each slide spec supports:

| Field    | Definition                                                                                                                                                                               |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| layout   | Layout selector: a numeric index (0-based, into layouts sorted by part name), a part-name match, or the layout's display name (e.g. `Title and Content`). Falls back to the first layout |
| content  | Object mapping keys to the layout's placeholders (see below). Newlines in a value become separate paragraphs                                                                             |
| elements | Array of freely positioned text/image boxes, appended on top of the placeholder content (see below)                                                                                      |

**`content` keys** are resolved to layout placeholders as follows: `title` → the title (or centre-title) placeholder; `subtitle` → the subtitle placeholder; `text`, `body`, or `content` → the first body placeholder. Any other key is treated as a direct placeholder-type name. The keys `layout` and `image` are ignored inside `content`.

**`elements`** is an array of freely positioned boxes. Each element requires a `type` and a position/size (`x`, `y`, `w`, `h`, with `w` and `h` greater than 0). Coordinates default to inches; set `unit` per element to `in`, `pt`, `px`, or `emu`.

| Field                                                             | Applies to | Definition                                                                            |
| ----------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------- |
| type                                                              | all        | `text`, `md`, or `image` (required)                                                   |
| x, y, w, h                                                        | all        | Position and size (numeric, required; `w`/`h` > 0)                                    |
| unit                                                              | all        | `in` (default), `pt`, `px`, or `emu`                                                  |
| name                                                              | all        | Shape name (optional)                                                                 |
| text / md                                                         | text, md   | Content; `md` enables the Markdown subset below                                       |
| fontSize                                                          | text, md   | Font size in points                                                                   |
| bold, italic, underline                                           | text, md   | Boolean run styling                                                                   |
| color                                                             | text, md   | Font colour as `#RRGGBB` (or `RRGGBB`)                                                |
| fontFamily                                                        | text, md   | Font family name                                                                      |
| align                                                             | text, md   | `left`, `center`, `right`, or `justify`                                               |
| valign                                                            | text, md   | Vertical anchor: `top`, `middle`, or `bottom`                                         |
| headingAlign, heading1FontSize … heading6FontSize, codeFontFamily | md         | Markdown-only overrides for heading alignment, per-level heading sizes, and code font |
| src                                                               | image      | Filesystem/Hadoop path, `http(s)://` URL (max 25 MB), or base64 `data:` URI           |
| base64                                                            | image      | Raw base64 image data (alternative to `src`)                                          |
| alt                                                               | image      | Alternative text / description                                                        |

#### Markdown subset

When a `md` field is supplied (simple slide, or a `text`/`md` element with `type: "md"`), a deliberately bounded subset of Markdown is translated into editable DrawingML so it stays formatted in PowerPoint:

* **Block syntax:** ATX headings (`#`–`######`) and setext headings (`===` / `---`), ordered and unordered lists (with nesting by indentation), fenced code blocks (` ``` `), paragraphs, and blank lines.
* **Inline syntax:** bold (`**` / `__`), italic (`*` / `_`), inline code (`` ` ``), backslash escapes, and hard line breaks (two trailing spaces).
