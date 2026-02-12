---
description: >-
  Different lister types allow listing and editing data with different
  characteristics and size.
---

# Listers

A lister is a combination of a list of records and an editor for creating, editing and deleting each record.

While it is possible to implement new lister types, Rierino already includes various alternatives for different use cases.&#x20;

## Basic Listers

These listers are displayed as side menus with basic filtering capabilities and action lists, typically used for managing limited number of records.

### Menu Lister

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

MenuLister provides a standard listing of all records with an inline editor for selected item.

This lister has the following special properties:

* **Hide on Select:** Whether lister should be collapsed when user selects an item

### Grouped Lister

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

GroupedLister provides a listing of all records that are grouped under one or more levels.

This lister has the following special properties:

* **Hide on Select:** Whether lister should be collapsed when user selects an item
* **Group By:** Json path (e.g. `data.group`) or pattern (e.g. `=(data.branch.ref || 'main')`) for field(s) to group listed records by

### Nested Lister

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

NestedLister provides a tiered listing of lazy loaded records, using parent-child relations, with an inline editor for selected item.

This lister has the following special properties:

* **Hide on Select:** Whether lister should be collapsed when user selects an item
* **Parent:** Json path for field(s) to nest records by

### Single Lister

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

SingleLister allows displaying only a single record as an object editor screen without listing other records, typically used for managing user's own profile, environment or similar singular entries.

### Handlebars Lister

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

HandlebarsLister[^1] allows building highly customized listers using handlebars template and custom events.&#x20;

This lister has the following special properties:

* **Template:** Handlebars template to render lister page.

