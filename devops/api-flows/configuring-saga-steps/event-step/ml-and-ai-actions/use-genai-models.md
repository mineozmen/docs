---
description: These actions provide ability to use GenAI models from various providers.
---

# Use GenAI Models

## Use GenAI Models Actions

### LLMChat

Performs a chat interaction with a target LLM model provider. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example       | Default |
| -------------- | -------------------------------------------------- | ------------- | ------- |
| Domain         | ID of the model to use                             | chatgpt\_chat | -       |
| Input Element  | Json path for the input in request event payload   | message       | -       |
| Output Element | Json path for the output in response event payload | output        | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "description": "ID of the model to use",
          "examples": ["chatgpt_chat"],
          "default": null
        },
        "inputElement": {
          "type": "string",
          "description": "Json path for the input in request event payload",
          "examples": ["message"],
          "default": null
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
          "default": null
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

Input element can include the following fields:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "chat": {
      "type": "string",
      "description": "ID of an ongoing chat for an assistant type use case with memory",
      "examples": ["test-chat"],
      "default": null
    },
    "message": {
      "description": "A text message or an object representing complex chat contents",
      "default": null,
      "oneOf": [
        {
          "type": "string",
          "examples": ["Hello"]
        },
        {
          "type": "object",
          "examples": [
            {
              "text": "Can you describe this image",
              "imageBase64": "base64"
            }
          ],
          "properties": {
            "text": {
              "type": "string",
              "description": "Text part of the user message",
              "examples": ["Hello"],
              "default": null
            },
            "textFile": {
              "type": "string",
              "description": "URI of a user message stored as a file",
              "examples": ["https://example.com/message.txt"],
              "default": null
            },
            "image": {
              "type": "string",
              "description": "URI of an image attachment, can be URL or data URI",
              "examples": ["data:image/png;base64,iVBORw0KGgoAAAANSUhE..."],
              "default": null
            },
            "imageBase64": {
              "type": "string",
              "description": "Base64 data of an image attachment",
              "examples": ["iVBORw0KGgoAAAANSUhE..."],
              "default": null
            },
            "imageBase64Mime": {
              "type": "string",
              "description": "Mime type of base64 image",
              "examples": ["image/jpg"],
              "default": null
            },
            "pdf": {
              "type": "string",
              "description": "URI of a PDF attachment",
              "examples": ["https://example.com/file.pdf"],
              "default": null
            },
            "pdfBase64": {
              "type": "string",
              "description": "Base64 data of a PDF attachment",
              "examples": ["iVBORw0KGgoAAAANSUhE..."],
              "default": null
            },
            "audio": {
              "type": "string",
              "description": "URI of an audio attachment",
              "examples": ["https://example.com/audio.mp3"],
              "default": null
            },
            "audioBase64": {
              "type": "string",
              "description": "Base64 data of an audio attachment",
              "examples": ["iVBORw0KGgoAAAANSUhE..."],
              "default": null
            },
            "video": {
              "type": "string",
              "description": "URI of a video attachment",
              "examples": ["https://example.com/video.mp4"],
              "default": null
            },
            "videoBase64": {
              "type": "string",
              "description": "Base64 data of a video attachment",
              "examples": ["iVBORw0KGgoAAAANSUhE..."],
              "default": null
            }
          }
        }
      ]
    },
    "messages": {
      "type": "array",
      "description": "Array of message entries",
      "default": null,
      "items": {
        "type": "object",
        "properties": {
          "text": {
            "type": "string",
            "description": "Text part of the user message",
            "default": null
          }
        }
      }
    },
    "prompt": {
      "type": "string",
      "description": "ID of the prompt to generate an additional text based user message",
      "examples": ["search-prompt"],
      "default": null
    },
    "input": {
      "type": "object",
      "description": "Data input elements to be used for populating prompt",
      "examples": [{"search": "keyword"}],
      "default": null
    }
  }
}
```

{% hint style="info" %}
Base64 contents can also be passed as data URI values with the correct prefix.
{% endhint %}

With event metadata parameters as:

{% tabs %}
{% tab title="Parameters (table)" %}
| Parameter       | Definition                                                                                                                                       | Example                                | Default |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- | ------- |
| Message Pattern | Used for tool sagas, allowing merging data from original event to saga call, with `arguments` as agent-generated input and `payload` as original | merge(arguments, {user: payload.user}) | -       |
| Full Result     | Whether response should include full AI call details, such as token counts and tool executions                                                   | true                                   | false   |
| Json Response   | Whether model response should be automatically parsed as a JSON object                                                                           | true                                   | false   |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "messagePattern": {
              "type": "string",
              "description": "Used for tool sagas, allowing merging data from original event to saga call",
              "examples": ["merge(arguments, {user: payload.user})"],
              "default": null
            },
            "fullResult": {
              "type": "boolean",
              "description": "Whether response should include full AI call details",
              "examples": [true],
              "default": false
            },
            "jsonResponse": {
              "type": "boolean",
              "description": "Whether model response should be automatically parsed as a json object",
              "examples": [true],
              "default": false
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

### LLMGenerateImage

Performs an image generation with a target LLM model provider. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example                      | Default |
| -------------- | -------------------------------------------------- | ---------------------------- | ------- |
| Domain         | ID of the model to use                             | dalle\_gen                   | -       |
| Input Element  | Json path for the input in request event payload   | {prompt: "", imageCount: ""} | -       |
| Output Element | Json path for the output in response event payload | imageBase64                  | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "description": "ID of the model to use",
          "examples": ["dalle_gen"],
          "default": null
        },
        "inputElement": {
          "description": "Json path for the input in request event payload",
          "default": null,
          "oneOf": [
            {
              "type": "string",
              "examples": ["parameters"]
            },
            {
              "type": "object",
              "examples": [{"prompt": "", "imageCount": ""}]
            }
          ]
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["imageBase64"],
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
{% tab title="Parameters (table)" %}
| Parameter   | Definition                                           | Example | Default |
| ----------- | ---------------------------------------------------- | ------- | ------- |
| Full Result | Whether response should include full AI call details | true    | false   |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "fullResult": {
              "type": "boolean",
              "description": "Whether response should include full AI call details",
              "examples": [true],
              "default": false
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

### LLMEditImage

Performs an image edit with a target LLM model provider. Event metadata fields applicable for this action are as follows:

{% tabs %}
{% tab title="Fields (table)" %}
| Field          | Definition                                         | Example                                                                                   | Default |
| -------------- | -------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------- |
| Domain         | ID of the model to use                             | dalle\_gen                                                                                | -       |
| Input Element  | Json path for the input in request event payload   | {prompt: "", image: {revisedPrompt: "", base64Data: "", mimeType: "", url: ""}, mask: ""} | -       |
| Output Element | Json path for the output in response event payload | output                                                                                    | -       |
{% endtab %}

{% tab title="Fields (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "domain": {
          "type": "string",
          "description": "ID of the model to use",
          "examples": ["dalle_gen"],
          "default": null
        },
        "inputElement": {
          "description": "Json path for the input in request event payload",
          "default": null,
          "oneOf": [
            {
              "type": "string",
              "examples": ["parameters"]
            },
            {
              "type": "object",
              "examples": [
                {
                  "prompt": "",
                  "image": {
                    "revisedPrompt": "",
                    "base64Data": "",
                    "mimeType": "",
                    "url": ""
                  },
                  "mask": ""
                }
              ]
            }
          ]
        },
        "outputElement": {
          "type": "string",
          "description": "Json path for the output in response event payload",
          "examples": ["output"],
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
{% tab title="Parameters (table)" %}
| Parameter   | Definition                                           | Example | Default |
| ----------- | ---------------------------------------------------- | ------- | ------- |
| Full Result | Whether response should include full AI call details | true    | false   |
{% endtab %}

{% tab title="Parameters (JSON Schema)" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "fullResult": {
              "type": "boolean",
              "description": "Whether response should include full AI call details",
              "examples": [true],
              "default": false
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
