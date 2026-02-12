---
description: >-
  Runner deployments utilize element, value and secret loaders for loading their
  configurations and key-value lookups, which are highly customizable
---

# Alternative Loaders

It is possible to define any number and type of new loaders with specialized configurations, and typical loaders deployed with Rierino are described in this section.&#x20;

{% hint style="info" %}
Loader configurations are predefined and automatically populated in a typical Rierino deployment. This section provides information for possibilities in setting up a completely isolated development or testing environment on a local machine.
{% endhint %}

## Element Loaders

Element loaders are used for loading runner members (such as systems, handlers, states) from a source, defined by "rierino.config.loader.class" property.

### NoopElementLoader

This loader (com.rierino.runner.loader.NoopElementLoader) uses only local properties file and does not load any additional elements from a source.

### StateElementLoader

This loader (com.rierino.runner.loader.StateElementLoader) uses a state manager, for loading elements, with the following configuration parameters:

| Setting                             | Definition                                      | Example                                     | Default |
| ----------------------------------- | ----------------------------------------------- | ------------------------------------------- | ------- |
| rierino.config.loader.state.manager | Fully qualified name of the state manager class | com.rierino.state.manager.MongoStateManager | -       |
| rierino.config.loader.system.\*     | Parameters for loader state manager's system    | db=devops                                   | -       |
| rierino.config.loader.runner.\*     | Parameters for the "runner" state manager       | collection=runner                           | -       |
| rierino.config.loader.element.\*    | Parameters for the "element" state manager      | collection=element                          | -       |

## Key-Value Loader

KV loaders are used for loading values and secrets that are injected into element parameters. Two types of KV loaders are used for runners:

### Value Loaders

Value loaders are used to replace $\{{VARIABLE\}} parameters. The loader to be used is defined by the "rierino.value.loader.class" property.

### Secret Loaders

Secret loaders are used to replace #\{{VARIABLE\}} parameters. The loader to be used is defined by the "rierino.secret.loader.class" property.

### Loader Types

#### EnvironmentKVLoader

This loader (com.rierino.runner.loader.EnvironmentKVLoader) loads VARIABLE from system environment variables.

#### FileKVLoader

This loader (com.rierino.runner.loader.FileKVLoader) loads VARIABLE from a text file with two options based on "rierino.\*.loader.singleDir" parameter:

* Single Directory: Reads from \[root]/\[key1.key2] file for VARIABLE=key1.key2
* Not a Single Directory: Reads from \[root]/\[key1]/\[key2] file for VARIABLE=key1.key2

where \[root] is defined by "rierino.\*.loader.root" parameter.

#### PropertiesKVLoader

This loader(com.rierino.runner.loader.PropertiesKVLoader) loads VARIABLE from a [properties file](#user-content-fn-1)[^1] which is defined by "rierino.\*.loader.path" parameter. It also has a "rierino.\*.loader.dynamic" parameter, which forces values to be looked up each time from the file when set to "true".

{% hint style="info" %}
Certain runner elements support "ephemeral" parameters, which are refreshed each time the element is reconnected (e.g. after a database connection failure). These parameters are defined using $!\{{VARIABLE\}} and #!\{{VARIABLE\}}.
{% endhint %}

{% hint style="info" %}
An additional convention exists for allowing use of environment variables for any value or secret, by adding a question mark before the variable name (e.g. #\{{?VARIABLE\}} or $\{{?VARIABLE\}}).&#x20;
{% endhint %}

[^1]: I.e. multi line property=value pairs