Handlebars lister allows use of a number of custom events (such as for displaying details of a specific record. To call a custom event, the following code can be used (e.g. as an onclick call):

```javascript
document.getElementById('{{@root.hbUUID}}').dispatchEvent(new CustomEvent( 'rie-call', { 'detail': {'eventName': '[EVENT]', 'params':[PARAMS]} } ))
```

Where \[EVENT] and applicable \[PARAMS] are as follows:

| Event   | Description                                                                                      | Params                                      |
| ------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------- |
| select  | Selects a specific record and displays its object editor                                         | {"id": \[ID to display] }                   |
| new     | Creates a new record                                                                             | -                                           |
| refresh | Refreshes current records                                                                        | -                                           |
| set     | Updates current state data, typically used for updating pagination or managing custom state data | {"pagination":{"pageSize": 10, "page": 2\}} |
| filter  | Applies current filters and pagination to update set of records                                  | {"status": "A"}                             |

## Advanced Listers

These listers support more advanced features, such as filtering and favorites, sharing the following special configurations:

### Common Configurations

#### Favorites & Title

* **Hide Title:** Hides lister title as well as create button and action menus
* **Title Pagination:** Displays pagination and page navigation next to lister title
* **List Favorites:** Whether favorites should be listed on top of the query table for easy navigation to them

#### Main & Extra Filters

These configurations define details of filters applicable for a query based table lister.

* **Path:** Path for the filter value
* **Widget:** Widget for displaying filter (e.g. TextFilter)
* **Properties:** Properties to pass on to filter widget (e.g. label, shortText, width)

#### Tab Filter

It is also possible[^1] to add a tab to query table lister, which allows switching between different sub-sets of lists easier (e.g. grouped by state). The configuration of tab filter is as follows:

* **Path:** Path for the filter value
* **Option Values:** List of id, name pairs for listing tabs and applying the filter value

#### Default Filters

List of default values to apply as initial filters when a user views the lister. The configuration of default filters is as follows:

* **Path:** Path for the filter value
* **Value:** Default value for the filter, which can be a JMESPath expression if starting with = sign.

### Query Table Lister

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

QueryTableLister is a relatively more sophisticated lister type, which allows filtering of large data sets and listing of results in a table format, with a pop-up editor for selected item.

This lister has the following special configurations:

* **Selectable:** Whether multiple rows can be selected to take bulk actions
* **Row Sorting:**
  * **Priority Path:** Json path in table records, which allows switching priority between 2 rows by updating their value together
  * **Priority Direction:** Whether priority path is sorted ascending (asc) or descending (desc), which is used in switching
  * **Sort API:** API URL for submitting reordered list of {ids:\[]} for sorting whole list of visible records using drag & drop
  * **Disable Sorting on Filter:** Whether sort API should be disabled when user has applied filters to the list
* **Table Properties:**
  * **Scroll Help:** Whether a scroll to top/bottom helper should be automatically displayed if the content grows longer than the window height.
  * **Paginated:** Whether table should display pagination at the bottom or not (default is true).
  * **Column Preferences:**
    * **Preference ID:** Unique ID to store user preferences for columns (enabling column selection & ordering by users), such as table-products.
    * **Allow Column Reorder:** Whether users should be able to change column orders.
    * **Allow Column Hide:** Whether users should be able to hide columns.
* **Column Sorting:**
  * **Order:** Parameter to send field to order by, if column sorting is used (defaults to "orderBy"). This parameter is activated if columns include orderField mappings.

#### List Columns

These configurations define details of columns displayed on a query based table lister.

* **Title:** Title of the column
* **Path:** Json path of the column data
* **Widget:** Widget for displaying column values (e.g. img)
* **Properties:** Properties to pass on to column widget (e.g. baseUrl)
* **Column Properties:** Properties to pass on to table column object (width, minWidth, maxWidth, hidden, defaultHidden[^2], orderField)

{% hint style="info" %}
A typical use case for setting a column as hidden is id columns, which are required for making a list selectable even when it is not preferred to display an id column.
{% endhint %}

#### Inline Menus

These menus allow displaying specific menu actions for each row of the query table lister (such as Call API for specific record id). They act similar to editor menu actions, but have access to table row data only, instead of full aggregate records, since tables don't necessarily load all details for listing.

### Board Lister

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

BoardLister allows using kanban-style board functionality, typically used for managing tasks and status updates, allowing drag & drop functionality to move records between columns for status changes.

This lister has the following special properties:

* **Name, Description, Image, Tags:** Path of the name, description, image and tags fields to display
* **Column Path:** Json path for assigning records to columns
* **OptionList / Options:** List of possible column values and labels

### Gantt Lister

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

GanttLister allows using gantt chart style functionality, typically used for managing activities with start and end dates, allowing editing date ranges of records through drag and drop.

This lister has the following special properties:

* **Name, Start Path, End Path, Type Path, Progress Path:** Path for the related field to display on gantt chart
* **Length Unit:** Scale displayed on the gantt chart (e.g. day, week, month, year)

### Map Lister

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

GoogleMapsLister allows using Google Maps for listing of location based records.

This lister has the following special properties:

* **Location:** Data field including lat, lng details of a record&#x20;
* **Location Title:** Data field to display on location of a record

### Calendar Lister

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

CalendarLister allows displaying records with start and end dates in a monthly/weekly/daily calendar or in agenda mode.

This lister has the following special properties:

* **Start Date Field:** Data field for the start date (defaults to createTime)
* **End Date Field:** Data field for the end date (defaults to createTime)
* **Background Field:** Field for coloring the date entry (defaults to #000)
* **Icon Field:** Field for the name of icon to display
* **Title Field:** Field for displaying text on the date entry (defaults to id)
* **Default View:** Default view to display on calendar (defaults to month)
* **Views:** List of views allowes on the calendar (defaults to month, day, agenda)
* **Event Creation Handlers:** Actions used for creating new records (e.g. double click)

### Custom Code Lister

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

CustomCodeLister allows using JSX code for rendering custom listers on the fly, where all lister props are passed on to custom code lister, as well as onSelect, setSelection and selection properties.

[^1]: since 0.5.2

[^2]: User can override with preference
