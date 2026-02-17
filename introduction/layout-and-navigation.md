---
description: >-
  A typical screen in Rierino consists of 3 main building blocks, used for
  navigation, listing records and editing them
icon: picture-in-picture
---

# Layout & Navigation

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption><p>Rierino Layout</p></figcaption></figure>

## 1) Navigation Bar

The navigation bar is the main way to move around the Admin UI. It provides access to the global home page, apps, screens, AI agents, and the user menu.

You can hide it on specific screens using the `noNav` parameter. Most deployments keep it visible for consistent navigation.

### Typical navigation flow

Most users follow this pattern:

1. Go to the global home page.
2. Switch to the right app.
3. Pick a screen from the screen icons.
4. Use the lister to find a record.
5. Use the editor to view or change details.

### Global home

The top logo icon takes users back to the global home page. This logo can be customized from the icon UI.

The global home page usually lists:

* All accessible apps
* User favorites and pinned entries (if configured)

### App switcher

Below the logo is the app icon. It opens the app switch menu. Selecting an app routes the user to that app’s landing page.

<figure><img src="../.gitbook/assets/image (160).png" alt="" width="375"><figcaption><p>App Switch</p></figcaption></figure>

### Screen shortcuts

Under the app switcher is a list of icons. Each icon represents a screen in the current app. Most screens combine a records lister and an object editor.

### AI agents

Just above the bottom is the AI icon ✨. It lets users connect to AI agents for help and task execution.

### User menu

At the bottom is the user menu. It shows the current user. It also provides quick access to common settings:

* Changing the language of admin UI
* Changing the theme / style of admin UI
* Changing the active branch (if enabled)
* Listing and accessing recently visited screens
* Accessing help center
* Logging out of the admin UI

<figure><img src="../.gitbook/assets/image (161).png" alt="" width="220"><figcaption><p>User Menu</p></figcaption></figure>

## 2) Records Lister

Most screens include a list of records. These can be technical records or business records. Common examples include products, customers, and tasks to be completed.

Two common layouts are used:

* A **collapsible menu lister** for quick navigation and smaller datasets.
* A **full-page lister** (for example, a table or a map) for larger views and richer filtering.

### What the lister is responsible for

In practice, the lister is the “finding and selecting” layer. It typically handles:

* Searching and filtering
* Sorting and paging
* Selecting one record to edit, or a set of records to act on
* Showing compact context before opening an editor

## 3) Object Editor

The object editor is used to view and edit a selected record. It is also used to create new records.

Editors can be shown:

* Inline on the page, next to the lister
* As a pop-up dialog, to keep the user focused on one task

### What the editor is responsible for

The editor is the “detail and action” layer. It typically handles:

* Displaying the full record and related fields
* Validating user input before saving
* Triggering actions (for example, through menus and buttons)
* Supporting multi-step tasks where users need guided data entry

Object editors support the following shortcuts:

<table><thead><tr><th width="206.9393310546875">Shortcut</th><th>Action</th></tr></thead><tbody><tr><td>Ctrl + S</td><td>Save current record</td></tr><tr><td>Ctrl + .</td><td>Creates a new record</td></tr></tbody></table>
