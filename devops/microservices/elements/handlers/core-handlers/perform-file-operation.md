---
description: >-
  This handler (com.rierino.handler.FSEventHandler) provides ability to interact
  with local and remote file systems for directory and file operations.
---

# Perform File Operation

## Handler Parameters

| Parameter      | Definition                                                   | Example                                   | Default |
| -------------- | ------------------------------------------------------------ | ----------------------------------------- | ------- |
| systems        | Comma separated list of file system names to use             | imageFs,cdnFs                             | -       |
| role.source    | System name for sourcing files from for sync file role calls | imageFs                                   | -       |
| role.target    | System name for pushing files to for sync file role calls    | cdnFs                                     | -       |
| role.fromRegex | Regex for filtering source files                             | /source/(?\<folder>\\\w+)/(?\<file>\\\w+) | -       |
| role.toPattern | Pattern for converting source paths to target paths          | /target/${folder}/${file}                 | -       |
| role.move      | Whether role calls should move files or just copy            | true                                      | false   |

## Actions

### MakeDir

Creates a new directory in given file system. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                     | Example | Default |
| ------ | ------------------------------ | ------- | ------- |
| Domain | Name of the file system to use | imageFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MakeDir action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to use",
          "example": "imageFs"
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
| Parameter | Definition                                        | Example  | Default |
| --------- | ------------------------------------------------- | -------- | ------- |
| Path Path | Json path in payload for directory path to create | filePath | path    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "MakeDir action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for directory path to create",
              "default": "path",
              "example": "filePath"
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

### List / ListPath

Lists files and directories for given path. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                     | Example | Default |
| ------ | ------------------------------ | ------- | ------- |
| Domain | Name of the file system to use | imageFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "List action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to use",
          "example": "imageFs"
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
| Parameter | Definition                                      | Example  | Default |
| --------- | ----------------------------------------------- | -------- | ------- |
| Path Path | Json path in payload for directory path to list | filePath | path    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "List action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for directory path to list",
              "default": "path",
              "example": "filePath"
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

### Delete / DeletePath

