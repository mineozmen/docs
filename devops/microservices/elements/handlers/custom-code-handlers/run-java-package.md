---
description: >-
  This handler (com.rierino.handler.JarLoadedEventHandler) provides ability to
  load and execute dynamic Java packages during runtime.
---

# Run Java Package

## Handler Parameters

| Parameter     | Definition                                                                     | Example            | Default        |
| ------------- | ------------------------------------------------------------------------------ | ------------------ | -------------- |
| jar.state     | Name of the state manager storing jar definitions                              | customJars         | handler\_jar   |
| jar.domain    | Category of jars within the jar state manager that can be used by this handler | basket\_operations | -              |
| jar.local.dir | Local directory to store dynamic jars                                          | /tmp               | java.io.tmpdir |

## Actions

### Process / ProcessPackage

Passes requested event to dynamic event handler for processing. Event metadata parameters applicable for this action are as follows:

{% tabs %}
{% tab title="Table" %}
| Parameter       | Definition                             | Example            | Default |
| --------------- | -------------------------------------- | ------------------ | ------- |
| Dynamic Handler | Name of the handler to execute request | basket\_operations | -       |
| Dynamic Action  | Name of the action to call on handler  | PriceBasket        | -       |
{% endtab %}

{% tab title="JSON Schema" %}
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Run Java Package Process/ProcessPackage action eventMeta.parameters",
  "type": "object",
  "properties": {
    "eventMeta": {
      "type": "object",
      "properties": {
        "parameters": {
          "type": "object",
          "properties": {
            "dynamicHandler": {
              "type": "string",
              "definition": "Name of the handler to execute request",
              "example": "basket_operations",
              "default": null
            },
            "dynamicAction": {
              "type": "string",
              "definition": "Name of the action to call on handler",
              "example": "PriceBasket",
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

{% hint style="info" %}
All parameters applicable for the selected handler are also passed on by the JarLoadedEventHandler.
{% endhint %}

## Packages

Handler packages provide ability to deploy or replace packaged components during runtime. These packages are typically deployed using an JarLoadedEventHandler and can be automatically redeployed in production with a package pulse or journal.

It is possible to extend the languages that are supported for dynamic handler packages with new event handlers. Rierino is shipped with dynamic package deployment for Java with the following properties:

* **Name**: Descriptive name of the handler package
* **Status**: Whether the handler package should still execute or not
* **Description**: Detailed explanation of the package contents
* **Package Files**: List and URLs of files to load for the package
* **Package Handlers**: List and fully qualified class names of handlers inside the package

{% hint style="info" %}
While hot swapping packages during runtime has its risks, this feature allows for faster deployment of mission critical changes or simpler CI/CD processes during development.
{% endhint %}
