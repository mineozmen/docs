---
description: >-
  Specialized handlers facilitate more advanced operations and are only
  available in Rierino Core+ edition
---

# Specialized Handlers

Specialized handlers cover advanced rule engines, CEP, document generation, SOAP, Camel, and Web of Things integrations.

Use this page for handler-level details:

* Class names
* Handler parameters
* Runtime dependencies
* Roles and commands

Use [Specialized Actions](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/) for action-level saga step fields and parameters.

### Apply Advanced Rules

Class: `com.rierino.handler.drools.DroolsProcessEventHandler`

Executes business rules using Drools BRMS.

#### Handler parameters

| Parameter      | Definition                                      | Example                 | Default |
| -------------- | ----------------------------------------------- | ----------------------- | ------- |
| `config.state` | State manager with rule domain configurations   | `ruledomain`            | -       |
| `domain`       | Rule domain name                                | `product_discount`      | -       |
| `rules.state`  | State manager with rule definitions             | `rule_product_discount` | -       |
| `buffer`       | Number of records to buffer before batch output | `100`                   | -       |

This handler supports caching.

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'drools', version:"${rierinoVersion}")
```

#### Roles

* `processPulse`
* `processJournal`
* `processRole`

#### Commands

* `PROCESSID`

Action details: [Apply Advanced Rules](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/apply-advanced-rules.md)

### Calculate Real-time Metrics

Class: `com.rierino.handler.siddhi.JsonSiddhiEventHandler`

Builds real-time intelligence and CEP flows using Siddhi.

#### Handler parameters

| Parameter               | Definition                                      | Example                    | Default          |
| ----------------------- | ----------------------------------------------- | -------------------------- | ---------------- |
| `dataflow.state`        | State manager with dataflow configurations      | `dataflow`                 | -                |
| `dataflow.ids`          | Dataflow ids to run on this instance            | `session_dna,customer_dna` | -                |
| `partition.stream`      | Stream supplying partition information          | `sessionq`                 | -                |
| `dataflow.store.system` | System storing Siddhi persistence data          | `hdfs_default`             | -                |
| `dataflow.local.dir`    | Local directory storing Siddhi persistence data | `/tmp`                     | `java.io.tmpdir` |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'siddhi', version:"${rierinoVersion}")
```

#### Roles

* `processRole`

Action details: [Calculate Real-time Metrics](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/calculate-real-time-metrics.md)

### Consume Web of Things

Class: `com.rierino.handler.wot.WOTConsumerEventHandler`

Reads properties from WoT things and invokes their actions.

#### Handler parameters

| Parameter      | Definition                            | Example   | Default |
| -------------- | ------------------------------------- | --------- | ------- |
| `thing.state`  | State manager with WoT configurations | `iot`     | `thing` |
| `thing.domain` | Thing domain to include               | `factory` | -       |
| `saga.handler` | Saga handler used for tool sagas      | `ai_saga` | `saga`  |

Action details: [Consume Web of Things](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/consume-web-of-things.md)

### Produce Web of Things

Class: `com.rierino.handler.wot.WOTProducerEventHandler`

Builds WoT thing descriptions from allowed sagas.

#### Handler parameters

| Parameter    | Definition                          | Example | Default |
| ------------ | ----------------------------------- | ------- | ------- |
| `saga.state` | State manager with saga definitions | -       | `saga`  |

Action details: [Produce Web of Things](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/produce-web-of-things.md)

### Generate Excel

Class: `com.rierino.handler.ExcelExportEventHandler`

Produces formatted Excel files from templates and input payload.

#### Handler parameters

| Parameter        | Definition                                 | Example         | Default         |
| ---------------- | ------------------------------------------ | --------------- | --------------- |
| `template.state` | State manager with Excel templates         | `template_xlsx` | `handler_excel` |
| `output.system`  | System alias used to store generated files | `s3_default`    | -               |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'excel', version:"${rierinoVersion}")
```

Action details: [Generate Excel](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/generate-excel.md)

### Generate PDF

Class: `com.rierino.handler.PDFExportEventHandler`

Produces PDF files from HTML content.

#### Handler parameters

| Parameter       | Definition                                 | Example      | Default |
| --------------- | ------------------------------------------ | ------------ | ------- |
| `output.system` | System alias used to store generated files | `s3_default` | -       |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'pdf', version:"${rierinoVersion}")
```

Action details: [Generate PDF](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/generate-pdf.md)

### Call SOAP API

Class: `com.rierino.handler.soap.SoapEventHandler`

Uses SOAP services for third-party integrations.

#### Handler parameters

| Parameter        | Definition                                     | Example         | Default            |
| ---------------- | ---------------------------------------------- | --------------- | ------------------ |
| `soap.state`     | State manager keeping SOAP service definitions | `soap_services` | `handler_soap`     |
| `soap.domain`    | Domain name used to filter SOAP definitions    | `pricing`       | -                  |
| `soap.local.dir` | Local directory used for service jars          | `/tmp`          | `[java.io.tmpdir]` |

#### Runtime dependency

```gradle
implementation (group:'com.rierino.custom', name: 'soap', version:"${rierinoVersion}")
```

Action details: [Call SOAP API](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/call-soap-api.md)

### Integrate with Camel

Class: `com.rierino.handler.camel.CamelEventHandler`

Uses Apache Camel producers for third-party integrations.

#### Handler parameters

This handler has no parameters. It uses configured runner systems as potential Camel producers.

#### Runtime dependency

```gradle
implementation (group:'com.rierino.runner', name: 'camel', version:"${rierinoVersion}")
```

Action details: [Integrate with Camel](../../../api-event-and-process-flows/configuring-saga-steps/event-step/specialized-actions/integrate-with-camel.md)
