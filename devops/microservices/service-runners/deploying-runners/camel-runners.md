---
description: >-
  These runners are based on Apache Camel library, receiving requests from Camel
  consumers (e.g. Timer) and returning results on Camel producers (e.g.
  ActiveMQ).
---

# Camel Runners

All Camel configurations are applicable, and can be passed on to these runners using global runner elements.

## Camel Event Runner

CamelEventRunner(_com.rierino.runner.camel.CamelEventRunner_) provides a variety of integration options, all of which can be used as input and output streams for use cases that require channels beyond what Spring and Samza runners support.

This event runner can consume message from all input streams mapped to it, with a camelRoute property and produce to any output system with a camelRoute setting. All input messages are converted to Json first and then to the message class defined for the input stream before processing as an event/journal/command, etc. All outputs are converted to an instance of the class required by the output endpoint.

{% embed url="https://camel.apache.org/" %}
Apache Camel
{% endembed %}
