---
description: Message center allows sending custom text messages to a data stream.
---

# Streaming Messages

Messages can be used for manually triggering actions for diagnosis, testing or manipulation purposes.

Only a few parameters are required for sending a message:

* **Script:** Body of the message to send, typically in Json form
* **System:** Name of the target system, as defined in API gateway (e.g. kafka\_default)
* **Topic:** Topic inside target system to publish on (e.g. manual\_trigger)
* **Key:** Key value for the message
