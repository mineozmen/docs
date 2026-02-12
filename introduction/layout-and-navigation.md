---
description: >-
  A typical screen in Rierino consists of 3 main building blocks, used for
  navigation, listing records and editing them
icon: picture-in-picture
---

# Layout & Navigation

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption><p>Rierino Layout</p></figcaption></figure>

## 1) Navigation Bar

A vertical navigation bar provides access to global home page, different apps, screens within selected app, AI agents and user menu. While it is possible to hide this bar using "noNav" parameter in app screens, it is usually displayed as the main navigation feature across the platform.

Top logo icon on this navigation bar, which can be customized from icon UI, allows users to go back to a global home page, which typically lists all accessible apps as well as favorites for the user.

Below this logo is the app icon, which activates the menu for switching between different apps, routing user to the landing page for that specific app.

<figure><img src="../.gitbook/assets/image (160).png" alt="" width="375"><figcaption><p>App Switch</p></figcaption></figure>

Under the app switch is a list of icons representing individual screens that are included within the current app, each typically displaying a records lister and an object editor for data entry and business operations.

Just above the bottom icon is an AI icon ✨ which allows users to connect to different AI agents to execute tasks or receive support.

At the bottom is the user menu, which displays current user information and allows:

* Changing the language of admin UI
* Changing the theme / style of admin UI
* Changing the active branch (if enabled)
* Listing and accessing recently visited screens
* Accessing help center
* Logging out of the admin UI

<figure><img src="../.gitbook/assets/image (161).png" alt="" width="220"><figcaption><p>User Menu</p></figcaption></figure>

## 2) Records Lister

Most screens include listing of technical or business data records, such as products, customers or tasks to be completed. In these screens, either a collapsible menu lister or a full page lister (such as table or a map) is presented.

## 3) Object Editor

For viewing details of records on a lister or creating new records, typically an object editor is used, which can be displayed inline as part of the page or as a pop-up dialog.

Object editors support the following shortcuts:

<table><thead><tr><th width="206.9393310546875">Shortcut</th><th>Action</th></tr></thead><tbody><tr><td>Ctrl + S</td><td>Save current record</td></tr><tr><td>Ctrl + .</td><td>Creates a new record</td></tr></tbody></table>
