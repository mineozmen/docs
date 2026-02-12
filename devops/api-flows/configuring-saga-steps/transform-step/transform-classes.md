# Transform Classes

It is possible to define any number and type of new transformation classes with specialized configurations. Rierino is shipped with the following default classes:

## Payload Manipulation

### Add Value To Payload

This class(_com.rierino.handler.transform.AddToPayloadTransform_) extends a selected payload element with a constant Json node provided as string.

| Parameter | Definition                                      | Example        |
| --------- | ----------------------------------------------- | -------------- |
| element   | Json path of payload element to create / extend | parameters     |
| value     | Json string to extend element with              | {"test":false} |

### Transform Payload with JMESPath

This class(_com.rierino.handler.transform.JMESPayloadTransform_) transforms event payload using a JMESPath pattern.

| Parameter      | Definition                                                                            | Example                                                      |
| -------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| pattern        | JMESPath pattern to calculate value                                                   | {language:parameters.language, currency:parameters.currency} |
| patternElement | Json path to payload element which defines JMESPath pattern to calculate value        | parameters.pattern                                           |
| extend         | Whether calculated value should replace payload or extend it                          | true                                                         |
| store          | Whether JMESPath expression should be cached for future executions (defaults to true) | false                                                        |
| inputElement   | Json path for the input element to transform                                          | parameters                                                   |
| outputElement  | Json path to output calculated results                                                | result                                                       |
| errorAs        | Element to include error message on, if the pattern fails                             | error                                                        |

{% embed url="https://jmespath.org" %}
JMESPath Page
{% endembed %}

### Transform Payload Array with JMESPath

This class(_com.rierino.handler.transform.JMESArrayPayloadTransform_) transforms array elements of an event payload using a JMESPath pattern with access to a parent data element as well (which is not supported by JMESPath out of box).

| Parameter      | Definition                                                                                                        | Example                                            |
| -------------- | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| pattern        | JMESPath pattern to calculate value                                                                               | {parentValue: data.value, entryValue: entry.value} |
| patternElement | Json path to payload element which defines JMESPath pattern to calculate value                                    | parameters.pattern                                 |
| dataElement    | Json path to payload element which will populate "data" field in JMES input                                       | parameters.parent                                  |
| arrayElement   | Json path to payload element which is an array will populate "entry" field in JMES input with each of its entries | parameters.results                                 |
| entryPath      | Json path to extract specific element from array "entry" to simplify JMESPath patterns                            | data.main                                          |
| filter         | Whether JMESPath returns a boolean value for filtering arrayElement only (defaults to true)                       | false                                              |
| excludeNulls   | Whether null results should be excluded from JMESPath results (defaults to true)                                  | false                                              |
| outputElement  | Json path to output results on event payload (null replaces arrayElement itself)                                  | output                                             |
| store          | Whether JMESPath expression should be cached for future executions (defaults to true)                             | false                                              |
| errorAs        | Element to include error message on, if the pattern fails                                                         | error                                              |

### Add Expression To Payload

This class(_com.rierino.handler.transform.AddExpToPayloadTransform_) adds the result of an expression to event payload.

| Parameter  | Definition                                                  | Example |
| ---------- | ----------------------------------------------------------- | ------- |
| element    | Json path of payload element to use / extend                | date    |
| as         | Json path to return results in (null for replacing element) | result  |
| expression | Expression to execute                                       | now     |

This transformation allows additional parameters based on the expression value:

#### replace

Replaces given char sequence in element with a new char sequence.

| Parameter   | Definition            | Example |
| ----------- | --------------------- | ------- |
| target      | Text to find          | old     |
| replacement | Text to replace with  | new     |

#### replaceAll

Replaces all matchings of a regex in element with a new char sequence.

| Parameter   | Definition            | Example |
| ----------- | --------------------- | ------- |
| target      | Regex to find         | \[0-9]  |
| replacement | Text to replace with  | 0       |

#### regex

Replaces named parameters found from a regex in element with given pattern.

| Parameter   | Definition               | Example                                   |
| ----------- | ------------------------ | ----------------------------------------- |
| target      | Regex to find            | /source/(?\<folder>\\\w+)/(?\<file>\\\w+) |
| replacement | Pattern to replace with  | /target/${folder}/${file}                 |

#### substring

Returns substring from element.

| Parameter  | Definition                                        | Example |
| ---------- | ------------------------------------------------- | ------- |
| beginIndex | Char index to start from                          | 0       |
| endIndex   | Char index to end with (null means till the end)  | 5       |

#### split

Splits element into an array with a given delimiter.

| Parameter | Definition             | Example |
| --------- | ---------------------- | ------- |
| delimiter | Regex delimiter to use | ,       |

#### join

Joins an element array with a given delimiter.

| Parameter | Definition                 | Example |
| --------- | -------------------------- | ------- |
| delimiter | Delimiter to join elements | ,       |

#### concat

Concatenates element with a prefix and/or suffix.

| Parameter | Definition       | Example |
| --------- | ---------------- | ------- |
| prefix    | Prefix to append | pre\_   |
| suffix    | Suffix to append | \_post  |

#### kv

Performs a lookup for configuration parameter value - such as secrets or connection parameters.

| Parameter | Definition                                    | Example                |
| --------- | --------------------------------------------- | ---------------------- |
| parameter | Parameter reference                           | rierino.mongo.main.uri |
| type      | Lookup/injection type (static, dynamic, both) | static                 |

### Lookup & Map within Payload

This class(_com.rierino.handler.transform.LookupPayloadTransform_) traverses a list of ids in event payload and enriches them with another list matching on ids.

