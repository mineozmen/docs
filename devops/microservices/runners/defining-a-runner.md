# Defining a Runner

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption><p>Runner Definition</p></figcaption></figure>

Clicking on the edit icon displays runner definition form, which has the first tab with the following data fields:

* **Name:** Descriptive name for the runner, which will be used as the label on runner screen and listings.
* **Status\*:** Whether the runner is currently active or not.
* **Domain:** Used for grouping runners within logical business domains (e.g. admin, cms).
* **Base Runners:** List of runners this runner should inherit elements and settings from. This configuration allows reuse of predefined lists of runner elements for functionalities shared among different runners (such as receiving commands, running queries), instead of copying same list of elements to each individually. Typically used base runners include:
  * Samza Base: Includes global configurations for all Samza runners
  * Spring Base: Includes global configurations for all Spring runners
  * Query Base: Includes state configurations for all runners with query handlers
  * Variable Base: Includes state configurations for all runners using variables (e.g. query and saga handlers)
  * Saga Base: Includes state configurations for all runners with saga handlers
  * Command Base: Includes stream configurations for all runners which should be listening to commands (i.e. effectively all runners)
* **Description:** Verbal description of the runner, used for referential purposes and documentation.

{% file src="../../../.gitbook/assets/train_rpc.json" %}
Example Runner Definition (Can be Imported on Runner Screen)
{% endfile %}
