---
description: >-
  Custom code handlers are included in all Rierino platform versions, providing
  ability to incorporate highly customized logic and actions
---

# Custom Code Handlers

Custom code handlers support dynamic scripts, runtime-loaded Java code, hot-swapped packages, and Python procedures.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Built-in runtime behavior

Use [Custom Code Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/) for action-level saga step fields and parameters.

### Run Scripts

Class: `com.rierino.handler.ScriptLoadedEventHandler`

Executes stored scripts through Javax scripting engines.

#### Handler parameters

| Parameter     | Definition                             | Example              | Default        |
| ------------- | -------------------------------------- | -------------------- | -------------- |
| `code.state`  | State manager storing code definitions | `customJSCodes`      | `handler_code` |
| `code.domain` | Code category allowed for this handler | `custom_calculation` | -              |

Action details: [Run Scripts](../../../api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-scripts.md)

### Run Java Code

Class: `com.rierino.handler.OpenHFTEventHandler`

Loads and executes dynamic Java handler code during runtime.

#### Handler parameters

| Parameter        | Definition                                 | Example             | Default          |
| ---------------- | ------------------------------------------ | ------------------- | ---------------- |
| `code.state`     | State manager storing code definitions     | `customCodes`       | `handler_code`   |
| `code.domain`    | Code category allowed for this handler     | `basket_operations` | -                |
| `code.local.dir` | Local directory used to store dynamic code | `/tmp`              | `java.io.tmpdir` |

Action details: [Run Java Code](../../../api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-java-code.md)

### Run Java Package

Class: `com.rierino.handler.JarLoadedEventHandler`

Loads and executes packaged Java handlers during runtime.

#### Handler parameters

| Parameter       | Definition                            | Example             | Default          |
| --------------- | ------------------------------------- | ------------------- | ---------------- |
| `jar.state`     | State manager storing jar definitions | `customJars`        | `handler_jar`    |
| `jar.domain`    | Jar category allowed for this handler | `basket_operations` | -                |
| `jar.local.dir` | Local directory used to store jars    | `/tmp`              | `java.io.tmpdir` |

Action details: [Run Java Package](../../../api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-java-package.md)

### Run Python Procedure

Class: `com.rierino.handler.py4j.Py4JEventHandler`

Calls Python code through Py4J for runtime event processing.

#### Handler parameters

This handler has no special parameters.

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'py4j', version:"${rierinoVersion}")
```

#### Runtime note

This handler typically requires an extra container alongside the runner, such as `ghcr.io/rierino-open/py-runner`.

Action details: [Run Python Procedure](../../../api-event-and-process-flows/configuring-saga-steps/event-step/custom-code-actions/run-python-procedure.md)
