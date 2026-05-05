---
description: >-
  Core handlers are included in all Rierino platform versions and define the
  built-in handler classes used by the most common workflows.
---

# Core Handlers

## Core Handlers

Core handlers define the built-in event handler classes available in every Rierino installation.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Roles and commands

Use [Core Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/) for action-level saga step fields and parameters.

### Data & Rules

#### Write Data

Class: `com.rierino.handler.WriteEventHandler`

Creates, updates, and deletes records in a state manager. It is typically used for write-side API calls.

**Handler parameters**

| Parameter | Definition                                            | Example | Default |
| --------- | ----------------------------------------------------- | ------- | ------- |
| `useDiff` | Generate journals with only actual changes by default | `true`  | `false` |

Action details: [Write Data](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/write-data.md)

#### Read Data

Class: `com.rierino.handler.ReadEventHandler`

Reads one, many, or all records from a state manager. It is typically used for read-side API calls.

**Handler parameters**

This handler requires no parameters. It supports caching.

Action details: [Read Data](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/read-data.md)

#### Query Data

Class: `com.rierino.handler.QueryEventHandler`

Executes or produces system-specific queries through a query manager. It is used for pass-through querying and more complex filtering.

**Handler parameters**

| Parameter     | Definition                              | Example         | Default |
| ------------- | --------------------------------------- | --------------- | ------- |
| `query.state` | State manager storing query definitions | `customQueries` | `query` |

This handler supports caching.

Action details: [Query Data](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/query-data.md)

#### Apply Rules

Class: `com.rierino.handler.SimpleRuleEventHandler`

Evaluates simple rules and returns the best match or all matches.

**Handler parameters**

| Parameter     | Definition                             | Example | Default |
| ------------- | -------------------------------------- | ------- | ------- |
| `rules.state` | State manager storing rule definitions | `rules` | -       |

Action details: [Apply Rules](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/apply-rules.md)

### Integration & Content

#### Call Rest API

Class: `com.rierino.handler.RestAPIEventHandler`

Calls internal or external REST services and returns their results for integration flows.

**Handler parameters**

This handler requires no parameters. The target system named in `domain` supplies the base URL and credentials for each call. This handler supports caching.

**Commands**

* `RECONFIGURE` reloads REST target system properties with latest values.

Action details: [Call Rest API](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/call-rest-api.md)

#### Generate Text/Html

Class: `com.rierino.handler.TemplateEventHandler`

Produces text output from templates and input payload. Typical outputs include plain text and HTML.

**Handler parameters**

| Parameter    | Definition                              | Example                                                    | Default        |
| ------------ | --------------------------------------- | ---------------------------------------------------------- | -------------- |
| `code.state` | State manager with template definitions | `template_code`                                            | `handler_code` |
| `code.field` | Data field holding template contents    | `template`                                                 | `code`         |
| `helper`     | Template helper class                   | `com.rierino.handler.util.helper.HandlebarsTemplateHelper` | -              |

This handler supports caching.

**Runtime dependency**

```gradle
implementation (group:'com.rierino.processors', name: 'handlebars', version:"${rierinoVersion}")
```

**Helpers**

* `com.rierino.handler.util.helper.HandlebarsTemplateHelper`
* `com.rierino.handler.util.helper.FreemarkerTemplateHelper`

Action details: [Generate Text/Html](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/generate-text-html.md)

#### Parse Html

Class: `com.rierino.handler.JsoupEventHandler`

Parses HTML text or a URL response and returns structured JSON.

**Handler parameters**

This handler requires no parameters.

Action details: [Parse Html](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/parse-html.md)

#### Send/Receive Emails

Class: `com.rierino.handler.MailEventHandler`

Sends emails and, when enabled, reads email details from an existing mail server.

**Handler parameters**

| Parameter  | Definition                          | Example | Default |
| ---------- | ----------------------------------- | ------- | ------- |
| `system`   | System defining email server access | `gmail` | -       |
| `sendOnly` | Send only without receiving         | `true`  | `false` |

**Runtime dependency**

```gradle
implementation (group:'com.rierino.processors', name: 'mail', version:"${rierinoVersion}")
```

Action details: [Send/Receive Emails](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/send-receive-emails.md)

### Security & Operations

#### Generate Secrets

Class: `com.rierino.handler.SecretEventHandler`

Encrypts, decrypts, hashes, validates hashes, generates tokens, and creates certificates.

**Handler parameters**

