---
description: Atom widgets are used to display data without editing capabilities.
---

# Atom Widgets

It is possible to extend the list and variety of atom widgets, and Rierino is shipped with the following widgets:

## Text Atom

TextAtom displays text using a text tag such as \<p> or \<span>. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property       | Definition                               | Example                      | Default |
| -------------- | ---------------------------------------- | ---------------------------- | ------- |
| Tag            | Tag to use (p, span, h1..h6, blockquote) | h1                           | p       |
| DOM Attributes | Attributes to apply to text tag          | {"className": "custom-text"} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TextAtom widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "tag": {
          "type": "string",
          "definition": "Tag to use (p, span, h1..h6, blockquote)",
          "example": "h1",
          "default": "p",
          "enum": ["p", "span", "h1", "h2", "h3", "h4", "h5", "h6", "blockquote"]
        },
        "dom": {
          "type": "object",
          "definition": "Attributes to apply to text tag",
          "example": { "className": "custom-text" },
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Image Atom

ImageAtom displays an image using \<img> tag. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property       | Definition                                            | Example                       | Default |
| -------------- | ----------------------------------------------------- | ----------------------------- | ------- |
| Base URL       | Base url for image to be used as a prefix to its data | http://cdn.example.com        | -       |
| DOM Attributes | Attributes to apply to image tag                      | {"className": "Custom-image"} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ImageAtom widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "baseUrl": {
          "type": "string",
          "definition": "Base url for image to be used as a prefix to its data",
          "example": "http://cdn.example.com",
          "default": null
        },
        "dom": {
          "type": "object",
          "definition": "Attributes to apply to image tag",
          "example": { "className": "Custom-image" },
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## View Atom

ViewAtom is a wrapper, which acts similar to ContentsEditor, displaying all its content widgets. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property       | Definition                                                       | Example                                  | Default |
| -------------- | ---------------------------------------------------------------- | ---------------------------------------- | ------- |
| Tag            | Tag to use (div, main, article, section, header, void)           | main                                     | div     |
| DOM Attributes | Attributes to apply to view tag                                  | {"className": "Custom-image"}            | -       |
| Contents       | Complete contents mapping object data paths to widgets and props | \[{"path":"name", "widget": "TextAtom"}] | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ViewAtom widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "tag": {
          "type": "string",
          "definition": "Tag to use (div, main, article, section, header, void)",
          "example": "main",
          "default": "div",
          "enum": ["div", "main", "article", "section", "header", "void"]
        },
        "dom": {
          "type": "object",
          "definition": "Attributes to apply to view tag",
          "example": { "className": "Custom-image" },
          "default": null
        },
        "contents": {
          "type": "array",
          "definition": "Complete contents mapping object data paths to widgets and props",
          "default": null,
          "items": {
            "type": "object",
            "properties": {
              "path": {
                "type": "string",
                "definition": "Data path to render",
                "example": "name",
                "default": null
              },
              "widget": {
                "type": "string",
                "definition": "Widget to render for the given path",
                "example": "TextAtom",
                "default": null
              },
              "props": {
                "type": "object",
                "definition": "Widget specific properties for this content entry",
                "example": {},
                "default": null
              }
            }
          },
          "example": [{ "path": "name", "widget": "TextAtom" }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Link Atom

LinkAtom is a wrapper, similar to ViewAtom, but using \<a> tag with a url reference and a single content entry. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property       | Definition                                                                 | Example                               | Default |
| -------------- | -------------------------------------------------------------------------- | ------------------------------------- | ------- |
| URL Link       | Url link (uses data value if not provided)                                 | http://example.com                    | -       |
| Text           | Text to display as link (uses data value or props.content if not provided) | Click here                            | -       |
| DOM Attributes | Attributes to apply to link tag                                            | {"className": "Custom-image"}         | -       |
| Content        | Content details mapping object data paths to widgets and props             | {"path":"name", "widget": "TextAtom"} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LinkAtom widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "definition": "Url link (uses data value if not provided)",
          "example": "http://example.com",
          "default": null
        },
        "text": {
          "type": "string",
          "definition": "Text to display as link (uses data value or props.content if not provided)",
          "example": "Click here",
          "default": null
        },
        "dom": {
          "type": "object",
          "definition": "Attributes to apply to link tag",
          "example": { "className": "Custom-image" },
          "default": null
        },
        "content": {
          "type": "object",
          "definition": "Content details mapping object data paths to widgets and props",
          "default": null,
          "properties": {
            "path": {
              "type": "string",
              "definition": "Data path to render",
              "example": "name",
              "default": null
            },
            "widget": {
              "type": "string",
              "definition": "Widget to render for the given path",
              "example": "TextAtom",
              "default": null
            },
            "props": {
              "type": "object",
              "definition": "Widget specific properties for this content entry",
              "example": {},
              "default": null
            }
          },
          "example": { "path": "name", "widget": "TextAtom" }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## HTML Atom

HtmlAtom displays its data as raw HTML inside a `<div>` tag.

{% tabs %}
{% tab title="Table" %}
| Property       | Configuration path | Definition                                   | Example                | Default |
| -------------- | ------------------ | -------------------------------------------- | ---------------------- | ------- |
| DOM Attributes | `props.dom`        | Attributes applied to the containing `<div>` | `{"className":"html"}` | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "HtmlAtom widget properties",
  "type": "object",
  "properties": { "props": { "type": "object", "properties": { "dom": { "type": "object", "description": "Attributes applied to the containing div" } } } }
}
```
{% endtab %}
{% endtabs %}

## Remote Editor Atom

RemoteEditorAtom renders hosted editor code provided as its data. See [Hosted Widgets](hosted-widgets.md#hosted-widgets).

{% tabs %}
{% tab title="Table" %}
No widget-specific properties.
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RemoteEditorAtom widget properties",
  "type": "object",
  "properties": {}
}
```
{% endtab %}
{% endtabs %}

## Repeat Atom

RepeatAtom is a wrapper, which repeats its content widget for its data array. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Configuration path | Definition                                                          | Example                               | Default |
| -------- | ------------------ | ------------------------------------------------------------------- | ------------------------------------- | ------- |
| Content  | `props.content`    | Content details mapping array entry data paths to widgets and props | `{"path":"name","widget":"TextAtom"}` | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RepeatAtom widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "content": {
          "type": "object",
          "definition": "Content details mapping array entry data paths to widgets and props",
          "default": null,
          "properties": {
            "path": {
              "type": "string",
              "definition": "Data path (within each array entry) to render",
              "example": "name",
              "default": null
            },
            "widget": {
              "type": "string",
              "definition": "Widget to render for the given path",
              "example": "TextAtom",
              "default": null
            },
            "props": {
              "type": "object",
              "definition": "Widget specific properties for this content entry",
              "example": {},
              "default": null
            }
          },
          "example": { "path": "name", "widget": "TextAtom" }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
