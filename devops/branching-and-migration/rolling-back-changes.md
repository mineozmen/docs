---
description: Rierino provides granular rollback control similar to migration
---

# Rolling Back Changes

Rollback operations are performed through a UI similar to branch & migration management from the devops application.

<figure><img src="../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

Users can select a specific date to return to, and list all the assets that have been updated since that date, with the latest historical snapshot available before that date.

Then, the users can select assets to return to identified latest snapshots and execute rollback.

{% hint style="info" %}
Rollback requires \[domain]\_history states enabled on domains, which are automatically populated when a branch is merged to main branch. Similar flows can be configured to generate snapshots on each migration or trigger them manually by users.
{% endhint %}