| Parameter  | Definition                                                                        | Example |
| ---------- | --------------------------------------------------------------------------------- | ------- |
| lookup     | Json path of payload element to lookup from                                       | lookup  |
| lookupId   | Name of id fields in lookup array                                                 | id      |
| traverse   | Json traverse expression of payload element to enrich                             | list.\* |
| traverseId | Name of id fields in traverse entries                                             | id      |
| as         | Name of field to inject lookup values in traversed path (null for merged results) | -       |

### Use Script on Payload

This class(_com.rierino.handler.transform.ScriptingPayloadTransform_) allows using scripting languages to transform event payload.

| Parameter   | Definition                                                               | Example                                          |
| ----------- | ------------------------------------------------------------------------ | ------------------------------------------------ |
| script      | Full scripting code returning output (or editing event payload directly) | value \* 2                                       |
| language    | Scripting engine to use (if helperClass is not defined)                  | groovy                                           |
| helperClass | Java class name for the scripting engine helper                          | com.rierino.handler.util.helper.JavaScriptHelper |
| nodePath    | Json path to the input node in event payload                             | parameters                                       |
| paramsPath  | Key-value pairs for passing multiple variables from different Json paths | foo=data.in,bar=data.val                         |
| outputPath  | Json path to output on event payload                                     | data.output                                      |

### Generate ID in Payload

This class(_com.rierino.handler.transform.GenerateIDTransform_) uses an [ID generator](../../../microservices/elements/state-managers/id-generators.md) to create a unique ID inside the payload.

| Parameter   | Definition                                                               | Example                                                                 |
| ----------- | ------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| element     | Json path to add generated id on                                         | id                                                                      |
| generator   | ID generator class                                                       | com.rierino.handler.util.generator.NumberIDGenerator                    |
| generatorId | [Unique ID](#user-content-fn-1)[^1] assigned to generator                | product\_gen                                                            |
| version     | ID generator version to use                                              | 1                                                                       |
| \*          | All additional parameters that are applicable for the ID generator class | numberGenerator=com.rierino.handler.util.generator.EpochNumberGenerator |

{% hint style="warning" %}
ID generators are cached inside GenerateIDTransform by generatorId value, so, parameter changes are not applied unless the service is restarted or version parameter value is updated. Both actions create a new instance of the generator.
{% endhint %}

### Copy In Payload

This class(_com.rierino.handler.transform.CopyInPayloadTransform_) copies data from one section of payload to another.

| Parameter | Definition                                        | Example            |
| --------- | ------------------------------------------------- | ------------------ |
| from      | Json path to copy from                            | parameters.filters |
| to        | Json path to copy to                              | filters            |
| move      | Whether element should be moved instead of a copy | true               |

### Remove From Payload

This class(_com.rierino.handler.transform.ReducePayloadTransform_) removes list of elements from event payload.

| Parameter | Definition                                   | Example                           |
| --------- | -------------------------------------------- | --------------------------------- |
| list      | Comma separated list of Json paths to remove | product.metadata,product.internal |

### Do Nothing

Thıs class(com.rierino.handler.transform.NoopTransform) performs no action on the event, and is typically used as a placeholder.

## Metadata Access

### Add Error To Payload

This class(_com.rierino.handler.transform.AddErrorToPayloadTransform_) copies event error details to event payload.

| Parameter | Definition                                      | Example |
| --------- | ----------------------------------------------- | ------- |
| element   | Json path of payload element to create / extend | error   |

### Add Metadata To Payload

This class(_com.rierino.handler.transform.AddMetaToPayloadTransform_) copies request metadata to event payload as "meta" element.

### Append Partition To Payload

This class(_com.rierino.handler.transform.AppendPartitionToPayloadTransform_) appends partition digits from a payload element to another payload element. Typically used for creating a linked entity with the same partition as a master entity.

| Parameter | Definition                                    | Example   |
| --------- | --------------------------------------------- | --------- |
| from      | Json path for id to get partition digits from | guest.id  |
| to        | Json path to add partition digits to          | basket.id |
| char      | Delimiter before the partition digits         | -         |

### Add Event Parameter To Payload

This class(_com.rierino.handler.transform.EventParameterToPayloadTransform_) copies list of elements from full event contents to payload. Parameters defined for this transform step act as key value pairs, where keys are the target payload paths and values are the event source content paths (e.g. sessionId=requestMeta.sessionId).

### Add Request Id To Payload

This class(_com.rierino.handler.transform.RequestIdToPayloadTransform_) adds requestId to event payload.

| Parameter | Definition                                      | Example   |
| --------- | ----------------------------------------------- | --------- |
| element   | Json path of payload element to create / extend | requestId |

### Set Event Result

This class(_com.rierino.handler.transform.SetResultTransform_) resets the result data of an event, typically used for returning specific contents in case of an error.

| Parameter        | Definition                                                                                  | Example                     |
| ---------------- | ------------------------------------------------------------------------------------------- | --------------------------- |
| status           | Result status to set for the event                                                          | FAIL                        |
| errorPattern     | JMES pattern to apply on event payload to return sensitive error payload (i.e. logged only) | {"failQuery": query}        |
| errorSafePattern | JMES pattern to apply on event payload to return safe error details (i.e. passed to client) | {"failId": id}              |
| errorCode        | Error code to return for the event                                                          | 123                         |
| errorMessage     | Error message to return for the event                                                       | Could not find requested id |
| errorHttp        | Http error code to return for the event                                                     | 404                         |

### Add Claims To Payload

This class(_com.rierino.handler.transform.AddClaimsToPayloadTransform_) adds claims from request metadata (if configured in gateway token) to event payload.

| Parameter | Definition                                                          | Example |
| --------- | ------------------------------------------------------------------- | ------- |
| element   | Json path of payload element to create / extend (claims as default) | user    |

[^1]: Should not overlap with existing states or other ID generators
