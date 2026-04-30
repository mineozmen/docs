---
description: >-
  Rierino provides low/no-code capabilities for designing and deploying
  configurations in real-time.
icon: circle-info
---

# Configuration Overview

## **What the Configuration app does**

The Configuration app is where you define reusable “logic as data”. Instead of hardcoding queries, rules, or script-based handlers into a service, you store them as managed records and let runners and sagas load them at runtime. This keeps behavior easier to audit, migrate, and adjust across environments.

<figure><img src="../.gitbook/assets/image (167).png" alt=""><figcaption><p>Configuration App</p></figcaption></figure>

## **Core Configuration capability areas**

Configuration capabilities fall into three main areas:

* **Queries:** Define portable, parameterized queries that can be executed from handlers and sagas.
* **Business Rules:** Store and manage rulesets as configuration, so decision logic can evolve without service redeploys.
* **Dynamic Handlers:** Add or customize handler logic during runtime, typically for rapid iteration and controlled extensions.

## **How Configuration components work together**

In practice, these show up inside API flows and microservices. A saga step might execute a stored query, evaluate a ruleset, or run a dynamic handler action, while the runner enforces what systems and state managers are allowed. That separation gives you fast iteration without losing control over access, deployment, or operational safety.
