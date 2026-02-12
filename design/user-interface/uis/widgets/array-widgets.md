---
description: Array widgets are used to display and update list of value or object fields.
---

# Array Widgets

It is possible to extend the list and variety of array widgets, and Rierino is shipped with the following widgets:

## Text Widgets

### Multi Text Editor

<figure><img src="../../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

ChipArrayEditor displays a list of values with removable Chip components. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property   | Definition                                                                              | Example | Default |
| ---------- | --------------------------------------------------------------------------------------- | ------- | ------- |
| Value Type | Type of value this widget should display and return                                     | integer | text    |
| Input DOM  | Additional dom properties to pass on to TextField component used for new value entries  | -       | -       |
| Value DOM  | Additional dom properties to pass on to Chip component used for listing current entries | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ChipArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
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
          "description": "Additional dom properties to pass on to TextField component used for new value entries",
          "default": null
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to Chip component used for listing current entries",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Selection Widgets

### Multi Select Editor

SelectArrayEditor displays a list of values selected from a predefined set of options (either by name or as an actual set). This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property       | Definition                                             | Example        | Default |
| -------------- | ------------------------------------------------------ | -------------- | ------- |
| Options Source | Name of the options list used for selection            | conditionClass | -       |
| Options List   | Actual contents of the options list used for selection | \[{"id":"1"}]  | -       |
| Placeholder    | Text to display as placeholder                         | Select option  | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "SelectArrayEditor widget properties",
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
        },
        "placeholder": {
          "type": "string",
          "description": "Text to display as placeholder",
          "default": null,
          "examples": ["Select option"]
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

### Multi Item Editor

ItemArrayEditor displays a list of ids of a lookup record (such as attribute, category), with structure similar to ItemEditor. Each value is selected from an AutoComplete drop down and list of current values are displayed as Chips. This widget has the following special properties in addition to [sourced widget properties](./#filtered-widgets):

{% tabs %}
{% tab title="Table" %}
| Property          | Definition                                                                                            | Example               | Default                         |
| ----------------- | ----------------------------------------------------------------------------------------------------- | --------------------- | ------------------------------- |
| Label Path        | Json path (or JMES pattern) to name field to display as label in drop down                            | data.main.name        | name or data.name               |
| Tooltip Path      | Json path (or JMES pattern) to description field to display as tooltip in drop down                   | data.main.description | description or data.description |
| Icon Path         | Json path (or JMES pattern) to icon name field to display in drop down                                | data.main.icon        | -                               |
| Group Path        | Json path (or JMES pattern) to a field for grouping values by in drop down                            | data.domain           | -                               |
| Value Type        | Type of value this widget should display and return                                                   | integer               | text                            |
| Branched          | Whether source of the list is 'branched', which allows listing of entries for the current branch only | true                  | false                           |
| Allow Free Format | Allow entry of values not in the item list                                                            | false                 | true                            |
| DOM               | Additional dom properties to pass on to Autocomplete component                                        | -                     | -                               |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ItemArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "itemLabel": {
          "type": "string",
          "description": "Json path (or JMES pattern) to name field to display as label in drop down",
          "default": "name or data.name",
          "examples": ["data.main.name"]
        },
        "itemTooltip": {
          "type": "string",
          "description": "Json path (or JMES pattern) to description field to display as tooltip in drop down",
          "default": "description or data.description",
          "examples": ["data.main.description"]
        },
        "itemIcon": {
          "type": "string",
          "description": "Json path (or JMES pattern) to icon name field to display in drop down",
          "default": null,
          "examples": ["data.main.icon"]
        },
        "itemGroup": {
          "type": "string",
          "description": "Json path (or JMES pattern) to a field for grouping values by in drop down",
          "default": null,
          "examples": ["data.domain"]
        },
        "valueType": {
          "type": "string",
          "description": "Type of value this widget should display and return",
          "default": "text",
          "examples": ["integer"]
        },
        "branched": {
          "type": "boolean",
          "description": "Whether source of the list is 'branched', which allows listing of entries for the current branch only",
          "default": false,
          "examples": [true]
        },
        "freeSolo": {
          "type": "boolean",
          "description": "Allow entering values not in the item list",
          "default": true
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

### Multi Item Search Editor

ItemSearchArrayEditor displays a list of ids of a lookup record (such as product) from a searchable table of values, with structure similar to ItemSearchEditor. This widget allows filtering results and should be preferred for selecting among large number of records. It has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property                  | Definition                                                                     | Example                                         | Default |
| ------------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------- | ------- |
| Item Source               | Name of data source mapping to use for fetching list of records and details    | product                                         | -       |
| Item Selector Path        | Json path to the id field for representing selected value                      | id                                              | -       |
| Columns                   | List of default table column configurations                                    | -                                               | -       |
| Item Selection Source     | Overriding data source name to use when displaying selected value details      | product-list                                    | -       |
| Selection Filter          | Filter parameter to pass selected values with when querying from detail source | productids                                      | -       |
| Selection Columns         | List of table column configurations for displaying selected value details      | {"path":"id","title":"ID"}                      | -       |
| Item List Source          | Overriding data source name to use when searching for values                   | product-search                                  | -       |
| List Main & Extra Filters | List of main and extra filters for searching from list source                  | "main":\[{"path":"name","widget":"TextFilter"}] | -       |
| List Columns              | List of table column configurations for displaying search results              | {"path":"id","title":"ID"}                      | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ItemSearchArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "source": {
          "type": "string",
          "description": "Name of data source mapping to use for fetching list of records and details",
          "default": null,
          "examples": ["product"]
        },
        "selector": {
          "type": "string",
          "description": "Json path to the id field for representing selected value",
          "default": null,
          "examples": ["id"]
        },
        "columns": {
          "type": "array",
          "description": "List of default table column configurations",
          "default": null,
          "items": { "type": "object" }
        },
        "detail": {
          "type": "object",
          "properties": {
            "source": {
              "type": "string",
              "description": "Overriding data source name to use when displaying selected value details",
              "default": null,
              "examples": ["product-list"]
            },
            "filter": {
              "type": "string",
              "description": "Filter parameter to pass selected values with when querying from detail source",
              "default": null,
              "examples": ["productids"]
            },
            "columns": {
              "type": "array",
              "description": "List of table column configurations for displaying selected value details",
              "default": null,
              "items": { "type": "object" },
              "examples": [[{ "path": "id", "title": "ID" }]]
            }
          }
        },
        "list": {
          "type": "object",
          "properties": {
            "source": {
              "type": "string",
              "description": "Overriding data source name to use when searching for values",
              "default": null,
              "examples": ["product-search"]
            },
            "filters": {
              "type": "object",
              "description": "List of main and extra filters for searching from list source",
              "default": null,
              "examples": [{
                "main": [{ "path": "name", "widget": "TextFilter" }]
              }]
            },
            "columns": {
              "type": "array",
              "description": "List of table column configurations for displaying search results",
              "default": null,
              "items": { "type": "object" },
              "examples": [[{ "path": "id", "title": "ID" }]]
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

### Multi Item Tree Editor

TreeArrayEditor displays hierarchical list of items in a tree-like structure, allowing easy selection of records. This widget has the following special properties in addition to [sourced widget properties](./#filtered-widgets):

{% tabs %}
{% tab title="Table" %}
| Property    | Definition                                                                 | Example            | Default               |
| ----------- | -------------------------------------------------------------------------- | ------------------ | --------------------- |
| Label Path  | Json path (or JMES pattern) to name field to display as label in drop down | data.main.name     | name or data.name     |
| Parent Path | Json path (or JMES pattern) to parent field for creating hierarchy         | data.main.parentId | parent or data.parent |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TreeArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "itemLabel": {
          "type": "string",
          "description": "Json path (or JMES pattern) to name field to display as label in drop down",
          "default": "name or data.name",
          "examples": ["data.main.name"]
        },
        "itemParent": {
          "type": "string",
          "description": "Json path (or JMES pattern) to parent field for creating hierarchy",
          "default": "parent or data.parent",
          "examples": ["data.main.parentId"]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Multi Item Table Editor

ItemTableArrayEditor displays a table of items, allowing easy selection of records while seeing their details. This widget has the following special properties in addition to [sourced widget properties](./#filtered-widgets):

{% tabs %}
{% tab title="Table" %}
| Property  | Definition                                                         | Example                               | Default |
| --------- | ------------------------------------------------------------------ | ------------------------------------- | ------- |
| Paginated | Whether table should be paginated or not                           | true                                  | false   |
| Page Size | Number of rows to display per page if table is paginated           | 20                                    | 10      |
| Columns   | Array of columns with field, key, title, default, local parameters | \[{"field": "name", "title": "Name"}] | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ItemTableArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "paginated": {
          "type": "boolean",
          "description": "Whether table should be paginated or not",
          "default": false,
          "examples": [true]
        },
        "pageSize": {
          "type": "integer",
          "description": "Number of rows to display per page if table is paginated",
          "default": 10,
          "examples": [20]
        },
        "columns": {
          "type": "array",
          "description": "Array of columns with field, key, title, default, local parameters",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{ "field": "name", "title": "Name" }]]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Nested Widgets

### Table Editor

<figure><img src="../../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

TableArrayEditor displays a list of objects in table form with all columns editable inline. Column widgets are typically automatically discovered based on field data types. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property             | Definition                                                                                                                                       | Example                                                                                                            | Default |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ------- |
| Single Column Widget | Widget to use for a single column table                                                                                                          | TextEditor                                                                                                         | -       |
| Single Column Props  | Extra properties to pass on to single column table                                                                                               | {"source": "category"}                                                                                             | -       |
| Column List          | UI description of all table columns similar to grid contents, including simpler widgets (img, time, date, array, table, pattern, option) as well | <p>[</p><p>{"path":"name", "widget":"TextEditor", "props": {}},</p><p>{"path":"image", "widget":"img"}</p><p>]</p> | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TableArrayEditor widget properties",
  "type": "object",
  "properties": {
    "valueProps": {
      "type": "object",
      "properties": {
        "widget": {
          "type": "string",
          "description": "Widget to use for a single column table",
          "default": null,
          "examples": ["TextEditor"]
        },
        "widgetProps": {
          "type": "object",
          "description": "Extra properties to pass on to single column table",
          "default": null,
          "examples": [{ "source": "category" }]
        },
        "contents": {
          "type": "array",
          "description": "UI description of all table columns similar to grid contents, including simpler widgets (img, time, date, array, table, pattern, option) as well",
          "default": null,
          "items": { "type": "object" },
          "examples": [[
            { "path": "name", "widget": "TextEditor", "props": {} },
            { "path": "image", "widget": "img" }
          ]]
        },
        "fieldProps": {
          "type": "object",
          "description": "Properties to pass on to all table columns (when contents is undefined)",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Object Table Editor

ObjectTableArrayEditor displays a list of objects in table form with selected fields as columns. Table fields are displayed using default widgets for their data types. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property     | Definition                                       | Example                                    | Default |
| ------------ | ------------------------------------------------ | ------------------------------------------ | ------- |
| Table Fields | List of Json paths to display as table columns   | \[{"path":"name", "widget": "TextEditor"}] | -       |
| Editor Tabs  | Tab contents of displayed editor for row editing | \[{"tab":"DEFINITION"}]                    | -       |
| Editor Title | Title to display on object editor                | Details                                    | -       |
| Editor Icon  | Title to display on object editor                | edit                                       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ObjectTableArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "tableFields": {
          "type": "array",
          "description": "List of Json paths to display as table columns",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{ "path": "name", "widget": "TextEditor" }]]
        },
        "uiDesign": {
          "type": "object",
          "properties": {
            "tabs": {
              "type": "array",
              "description": "Tab contents of displayed editor for row editing",
              "default": null,
              "items": { "type": "object" },
              "examples": [[{ "tab": "DEFINITION" }]]
            },
            "title": {
              "type": "string",
              "description": "Title to display on object editor",
              "default": null,
              "examples": ["Details"]
            },
            "titleIcon": {
              "type": "string",
              "description": "Title to display on object editor",
              "default": null,
              "examples": ["edit"]
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

### Grid Editor

GridArrayEditor displays a list of objects in a Grid structure. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property  | Definition                                                | Example                                               | Default |
| --------- | --------------------------------------------------------- | ----------------------------------------------------- | ------- |
| Grid Size | Size of each grid entry (out of 12 per row)               | 6                                                     | 12      |
| Grid List | List of grids, each with content specifications and props | \[{"contents":\[{"path":"name"}], "props":{"sm":6\}}] | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "GridArrayEditor widget properties",
  "type": "object",
  "properties": {
    "valueProps": {
      "type": "object",
      "properties": {
        "entrySize": {
          "type": "integer",
          "description": "Size of each grid entry (out of 12 per row)",
          "default": 12,
          "examples": [6]
        },
        "grids": {
          "type": "array",
          "description": "List of grids, each with content specifications and props",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{ "contents": [{ "path": "name" }], "props": { "sm": 6 } }]]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Data Grid Editor

DataGridEditor displays a rich and interactive Excel-like grid structure, supporting features such as row grouping, pivoting and formulas. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property            | Definition                                                     | Example                                                                                                         | Default |
| ------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------- |
| Column List         | List of AG Grid columns and their features                     | <p>[{<br>"field": "quantity",<br>"headerName": "Qty",<br>"maxWidth": 100,<br>"cellDataType": "number"<br>}]</p> | -       |
| Show Add Row Button | Whether an add button should be displayed to allow adding rows | true                                                                                                            | false   |
| Height              | Height of the data grid                                        | 100px                                                                                                           | 80vh    |
| Enabled Features    | Features to enable on AG Grid                                  | {filtering: true, sideBar: true}                                                                                | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DataGridEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "ui": {
          "type": "object",
          "properties": {
            "showAddRowButton": {
              "type": "boolean",
              "description": "Whether an add button should be displayed to allow adding rows",
              "default": false,
              "examples": [true]
            },
            "height": {
              "type": "string",
              "description": "Height of the data grid",
              "default": "80vh",
              "examples": ["100px"]
            }
          }
        },
        "enabledFeatures": {
          "type": "object",
          "description": "Features to enable on AG Grid",
          "default": null,
          "examples": [{ "filtering": true, "sideBar": true }]
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "columns": {
          "type": "array",
          "description": "List of AG Grid columns and their features",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{
            "field": "quantity",
            "headerName": "Qty",
            "maxWidth": 100,
            "cellDataType": "number"
          }]]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://www.ag-grid.com/" %}

### Step Editor

StepArrayEditor is a variation of GridArrayEditor, where each entry is considered a step and visualized as such. This widget has the same special properties as the [Grid Editor](array-widgets.md#grid-editor).

### Tab Editor

TabArrayEditor displays a list of objects inside a tab panel, where adding new entries results in adding new tabs. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property       | Definition                                                             | Example                                               | Default           |
| -------------- | ---------------------------------------------------------------------- | ----------------------------------------------------- | ----------------- |
| Tab Label Path | Json path for displaying as tab labels                                 | tab                                                   | name or data.name |
| Grid List      | List of grids for each tab, each with content specifications and props | \[{"contents":\[{"path":"name"}], "props":{"sm":6\}}] | -                 |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "TabArrayEditor widget properties",
  "type": "object",
  "properties": {
    "valueProps": {
      "type": "object",
      "properties": {
        "tabLabel": {
          "type": "string",
          "description": "Json path for displaying as tab labels",
          "default": "name or data.name",
          "examples": ["tab"]
        },
        "grids": {
          "type": "array",
          "description": "List of grids for each tab, each with content specifications and props",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{ "contents": [{ "path": "name" }], "props": { "sm": 6 } }]]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Customize Editor

CustomizeArrayEditor is a special editor which displays data customizations for records, with options to add/remove applicable customizations mapping customBy attributes of records with customization ids. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property             | Definition                                                                                 | Example                                               | Default       |
| -------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------- | ------------- |
| Customization Source | Name of data source mapping to use for fetching list of customization names, types and ids | product\_customization                                | customization |
| Grid List            | List of grids, each with content specifications and props for customized data entries      | \[{"contents":\[{"path":"name"}], "props":{"sm":6\}}] | -             |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CustomizeArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "source": {
          "type": "string",
          "description": "Name of data source mapping to use for fetching list of customization names, types and ids",
          "default": "customization",
          "examples": ["product_customization"]
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "grids": {
          "type": "array",
          "description": "List of grids, each with content specifications and props for customized data entries",
          "default": null,
          "items": { "type": "object" },
          "examples": [[{ "contents": [{ "path": "name" }], "props": { "sm": 6 } }]]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Other Widgets

### Multi Media Editor

MediaArrayEditor displays a list of images or videos, each represented by their relative URL path. This widget has the following special properties in addition to those listed in [Media Editor](value-widgets.md#media-editor):

{% tabs %}
{% tab title="Table" %}
| Property  | Definition                                                                 | Example | Default |
| --------- | -------------------------------------------------------------------------- | ------- | ------- |
| Value DOM | Additional dom properties to pass on to div encapsulating each image       | -       | -       |
| Input DOM | Additional dom properties to pass on to TextField for entering image paths | -       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaArrayEditor widget properties",
  "type": "object",
  "properties": {
    "valueProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to div encapsulating each image",
          "default": null
        }
      }
    },
    "inputProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to TextField for entering image paths",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Multi Media Metadata Editor

MediaMetadataArrayEditor displays a list of images or videos with metadata fields editable, each represented by their relative URL path. This widget has the following special properties in addition to those listed in [Media Editor](value-widgets.md#media-editor):

{% tabs %}
{% tab title="Table" %}
| Property                              | Definition                                                                 | Example                                                                  | Default |
| ------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------ | ------- |
| Media Path                            | Json path for the media url in editor contents                             | imageUrl                                                                 | media   |
| UI Design (Editor Icon, Title & Tabs) | Complete editor UI configurations for detail contents                      | {"title": "Image", "titleIcon": "media", "tabs":\[{"tab":"DEFINITION"}]} | -       |
| Value DOM                             | Additional dom properties to pass on to div encapsulating each image       | -                                                                        | -       |
| Input DOM                             | Additional dom properties to pass on to TextField for entering image paths | -                                                                        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MediaMetadataArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "mediaPath": {
          "type": "string",
          "description": "Json path for the media url in editor contents",
          "default": "media",
          "examples": ["imageUrl"]
        },
        "uiDesign": {
          "type": "object",
          "description": "Complete editor UI configurations for detail contents",
          "default": null,
          "examples": [{
            "title": "Image",
            "titleIcon": "media",
            "tabs": [{ "tab": "DEFINITION" }]
          }]
        }
      }
    },
    "valueProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to div encapsulating each image",
          "default": null
        }
      }
    },
    "inputProps": {
      "type": "object",
      "properties": {
        "dom": {
          "type": "object",
          "description": "Additional dom properties to pass on to TextField for entering image paths",
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Handlebars Preview Editor

[HandlebarsPreviewArrayEditor ](#user-content-fn-1)[^1]is used for collecting list of records while rendering each record with a Handlebars template, providing form of a web page / section builder. This widget has the following special properties in addition to [sourced widget properties](./#filtered-widgets):

{% tabs %}
{% tab title="Table" %}
<table><thead><tr><th width="211">Property</th><th>Definition</th><th>Example</th><th>Default</th></tr></thead><tbody><tr><td>Source Partial Path</td><td>Json path for element for reading partial <a data-footnote-ref href="#user-content-fn-2">template in source</a></td><td>data.inline</td><td>data.partial</td></tr><tr><td>Source Preview Path</td><td>Json path for element for reading preview <a data-footnote-ref href="#user-content-fn-3">template in source</a></td><td>data.render</td><td>data.preview</td></tr><tr><td>Component Id Path</td><td>Json path for id of template component to use for data entry</td><td>data.component</td><td>component</td></tr><tr><td>Is Disabled Path</td><td>Json path for detecting whether current data entry should be disabled</td><td>data.disable</td><td>disabled</td></tr><tr><td>Content Path</td><td>Json path for data to use in populating template from entry</td><td>data.body</td><td>content</td></tr><tr><td>Min Height</td><td>Minimum height for rendered list</td><td>100</td><td>-</td></tr><tr><td>Edit Only</td><td>Whether it should be possible to change structure of contents by default</td><td>true</td><td>false</td></tr><tr><td>UI Design (Editor Icon, Title &#x26; Tabs)</td><td>Complete editor UI configurations for editing entry contents</td><td>{"tabs":[{"tab":"DEFINITION"}]}</td><td>-</td></tr></tbody></table>
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "HandlebarsPreviewArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "sourcePartialPath": {
          "type": "string",
          "description": "Json path for element for reading partial template in source",
          "default": "data.partial",
          "examples": ["data.inline"]
        },
        "sourcePreviewPath": {
          "type": "string",
          "description": "Json path for element for reading preview template in source",
          "default": "data.preview",
          "examples": ["data.render"]
        },
        "componentPath": {
          "type": "string",
          "description": "Json path for id of template component to use for data entry",
          "default": "component",
          "examples": ["data.component"]
        },
        "disabledPath": {
          "type": "string",
          "description": "Json path for detecting whether current data entry should be disabled",
          "default": "disabled",
          "examples": ["data.disable"]
        },
        "contentPath": {
          "type": "string",
          "description": "Json path for data to use in populating template from entry",
          "default": "content",
          "examples": ["data.body"]
        },
        "minHeight": {
          "type": "integer",
          "description": "Minimum height for rendered list",
          "default": null,
          "examples": [100]
        },
        "editOnly": {
          "type": "boolean",
          "description": "Whether it should be possible to change structure of contents by default",
          "default": false,
          "examples": [true]
        },
        "uiDesign": {
          "type": "object",
          "description": "Complete editor UI configurations for editing entry contents",
          "default": null,
          "examples": [{ "tabs": [{ "tab": "DEFINITION" }] }]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

This editor loads all [partial templates](#user-content-fn-4)[^4] from a data source (e.g. page components). For each record in current data list, it uses a component field to identify the preview template to use and populates that template with record's content field values.

### Preview Editor

EditorPreviewArrayEditor is a special editor for designing UI screens, allowing definition of list of editors to include within a grid. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property         | Definition                        | Example                          | Default |
| ---------------- | --------------------------------- | -------------------------------- | ------- |
| Design Details   | Complete editor UI configurations | {"tabs":\[{"tab":"DEFINITION"}]} | -       |
| Design Reference | Editor design reference           | component                        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "EditorPreviewArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "uiDesign": {
          "type": "object",
          "description": "Complete editor UI configurations",
          "default": null,
          "examples": [{ "tabs": [{ "tab": "DEFINITION" }] }]
        },
        "uiDesignRef": {
          "type": "string",
          "description": "Editor design reference",
          "default": null,
          "examples": ["component"]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Multi Attribute Editor

AttributeArrayEditor is a special component for editing a dynamic array with attribute & value / valueId fields, where list of applicable attributes are identified based on another data element. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property                                 | Definition                                                                                    | Example                     | Default |
| ---------------------------------------- | --------------------------------------------------------------------------------------------- | --------------------------- | ------- |
| Attribute Value Source                   | Id of the UI source for populating attribute value list for attributes of lookup type         | attribute\_value            | -       |
| Attribute Value Label                    | Data element to use as label for the values from attribute value source                       | data.name.enUS              | -       |
| Attribute Value Tooltip                  | Data element to use as description for the values from attribute value source                 | data.description.enUS       | -       |
| Attribute Value Attribute Field          | Data element to use for selecting values of a given attribute                                 | data.attributeId            | -       |
| Attribute Value Server Side Filter Field | Data element from attribute value source to filter applicable attribute values on server side | status                      | -       |
| Attribute Source                         | Id of the UI source for populating attribute list                                             | attribute\_with\_categories | -       |
| Attribute Label                          | Data element to use as label for the attribute editor from attribute source                   | data.name.enUS              | -       |
| Attribute Tooltip                        | Data element to use as description for the attribute editor from attribute source             | data.description.enUS       | -       |
| Attribute Server Side Filter Field       | Data element from attribute source to filter applicable attributes on server side             | status                      | -       |
| Attribute Client Side Filter Field       | Data element from attribute source to filter applicable attributes locally                    | categories                  | -       |
| Attribute Filter Value Path              | Data element from current root record to compare against filter field values                  | data.category               |         |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "AttributeArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "attributeValueSource": {
          "type": "string",
          "description": "Id of the UI source for populating attribute value list for attributes of lookup type",
          "default": null,
          "examples": ["attribute_value"]
        },
        "attributeValueLabel": {
          "type": "string",
          "description": "Data element to use as label for the values from attribute value source",
          "default": null,
          "examples": ["data.name.enUS"]
        },
        "attributeValueTooltip": {
          "type": "string",
          "description": "Data element to use as description for the values from attribute value source",
          "default": null,
          "examples": ["data.description.enUS"]
        },
        "attributeValueAttribute": {
          "type": "string",
          "description": "Data element to use for selecting values of a given attribute",
          "default": null,
          "examples": ["data.attributeId"]
        },
        "attributeValueFilterParam": {
          "type": "string",
          "description": "Data element from attribute value source to filter applicable attribute values on server side",
          "default": null,
          "examples": ["status"]
        },
        "attributeSource": {
          "type": "string",
          "description": "Id of the UI source for populating attribute list",
          "default": null,
          "examples": ["attribute_with_categories"]
        },
        "attributeLabel": {
          "type": "string",
          "description": "Data element to use as label for the attribute editor from attribute source",
          "default": null,
          "examples": ["data.name.enUS"]
        },
        "attributeTooltip": {
          "type": "string",
          "description": "Data element to use as description for the attribute editor from attribute source",
          "default": null,
          "examples": ["data.description.enUS"]
        },
        "filterParam": {
          "type": "string",
          "description": "Data element from attribute source to filter applicable attributes on server side",
          "default": null,
          "examples": ["status"]
        },
        "filterField": {
          "type": "string",
          "description": "Data element from attribute source to filter applicable attributes locally",
          "default": null,
          "examples": ["categories"]
        },
        "filterPath": {
          "type": "string",
          "description": "Data element from current root record to compare against filter field values",
          "default": null,
          "examples": ["data.category"]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Multi Action Editor

ActionArrayEditor is a special component for editing action items for business rules. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property | Definition                                  | Example           | Default |
| -------- | ------------------------------------------- | ----------------- | ------- |
| Domain   | Rule domain for loading available functions | product\_discount | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ActionArrayEditor widget properties",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "description": "Rule domain for loading available functions",
          "default": null,
          "examples": ["product_discount"]
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Array To Object Editor

ArrayToObjectEditor allows editing of a key-value array using Object Widgets, which is useful for cases where the list of keys is predefined. This widget has the same special properties as [Contents Editor](object-widgets.md#contents-editor).

[^1]: Since v0.4.2

[^2]: Typically wrapped in \{{#\*inline "\[ID]"\}}

[^3]: Typically using \{{> \[ID]\}} with default parameter values

[^4]: Which should not render contents on their own
