---
description: >-
  Object widgets are used to display and update nested fields (such as main
  properties of a product).
---

# Object Widgets

It is possible to extend the list and variety of object widgets, and Rierino is shipped with the following widgets:

## Localized Editor

<figure><img src="../../../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

LocalizedEditor provides ability to display and enter localized (e.g. by language, currency) values as a nested object. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property          | Definition                                                         | Example                           | Default    |
| ----------------- | ------------------------------------------------------------------ | --------------------------------- | ---------- |
| Locales Source    | Name of the options list used for populating locales               | language                          | -          |
| Locales List      | Id-name pairs of options to list                                   | \[{"id":"en", "name": "English"}] | -          |
| Filter Values     | List of options to allow for locale selection                      | \[en, de]                         | -          |
| Filter Value Path | Json path for applying filter on list of locale options to display | data.regions                      | -          |
| Widget            | Widget to use for displaying locale specific data                  | ChipArrayEditor                   | TextEditor |
| Widget Properties | Properties to pass on to localized input component                 | {"dom":{"multiline":true\}}       | -          |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "LocalizedEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "options": {
          "type": "string",
          "definition": "Name of the options list used for populating locales",
          "example": "language"
        },
        "optionValues": {
          "type": "array",
          "definition": "Id-name pairs of options to list",
          "items": {
            "type": "object",
            "properties": {
              "id": { "type": "string", "example": "en" },
              "name": { "type": "string", "example": "English" }
            }
          },
          "example": [{ "id": "en", "name": "English" }]
        },
        "filterValues": {
          "type": "array",
          "definition": "List of options to allow for locale selection",
          "items": { "type": "string" },
          "example": ["en", "de"]
        },
        "filterValuePath": {
          "type": "string",
          "definition": "Json path for applying filter on list of locale options to display",
          "example": "data.regions"
        }
      }
    },
    "valueProps": {
      "type": "object",
      "definition": "Props passed to the localized input component.",
      "properties": {
        "widget": {
          "type": "string",
          "definition": "Widget to use for displaying locale specific data",
          "example": "ChipArrayEditor",
          "default": "TextEditor"
        }
      },
      "example": {
        "widget": "ChipArrayEditor",
        "dom": { "multiline": true }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Json Editor

<figure><img src="../../../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

JsonEditor provides ultimate flexibility to display and edit complex data structures with a structured UI component and free text combination. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property     | Definition                                                                 | Example | Default |
| ------------ | -------------------------------------------------------------------------- | ------- | ------- |
| Use Switch   | Whether json/text editors should be switched between or presented together | true    | false   |
| Default Mode | Default mode to display if switch is enabled                               | code    | tree    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "JsonEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "switch": {
          "type": "boolean",
          "definition": "Whether json/text editors should be switched between or presented together",
          "example": true,
          "default": false
        },
        "mode": {
          "type": "string",
          "definition": "Default mode to display if switch is enabled",
          "example": "code",
          "default": "tree"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Map Editor

MapEditor provides ability to modify map type objects with a table of key-value pairs. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property                       | Definition                                                                                   | Example                               | Default |
| ------------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------- | ------- |
| Key UI (Widget & Properties)   | UI component configuration for the key column                                                | {"widget": "TextEditor", "props":{\}} | -       |
| Predefined Key Options         | Whether editor should have a predefined list of key entries (uses keyUi options as the list) | true                                  | false   |
| Value UI (Widget & Properties) | UI component configuration for the value column                                              | {"widget": "TextEditor", "props":{\}} | -       |
| Flatten Objects                | Whether editor should flatten and edit nested objects as well                                | true                                  | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MapEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "keyUi": {
          "type": "object",
          "definition": "UI component configuration for the key column",
          "properties": {
            "widget": { "type": "string", "example": "TextEditor" },
            "props": { "type": "object", "example": {} },
            "preset": {
              "type": "boolean",
              "definition": "Whether editor should have a predefined list of key entries (uses keyUi options as the list)",
              "example": true,
              "default": false
            }
          },
          "example": { "widget": "TextEditor", "props": {} }
        },
        "valueUi": {
          "type": "object",
          "definition": "UI component configuration for the value column",
          "properties": {
            "widget": { "type": "string", "example": "TextEditor" },
            "props": { "type": "object", "example": {} }
          },
          "example": { "widget": "TextEditor", "props": {} }
        },
        "flatten": {
          "type": "boolean",
          "definition": "Whether editor should flatten and edit nested objects as well",
          "example": true,
          "default": false
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Map Object Editor

MapObjectEditor provides ability to modify map type objects with a table of key-value pairs where the value is an object. This widget uses the same properties as [Object Table Editor](array-widgets.md#object-table-editor), as well as the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property                           | Definition                                                                                   | Example                               | Default |
| ---------------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------- | ------- |
| Key UI (Widget & Properties)       | UI component configuration for the key column                                                | {"widget": "TextEditor", "props":{\}} | -       |
| Predefined Key Options             | Whether editor should have a predefined list of key entries (uses keyUi options as the list) | true                                  | false   |
| Table Fields                       | List of fields to display on map table                                                       | -                                     | -       |
| Editor UI (Icon, Title & Contents) | Design of object editor to use for editing                                                   | -                                     | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MapObjectEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "keyUi": {
          "type": "object",
          "definition": "UI component configuration for the key column",
          "properties": {
            "widget": { "type": "string", "example": "TextEditor" },
            "props": { "type": "object", "example": {} },
            "preset": {
              "type": "boolean",
              "definition": "Whether editor should have a predefined list of key entries (uses keyUi options as the list)",
              "example": true,
              "default": false
            }
          },
          "example": { "widget": "TextEditor", "props": {} }
        },
        "uiDesign": {
          "type": "object",
          "definition": "UI component configuration for value editing"
        },
        "tableFields": {
          "type": "array",
          "defintion": "List of fields to display for value objects"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Contents Editor

ContentsEditor provides ability to display and edit multiple data elements. This editor only displays contents within a single grid, without any tabs or additional layouts applied. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property | Definition                                                                                 | Example                                    | Default |
| -------- | ------------------------------------------------------------------------------------------ | ------------------------------------------ | ------- |
| Contents | Complete contents mapping object data paths to widgets and props, similar to grid contents | \[{"path":"name", "widget": "TextEditor"}] | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ContentsEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "contents": {
          "type": "array",
          "definition": "Complete contents mapping object data paths to widgets and props, similar to grid contents",
          "items": {
            "type": "object",
            "properties": {
              "path": { "type": "string", "example": "name" },
              "widget": { "type": "string", "example": "TextEditor" }
            }
          },
          "example": [{ "path": "name", "widget": "TextEditor" }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Grid Contents Editor

GridsEditor provides ability to display and edit multiple data elements. This editor displays contents within multiple grids, but without any tabs. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property | Definition                                                    | Example                                                    | Default |
| -------- | ------------------------------------------------------------- | ---------------------------------------------------------- | ------- |
| Grids    | Complete grids mapping object data paths to widgets and props | \[{"contents": \[{"path":"name", "widget": "TextEditor"}]] | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "GridsEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "grids": {
          "type": "array",
          "definition": "Complete grids mapping object data paths to widgets and props",
          "items": {
            "type": "object",
            "properties": {
              "contents": {
                "type": "array",
                "items": {
                  "type": "object",
                  "properties": {
                    "path": { "type": "string", "example": "name" },
                    "widget": { "type": "string", "example": "TextEditor" }
                  }
                },
                "example": [{ "path": "name", "widget": "TextEditor" }]
              }
            }
          },
          "example": [{ "contents": [{ "path": "name", "widget": "TextEditor" }] }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Schema Editor

SchemaEditor provides ability to view and edit JSON schema contents for data modeling use cases. This widget has no extra properties.

## Media Metadata Editor

ContentsEditor displays an image or video with metadata fields editable, represented by its relative URL path. This widget has the following special properties in addition to those listed in [Media Editor](value-widgets.md#media-editor):

{% tabs %}
{% tab title="Properties" %}
| Property                           | Definition                                                                | Example                          | Default |
| ---------------------------------- | ------------------------------------------------------------------------- | -------------------------------- | ------- |
| Editor UI (Icon, Title & Contents) | Complete editor UI configurations for detail contents                     | {"tabs":\[{"tab":"DEFINITION"}]} | -       |
| Value DOM                          | Additional dom properties to pass on to div encapsulating the image       | -                                | -       |
| Input DOM                          | Additional dom properties to pass on to TextField for entering image path | -                                | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaMetadataEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "uiDesign": {
          "type": "object",
          "definition": "Complete editor UI configurations for detail contents",
          "example": { "tabs": [{ "tab": "DEFINITION" }] }
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "definition": "Additional dom properties to pass on to div encapsulating the image"
        }
      }
    },
    "inputProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "definition": "Additional dom properties to pass on to TextField for entering image path"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Dynamic Editor

DynamicParameterEditor provides ability to use a dynamic UI, where displayed widgets and data depend on a specific data value (e.g. parameters required for a business rule). This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property    | Definition                                                                          | Example                             | Default |
| ----------- | ----------------------------------------------------------------------------------- | ----------------------------------- | ------- |
| Source      | Name of data source mapping to use for fetching dynamic UI configuration            | rule\_basket\_promotion             | -       |
| Source Path | Json path in source data which specifies UI details                                 | data.parameters.conditionParameters | -       |
| Id Path     | Json path in current record, which is used for selecting dynamic UI entry in source | data.rule                           | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DynamicParameterEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "source": {
          "type": "string",
          "definition": "Name of data source mapping to use for fetching dynamic UI configuration",
          "example": "rule_basket_promotion"
        },
        "sourcePath": {
          "type": "string",
          "definition": "Json path in source data which specifies UI details",
          "example": "data.parameters.conditionParameters"
        },
        "idPath": {
          "type": "string",
          "definition": "Json path in current record, which is used for selecting dynamic UI entry in source",
          "example": "data.rule"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Dynamic Object Editor

DynamicObjectEditor provides ability to embed and display data in the form of another ObjectEditor. It is typically used for displaying data input by another user for approval. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property          | Definition                  | Example   | Default |
| ----------------- | --------------------------- | --------- | ------- |
| UI Reference Path | Json path of editor id      | data.type | -       |
| UI Reference      | Static id for the UI editor | product   | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DynamicObjectEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "refPath": {
          "type": "string",
          "definition": "Json path of editor id",
          "example": "data.type"
        },
        "ref": {
          "type": "string",
          "definition": "Static id for the UI editor",
          "example": "product"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Handlebars Display

HandlebarsDisplay provides ability to render read-only Handlebars templates using UI data, typically used for preview functionality or static messages. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property         | Definition                                                                  | Example                                                        | Default |
| ---------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------- | ------- |
| Template Code    | Full static template text to use                                            | <p>&#x3C;span></p><p>Testing</p><p>&#x3C;/span></p>            | -       |
| Template Source  | Source to use dynamic templates from                                        | preview\_template                                              | -       |
| Template Id      | Static id for selecting template from the source                            | test-template                                                  | -       |
| Template Id Path | Json path of dynamic id for selecting template from the source              | data.templateId                                                | -       |
| Source Path      | Json path of template text in source records                                | data.content                                                   | -       |
| Data Lookups     | List of data sources to pass on to template as lookups                      | {"product-lkp": { source: "product", idPath: "data.product"\}} | -       |
| As iframe        | Whether widget should be rendered as an iframe (or div)                     | true                                                           | false   |
| Disable Print    | Whether widget should allow disable printing                                | true                                                           | false   |
| Editor Source    | Source UI for editor references (in case template is editable with pop-ups) | page\_component                                                | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "HandlebarsDisplay widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "template": {
          "type": "string",
          "definition": "Full static template text to use",
          "example": "<p>&#x3C;span></p><p>Testing</p><p>&#x3C;/span></p>"
        },
        "source": {
          "type": "string",
          "definition": "Source to use dynamic templates from",
          "example": "preview_template"
        },
        "sourceId": {
          "type": "string",
          "definition": "Static id for selecting template from the source",
          "example": "test-template"
        },
        "idPath": {
          "type": "string",
          "definition": "Json path of dynamic id for selecting template from the source",
          "example": "data.templateId"
        },
        "sourcePath": {
          "type": "string",
          "definition": "Json path of template text in source records",
          "example": "data.content"
        },
        "lookups": {
          "type": "object",
          "definition": "List of data sources to pass on to template as lookups",
          "example": { "product-lkp": { "source": "product", "idPath": "data.product" } }
        },
        "asIframe": {
          "type": "boolean",
          "definition": "Whether widget should be rendered as an iframe (or div)",
          "example": true,
          "default": false
        },
        "noPrint": {
          "type": "boolean",
          "definition": "Whether widget should allow disable printing",
          "example": true,
          "default": false
        },
        "editorSource": {
          "type": "string",
          "definition": "Source UI for editor references (in case template is editable with pop-ups)",
          "example": "page_component"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
When not structured as iframe, handlebars displays use shadow #root to isolate its styling, so it will be subject to shadow dom constraints (e.g. not supporting @font-face)
{% endhint %}

{% hint style="info" %}
To make a handlebars display editable with pop-ups, define props.editorSource and add "data-path", "data-editor", "data-action" attributes to rendered html tags. For "add" type data actions used on arrays, it is also possible to use "data-default" and "data-default-pattern" attributes for adding new records with default data values.
{% endhint %}

{% hint style="info" %}
To add drag & drop features to handlebars displays, "data-drag-value", "data-drag-path", "data-drop-value", "data-drop-event", "data-drop-params", "data-drop-path" attributes can be added to rendered html tags.
{% endhint %}

## Handlebars Builder

HandlebarsTemplateEditor is a drag & drop UI builder for creating Handlebars templates and static HTML contents. This widget stores generated contents as "markup" as well as platform independent structured elements.

## TipTap Editor

<figure><img src="../../../../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>

TipTapEditor is a WYSIWYG editor, allowing editing and visualization of rich content, which produces JSON output in { type, content, html } form that can be used for platform independent UI generation or direct HTML injection. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property  | Definition                                                                 | Example | Default |
| --------- | -------------------------------------------------------------------------- | ------- | ------- |
| Text Data | Whether to use/generate text only data or structured (object) data instead | true    | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TipTapEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "text": {
          "type": "boolean",
          "definition": "Whether to use/generate text only data or structured (object) data instead",
          "example": true,
          "default": false
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## MDX Editor

MDXEditor is a template based editor, allowing building interactive custom content, using a blend of JSX and markdown. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property      | Definition                           | Example                                                                  | Default |
| ------------- | ------------------------------------ | ------------------------------------------------------------------------ | ------- |
| Template Code | MDX code to display & edit data with | <p>Hello World<br>&#x3C;TextEditor data={data} onChange={onChange}/></p> | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MDXEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "template": {
          "type": "string",
          "definition": "MDX code to display & edit data with",
          "example": "<p>Hello World<br>&#x3C;TextEditor data={data} onChange={onChange}/></p>"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
All props of the MDXEditor are passed to its MDX code, along with all registered widgets to allow using them as components and an onChange event to pass changes inside the editor.
{% endhint %}

{% embed url="https://nextjs.org/docs/app/guides/mdx" %}

## Mermaid Display

MermaidDisplay is a visualization editor, allowing creating Mermaid diagrams using its special markdown language. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property         | Definition                                                   | Example                                                            | Default |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------------ | ------- |
| Diagram Template | Handlebars template used to produce mermaid diagram contents | `<p>flowchart TD<br>A[Start] --> B{Decision}<br>B --> C[Next]</p>` | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MermaidDisplay widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "template": {
          "type": "string",
          "definition": "Handlebars template used to produce mermaid diagram contents",
          "example": "<p>flowchart TD<br>A[Start] --> B{Decision}<br>B --> C[Next]</p>"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If props.template is not provided, this display can use full text contents as an input instead.
{% endhint %}

{% embed url="https://mermaid.js.org/intro/syntax-reference.html" %}

## 3D Model Display

ThreeSceneEditor is a visualization editor, allowing display of 3D models with visual parameters such as color overridden by dynamic data inputs. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property          | Definition                                    | Example                       | Default |
| ----------------- | --------------------------------------------- | ----------------------------- | ------- |
| Model File URL    | URL for glb file which defines the 3D model   | https://example.com/model.glb | -       |
| Height            | Height of the model as an editor              | 100                           | -       |
| Show Toolbar      | Whether model editor should display toolbar   | true                          | false   |
| Show Inspector    | Whether model editor should display inspector | true                          | false   |
| Show Grid         | Whether model editor should display grid      | true                          | false   |
| Canvas Props      | Properties to pass to 3D model canvas         | -                             | -       |
| Grid Props        | Properties to pass to 3D model grid           | -                             | -       |
| Orbit Controls    | Properties to pass to 3D model controls       | -                             | -       |
| Ambient Light     | Properties to pass to 3D model light          | -                             | -       |
| Directional Light | Properties to pass to 3D model light          | -                             | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ThreeSceneEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string",
          "definition": "URL for glb file which defines the 3D model",
          "example": "https://example.com/model.glb"
        },
        "height": {
          "type": "integer",
          "definition": "Height of the model as an editor",
          "example": 100
        },
        "showToolbar": {
          "type": "boolean",
          "definition": "Whether model editor should display toolbar",
          "example": true,
          "default": false
        },
        "showInspector": {
          "type": "boolean",
          "definition": "Whether model editor should display inspector",
          "example": true,
          "default": false
        },
        "showGrid": {
          "type": "boolean",
          "definition": "Whether model editor should display grid",
          "example": true,
          "default": false
        },
        "canvas": {
          "type": "object",
          "definition": "Properties to pass to 3D model canvas"
        },
        "grid": {
          "type": "object",
          "definition": "Properties to pass to 3D model grid"
        },
        "orbitControls": {
          "type": "object",
          "definition": "Properties to pass to 3D model controls"
        },
        "ambientLight": {
          "type": "object",
          "definition": "Properties to pass to 3D model light"
        },
        "directionalLight": {
          "type": "object",
          "definition": "Properties to pass to 3D model light"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Whiteboard Editor

WhiteboardEditor is a special component allowing drawing of free format images and diagrams, storing them as data inside current record for future reference. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property       | Definition                                            | Example | Default    |
| -------------- | ----------------------------------------------------- | ------- | ---------- |
| Editor Height  | Height of the editor                                  | 100     | -          |
| Hide Toolbar   | Whether toolbar for editing contents should be hidden | true    | false      |
| File Name Base | Name to use for exporting images as files             | project | whiteboard |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "WhiteboardEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "hideToolbar": {
          "type": "boolean",
          "definition": "Whether toolbar for editing contents should be hidden",
          "example": true,
          "default": false
        },
        "height": {
          "type": "integer",
          "definition": "Height of the editor",
          "example": 100
        },
        "fileNameBase": {
          "type": "string",
          "definition": "Name to use for exporting images as files",
          "example": "project",
          "default": "whiteboard"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## PDF Display

ObjectPdfEditor is a component for rendering a template with dynamic data contents as an embedded PDF file. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property                | Definition                                                | Example                       | Default |
| ----------------------- | --------------------------------------------------------- | ----------------------------- | ------- |
| PDF Predefined Template | Existing JSX template stored in handler codes             | invoice\_template             | -       |
| PDF Template            | Full JSX text contents of the PDF template                | \<Page>\</Page>               | -       |
| PDF Styles              | Named CSS styles to apply for different template sections | {"main": {"color": "black"\}} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ButtonAction widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "templateId": {
          "type": "string",
          "definition": "Existing JSX template stored in handler codes"
        },
        "templateText": {
          "type": "string",
          "definition": "Full JSX text contents of the PDF template"
        },
        "templateStyle": {
          "type": "object",
          "definition": "Named CSS styles to apply for different template sections"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Button Action

ButtonAction is a specialized component, which allows triggering one or more actions from within a UI using buttons. Button actions receive data assigned to this widget. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property           | Definition                                                                                | Example                                           | Default |
| ------------------ | ----------------------------------------------------------------------------------------- | ------------------------------------------------- | ------- |
| Buttons            | List of buttons to display with UI details                                                | \[{title: "Accept", icon: "ok", type: "CallAPI"}] | -       |
| Read Only Behavior | Action behavior when widget is set to be read-only (i.e. snapshot) (hide, display, click) | click                                             | display |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ButtonAction widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "buttons": {
          "type": "array",
          "definition": "List of buttons to display with UI details",
          "items": {
            "type": "object",
            "properties": {
              "title": { "type": "string", "example": "Accept" },
              "icon": { "type": "string", "example": "ok" },
              "type": { "type": "string", "example": "CallAPI" }
            }
          },
          "example": [{ "title": "Accept", "icon": "ok", "type": "CallAPI" }]
        },
        "readOnlyBehavior": {
          "type": "string",
          "definition": "Action behavior when widget is set to be read-only (i.e. snapshot) (hide, display, click)",
          "example": "click",
          "default": "display"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## File Manager

FileManagerEditor provides ability to manage a file system, by listing, creating and deleting files and folders. This widget supports all non-media specific properties listed in [Media Editor](value-widgets.md#media-editor).

## Data Visualization Display

DVDisplay provides ability to display data visualization components such as charts inside object editors, typically using an array input. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property             | Definition                                       | Example   | Default |
| -------------------- | ------------------------------------------------ | --------- | ------- |
| Component Type       | Visualization component type                     | RPlotly   | -       |
| Component Properties | Visualization component type specific properties | width=200 | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DVDisplay widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "definition": "Visualization component options. Component-specific options depend on props.type.",
      "properties": {
        "type": {
          "type": "string",
          "definition": "Visualization component type",
          "example": "RPlotly"
        }
      },
      "example": {
        "type": "RPlotly",
        "width": 200
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Data Visualization Editor

DVLayoutEditor provides ability to design data visualization layouts, used for embedded dashboards and reports. This widget has no extra properties, as it creates and edits data inline with data visualization data format.

## Condition Editor

ConditionEditor is a special component for editing conditional logic for business rules and queries through a user friendly interface. This widget has the following special properties:

{% tabs %}
{% tab title="Properties" %}
| Property | Definition                                                                      | Example           | Default |
| -------- | ------------------------------------------------------------------------------- | ----------------- | ------- |
| Domain   | Rule domain (for loading available functions, if editor is for the rule engine) | product\_discount | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ConditionEditor widget options",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Rule domain (for loading available functions, if editor is for the rule engine)",
          "example": "product_discount"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}
