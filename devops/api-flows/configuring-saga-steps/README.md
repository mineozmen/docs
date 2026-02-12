# Configuring Saga Steps

Saga flows start with a single START node and can have one or more SUCCESS / FAIL nodes as the exit point. Other nodes are linked as sequential steps in between them and have the following common parameters:

* **Name:** Descriptive name of the step, which will be displayed on saga graph
* **Description:** Detailed description of what this step will perform
* **Stroke:** Stroke color of the step on saga graph
* **Fill:** Fill color of the step on saga graph
* **Auto Fail:** Whether saga should automatically fail if this step fails
* **Continue on Fail:** Whether saga step should continue on fail (to override in case saga itself is configured as Auto Fail)
