---
description: >-
  Indirect widgets are used to display and edit data not about a selected
  records, but other entries which are dependent on or related to them.
---

# Indirect Widgets

It is possible to extend the list and variety of indirect widgets, and Rierino is shipped with the following widgets:

## Dependent Object Editor

DependentObjectEditor provides ability to search for, create and edit entries that are dependent to current record (such as variants of a product or values of an attribute). This editor uses DependentQueryTableLister for listing dependent objects by default, which is configured similar to the QueryTableLister. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property              | Definition                                                                                                                                 | Example                   | Default                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------- | ------------------------- |
| Type                  | Name of the schema and UI configuration that will be used for listing and editing dependent objects                                        | product\_variant          | -                         |
| Source                | Name of data source mapping to use for fetching dependent objects                                                                          | product\_variants         | value of type property    |
| Query Field           | Filter name that will be passed on for querying the source                                                                                 | productid                 | -                         |
| Data Field            | Json path for setting value of current object in dependent entries                                                                         | data.productId            | -                         |
| No Create             | Whether record creation should be restricted                                                                                               | true                      | false                     |
| Dependent Lister Type | Type of dependent lister (DependentCustomCodeLister, DependentHandlebarsLister or DependentQueryTableLister) to use for displaying records | DependentHandlebarsLister | DependentQueryTableLister |
| Keep Filters          | Whether original lister filters defined in referenced UI should be displayed or not                                                        | true                      | false                     |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DependentObjectEditor widget config",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "description": "Name of the schema and UI configuration that will be used for listing and editing dependent objects.",
          "examples": ["product_variant"]
        },
        "source": {
          "type": "string",
          "description": "Name of data source mapping to use for fetching dependent objects. Defaults to the value of `props.type`.",
          "examples": ["product_variants"]
        },
        "mapping": {
          "type": "object",
          "properties": {
            "queryField": {
              "type": "string",
              "description": "Filter name that will be passed on for querying the source.",
              "examples": ["productid"]
            },
            "dataField": {
              "type": "string",
              "description": "Json path for setting value of current object in dependent entries.",
              "examples": ["data.productId"]
            }
          }
        },
        "noCreate": {
          "type": "boolean",
          "description": "Whether record creation should be restricted.",
          "default": false,
          "examples": [true]
        },
        "dependentLister": {
          "type": "string",
          "description": "Type of dependent lister to use for displaying records.",
          "default": "DependentQueryTableLister",
          "examples": ["DependentHandlebarsLister"]
        },
        "keepFilters": {
          "type": "boolean",
          "description": "Whether original lister filters defined in referenced UI should be displayed or not.",
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

## Related Object Editor

RelatedObjectEditor provides ability to create relation between current record and other existing entries (such as adding products to a collection). This editor allows searching for existing entries using configurable filters. This widget has the following special properties:

{% tabs %}
{% tab title="Table" %}
| Property                   | Definition                                                                          | Example                                         | Default |
| -------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------- | ------- |
| Object Source              | Name of data source mapping to use for fetching list of related records and details | product                                         | -       |
| Object Selector Path       | Json path to the id field for representing related record                           | id                                              | -       |
| Object Detail Source       | Overriding data source name to use when displaying related record details           | product-list                                    | -       |
| Detail Filter              | Filter parameter to pass related records with when querying from detail source      | productids                                      | -       |
| Detail Columns             | List of table column configurations for displaying related record details           | {"path":"id","title":"ID"}                      | -       |
| Object List Source         | Overriding data source name to use when searching for records                       | product-search                                  | -       |
| List Main & Detail Filters | List of main and extra filters for searching from list source                       | "main":\[{"path":"name","widget":"TextFilter"}] | -       |
| List Columns               | List of table column configurations for displaying search results                   | {"path":"id","title":"ID"}                      | -       |
| Path to Assign from        | Json path for value to assign to related entities from current record               | id                                              | -       |
| Path to Assign to          | Json path of related entities to update                                             | data.main.collections                           | -       |
| Add Url                    | Url for adding relation to new entities                                             | request/rpc/productw/AddToCollection            | -       |
| Remove URL                 | Url for removing relation from existing entities                                    | request/rpc/productw/RemoveFromCollection       | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RelatedObjectEditor widget config",
  "type": "object",
  "properties": {
    "props": {
      "type": "object",
      "properties": {
        "source": {
          "type": "string",
          "description": "Name of data source mapping to use for fetching list of related records and details.",
          "examples": ["product"]
        },
        "selector": {
          "type": "string",
          "description": "Json path to the id field for representing related record.",
          "examples": ["id"]
        },
        "detail": {
          "type": "object",
          "properties": {
            "source": {
              "type": "string",
              "description": "Overriding data source name to use when displaying related record details.",
              "examples": ["product-list"]
            },
            "filter": {
              "type": "string",
              "description": "Filter parameter to pass related records with when querying from detail source.",
              "examples": ["productids"]
            },
            "columns": {
              "type": "array",
              "description": "List of table column configurations for displaying related record details.",
              "items": {
                "type": "object",
                "properties": {
                  "path": { "type": "string" },
                  "title": { "type": "string" }
                }
              },
              "examples": [[{ "path": "id", "title": "ID" }]]
            }
          }
        },
        "list": {
          "type": "object",
          "properties": {
            "source": {
              "type": "string",
              "description": "Overriding data source name to use when searching for records.",
              "examples": ["product-search"]
            },
            "filters": {
              "type": "object",
              "description": "List of main and extra filters for searching from list source.",
              "properties": {
                "main": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "path": { "type": "string" },
                      "widget": { "type": "string" }
                    }
                  }
                }
              },
              "examples": [
                {
                  "main": [{ "path": "name", "widget": "TextFilter" }]
                }
              ]
            },
            "columns": {
              "type": "array",
              "description": "List of table column configurations for displaying search results.",
              "items": {
                "type": "object",
                "properties": {
                  "path": { "type": "string" },
                  "title": { "type": "string" }
                }
              },
              "examples": [[{ "path": "id", "title": "ID" }]]
            }
          }
        },
        "updateConfig": {
          "type": "object",
          "properties": {
            "value": {
              "type": "string",
              "description": "Json path for value to assign to related entities from current record.",
              "examples": ["id"]
            },
            "key": {
              "type": "string",
              "description": "Json path of related entities to update.",
              "examples": ["data.main.collections"]
            },
            "addUrl": {
              "type": "string",
              "description": "Url for adding relation to new entities.",
              "examples": ["request/rpc/productw/AddToCollection"]
            },
            "removeUrl": {
              "type": "string",
              "description": "Url for removing relation from existing entities.",
              "examples": ["request/rpc/productw/RemoveFromCollection"]
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

## History Display

HistoryDisplay is a specialized editor for displaying historical versions of an item.

## Runner Element Viewer

ElementViewer is a specialized editor for displaying details of an element added to a runner.