| Parameter                       | Definition                              | Example         | Default                     |
| ------------------------------- | --------------------------------------- | --------------- | --------------------------- |
| `key.state`                     | State manager with key definitions      | `secret_key`    | -                           |
| `key`                           | Constant key for signing operations     | `1234567890ABC` | -                           |
| `encryptkey`                    | Constant key for encryption operations  | `1234567890ABC` | -                           |
| `provider`                      | Security provider to use                | `BC`            | `auto`                      |
| `issuer`                        | Issuer included in generated tokens     | `Rierino`       | -                           |
| `algorithm`                     | Default encryption algorithm            | -               | `AES/ECB/PKCS5Padding`      |
| `keyAlgorithm`                  | Default key generation algorithm        | -               | `AES`                       |
| `hashAlgorithm`                 | Default hashing algorithm               | -               | `SHA-256`                   |
| `certificateKeySize`            | Default certificate key size            | -               | `2048`                      |
| `certificateAlgorithm`          | Default certificate key algorithm       | -               | `RSA`                       |
| `certificateSignatureAlgorithm` | Default certificate signature algorithm | -               | `SHA256withRSA`             |
| `certificateLifetime`           | Default certificate lifetime in days    | -               | `1`                         |
| `certificateDN`                 | Default distinguished name              | -               | `CN=rierino.com, O=Rierino` |

Action details: [Generate Secrets](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/generate-secrets.md)

#### Orchestrate User Task

Class: `com.rierino.handler.TaskEventHandler`

Adds human tasks and time-based waits to APIs and backend flows.

**Handler parameters**

| Parameter           | Definition                                    | Example                     | Default             |
| ------------------- | --------------------------------------------- | --------------------------- | ------------------- |
| `task.action`       | Action to apply on process records on proceed | `delete`                    | `update`            |
| `role.domain`       | State manager storing task process data       | `workflow_product`          | `workflow`          |
| `role.idPath`       | Process id field in role data                 | `id`                        | `processId`         |
| `role.enrichPath`   | Role data path used for enrichment on proceed | `data`                      | -                   |
| `role.fromTimePath` | Role data path used as timeout start          | `fromDT`                    | `fromTime`          |
| `role.toTimePath`   | Role data path used as timeout end            | `toDT`                      | `toTime`            |
| `timer.onFail`      | Fail strategy for timeout timer processing    | `DLQ`                       | default action fail |
| `timer.dlq.stream`  | DLQ stream for timeout failures               | `timer_fail`                | -                   |
| `timeout.states`    | States monitored for timeouts                 | `workflow,workflow_product` | -                   |
| `timeoutBuffer`     | Buffer size for timeout processing            | `100`                       | -                   |
| `timerMs`           | Milliseconds between timeout calculations     | `60000`                     | `2000`              |

**Roles**

* `proceedTask`
* `timeoutTask`
* `timeoutBetween`

**Commands**

* `TIMEOUT`

Action details: [Orchestrate User Task](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/orchestrate-user-task.md)

#### Perform File Operation

Class: `com.rierino.handler.FSEventHandler`

Interacts with local or remote file systems for directory and file operations.

**Handler parameters**

| Parameter        | Definition                                            | Example                                 | Default |
| ---------------- | ----------------------------------------------------- | --------------------------------------- | ------- |
| `systems`        | Comma-separated file systems available to the handler | `imageFs,cdnFs`                         | -       |
| `role.source`    | Source system for sync file role calls                | `imageFs`                               | -       |
| `role.target`    | Target system for sync file role calls                | `cdnFs`                                 | -       |
| `role.fromRegex` | Regex for filtering source files                      | `/source/(?<folder>\\w+)/(?<file>\\w+)` | -       |
| `role.toPattern` | Pattern for converting source paths to target paths   | `/target/${folder}/${file}`             | -       |
| `role.move`      | Move instead of copy during role calls                | `true`                                  | `false` |

**Roles**

* `syncFile`

Action details: [Perform File Operation](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/perform-file-operation.md)

#### Run Shell Command

Class: `com.rierino.handler.ShellEventHandler`

Executes shell commands and can track their process state.

**Handler parameters**

| Parameter      | Definition                                    | Example   | Default |
| -------------- | --------------------------------------------- | --------- | ------- |
| `processState` | State manager recording shell process results | `process` | -       |

Action details: [Run Shell Command](../../../api-event-and-process-flows/configuring-saga-steps/event-step/core-actions/run-shell-command.md)
