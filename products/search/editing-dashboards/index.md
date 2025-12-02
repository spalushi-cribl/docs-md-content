# Edit a Dashboard


Learn how to edit a Dashboard in Cribl Search.

---

Within a [Dashboard](dashboards), you can add and edit visualization panels. You can also add Dashboard-level Inputs
that apply to all visualizations on the Dashboard.


> To edit the built-in Sample Dashboard, you need to clone it first. Go to **Dashboards** > **Sample Dashboard** > ![](actions-dropdown.light.svg)  > **Clone**.
{.box .info}

## Add a Visualization {#add}

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **e** on your keyboard.
1. From the **Add** drop-down, select **Visualization**.

Next, choose how to define your visualization: by reviewing Cribl Copilot AI suggestions; or by configuring it yourself,
based on a new or existing search.

### Cribl Copilot AI Suggestions {#copilot}

To have Cribl Copilot suggest visualizations that display your data effectively:

1. At the top of the page, select the Cribl Copilot button ![](copilot-icon.svg).
1. The right drawer, now labeled **Copilot**, will prompt you to select or change the base Dataset for this
   visualization.
1. From the tiles that Cribl Copilot suggests, select **Add** to add the visualization you choose.

For details, see [Add a Visualization when Editing a Dashboard](copilot-dashboards#editing).

### New Search {#new}

If you want to base your visualization on a **New Search** (the default setting in the right drawer):

1. Either enter your query directly, or select a **Parent search ID** to pre-populate your query from a parent search
   (see [Apply a Parent Search](#parent) below).
1. Optionally, if you've started with a parent search, you can append terms to the query before proceeding.
1. Select **Search**.

To start over, select **Reset**. This clears the query and all chart settings.

### Existing Search {#existing}

If you prefer to base your visualization on an existing [saved search](search-overview#saved) (which can be a
[scheduled search](scheduled-searches)):

1. In the right drawer, select **Saved**.
1. Select a **Saved search ID** to populate your query.
1. Optionally, append terms to the query before proceeding.
1. Set the **Run mode**:
   - **Run as new search** – Always rerun the query, even if the Dashboard has [data caching](#cache) enabled.
   - **Fetch results from last run** – Get results from the most recent run. (If the Dashboard has data caching enabled,
     Cribl Search will ignore this setting.) Cribl recommends selecting this option if you're basing this visualization
     on a scheduled search.
1. Select **Search**.

To start over, select **Reset**. This clears the query and all chart settings.

## Edit a Visualization {#edit}

To edit an existing visualization:

1. Open an existing Dashboard.
1. At the top right of the visualization panel you want to modify, select **Edit** ![](edit-button.light.svg). Depending
   on the layout, you might need to select the Actions ![](actions-dropdown.light.svg) button first.
1. Edit the visualization just like you [edit a Chart display](charting). The options available depend on the
   [Chart type](chart-types) used.

If you add a visualization by creating a new search, you’ll see additional options to configure the type, format, and
data display of the visualization.

If you create a visualization from a saved search, you can select **Edit** ![](edit-button.light.svg) at the
visualization panel's top right to revise the visualization.

## Hide a Visualization {#hide}

You can hide any visualization panel by editing it and selecting the **Hide this panel** check box.

- In Dashboard edit mode, hidden panels are collapsed into a **Hidden Panels** section.
- To unhide a panel, edit the panel and clear the **Hide this visualization** check box.

## Interact with Visualizations in a Dashboard {#interact}

Within any visualization panel, you can hover over graphics or expand list events to see more details. Also, the Actions
![](actions-dropdown.light.svg) buttons on the Dashboard, and on individual panels, offer further options.

### Rearrange Visualization Panels {#rearrange}

To arrange the visualizations in a Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press
**e** on your keyboard. Use the drag handle at the top of any panel to drag it to a new position, and use the resize
![](resize-handle.light.svg) handle at the bottom right or left to resize it.

### Refresh Visualizations {#refresh}

Refresh the data in a visualization by clicking the Refresh ![](refresh-button.light.svg) button at the top right of the
panel.

To refresh all of the Dashboard's visualizations automatically, see
[Refresh Your Dashboards Automatically](#autorefresh).

### Manage Visualizations {#panel-actions}

Select a visualization panel's own Actions ![](actions-dropdown.light.svg) button to expose the menu options shown
below.


![Visualization actions](search-panel-actions.png)
{scale="50%"}

The options here include:

- **Edit**: Opens the visualization panel for editing.
- **Clear Cache and Refresh**: A hard refresh of the visualization, to match current underlying data.<br>To refresh all
  of the Dashboard's visualizations automatically, see [Refresh Your Dashboards Automatically](#autorefresh).
- **Open Search Details**: Opens a read-only version of the [Details modal](search-details#details), listing the
  search's query expression, execution times, and duration.
- **Open in Search**: Opens an editable version of the query expression.
- **Full Screen**: Zooms up an expanded display of the visualization, in a modal within your browser window.
- **Copy to Clipboard**: Copies the current visualization, as a static image, to the system clipboard.
- **Export Visualization as PNG/JPG**: Saves the current visualization as a static image in `.png` or `.jpg` format.

## Apply a Parent Search {#parent}

To reduce effort and significantly improve Dashboard loading time, you can reuse search results by saving them as
**parent searches**. You can instantly add an existing search query to a visualization panel by designating another
visualization in the same Dashboard as the parent search. This enables you to aggregate results from both searches into
the same visualization.

1. Edit an existing Dashboard, and then select **Edit** ![](edit-button.light.svg) at the top right of the visualization
   panel you want to modify.
1. Under **Parent search**, select the name of the visualization you want to use as the parent.
1. The parent search query becomes the new first line of your query in the **Search** box. Select **Search** to test
   your new aggregate query, and then select **Save** to save your Dashboard.

### Hide a Parent Search {#parent-hide}

You can [hide any visualization](#hide) by editing it and selecting the **Hide this panel** check box. This can be
especially useful for visualizations you create for the sole purpose of acting as parent searches.

### Parent Search Limitations {#parent-limitations}

You can apply only one parent search per visualization panel.

When you apply a parent search, child searches wait until the parent search has completed. So you will not see "preview"
results.

When you’ve applied a parent search to a visualization, and you open that visualization (using **Actions** >
**Open in Search**), you won't be able to edit the child search's time parameters. This is because the child search is
using the parent search's time span.

## Add Inputs {#add-inputs}

To enable viewers to filter and control visualizations in your Dashboard, you can add Dashboard-level Inputs. For more
information, see [Add Inputs to Your Cribl Search Dashboard](dashboard-inputs).

## Add Interactions {#add-interactions}

To enable viewers to drill down into details when they select Dashboard data points, you can add Interactions to
visualization panels. For more information, see
[Add Interactions to Your Cribl Search Dashboard](dashboard-interactions).

## Add Markdown

You can add a Markdown panel to your Dashboard to enrich it with any custom text. Use markdown to add explanations,
captions, and additional information to your Dashboard. To add a Markdown panel:

1. In an open Dashboard, select **Edit** ![](edit-button.light.svg) at the top right, or press **e** on your keyboard.
1. From the **Add** drop-down menu, select **Markdown**.
1. In the **Edit** pane, replace the sample text with any markdown content you want.<br> The panel lets you use the
   features of [Github-flavored markdown](https://github.github.com/gfm/), including links, tables, codeblocks, and
   images (linking to external sources).<br>You can also use tokens, defined in [Inputs](dashboard-inputs), surrounded by
   `$`. For example, if the Input ID is `firstname`, reference `$firstname$` in the markdown.<br> As you edit, you will
   see a live preview of the rendered text.
1. Select **Save**.


![Cribl Search Dashboard in edit mode with a Markdown panel, Markdown editor on the right and preview on the left.](markdown-panel.png)
{scale="90%" caption="Markdown panel with the pre-built markdown sample"}


> The Markdown panel doesn't support raw HTML.
{.box .success}

## Rename a Dashboard {#rename}

To change the name of an existing Dashboard:

1. From the Cribl Search sidebar, select **Dashboards**.
1. Select the row of the Dashboard that you want to rename.
1. At the top, next to the current Dashboard name, select Settings, and then select the **Edit**
   ![Edit icon](edit-button.light.svg) button.
1. Enter the new Dashboard name, and select **Save**.

## Edit Dashboards as JSON {#json}

You can edit your Dashboards in a JSON editor built into Cribl Search, or import and export JSON files with complete
configurations. This can significantly speed up setting up more complex Dashboards.

To edit an existing Dashboard as JSON:

1. From the Cribl Search sidebar, select **Dashboards**, and open a Dashboard.
1. At the top right, select the Actions ![](actions-dropdown.light.svg) button.
1. Select one of the following:
   - **Edit as JSON** to open the Dashboard configuration in the Cribl Search JSON editor.
   - **Copy Dashboard JSON** to copy the Dashboard JSON to clipboard.
   - **Export Dashboard JSON** to download a JSON file with the Dashboard configuration.
   - **Import Dashboard JSON** to upload a JSON file with a new configuration.

## Refresh Your Dashboards Automatically {#autorefresh}

You can refresh a Dashboard automatically, either by [running it on schedule](#scheduling), or by configuring its
[caching](#cache).

### Configure a Dashboard's Caching {#cache}

You can modify the time for which a Dashboard is cached (in other words, how often it is refreshed).

1. Start [creating](#dashboards-tab) or [editing](editing-dashboards) a Dashboard.
1. Under **Schedule**, select **Cache data**.
1. In **Cache data for**, set the time-to-live (TTL) for the Dashboard cache. This setting defines how often data in the
   Dashboard is refreshed.


![Configuring caching for a new Dashboard](search-dashboard-cache.png)
{scale="90%"}

### Run a Dashboard on Schedule {#scheduling}

You can schedule the searches on **all** of a Dashboard's visualizations to refresh on the default interval you define.

This can be advantageous on a Dashboard where you don't need to display up-to-the-minute data, but you prefer not to
wait for the display to refresh. Schedule the underlying searches to refresh daily, hourly, or every few minutes. When
the visualizations render, they'll fetch the results from the last scheduled run.

After scheduling the Dashboard as a whole, you retain the option to customize the schedules of the searches that
underlie individual visualization panels and Inputs.

To run a Dashboard on a schedule:

1. Start [creating](#dashboards-tab) or [editing](editing-dashboards) a Dashboard.
1. Under **Schedule**, select **Run on schedule**.
1. Configure one of the two:
   - Define the timezone, the base interval (anything from `year` down to `minute`), and refresh time.
   - Or, define a **Cron** schedule string.
1. In **Keep last executions**, define how many past versions of the Dashboard data to keep.<br>This is a rolling buffer
   – on each refresh of the Dashboard, Cribl Search will purge the oldest version beyond this count. Defaults to `2`.


![Setting a schedule for a new Dashboard](search-dashboard-schedule.png)
{scale="90%"}

> ##### Changing Scheduled Dashboard Ownership {#ownership}
>
> If the creator of a scheduled Dashboard is no longer an active Cribl Search Member, active Members with access to the
> Dashboard will be unable to re-run the Dashboard. They will need to [export](#json) the Dashboard's configuration,
> then re-create the Dashboard by importing the config, and finally schedule the new Dashboard to run under their
> ownership. {.box .warning}
