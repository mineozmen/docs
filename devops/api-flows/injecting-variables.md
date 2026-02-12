# Injecting Variables

[API flows](./) as well as [queries](../../configuration/queries/) allow use of variables for creating more dynamic actions without the need to create complex conditional flows or query logic.

Variables can be referenced within metadata fields of [event steps](configuring-saga-steps/event-step.md) as well as query fields and conditions using %%VARIABLE%% notation, with the following extra features:&#x20;

* Using multiple variables within a single field: %%VARIABLE1%%\_%%VARIABLE2%%.
* Using default value when missing: %%VARIABLE|DEFAULT%%

Opening the **Variable** screen from **Devops** app menu or navigation bar, you will come across the list of defined variables, allowing adding new entries as required. It is recommended to define variables on this screen before using them in flows or queries, as this approach ensures type safety while providing ability to add validation constraints on variable values. It is also possible to use variables not listed on this screen, which automatically leads to retrieving variable value from input payload with the assumption that the variable is of string type.

Variable values are extracted from payload of the event which is currently being processed by a runner, with the following flow:

* If a path\_\[VARIABLE] parameter is defined in event metadata, it is used as the variable path
* Else, if the variable is defined in Variables screen, path provided in that screen is used
* Else, variable name is used as its path

If the variable path starts with $., then it is searched in the root of event payload, otherwise it is searched in the input element payload.

In addition to event payload values, following built-in variables can be used to access special values:

| Variable                                 | Meaning                                                                        |
| ---------------------------------------- | ------------------------------------------------------------------------------ |
| .id                                      | Runner id                                                                      |
| .name                                    | Runner name                                                                    |
| .deploymentVersion                       | Runner deployment version                                                      |
| .parameters.\[parameter]                 | Runner parameter \[parameter]                                                  |
| .system.parameters.\[parameter]          | Runner parameter \[parameter] for system matching event metadata domain        |
| .state.partition                         | Runner partition for state matching event metadata domain                      |
| .state.partitionMod                      | Runner partition modulo for state matching event metadata domain               |
| .query.parameters.\[parameter]           | Runner parameter \[parameter] for query manager matching event metadata domain |
| @.\[element]                             | Event element \[element] (e.g. @.requestMeta.id)                               |
| $!\{{\[setting]\}} or #!\{{\[setting]\}} | Ephemeral key-value / secret setting in runner configuration                   |
| $\{{\[setting]\}} or #\{{\[setting]\}}   | Key-value / secret setting in runner configuration                             |

{% hint style="info" %}
In queries, if the value is not available for a variable, the related field or condition is omitted (e.g. condition is considered as true, field is not returned in response). Making it possible to use a single query configuration for different combination of input variables.&#x20;
{% endhint %}
