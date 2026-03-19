---
description: >-
  This FAQ covers the core Design concepts, screen-building model, API wiring,
  and schema responsibilities across the platform
icon: comment-question
---

# FAQ

## Frequently asked questions

### What is the Design app responsible for?

<details>

<summary>Show answer</summary>

Design is where you build and evolve the admin UI.

It covers screen structure, interaction patterns, API wiring, and data shape so teams can change internal experiences quickly.

See [Overview](overview.md).

</details>

### What are the main building blocks in Design?

<details>

<summary>Show answer</summary>

Think in three main groups:

* **User Interface** for apps, screens, widgets, menus, and visual behavior
* **API Mapping** for connecting screens to backend endpoints
* **Data Schema** for defining record structure, defaults, and validation rules

Supporting assets such as options, translations, icons, styles, and components help standardize the UI across apps.

See [Overview](overview.md).

</details>

### What is the difference between an app and a UI?

<details>

<summary>Show answer</summary>

An **app** is a container for navigation and screens.

A **UI** is one specific screen inside that app.

Apps usually map to business areas or user roles. UIs map to specific tasks such as listing products, editing orders, or reviewing records.

See [User Interface](user-interface/).

</details>

### What is the difference between a lister and an editor?

<details>

<summary>Show answer</summary>

A **lister** helps users find and select records.

An **editor** helps users view or update a selected record.

Most business screens use both together.

See [Listers](user-interface/uis/listers.md) and [Widgets](user-interface/uis/widgets/).

</details>

### What role do widgets play?

<details>

<summary>Show answer</summary>

Widgets are the field-level building blocks of a UI.

They control how values are displayed, edited, validated, grouped, or embedded.

Different widget types exist for simple values, arrays, nested objects, indirect data, read-only display, and hosted custom components.

See [Widgets](user-interface/uis/widgets/).

</details>

### When do I need a Source?

<details>

<summary>Show answer</summary>

You need a Source when a screen should talk to a non-default backend endpoint.

That usually means one of these:

* the UI should call a custom runner or saga
* the list view should use a dedicated query endpoint
* some operations should be overridden or disabled

If no matching Source exists, the UI falls back to the default admin CRUD behavior.

See [API Mapping](api-mapping/).

</details>

### How does a UI connect to a Source?

<details>

<summary>Show answer</summary>

The usual convention is matching ids.

When a UI and a Source share the same id, the UI uses that Source for list, get, create, update, and delete behavior.

This keeps the UI definition separate from endpoint details while still linking them cleanly.

</details>

### What is the role of a schema in Design?

<details>

<summary>Show answer</summary>

A schema defines the shape of the data behind the screen.

It drives structure, validation, labels, defaults, and consistency across editors and APIs.

Even when the underlying store is schema-flexible, defining schemas usually makes the system easier to maintain.

See [Data Schema](data-schema/).

</details>

### Does a UI automatically pick up a schema?

<details>

<summary>Show answer</summary>

Yes, in the common case.

If a schema has the same id as a UI, that schema is used automatically for labels and validation.

You can also point a UI to a different schema when reuse makes more sense.

</details>

### What is the relationship between UI, Source, and Schema?

<details>

<summary>Show answer</summary>

They solve different concerns:

* **UI** defines how the screen behaves
* **Source** defines which backend endpoints the screen calls
* **Schema** defines the structure and validation of the data

That separation makes screens easier to reuse and change. You can update the API route or data model without redesigning the whole screen.

</details>

### Do I need to define a Source and Schema for every UI?

<details>

<summary>Show answer</summary>

No.

A UI can work without a custom Source if the default CRUD behavior is enough.

A schema is also not always mandatory, but it is strongly recommended for clarity, validation, and long-term maintainability.

</details>

### What are options, translations, icons, and styles for?

<details>

<summary>Show answer</summary>

They are shared UI assets.

* **Options** define reusable dropdown values
* **Translations** manage labels and language variants
* **Icons** define reusable visual symbols
* **Styles** control theme and appearance

These assets keep many screens consistent without copying the same setup repeatedly.

See [User Interface](user-interface/).

</details>

### What are components in Design?

<details>

<summary>Show answer</summary>

Components define the available editor and filter building blocks used by the platform.

Most teams do not change them often.

They matter more when extending the platform with new custom UI capabilities.

See [Components](user-interface/components.md).

</details>

### Can I build an internal app without writing front-end code?

<details>

<summary>Show answer</summary>

Yes.

That is one of the main purposes of Design.

You can define apps, screens, forms, lists, widgets, and menus through configuration, then connect them to backend APIs.

</details>

### Is Design meant for public customer-facing front-ends?

<details>

<summary>Show answer</summary>

Its main strength is internal admin and operational UI.

You can use the platform headlessly for external front-ends, but Design is optimized for back-office tools, admin flows, and internal business screens.

</details>

### How does Design relate to Devops?

<details>

<summary>Show answer</summary>

Design builds the admin experience.

Devops provides the backend services and APIs that the screens call.

A common setup is: Devops exposes an endpoint, API Mapping connects a UI to it, and Schema keeps the record structure consistent.

See [Overview](overview.md) and [Devops FAQ](../devops/faq.md).

</details>

### When should I use special menu actions or extended scope features?

<details>

<summary>Show answer</summary>

Use them when a simple form screen is not enough.

Menu actions help trigger operations from listers, editors, selections, or widgets. Extended scope features help with conditional display, extra data, defaults, and context-aware behavior.

These features are useful when the UI needs workflow behavior, not just basic CRUD editing.

See [Menus](user-interface/uis/menus/) and [Extended Scope](user-interface/uis/extended-scope/).

</details>

### What should I understand first if I am new to Design?

<details>

<summary>Show answer</summary>

Start with these concepts:

* **App** for navigation context
* **UI** for the actual screen
* **Lister and editor** for interaction pattern
* **Widget** for field behavior
* **Source** for backend wiring
* **Schema** for data structure and validation

Once these are clear, the rest of the Design section becomes much easier to follow.

</details>

### Where should I go next for deeper Design FAQs?

<details>

<summary>Show answer</summary>

Start with the subsection that matches your goal:

* Need apps, screens, widgets, or menus → [User Interface](user-interface/)
* Need endpoint wiring or custom list actions → [API Mapping](api-mapping/)
* Need validation or data structure rules → [Data Schema](data-schema/)

Subsection-specific FAQs can then go deeper into screen types, widget behavior, source actions, and schema patterns.

</details>
