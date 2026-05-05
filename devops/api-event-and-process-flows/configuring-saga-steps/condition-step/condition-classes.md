# Condition Classes

It is possible to define any number and type of new condition classes with specialized configurations. Rierino is shipped with a number of default classes.

## ElementValueCondition

This class(_com.rierino.handler.saga.condition.ElementValueCondition_) returns the value of a specific payload element to be used as a condition value.

| Parameter | Definition                      | Example             |
| --------- | ------------------------------- | ------------------- |
| element   | Json path of the element to use | parameters.language |

## ErrorCodeCondition

This class(_com.rierino.handler.saga.condition.ErrorCodeCondition_) returns the value of event error code (or '-' if null) to be used as a condition value.

## ErrorHttpCondition

This class(_com.rierino.handler.saga.condition.ErrorHttpCondition_) returns the http status code of event error (or '-' if null) to be used as a condition value.

## EventStatusCondition

This class(_com.rierino.handler.saga.condition.EventStatusCondition_) returns the status name of event result (or '-' if null) to be used as a condition value.

## HasElementCondition

This class(_com.rierino.handler.saga.condition.HasElementCondition_) returns "true" or "false" based on whether a specific payload element exists or not.

| Parameter | Definition                      | Example     |
| --------- | ------------------------------- | ----------- |
| element   | Json path of the element to use | customer.id |

## JMESCondition

This class(_com.rierino.handler.saga.condition.JMESCondition_) returns a text value based on evaluation of a JMESPath pattern.

| Parameter      | Definition                                                                            | Example              |
| -------------- | ------------------------------------------------------------------------------------- | -------------------- |
| pattern        | JMESPath pattern to apply                                                             | language=='enUS'     |
| patternElement | Json path to payload element which defines JMESPath pattern to apply                  | parameters.condition |
| store          | Whether JMESPath expression should be cached for future executions (defaults to true) | false                |
| inputElement   | Json path of the payload to run pattern on                                            | parameters           |

{% embed url="https://jmespath.org" %}
JMESPath Page
{% endembed %}

## JsonSchemaCondition

This class(_com.rierino.handler.saga.condition.JsonSchemaCondition_) evaluates event payload against a Json schema constraints and returns "success" or "fail".

| Parameter | Definition                           | Example           |
| --------- | ------------------------------------ | ----------------- |
| schema    | String representation of Json schema | {"type":"string"} |

{% embed url="https://json-schema.org" %}
Json Schema Page
{% endembed %}

## RegexCondition

This class(_com.rierino.handler.saga.condition.RegexCondition_) evaluates event payload against a regex expression and returns "true" or "false".

| Parameter | Definition                           | Example            |
| --------- | ------------------------------------ | ------------------ |
| element   | Json path of the element to evaluate | customer.data.name |
| regex     | Regex expression to evaluate         | \[a-zA-Z]+         |

## SuccessCondition

This class(_com.rierino.handler.saga.condition.SuccessCondition_) returns "success" or "fail" based on whether the previous saga step had an error or not.
