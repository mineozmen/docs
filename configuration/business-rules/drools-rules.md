---
description: These queries are converted to Drools code by DroolsCodeProducer.
---

# Drools Rules

Drools specific features for special use cases are as follows:

## Rule Domains

* **Main Fact:** Main fact to be inserted into rules, which defaults to "DroolsMessageWrapper" which wraps received data for an event.
* **Extra Code:** Additional code to append (such as static variables, data structure declarations, special initialization and finalization rule codes) before the generated rule blocks.
* **Common Pre Condition:** Code to repeat for each rule in a domain, before given rule condition.
* **Common Post Condition:** Code to repeat for each rule in a domain, after given rule condition.
* **Common Condition Code:** Condition to add for each rule's main fact entry.
* **Common Pre Action Code:** Code to repeat for each rule in a domain, before given rule action (e.g. initializations).
* **Common Post Action Code:** Code to repeat for each rule in a domain, after given rule action (e.g. logging).
* **As List:** Whether batched / buffered rules should be executed using a list construct (requiring special rule entries) or iterating through each entry individually (possibly slower performance).

## Rules

* **Rule Attributes:** Code to append as attributes for this rule.
* **Salience:** Priority level for this rule.
* **Pre Condition:** Code to use before actual rule condition.
* **Post Condition:** Code to use after actual rule condition.
* **Disable Common Condition:** Whether rule should ignore common condition code from the domain.
* **Disable Common Pre Condition:** Whether rule should ignore common pre condition code from the domain.
* **Disable Common Post Condition:** Whether rule should ignore common post condition code from the domain.
* **Disable Common Pre Action:** Whether rule should ignore common pre action code from the domain.
* **Disable Common Post Action:** Whether rule should ignore common post action code from the domain.

{% embed url="https://www.drools.org/learn/documentation.html" %}
Drools Guide
{% endembed %}
