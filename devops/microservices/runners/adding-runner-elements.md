# Adding Runner Elements

Elements are the building blocks for all runners, hence the microservices. They also provide the mechanism for extending Rierino capabilities by integrating new systems, databases and functions. While Rierino is shipped with a rich set of runner elements, it is possible to create new elements to address any new business use case.

Elements can be added to runners simply by dragging and dropping from the runner toolbar, based on the type of element you would like to add.

<figure><img src="../../../.gitbook/assets/image (124).png" alt=""><figcaption><p>Runner Element Configuration</p></figcaption></figure>

Once added, you can click on the edit icon for your new element to configure its common settings:

* **Element\*:** Selection of the element to add to the runner.
* **Alias\*:** Unique name to assign to the runner element, which will be used for referencing to it in event metadata and by other runner elements. It is possible to use the same element with different aliases within the same or across different runners. An alias is similar to assigning an object instance a variable name for reference.
* **Member Type\*:** Whether the element requires runner to restart upon its change (structure), can be reinstantiated with a rebuild command (operation) or even simply reassigned using a remap command (mapping). Within out-of-box elements, only event streams need to be configured as structure members, as their partition assignments are done during system start. The remaining elements are typically configured as operation members, aside from action elements, which can be configured as mapping members.
* **Status:** Whether the element is active or not. If the status is set to Draft or Passive, the runner does not load element configuration during runtime.
* **Description:** Verbal definition of the element for referential purposes.

Once you select the element from the dropdown, you can find its predefined settings from the element tab and decide whether you would like to override any of these settings.

Most runner element types allow overriding of their predefined settings via the override tab:

* **Setting Name\*:** Specific setting to replace (e.g. parameter.query.state)
* **Setting Description:** Verbal definition of reason for replacement for referential purposes.
* **Setting Value\*:** New value to apply for this setting.
* **Runner Type:** Type of runner to apply setting for (in case it is only applicable for a certain runner implementation such as samza).

Actions and streams allow assignment of default or mandatory event metadata settings as well, which can be useful for enforcing use of specific states for requests received on a specific input stream, or avoiding repetition of same metadata for each request. These are set on metadata tab for these types of elements:

* **Name:** Metadata to override (e.g. domain, handler).
* **Value:** Value to set metadata field to (e.g. product, read).
* **Override:** Whether the value should be used as default (hence, can be replaced by the requesting party) or override (hence, is mandatory).
* **Stream:** For actions, indicates which input stream should trigger metadata override.

Handlers and roles can also relate themselves to specific input / output streams, so that each event received from a specific input stream is handled by a specific handler or role, and roles can forward their results to a specific output stream. These are set on the I/O tab for these types of elements:

* **For Handlers:**
  * **Role:** Alias of the role to use for data received ("event" for typical events, "journal", "pulse", "command" or custom roles for other types).
  * **Inputs:** List of input streams that should be treated as a call to the handler with the given role (e.g. productq).
* **For Roles:**
  * **Inputs:** List of input streams that should be treated as data for this role (e.g. command).
  * **Outputs:** Input to "system-stream-key path" mapping for directing received role data to output streams on the right partition (e.g. kafka\_command-command\_result-request.id).

You can add as many elements as you like to your runner, which will provide it with access to other systems, load it with new functionalities or allow it to use predefined list of actions. The number and mix of elements you add to a runner depends on your preferred deployment strategy (e.g. building very specialized microservices with few elements that have low resource consumption or building more monolithic services with many elements serving variety of requests).

As you add new elements to a runner, you will notice that they are automatically linked to each other, based on their dependencies (such as a state manager linking to the database system it is defined for). Typically, if you have streams, state or query managers without links to any system elements, you most likely forgot to add their dependencies to your runner.&#x20;

Essential links between elements are as follows:

* **System - Stream:** Streams typically rely on systems for getting shared connection properties such as server ips.
* **System - State / Query:** State and query managers typically rely on systems for getting shared connection properties such as connection strings.
* **Handler - Action:** Actions require a handler for execution, as they are mainly logical mappings of function names to handler implementations.
* **State - State:** Some states use others as cache or loaders.
