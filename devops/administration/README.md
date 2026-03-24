---
description: >-
  Operate deployments, issue runner commands, and send diagnostic messages
  across the platform from the Devops admin tools.
icon: toolbox
---

# Administration

This section covers the operational controls used after services are designed and deployed. It focuses on day-to-day runtime management, including deployment actions, runner commands, and manual message injection for testing or diagnosis.

### What this section covers

* [Managing Deployments](managing-deployments.md) explains how to deploy, undeploy, restart, inspect, and health-check deployed runner packages.
* [Sending Commands](sending-commands.md) covers the command center for issuing runner and element commands such as ping, rebuild, remap, pause, stop, log, and commit.
* [Streaming Messages](streaming-messages.md) explains how to send custom payloads directly to a target stream for testing, troubleshooting, or manual triggers.

### How the workflow fits together

1. Deploy or restart a runner package.
2. Verify deployment health and inspect pods or logs.
3. Send commands to rebuild, remap, pause, or inspect runners.
4. Publish test or diagnostic messages to streams when needed.

These tools are complementary. Deployment actions manage runtime packages. Commands control runner behavior. Manual messages help test stream-driven flows without building a dedicated UI or API.

### Key ideas

#### Deployment actions control runtime packages

Deployment operations target the deployed unit, not just the configuration record. They are typically backed by Kubernetes or external automation such as Jenkins.

#### Commands target runners and elements

Commands are useful when you need live operational control. Common examples include pinging runners, rebuilding operational elements, or changing log levels temporarily.

#### Messages help with diagnostics and ad hoc triggers

Manual messages are useful for debugging event-driven flows, replaying test payloads, or pushing one-off data into a topic.

### Common use cases

* Deploy a new runner version after configuration changes.
* Restart a deployment and inspect recent pod logs.
* Rebuild or remap runners after operational edits.
* Pause or stop a runner during troubleshooting.
* Send a test payload to Kafka to validate an async flow.

### Start here

* Start with [Managing Deployments](managing-deployments.md) when you need to control deployed services.
* Go to [Sending Commands](sending-commands.md) when you need live control over runners or elements.
* Go to [Streaming Messages](streaming-messages.md) when you want to inject custom data into a stream.
