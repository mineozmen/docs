---
description: Links between saga steps define the flow of execution from start to end.
---

# Step Link

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption><p>Link Definition</p></figcaption></figure>

Links define the order and physical flow of data between different saga steps. It is possible to link two saga steps with each other, by clicking on the link icon on the source step first and then clicking on the target step.

## Link Configuration

Once link is created, it should be edited using the edit icon displayed when it is selected, using the following settings:

* **Name:** Descriptive name of the link, which is displayed on saga flow.
* **Condition Values\*:** List of values for which this link will be followed. This setting is only applicable if the source of the link is a condition which produces different values for different event inputs. If condition values is left empty, this link is always followed. If it is set to "\*", this link is followed if no other link qualifies for the current condition. Apart from flows which have special merge steps, or asynchronous activities, it is advisable to have only 1 link qualifying for each possible condition value.
* **System:** Alias of the streaming system which will be used for sending an event to its target event runner. It is only used when the target is an event step and defaults to the default system when left blank.
* **Stream\*:** Alias of the stream which will be used for sending the event. Similar to system setting, it is only used when the target is an event step. A special value "local" is used for executing the next step on the same runner which is orchestrating current saga flow. Once you enter a stream name, you can click on the icon over this field to see which runners are configured to receive requests on this stream, to make sure that you are using the right stream name for coordination.
* **Description:** Verbal description of the link, used for reference and documentation.
* **Priority:** Numerical priority of the link, used for selecting execution order of links in case more than one link condition is met. Links with higher value are followed first.
* **Stroke:** Color of the link, used for visualization of the saga flow.
* **Key Strategy:** For links executing on partitioned streams (e.g. Kafka topics), key strategy defines data element to use as the partition key. This setting is not used for local or REST based communications, but is important when microservices consuming the stream have states specific to their assigned partitions (e.g. using local caches of their respective partitions only or using a partitioned write event handler avoiding write conflicts between instances). The following alternative strategies are available:
  * **Request ID:** Uses ID field in request metadata, which is a globally unique value assigned by the API gateway. Since this value stays the same throughout a request's flow, it is useful when the partition needs to be the same between different event steps.
  * **Input ID:** Uses ID field in event payload's input element, which is defined by the event metadata settings. This strategy is typically used for directing events to database microservices, ensuring selecting of the partition relevant to target record's id.
  * **Output ID:** Similar to input ID, but uses the ID field in event payload's output element instead.
  * **Event Path:** Uses an element from event's full contents, specified by the "key path" parameter. This strategy is useful for selecting a partition based on non-payload fields, such as request origin.
  * **Payload Path:** Uses an element from event payload, specified by the "key path" parameter. This strategy is useful for selecting a partition based on a non-input id field.
  * **Fixed (Zero):** Sends all events to partition 0. Typically used for development, testing or single partition deployments only.
  * **Random:** Sends all events to a random partition with an option to define the list of possible values to select from (e.g. when a specific partition is overloaded or down). Typically used for balancing event load across instances.
  * **Round Robin:** Sends sequential events to different partitions in a round robin version, with an option to define the list of possible values to select from. Similar to random strategy, typically used for balancing event load across instances.

{% hint style="info" %}
Key strategy is a critical decision, as it allows sending requests to the right runners in case of a partitioned deployment (such as basket data cahinched by partition on different runners). In non-partitioned use cases, typically request ID or random keys are used.
{% endhint %}

{% hint style="info" %}
When link system is a "rest" connection, it uses \[base url]/\[stream]/\[action] call to the target event runner.
{% endhint %}

{% hint style="info" %}
Starting from version 0.8.2, if Stream is left empty, "local" is used as the default value and the next step is executed locally, to simplify saga creation process.
{% endhint %}

## Automated Condition Values

Event steps automatically produce their result status (e.g. "SUCCESS", "FAIL") as the condition value, which can be used to split flows based on success/fail of an event step without adding an explicit condition step to check its result.
