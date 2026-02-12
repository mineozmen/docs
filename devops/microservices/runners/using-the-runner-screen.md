# Using the Runner Screen

Opening the **Runner** screen from **Devops** app menu or navigation bar, you will come across a visual graph editor, allowing design of runners by adding new elements.

Runners are listed on the left side of this screen, grouped by their domain information.

The runner screen has similar functionalities as any other UI screen, such as save, delete, duplicate, item import and export actions and menus. Runners designed on this screen are stored in JSON format, the same as any other data source.

On top of this screen, you will notice that the runner ID is editable, this allows you to assign a meaningful unique ID to each runner for your reference. Since these IDs will be recorded within event logs, this approach makes them more manageable than assigning random or sequential IDs.

<figure><img src="../../../.gitbook/assets/image (120).png" alt=""><figcaption><p>Runner Icon Bar</p></figcaption></figure>

Below the ID field, you can find the icon bar, which allows construction of a runner as well as changing its layout.

If you click on the edit icon on the left of this bar, you will see the pop-up editor for defining runner details.

At the center of the icon bar, you can see different types of runner elements, which you can and drop on to the grid to add new systems, states and other element types.

At the right side, you can see the icons for changing the layout of runner after you've added its elements, such as automatically aligning steps, hiding / displaying the grid as well as changing its size.

{% hint style="warning" %}
When a runner configuration changes, it is important to execute a "Rebuild" command, as existing deployments of a runner are not updated automatically unless rebuildMs setting is configured explicitly. This is a design decision made to avoid unintended updates to runners due to user errors. &#x20;
{% endhint %}
