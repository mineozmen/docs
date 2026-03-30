---
description: >-
  Value widgets are used to display and update single value fields (such as
  text, number, boolean).
---

# Value Widgets

It is possible to extend the list and variety of value widgets, and Rierino is shipped with the following widgets:

## Text Widgets

### Text Editor

<figure><img src="../../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

TextEditor provides ability to enter free text data. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property     | Definition                                          | Example | Default |
| ------------ | --------------------------------------------------- | ------- | ------- |
| Value Type   | Type of value this widget should display and return | integer | text    |
| Is Multiline | Whether text should be displayed as multiple lines  | true    | false   |
| Rows         | Number of rows to display (if multiline)            | 5       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TextEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "valueType": {
          "type": "string",
          "description": "Type of value this widget should display and return",
          "default": "text",
          "examples": ["number"]
        },
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to TextField component. Keys depend on the underlying UI component.",
          "default": null,
          "examples": [{ "multiline": true, "rows": 5 }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Code Editor

<figure><img src="../../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

CodeEditor provides ability to enter multi-line code with syntax highlighting and autocomplete in various languages (such as java, javascript, python) using Ace editor. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property              | Definition                              | Example | Default    |
| --------------------- | --------------------------------------- | ------- | ---------- |
| Language              | Programming language to use for syntax  | java    | javascript |
| Theme                 | Color theme for the editor              | -       | -          |
| Max Lines             | Maximum lines to display                | 100     | -          |
| Min Lines             | Minimum lines to display                | 10      | -          |
| Cursor Start          | Starting position for the cursor        | -       | -          |
| Tab Size              | Characters for tab                      | 4       | -          |
| Enable Wrap           | Whether word wrapping should be allowed | true    | false      |
| Show Gutter           | Whether gutter should be displayed      | false   | true       |
| Show Print Margin     | Whether margin should be displayed      | false   | true       |
| Highlight Active Line | Whether active line should be colored   | false   | true       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CodeEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "language": {
          "type": "string",
          "description": "Programming language to use for syntax",
          "default": "javascript",
          "examples": ["java"]
        },
        "syntaxThreshold": {
          "type": "integer",
          "description": "Maximum text length to allow for syntax processing",
          "default": 50000,
          "examples": [100000]
        },
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Ace component. Keys depend on the underlying UI component.",
          "default": null,
          "examples": [{
        		"highlightActiveLine": false,
        		"showGutter": false,
        		"showPrintMargin": false,
        		"wrapEnabled": true,
        		"theme": "dark",
        		"maxLines": 100,
        		"minLines": 10,
        		"cursorStart": 0,
        		"tabSize": 4
        	}]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Markdown Editor

<figure><img src="../../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

MarkdownEditor provides ability to enter and preview contents using Markdown language, which is stored as text. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Definition                                         | Example | Default |
| -------- | -------------------------------------------------- | ------- | ------- |
| DOM      | Dom properties to pass on to wrapper div component | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MarkdownEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Dom properties to pass on to wrapper div component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://en.wikipedia.org/wiki/Markdown" %}
Markdown Language
{% endembed %}

### Label Editor

LabelEditor provides ability to flat values with a TextField. This widget uses the same properties as TextEditor.

### Value Display

ValueDisplay provides ability to display static values using Typography. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Definition                                                   | Example | Default |
| -------- | ------------------------------------------------------------ | ------- | ------- |
| DOM      | Additional dom properties to pass on to Typography component | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ValueDisplay widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Typography component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Delimited Text Editor

TextToArrayEditor provides ability to edit delimited test fields as array elements (ensuring automated and correct application of delimiters). This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property  | Definition                               | Example | Default |
| --------- | ---------------------------------------- | ------- | ------- |
| Delimiter | Delimiter to use for joining text values | ;       | ,       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TextToArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "delimiter": {
          "type": "string",
          "description": "Delimiter to use for joining text values",
          "default": ",",
          "examples": [";"]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Selection Widgets

### Select Editor

<figure><img src="../../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

SelectEditor provides ability to select from a predefined list of options (either by name or as an actual list). If options have "color" attributes defined, they are also displayed with the given color. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property       | Definition                                             | Example                   | Default |
| -------------- | ------------------------------------------------------ | ------------------------- | ------- |
| Options Source | Name of the options list used for selection            | status                    | -       |
| Options List   | Actual contents of the options list used for selection | \[{"id":"A"}, {"id":"I"}] | -       |
| Placeholder    | Text to display as placeholder                         | Select option             | -       |
| Clear Option   | Text to display for setting selected value as null     | None                      | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SelectEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "options": {
          "type": "string",
          "description": "Name of the options list used for selection",
          "default": null,
          "examples": ["status"]
        },
        "optionValues": {
          "type": "array",
          "description": "Actual contents of the options list used for selection",
          "default": null,
          "items": {
            "type": "object",
            "description": "A single entry in the options list. Structure depends on option usage.",
            "properties": {
              "id": { "type": "string", "description": "Option id", "default": null, "examples": ["A"] },
              "name": { "type": "string", "description": "Option label", "default": null },
              "color": { "type": "string", "description": "Optional color", "default": null }
            }
          },
          "examples": [[{ "id": "A" }, { "id": "I" }]]
        },
        "placeholder": {
          "type": "string",
          "description": "Text to display as placeholder",
          "default": null,
          "examples": ["Select option"]
        },
        "clearLabel": {
          "type": "string",
          "description": "Text to display for setting selected value as null",
          "default": null,
          "examples": ["None"]
        },
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Select component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Autocomplete Editor

AutoCompleteTextEditor provides a list of options to choose from, which is filtered with autocomplete feature. It is also possible to enter a value not included in the list of options to this widget. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property       | Definition                                                | Example        | Default |
| -------------- | --------------------------------------------------------- | -------------- | ------- |
| Options Source | Name of the options list used for autocomplete            | conditionClass | -       |
| Options List   | Actual contents of the options list used for autocomplete | -              | -       |
| Value Type     | Type of value this widget should display and return       | integer        | text    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "AutoCompleteTextEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "options": {
          "type": "string",
          "description": "Name of the options list used for autocomplete",
          "default": null,
          "examples": ["conditionClass"]
        },
        "optionValues": {
          "type": "array",
          "description": "Actual contents of the options list used for autocomplete",
          "default": null,
          "items": {
            "type": "object",
            "description": "A single entry in the options list. Structure depends on option usage."
          }
        },
        "valueType": {
          "type": "string",
          "description": "Type of value this widget should display and return",
          "default": "text",
          "examples": ["integer"]
        }
      }
    },
    "inputProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Autocomplete component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Box Select Editor

