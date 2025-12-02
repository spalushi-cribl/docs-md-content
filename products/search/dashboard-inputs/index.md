# Add Inputs to Your Cribl Search Dashboard


Enable Dashboard viewers to control visualizations through interactive widgets.

---

## Why Use Dashboard Inputs {#why}

An Input is a small interactive area at the top of your Dashboard. When a user selects or enters values, the linked
visualizations update to reflect the new criteria. This allows Dashboard viewers to quickly filter data, without needing
to directly edit the underlying search queries.

## Dashboard Input Types {#types}

Using different types of Inputs, you can enable Dashboard viewers to:

- [Change the time range of visualizations](#time-range), with a **Time Range** Input.
- [Filter visualizations by freeform text](#text), with a **Text** Input.
- [Filter visualizations by numbers](#number), with a **Number** Input.
- [Filter visualizations by a preconfigured set of dynamic values](#dropdown), with a **Dropdown** Input.


![Different types of Dashboard Inputs](search-dashboard-inputs.png)
{scale="100%"}

## Add a Dashboard Input {#add}

To add an Input to a new or existing Dashboard:

1. Go to the **Dashboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right, select **Add** > **Input**. Give the Input a title.
1. Set the Input ID. You'll reference this ID in the searches that you want the Input to control. Use alphanumeric
   characters, underscores, and hyphens only.
1. In the **Type** drop-down, select the Input type you want. See the details about each type:
   - [Time Range](#time-range)
   - [Text](#text)
   - [Number](#number)
   - [Dropdown](#dropdown)
1. When done, select **Save** at the top of the Dashboard. (To skip this step, see
   [Auto-Apply Input Changes](#auto-apply).)

Now, [link the Input](#link) to one or more visualizations in the Dashboard.

## Link a Dashboard Input To a Visualization {#link}

After adding an Input to a Dashboard, you need to link it to one or more visualization panels. This way, when a user
interacts with the Input, the linked visualizations update accordingly.

1. [Add an Input](#add) to your Dashboard.
1. [Add](editing-dashboards#add) or [edit](editing-dashboards#edit) a visualization panel that you want to control.
1. Depending on the Input type, follow the specific linking instructions:
   - [Time Range](#time-range)
   - [Text](#text)
   - [Number](#number)
   - [Dropdown](#dropdown)

## Enable Viewers to Change a Visualization's Time Range {#time-range}

Using a **Time Range Input**, you can enable viewers to control the time range of the visualization panels with a time
picker at the top of your Dashboard.


![A Time Range Input](search-dashboard-inputs-timerange.png)
{scale="100%"}

First, add a **Time Range Input**:

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right, select **Add** > **Input**. Give the Input a title.
1. Set the Input ID. Use alphanumeric characters, underscores, and hyphens only.
1. In the **Type** drop-down, select **Time Range**.
1. Set the time picker's **Default value**. This is the time range applied when the Dashboard first loads.
1. When done, select **Save** at the top of the Dashboard. (To skip this step, see
   [Auto-Apply Input Changes](#auto-apply).)<br>The new Input appears at the top of your Dashboard, where you can edit,
   clone, or delete it.

Now, link your Time Range Input to one or more visualizations:

1. [Add](editing-dashboards#add) or [edit](editing-dashboards#edit) a visualization panel that you want to control.
1. Select the clock button in the date and time selector.
1. Select the **Link** tab.
1. Select the Time Range Input you just created. The Input ID appears in the visualization's date and time field.
   
   ![Time range with linked Input](search-input-linked-time-range.png)

Now, when viewers select a time range in the Input, the linked visualizations update.

## Enable Viewers to Filter a Visualization by Text {#text}

Using a **Text Input**, you can enable Dashboard viewers to enter freeform text so that linked visualization panels
display only data with that text value. Think of this as a free-text search box.


![A Text Input](search-dashboard-inputs-text.png)
{scale="100%"}

First, add a **Text Input**:

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right, select **Add** > **Input**. Give the Input a title that gives the Dashboard viewer an idea of what
   they can type.
1. Set the Input ID. Use alphanumeric characters, underscores, and hyphens only.
1. In the **Type** drop-down, select **Text**.
1. Set the text field's **Default value**. This is the filter applied when the Dashboard first loads.
1. When done, select **Save** at the top of the Dashboard. (To skip this step, see
   [Auto-Apply Input Changes](#auto-apply).) The new Input appears at the top of your Dashboard, where you can edit,
   clone, or delete it.

Then, link your Time Range Input to one or more visualizations:

1. [Add](editing-dashboards#add) or [edit](editing-dashboards#edit) a visualization panel that you want to filter.
1. In the visualization’s search query, reference the **Input ID** of the Text Input you just created. Surround the ID
   by `$`. For example, if the Input ID is `firstname`, use `$firstname$`.

Now, when viewers type into the Input's text field, the linked visualizations update.

## Enable Viewers to Filter a Visualization by Numbers {#number}

Using a **Number Input**, you can enable Dashboard viewers to filter visualizations by a number they enter into an
simple text area at the top of your Dashboard.


![A Number Input](search-dashboard-inputs-number.png)
{scale="100%"}

First, add a **Number Input**:

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right, select **Add** > **Input**. Give the Input a title that gives the Dashboard viewer an idea of what
   they can type.
1. Set the Input ID. Use alphanumeric characters, underscores, and hyphens only.
1. In the **Type** drop-down, select **Number**.
1. Set the number field's **Default value**. This is the filter applied when the Dashboard first loads.
1. You can set the **Minimum value** and **Maximum value**, to restrict what viewers can enter.
1. When done, select **Save** at the top of the Dashboard. (To skip this step, see
   [Auto-Apply Input Changes](#auto-apply).) The new Input appears at the top of your Dashboard, where you can edit,
   clone, or delete it.

Then, link your Number Input to one or more visualizations:

1. [Add](editing-dashboards#add) or [edit](editing-dashboards#edit) a visualization panel that you want to filter.
1. In the visualization’s search query, reference the **Input ID** of the Number Input you just created. Surround the ID
   by `$`. For example, if the Input ID is `value`, use `$value$`.

Now, when viewers type into the Input's number field, the linked visualizations update.

## Enable Viewers to Filter a Visualization by Drop-Down Options {#dropdown}

Using a **Dropdown Input**, you can enable Dashboard viewers to filter visualizations by selecting one or more values
from a drop-down list at the top of your Dashboard.

The list of values can be static (where you configure all options available to the user), or dynamic (where the results
of a search provide the values for the drop-down).


![A Dropdown Input](search-dashboard-inputs-dropdown.png)
{scale="100%"}

First, add a **Dropdown Input**:

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right, select **Add** > **Input**.
1. Set the Input ID. Use alphanumeric characters, underscores, and hyphens only.
1. In the **Type** drop-down, select **Dropdown**.
1. Select how to populate the drop-down list: **New search**, **Saved search**, or **Manually add dropdown items**.
1. In the **Field name** (with the search options) or in the **Values** (with the manual option), enter a caption to
   help the viewer understand the item's purpose.
1. When done, select **Save** at the top of the Dashboard. (To skip this step, see
   [Auto-Apply Input Changes](#auto-apply).)<br>The new Input appears at the top of your Dashboard, where you can edit,
   clone, or delete it.

Then, link your Dropdown Input to one or more visualizations:

1. [Add](editing-dashboards#add) or [edit](editing-dashboards#edit) a visualization panel that you want to control.
1. In the visualization’s search query, reference the **Input ID** of the Number Input you just created. Surround the ID
   by `$`. For example, if the Input ID is `firstname`, use `$firstname$`.

## Auto-Apply Input Changes {#auto-apply}

You can auto-apply your Dashboard Inputs changes, without having to confirm them. To set this up for a Dashboard:

1. Go to the **Dashboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. Open a Dashboard.
1. At the top right of the Dashboard, select the Actions ![](actions-dropdown.light.svg) button.
1. Select **Apply Input Changes Automatically**.
