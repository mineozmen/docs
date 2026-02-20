---
description: >-
  Rierino provides low/no-code capabilities for designing and deploying admin UI
  in real-time.
icon: circle-info
---

# Overview

The Design app is where you build and evolve the admin experience. It covers the full loop from screen layout, to API wiring, to data shape, so you can ship UI changes fast without coupling them to backend code releases.

<figure><img src="../.gitbook/assets/image (166).png" alt=""><figcaption><p>Design App</p></figcaption></figure>

Design capabilities fall into three main areas:

* **User Interface:** Design apps, screens, navigation, and interaction patterns for admins.
* **API Mapping:** Connect UIs to backend endpoints via Sources and environment settings.
* **Data Schema:** Define and govern the structure of records used across UIs and APIs.

In practice, you usually touch all three. You define a UI (listers, editors, widgets), point it to a Source (list/get/create/update/delete endpoints), and align the fields with a Schema so validation and defaults stay consistent across screens. That separation keeps screens reusable and makes it easier to change API routes or data models without rebuilding everything from scratch.