BoxSelectEditor provides ability to select from a predefined list of options all displayed with their icons and name on screen as grid items. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property       | Definition                                             | Example        | Default |
| -------------- | ------------------------------------------------------ | -------------- | ------- |
| Options Source | Name of the options list used for selection            | conditionClass | -       |
| Options List   | Actual contents of the options list used for selection | \[{"id":"1"}]  | -       |
| Size           | Grid size of each displayed entry                      | 3              | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "BoxSelectEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "options": {
          "type": "string",
          "description": "Name of the options list used for selection",
          "default": null,
          "examples": ["conditionClass"]
        },
        "optionValues": {
          "type": "array",
          "description": "Actual contents of the options list used for selection",
          "default": null,
          "items": {
            "type": "object",
            "description": "A single entry in the options list. Structure depends on option usage."
          },
          "examples": [[{ "id": "1" }]]
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "size": {
          "type": "integer",
          "description": "Grid size of each displayed entry",
          "default": null,
          "examples": [3]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Item Editor

ItemEditor provides ability to select id of a lookup record (such as attribute, category) from an AutoComplete drop down. This widget can load full list of records or apply a given set of filters as well. It has the same properties as [multi item editor](array-widgets.md#multi-item-editor).

### Item Search Editor

ItemSearchEditor provides ability to select id of a lookup record (such as product) from a searchable table of values. This widget allows filtering results and should be preferred for selecting among large number of records. It has the same properties as [multi item search editor](array-widgets.md#multi-item-search-editor).

### Schema Select Editor

SchemaSelectEditor provides ability to select path of data elements from a json schema, typically used in UI design interface. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property    | Definition                                                                       | Example                 | Default |
| ----------- | -------------------------------------------------------------------------------- | ----------------------- | ------- |
| Schema      | ID of the json schema to use (for already created schemas)                       | product                 | -       |
| Schema Path | Json path for selecting ID of the json schema to use from current item or object | data.schema             | -       |
| Schema Spec | Json schema specification to use                                                 | {"id": {"title":"ID"\}} | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SchemaSelectEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "schemaId": {
          "type": "string",
          "description": "ID of the json schema to use (for already created schemas)",
          "default": null,
          "examples": ["product"]
        },
        "schemaIdPath": {
          "type": "string",
          "description": "Json path for selecting ID of the json schema to use from current item or object",
          "default": null,
          "examples": ["data.schema"]
        },
        "schemaSpec": {
          "type": "object",
          "description": "Json schema specification to use",
          "default": null,
          "examples": [{ "id": { "title": "ID" } }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

If none of the schema properties are provided, this editor assumes it is used within a UI design editor and uses UI item's id, schema and schemaPath properties to retrieve the right schema data.

## Other Widgets

### Switch Editor

<figure><img src="../../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

SwitchEditor provides ability to enter true/false values with a switch. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Definition                                                | Example | Default |
| -------- | --------------------------------------------------------- | ------- | ------- |
| Inline   | Whether label should be above or inline with the Switch   | true    | false   |
| Default  | Default to display if value is not set for the editor yet | true    | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SwitchEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "inline": {
          "type": "boolean",
          "description": "Whether label should be above or inline with the Switch",
          "default": false,
          "examples": [true]
        },
        "defaultValue": {
          "type": "boolean",
          "description": "Default to display if value is not set for the editor yet",
          "default": false,
          "examples": [true]
        },
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Switch component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Date Time Editor

<figure><img src="../../../../.gitbook/assets/image (8).png" alt="" width="206"><figcaption></figcaption></figure>

DateTimeEditor provides ability to select date/time with time zone information and returns a value based on level selected (i.e. quarter, month, day or time) using DatePicker. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property          | Definition                           | Example     | Default |
| ----------------- | ------------------------------------ | ----------- | ------- |
| Level             | Level of date to be selected         | day         | time    |
| Pplaceholder      | Text to display as placeholder       | Select date | -       |
| Locale            | Date/time locale                     | -           | -       |
| Peek Next Month   | Whether to show days from next month | true        | false   |
| Show Week Numbers | Whether to show week numbers         | true        | false   |
| As Modal          | Whether to display as modal          | true        | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DateTimeEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "level": {
          "type": "string",
          "description": "Level of date to be selected",
          "default": "time",
          "examples": ["day"],
          "enum": ["time", "day", "month", "quarter"]
        },
        "placeholder": {
          "type": "string",
          "description": "Text to display as placeholder",
          "default": null,
          "examples": ["Select date"]
        },
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to DatePicker component",
          "default": null,
          "examples": [{
        		"locale": "locale",
        		"peekNextMonth": true,
        		"showWeekNumbers": true,
        		"withPortal": true
		      }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Values expected and returned for different date levels are as follows:

| Level   | Value                      |
| ------- | -------------------------- |
| time    | Epoch time in ms           |
| day     | yyyymmdd without time zone |
| month   | yyyymm without time zone   |
| quarter | yyyyq without time zone    |

### Color Editor

<figure><img src="../../../../.gitbook/assets/image (7).png" alt="" width="164"><figcaption></figcaption></figure>

ColorEditor provides ability to pick colors and return their hex code using SketchPicker. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Definition                                                     | Example | Default |
| -------- | -------------------------------------------------------------- | ------- | ------- |
| DOM      | Additional dom properties to pass on to SketchPicker component | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ColorEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to SketchPicker component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Slider Editor

<figure><img src="../../../../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

SliderEditor provides ability to enter single and {min, max} range type numerical values with a slider. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property    | Definition                                         | Example  | Default    |
| ----------- | -------------------------------------------------- | -------- | ---------- |
| Orientation | Whether the slider should be displayed vertical    | vertical | horizontal |
| Size        | Defines width of the slider                        | small    | base       |
| Min         | Minimum value to allow                             | 1        | 0          |
| Max         | Maximum value to allow                             | 10       | 100        |
| Step        | Numerical increments on each step                  | 2        | 1          |
| Is Range    | Whether slider requires 2 values or a single value | true     | false      |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SliderEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "orientation": {
          "type": "string",
          "description": "Whether the slider should be displayed vertical",
          "default": "horizontal",
          "examples": ["vertical"],
          "enum": ["horizontal", "vertical"]
        },
        "size": {
          "type": "string",
          "description": "Defines width of the slider",
          "default": "base",
          "examples": ["small"]
        },
        "min": {
          "type": "number",
          "description": "Minimum value to allow",
          "default": 0,
          "examples": [1]
        },
        "max": {
          "type": "number",
          "description": "Maximum value to allow",
          "default": 100,
          "examples": [10]
        },
        "step": {
          "type": "number",
          "description": "Numerical increments on each step",
          "default": 1,
          "examples": [2]
        },
        "range": {
          "type": "boolean",
          "description": "Whether slider requires 2 values or a single value",
          "default": false,
          "examples": [true]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Rating Editor

RatingEditor provides ability to select a numerical values with a rating indicator. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property    | Definition                                      | Example  | Default    |
| ----------- | ----------------------------------------------- | -------- | ---------- |
| Orientation | Whether the rating should be displayed vertical | vertical | horizontal |
| Min         | Minimum value to allow                          | 1        | 0          |
| Max         | Maximum value to allow                          | 10       | 100        |
| Step        | Numerical increments on each star               | 2        | 1          |
| Fractions   | Fractional increments allowed                   | 0.5      | -          |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RatingEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "orientation": {
          "type": "string",
          "description": "Whether the rating should be displayed vertical",
          "default": "horizontal",
          "examples": ["vertical"],
          "enum": ["horizontal", "vertical"]
        },
        "min": {
          "type": "number",
          "description": "Minimum value to allow",
          "default": 0,
          "examples": [1]
        },
        "max": {
          "type": "number",
          "description": "Maximum value to allow",
          "default": 100,
          "examples": [10]
        },
        "step": {
          "type": "number",
          "description": "Numerical increments on each star",
          "default": 1,
          "examples": [2]
        },
        "fractions": {
          "type": "number",
          "description": "Fractional increments allowed",
          "default": null,
          "examples": [0.5]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Media Editor

<figure><img src="../../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

MediaEditor provides ability to pick images and videos using their url paths. It also allows management of media files through embedded file system editor. All media related editors share the following properties:

{% tabs %}
{% tab title="Table" %}
| Property               | Definition                                                                                | Example                                                                      | Default                                       |
| ---------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | --------------------------------------------- |
| Media Type             | Media type to display (video, image, iframe, embed, custom, auto)                         | video                                                                        | auto                                          |
| Base URL               | Base url to use as prefix for media paths                                                 | https://my.cdn.com                                                           | -                                             |
| With Base              | Whether baseUrl should be automatically stored with path or not                           | true                                                                         | false                                         |
| Prefix                 | Prefix to add & store with file path                                                      | /images/                                                                     | -                                             |
| File System            | File system to connect through API gateway for file management                            | fs\_cdn                                                                      | -                                             |
| Dedicated FS Folder    | Whether user should access a dedicated folder on the file system                          | true                                                                         | false                                         |
| Thumbnail Extensions   | List of file extensions to show as thumbnails from the file system                        | -                                                                            | \['png', 'jpg', 'jpeg', 'gif', 'svg', 'webp'] |
| Paginated              | Whether the file list is loaded lazily in pages (in case folders include too many files)  | true                                                                         | false                                         |
| Display Video Controls | Whether to display controls for "video" media type                                        | false                                                                        | true                                          |
| Custom Tag             | Html tag for "custom" media type                                                          | model-viewer                                                                 | -                                             |
| Custom Attributes      | Html attributes for "custom" media type                                                   | {"camera-controls": true}                                                    | -                                             |
| Max Bytes              | Maximum bytes allowed for uploads                                                         | 10000000                                                                     | -                                             |
| Allowed Extensions     | File extensions allowed for uploads                                                       | \[jpg,png]                                                                   | -                                             |
| Restricted Extensions  | File extensions not allowed for uploads                                                   | \[exe,bat]                                                                   | -                                             |
| Allowed File Paths     | File path regex allowed for uploads                                                       | .\*-img.jpg                                                                  | -                                             |
| Image Check            | Image width, height, aspect ratio (numerical or "w:h") constraints (applied on selection) | {height: 100, minWidth: 200}                                                 | -                                             |
| Rename Pattern         | JMESPath pattern for renaming uploaded files automatically                                | join('', \[ file.name.split('.')\[0], now(), '.', file.name.split(.)\[-1] ]) | -                                             |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaEditor shared properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "media": {
          "type": "string",
          "description": "Media type to display (video, image, iframe, embed, custom, auto)",
          "default": "auto",
          "examples": ["video"],
          "enum": ["video", "image", "iframe", "embed", "custom", "auto"]
        },
        "baseUrl": {
          "type": "string",
          "description": "Base url to use as prefix for media paths",
          "default": null,
          "examples": ["https://my.cdn.com"]
        },
        "withBase": {
          "type": "boolean",
          "description": "Whether baseUrl should be automatically stored with path or not",
          "default": false,
          "examples": [true]
        },
        "prefix": {
          "type": "string",
          "description": "Prefix to add & store with file path",
          "default": null,
          "examples": ["/images/"]
        },
        "fileSystem": {
          "type": "string",
          "description": "File system to connect through API gateway for file management",
          "default": null,
          "examples": ["fs_cdn"]
        },
        "dedicated": {
          "type": "boolean",
          "description": "Whether user should access a dedicated folder on the file system",
          "default": false,
          "examples": [true]
        },
        "thumbExtensions": {
          "type": "array",
          "description": "List of file extensions to show as thumbnails from the file system",
          "default": ["png", "jpg", "jpeg", "gif", "svg", "webp"],
          "items": { "type": "string" }
        },
        "paginated": {
          "type": "boolean",
          "description": "Whether the file list is loaded lazily in pages (in case folders include too many files)",
          "default": false,
          "examples": [true]
        },
        "controls": {
          "type": "boolean",
          "description": "Whether to display controls for \"video\" media type",
          "default": true,
          "examples": [false]
        },
        "tag": {
          "type": "string",
          "description": "Html tag for \"custom\" media type",
          "default": null,
          "examples": ["model-viewer"]
        },
        "attributes": {
          "type": "object",
          "description": "Html attributes for \"custom\" media type",
          "default": null,
          "examples": [{ "camera-controls": true }]
        },
        "maxBytes": {
          "type": "integer",
          "description": "Maximum bytes allowed for uploads",
          "default": null,
          "examples": [10000000]
        },
        "allowedExts": {
          "type": "array",
          "description": "File extensions allowed for uploads",
          "default": null,
          "examples": [["jpg", "png"]],
          "items": { "type": "string" }
        },
        "restrictedExts": {
          "type": "array",
          "description": "File extensions not allowed for uploads",
          "default": null,
          "examples": [["exe", "bat"]],
          "items": { "type": "string" }
        },
        "allowedRegex": {
          "type": "string",
          "description": "File path regex allowed for uploads",
          "default": null,
          "examples": [".*-img.jpg"]
        },
        "imageCheck": {
          "type": "object",
          "description": "Image width, height, aspect ratio (numerical or \"w:h\") constraints (applied on selection)",
          "default": null,
          "examples": [{ "height": 100, "minWidth": 200 }]
        },
        "renamePattern": {
          "type": "string",
          "description": "JMESPath pattern for renaming uploaded files automatically",
          "default": null,
          "examples": [
            "join('', [ file.name.split('.')[0], now(), '.', file.name.split(.)[-1] ])"
          ]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

File upload functionality in media editor also includes the following properties, which are typically used for bulk upload operations (in file pages inside apps):

{% tabs %}
{% tab title="Table" %}
| Property              | Definition                                                                               | Example                          | Default |
| --------------------- | ---------------------------------------------------------------------------------------- | -------------------------------- | ------- |
| From Root             | Whether all uploads should be applied on root or the current folder                      | true                             | false   |
| Preprocess URL        | URL to call before uploading each file                                                   | request/rpc/RenameFile           | -       |
| Postprocess URL       | URL to call after uploading each file                                                    | request/rpc/RegisterFile         | -       |
| Preprocess Batch URL  | URL to call before uploading all selected files (can return "groups" or "files" array)   | request/rpc/RenameFiles          | -       |
| Postprocess Group URL | URL to call after uploading each group of files                                          | request/rpc/RegisterFilesByGroup | -       |
| Postprocess Batch URL | URL to call after uploading all selected files                                           | request/rpc/RegisterFiles        | -       |
| Output Contents       | UI to display at the end of upload operation (e.g. displaying successful, failed images) | \[ {ui: {props: {} } } ]         | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaEditor bulk upload properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "fromRoot": {
          "type": "boolean",
          "description": "Whether all uploads should be applied on root or the current folder",
          "default": false,
          "examples": [true]
        },
        "preprocessURL": {
          "type": "string",
          "description": "URL to call before uploading each file",
          "default": null,
          "examples": ["request/rpc/RenameFile"]
        },
        "postprocessURL": {
          "type": "string",
          "description": "URL to call after uploading each file",
          "default": null,
          "examples": ["request/rpc/RegisterFile"]
        },
        "preprocessBatchURL": {
          "type": "string",
          "description": "URL to call before uploading all selected files (can return \"groups\" or \"files\" array)",
          "default": null,
          "examples": ["request/rpc/RenameFiles"]
        },
        "postprocessGroupURL": {
          "type": "string",
          "description": "URL to call after uploading each group of files",
          "default": null,
          "examples": ["request/rpc/RegisterFilesByGroup"]
        },
        "postprocessBatchURL": {
          "type": "string",
          "description": "URL to call after uploading all selected files",
          "default": null,
          "examples": ["request/rpc/RegisterFiles"]
        },
        "outputContents": {
          "type": "array",
          "description": "UI to display at the end of upload operation (e.g. displaying successful, failed images)",
          "default": null,
          "examples": [[{ "ui": { "props": {} } }]],
          "items": { "type": "object" }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

This widget also has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property  | Definition                                                        | Example | Default |
| --------- | ----------------------------------------------------------------- | ------- | ------- |
| Value DOM | Additional dom properties to pass on to wrapper div component     | -       | -       |
| Input DOM | Additional dom properties to pass on to input TextField component | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaEditor extra UI properties",
  "type": "object",
  "properties": {
    "valueProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to wrapper div component",
          "default": null
        }
      }
    },
    "inputProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to input TextField component",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### File Upload

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

FileUpload provides ability to select a local file using file browser and pass its path as the value. This widget also allows importing records from a file onto the current record with the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property          | Definition                                                                        | Example | Default                                                  |
| ----------------- | --------------------------------------------------------------------------------- | ------- | -------------------------------------------------------- |
| Enable Import     | Whether widget should import data or not                                          | -       | false                                                    |
| Import as Base64  | Imports file as a data URL, typically used for uploading media files or documents | -       | false                                                    |
| Import Extensions | List of extensions that should be allowed for import                              | -       | csv, json, jsonl, ndjson, xml, xlsx, xls (if not base64) |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "FileUpload widget import properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "import": {
          "type": "object",
          "properties": {
            "enabled": {
              "type": "boolean",
              "description": "Whether widget should import data or not",
              "default": false
            },
            "base64": {
              "type": "boolean",
              "description": "Imports file as a data URL, typically used for uploading media files or documents",
              "default": false
            },
            "extensions": {
              "type": "array",
              "description": "List of extensions that should be allowed for import (if not base64)",
              "default": ["csv", "json", "jsonl", "ndjson", "xml", "xlsx", "xls"],
              "items": { "type": "string" }
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
