---
description: >-
  Reference for the common settings, step types, and links used to build and
  control saga execution flows.
---

# Configuring Saga Steps

Saga flows start with a single START node and can have one or more SUCCESS / FAIL nodes as the exit point. Other nodes are linked as sequential steps in between them and have the following common parameters:

* **Name:** Descriptive name of the step, which will be displayed on saga graph
* **Description:** Detailed description of what this step will perform
* **Stroke:** Stroke color of the step on saga graph
* **Fill:** Fill color of the step on saga graph
* **Auto Fail:** Whether saga should automatically fail if this step fails
* **Continue on Fail:** Whether saga step should continue on fail (to override in case saga itself is configured as Auto Fail)

### Step types

Saga graphs are built from a small set of step types. Each type has different configuration fields and runtime behavior.

#### Control steps

These steps define entry and exit points of the flow:

* **Start:** Entry point for the saga execution.
* **Success:** A successful exit point.
* **Fail:** A failure exit point.

#### Action and logic steps

These steps do the work between Start and exit nodes:

* [Event Step](event-step/): calls an event handler action with parameters.
* [Transform Step](transform-step/): transforms the current payload and passes the output forward.
* [Condition Step](condition-step/): evaluates a condition and routes to different next steps.

#### Connecting steps

* [Step Link](step-link.md): defines how the flow moves from one step to the next. Use condition values to model branching and default (`*`) paths.

{% hint style="info" %}
Double clicking on a step on stencil automatically adds it as a connected step for the currently selected node on saga flow.
{% endhint %}