Deletes a file or directory in given file system. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                     | Example | Default |
| ------ | ------------------------------ | ------- | ------- |
| Domain | Name of the file system to use | imageFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Delete action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to use",
          "example": "imageFs"
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
| Parameter | Definition                                             | Example  | Default |
| --------- | ------------------------------------------------------ | -------- | ------- |
| Path Path | Json path in payload for file/directory path to delete | filePath | path    |
| Recursive | Whether deletion should include sub-directories        | true     | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Delete action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for file/directory path to delete",
              "default": "path",
              "example": "filePath"
            },
            "recursive": {
              "type": "boolean",
              "definition": "Whether deletion should include sub-directories",
              "default": false,
              "example": true
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

### Rename / RenamePath

Renames a file or directory in given file system. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                     | Example | Default |
| ------ | ------------------------------ | ------- | ------- |
| Domain | Name of the file system to use | imageFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Rename action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to use",
          "example": "imageFs"
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
| Parameter | Definition                                        | Example  | Default |
| --------- | ------------------------------------------------- | -------- | ------- |
| Path Path | Json path in payload for file/directory to rename | filePath | path    |
| To Path   | Json path in payload for new file/directory name  | newPath  | to      |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Rename action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for file/directory to rename",
              "default": "path",
              "example": "filePath"
            },
            "toPath": {
              "type": "string",
              "definition": "Json path in payload for new file/directory name",
              "default": "to",
              "example": "newPath"
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

### CopyFile

Copies or moves a file from one file system to another. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                            | Example | Default |
| ------ | ------------------------------------- | ------- | ------- |
| Domain | Name of the source file system to use | imageFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CopyFile action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the source file system to use",
          "example": "imageFs"
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
| Parameter | Definition                                      | Example  | Default |
| --------- | ----------------------------------------------- | -------- | ------- |
| target    | Name of the target file system                  | cdnFs    | -       |
| Path Path | Json path in payload for file/directory to copy | filePath | path    |
| To Path   | Json path in payload for target file/directory  | newPath  | to      |
| Move      | Whether file should be deleted from the source  | true     | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CopyFile action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "target": {
              "type": "string",
              "definition": "Name of the target file system",
              "example": "cdnFs"
            },
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for file/directory to copy",
              "default": "path",
              "example": "filePath"
            },
            "toPath": {
              "type": "string",
              "definition": "Json path in payload for target file/directory",
              "default": "to",
              "example": "newPath"
            },
            "move": {
              "type": "boolean",
              "definition": "Whether file should be deleted from the source",
              "default": false,
              "example": true
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

### Write / WriteFile

Writes contents to a single file or a set of files for a given file system:

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                          | Example    | Default |
| ------ | ----------------------------------- | ---------- | ------- |
| Domain | Name of the file system to write to | dataLakeFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Write action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to write to",
          "example": "dataLakeFs"
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
| Parameter | Definition                                                                                               | Example        | Default |
| --------- | -------------------------------------------------------------------------------------------------------- | -------------- | ------- |
| Path Path | Json path in payload for directory path to list (from payload root)                                      | parameters.id  | path    |
| Mode      | Type of file(s) to output (sequence[^1], single[^2])                                                     | sequence       | single  |
| Prefix    | Prefix to add to each file's name (for sequence mode output)                                             | tracking/views | -       |
| Suffix    | Suffix to add to each file's name (for sequence mode output)                                             | \_out.json     | -       |
| Content   | Event contents to write to file (for sequence mode, event[^3], request[^4] or payload[^5])               | request        | payload |
| Overwrite | Whether to overwrite or append contents to output (for single mode output)                               | true           | false   |
| Base64    | Whether the input should be treated as base64 text and decoded for generating the file (e.g. image, PDF) | true           | false   |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Write action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for directory path to list (from payload root)",
              "default": "path",
              "example": "parameters.id"
            },
            "mode": {
              "type": "string",
              "definition": "Type of file(s) to output (sequence, single)",
              "enum": [
                "sequence",
                "single"
              ],
              "default": "single",
              "example": "sequence"
            },
            "prefix": {
              "type": "string",
              "definition": "Prefix to add to each file's name (for sequence mode output)",
              "example": "tracking/views"
            },
            "suffix": {
              "type": "string",
              "definition": "Suffix to add to each file's name (for sequence mode output)",
              "example": "_out.json"
            },
            "content": {
              "type": "string",
              "definition": "Event contents to write to file (for sequence mode, event, request or payload)",
              "enum": [
                "event",
                "request",
                "payload"
              ],
              "default": "payload",
              "example": "request"
            },
            "overwrite": {
              "type": "boolean",
              "definition": "Whether to overwrite or append contents to output (for single mode output)",
              "default": false,
              "example": true
            },
            "base64": {
              "type": "boolean",
              "definition": "Whether the input should be treated as base64 text and decoded for generating the file (e.g. image, PDF)",
              "default": false,
              "example": true
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

<details>

<summary>Example</summary>

Input

```json
{
    "customerId": "customer-1",
    "productId": "product-1",
    "pageId": "product-detail"
}
```

Event Metadata

![](<../../../../../.gitbook/assets/image (105).png>)

</details>

For sequence output mode, this handler uses a path writer, which is responsible for generating unique sequence file paths/names for wrting contents to. The writer can be configured by adding path.\* parameters to file system definition.

{% hint style="info" %}
Using "\[uuid]" in prefix or suffix allows generation of globally unique paths and file names, as this entry is replaced with a UUID value generated.
{% endhint %}

### Read / ReadFile

Reads and returns contents of a file (in a cachable manner):

{% tabs %}
{% tab title="Table" %}
| Field  | Definition                           | Example    | Default |
| ------ | ------------------------------------ | ---------- | ------- |
| Domain | Name of the file system to read from | contentsFs | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read action eventMeta fields",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "definition": "Name of the file system to read from",
          "example": "contentsFs"
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
| Parameter | Definition                                          | Example | Default |
| --------- | --------------------------------------------------- | ------- | ------- |
| Path Path | Json path in payload for directory path to list     | id      | path    |
| Format    | Format of the file contents to retrieve (json,text) | json    | text    |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Read action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "pathPath": {
              "type": "string",
              "definition": "Json path in payload for directory path to list",
              "default": "path",
              "example": "id"
            },
            "format": {
              "type": "string",
              "definition": "Format of the file contents to retrieve (json, text)",
              "enum": [
                "json",
                "text"
              ],
              "default": "text",
              "example": "json"
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

## Roles

### syncFile

Uses pulse records to trigger delete, copy or move of files from one file system to another, based on handler role parameters.

[^1]: Series of files (mostly used for data lakes)

[^2]: Single file

[^3]: Full event contents including metadata

[^4]: Event payload and request metadata

[^5]: Input payload with input pattern applied
