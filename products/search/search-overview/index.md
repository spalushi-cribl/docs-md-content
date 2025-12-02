# Cribl Search Tour


Find your way around the Cribl Search UI.

---

## Search Home {#home}

**Search Home** is where you run your current search.


![Search Home, before running a search](search-page-overview-before-search.png)
{scale="100%"}

| Letter | Description                                                                                                                                                           |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A      | **Search Home** is where you run the current search.                                                                                                                  |
| B      | **History** keeps a detailed account of [previous searches](#history).                                                                                                |
| C      | **Saved Searches** contain any searches you [save](#saved).                                                                                                           |
| D      | **Dashboards** enable you to [visualize](dashboards) your search results in a variety of ways.                                                                        |
| E      | **Data** is where you can manage your [Datasets](basic-concepts#datasets), [Dataset Providers](basic-concepts#dataset-providers), and [Datatypes](datatypes).         |
| F      | **Knowledge** contains your [lookups](lookups), [Parsers](parsers), [regexes](regexes), [Grok patterns](grok-patterns), and [Macros](macros)                          |
| G      | **Packs** is where you manage your [Cribl Search Packs](packs).                                                                                                       |
| H      | This gear button lets you change the [UI options](build-a-search#search-options) of the query box.                                                                    |
| I      | **Available Datasets**. The ![Lakehouse](search-lakehouse.png) icon indicates [Cribl Lake Datasets](/lake/datasets) attached to a [Lakehouse](/lake/lakehouse). |
| J      | The query box, where you [write your query](your-first-query).                                                                                                        |
| K      | This **History** displays the most recent searches.                                                                                                                   |
| L      | **Sample Searches** are predefined searches that enable you to explore Cribl Search.                                                                                  |
| M      | The **SEARCH** button runs the query.                                                                                                                                 |

After running a search, your **Search Home** page looks like this:


![Search Home, after running a search](search-page-overview-after-search.png)
{scale="100%"}

| Letter | Description                                                                                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| A      | The **Events** tab shows non-aggregate search results as [events](#events).                                                                  |
| B      | The **Fields** tab displays search results as [fields](#fields).                                                                             |
| C      | The **Chart** tab [visualizes](charting) aggregate search results.                                                                           |
| D      | This gear button lets you change the results view options.                                                                                   |
| E      | The [Histogram](#histogram) lets you view the results for the time you select.                                                               |
| F      | The **Sampling** drop-down controls the [sampling ratio](build-a-search#sampling) of the current search.                                     |
| G      | The **Time range** field sets the [date and time range](build-a-search#time-range) of the current search.                                    |
| H      | The **Details** modal shows the search [details](search-details), such as [logs](search-details#logs) and [metrics](search-details#metrics). |
| I      | The **Actions** drop-down lets you [save](#saved) the current search, [export](#export) the results, and more.                               |


> If you enable [Cribl Copilot](https://cribl.io/products/copilot/), your **Search Home** might also show
> [query suggestions](copilot-kql), and other Copilot features.
{.box .info}

### History {#history}

**History** is where you can view, rerun, save, and delete previously run searches, filter and examine their results,
and add them to [Dashboards](dashboards).

[Search Admins](cloud-managing#search-member-roles) see the history of all searches run within their Organization.
Search Users and Editors see only their own searches.

At the upper left of **History**, use the **Filter history** field to define general filters for the searches. At the
upper right, filter the searches by selecting **All**, **Scheduled**, or **Not Scheduled**.

To refresh the searches, select the refresh button ![](refresh-button.light.svg) next to the **Last refreshed**
timestamp.

**History** includes the following columns:

| Column            | Description                                                                                                                                                                                                                                  |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Search String** | The search string used in the search.<br><br>For [scheduled searches](scheduled-searches), this column also displays the scheduled search **name** (hover over to copy) and the number of executions (select to view the execution history). |
| **Search ID**     | Unique ID for the search. Select a search's ID to launch the [**Details** modal](search-details#details) for the search.                                                                                                                     |
| **Event Count**   | Number of events that the search returned.                                                                                                                                                                                                   |
| **Type**          | The search's job type: `standard`, `scheduled`, `command`, or `dashboard`.                                                                                                                                                                   |
| **Status**        | The search's status: `Running`, `Completed`, or `Canceled`. If a `Completed` search includes items, the status is represented by the number of items and a green icon.                                                                       |
| **Dataset**       | The name of the [Dataset](basic-concepts#datasets) that is defined for the search.                                                                                                                                                           |
| **User**          | The name of the Member who ran the search.                                                                                                                                                                                                   |
| **Duration (s)**  | Duration of the search's run, in seconds.                                                                                                                                                                                                    |
| **Latest Run**    | Date and time when the search was run.                                                                                                                                                                                                       |
| **Actions**       | Menu of [available actions](#history-actions-options) for the search. Select the Actions button ![](actions-dropdown.light.svg) to access these options.                                                                                     |

Most of the columns in **History** support sorting and filtering:

- To sort, hover over a column heading to display the sorting arrows ![](sort-arrows.light.svg) at the right side of the
  column label. Select the up or down arrow to change the column's sort order.
- To filter, hover over a column heading, select the funnel button ![](funnel-filter-button-light.png) and define the
  filter you want to apply.

**History** also provides options for opening and deleting searches:

- To open a search in **Search Home**, select its row. Mind that this doesn't rerun the search, but only loads the
  cached results.
- To delete one or more searches, select the check boxes on the left side of their rows and select **Delete Selected
  Jobs** at the bottom of the page.

#### Actions Options {#history-actions-options}

The **Actions** menu in **History** includes the following options:

- **Details**: Launches the [**Details** modal](search-details#details).
- **Rerun**: Opens [**Search Home**](#home) and reruns the search.
- **Save**: Launches the **Save Search** modal, in which you can name, describe, and adjust the search and save it for
  future access [**Saved Searches**](#saved).
- **Search the Results**: Opens a new browser tab, on which you can
  [query the result set of the search](search-results#search-the-results).
- **Add to Dashboard**: Turn the search into a visualization panel and add it to a new or existing
  [Dashboard](dashboards).
- **Cancel**: Cancel the search. Included in the **Actions** menu only for searches that are still running.
- **Delete**: Delete the search.

#### Toggle Between Personal and Organization History {#admin-history}

As an Admin [Search Member](cloud-managing#search-member-roles), you can quickly switch between viewing the history of
your own searches and the history of all searches run in your Organization.

1. From the Cribl Search sidebar, select **Search Home**.
1. Select **History** next to **Sample Searches** (note that it's separate from the main [**History**](#history)).
1. From the drop-down to the right, select either **My History** (default) or **All History**.


![Toggling Between Personal and Organization History](search-history-my-all.png)
{scale="100%"}

### Saved Searches {#saved}



The **Saved Searches** left tab is where you can add, run, clone, and delete saved searches, and add them to [Dashboards](dashboards). You can also export, share, and import saved searches in [Packs](packs).

[Search Admins](cloud-managing#search-member-roles) see all searches saved within their Organization. (Admins can also schedule searches saved by [active users](#ownership).) Search Users and Editors see only their own saved searches.

At the upper left of **Saved Searches**, use the **Filter saved searches** field to define general filters for the
searches. To create a new saved search, select **Add Search** at the upper right. Filter the saved searches by selecting
**All**, **Scheduled**, or **Not Scheduled**.

After you save a search, you can [schedule](scheduled-searches) it to run at a specified time interval.

**Saved Searches** include the following columns:

| Column            | Description                                                                                                                                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Name**          | The user-provided name and description of the search.                                                                                                                                                                                       |
| **Search String** | The search string used in the search. Select `...more` to launch the **Save Search** modal, in which you can adjust the search and save it as a new search.                                                                                 |
| **Earliest**      | The earliest time within the search's time range. If the value is `0`, the search includes all relevant information without restrictions on how far back in time it looks.                                                                  |
| **Latest**        | The latest time within the search's time range. If the value is `now`, the time range includes the most recent information available at the time the search is executed.                                                                    |
| **Schedule**      | For [scheduled searches](scheduled-searches), the [schedule](scheduled-searches#schedule) on which the search runs. For unscheduled searches, `Off`.                                                                                        |
| **Next Run**      | For [scheduled searches](scheduled-searches), the date and time at which the search will run next. For unscheduled searches, `N/A`.                                                                                                         |
| **Created By**    | The name of the user who saved the search.                                                                                                                                                                                                  |
| **Notifications** | Indicates whether at least one [Notification](notifications) is enabled on a saved search. Select the indicator to open the search's [configuration modal](scheduled-searches#schedule-a-search), where you can access the alert's details. |
| **Actions**       | Menu of [available actions](#saved-actions-options) for the search. Select **Run** to run the saved search. Select the Actions button ![](actions-dropdown.light.svg) to access these options.                                              |

Most of the columns in **Saved Searches** support sorting and filtering:

- To sort, hover over a column heading to display the sorting arrows ![](sort-arrows.light.svg) at the right side of the
  column label. Select the up or down arrow to change the column's sort order.
- To filter, hover over a column heading, select the funnel button ![](funnel-filter-button-light.png) and define the
  filter you want to apply.

Select any row in the table to launch the **Save Search** modal, in which you can adjust the saved search and save it as
a new search for future access from **Saved Searches**.

To delete one or more searches, select the check boxes on the left side of their rows and select **Delete Selected
searches** at the bottom of the page. If the search you want to delete belongs to a Pack, see [Delete a Pack Resource](packs#delete).

#### Actions Options {#saved-actions-options}

The **Actions** menu in **Saved Searches** includes the following options:

- **Clone**: Launches the **Save Search** modal, in which you can adjust the search and save it as a new search.
- **Add to Dashboard**: Turn the search into a visualization panel and add it to a new or existing
  [Dashboard](dashboards).

> ##### Changing Saved Search Ownership {#ownership}
>
> If the creator of a saved search is no longer an active Cribl Search Member, [Search Admins](cloud-managing#search-member-roles) with access to the search can continue to run it ad hoc. (They will now appear as the user requesting the search.) However, they will be unable to schedule the search. The Admin will need to copy the search's details, and to then re-create, save, and schedule the search under their ownership.
{.box .warning}

### Sample Searches

Cribl Search provides several searches against its own internal logs. You can find them in **Search Home** > **Sample
Searches**.


![Sample Searches](search-sample-searches.png)
{scale="100%"}

## Search Results

A search returns [results](search-results) on three tabs:

- **Events** – displays non-aggregated search results. See [Events Tab](#events) for details.
- **Fields** – displays all of the returned fields on a table. See [Fields Tab](#fields) for details.
- **Chart** – displays aggregated search results on a Chart and table. See [Chart Tab](#charts) for details.

Based on the type of the results, Cribl Search automatically selects the appropriate tab. However, you can override this
by using the [`render`](render) operator.

#### Track the Progress of Your Search

Once a search starts running, you can see its progress next to the [**Details**](search-details) indicator.


![Search progress](search-progress.png)
{scale="100%"}

Select ![Events tab](search-expand-toolbar.png) to expand the toolbar. Depending on the type and state of your search,
you'll see the following details:

- Name of the search (if it's a [saved search](#saved)).
- **Latest run**: The timestamp of the most recent initiation of that search. If you opened the search from
  [History](#history), this tells you how old the displayed results are.
- Total number of objects that Cribl Search has **Discovered**, successfully **Scanned**, or **Skipped** (for example,
  because they were in an [unsupported Amazon S3 storage class](set-up-s3#storage-class)).
- Number of **results** returned.
- Duration of the search.

### Events Tab {#events}

The **Events** tab displays a table with the raw log events returned from the search, along with a histogram, and the
field browser.


> To [display results in the **Events** tab](common-examples#display-results-as-events-or-tables) regardless of the search's type,
> append your query with [`| render event`](render).
{.box .info}


![Events tab](search-events-tab.png)
{scale="100%"}

Select any of the rows to open the event details drawer, which displays the fields of the selected event.


![Event details drawer](search-events-drawer.png)
{scale="100%"}

Now, you can navigate between events using your keyboard's **Up** and **Down** arrow keys.

In the drawer, you can:

- Filter the fields displayed. Type into the search bar at the top right.
- View the fields in JSON format. Select **JSON** at the top.
- View the `_raw` field, which contains the original event data. Select **\_raw** at the top.
- Look up events outside of the time frame of the current search. Use the [**Lookaround**](#lookaround) at the top.
- Copy the event's JSON to the system clipboard. Select ![Copy](copy.svg) at the top.
- Pin the drawer open. Select ![Pin](pin.svg) at the top right.

You can also quickly incorporate any of the fields into your next search. Select a field to see these options:

- **Add field in search**: Include this field name and value in the current query. If your query already includes this
  field, you'll see the **Remove field from search** option.
- **Exclude field from search**: Exclude this field value in the query (for example, `action!="ACCEPT"`).
- **New search with field**: Open another Cribl Search window, and include this field name and value in the new query.
- **Copy value to clipboard**: Copy the value of this field to the system clipboard.

To close the drawer, click anywhere outside of it, or select **X** at the top right.

#### Change the Event View Options {#toggle}

When viewing the [**Events** tab](#events), you can change how the results are displayed.

Select the gear button in the heading row (to the left of **Time**).


![Event view options](search-events-options.png)
{scale="50%"}

Here, you can:

- Toggle **Event details panel** off to disable the event details drawer. You'll now be able to view fields directly in
  the table, by selecting **>** at the left of an event row.
- Toggle **Display** to view the original **Event**s, or a **Table** with only the returned fields.<br>(The **Table**
  option allows you to adjust the order of columns displayed. Select and drag a column to the desired location.)
- Toggle **Line numbers** on or off to display or hide line numbers.
- Toggle **Wrap cells** on to prevent values from overflowing.


#### Events Shortcuts {#events-shortcuts}

In Events view, you can hover over field names in the sidebar to display an **Add field to column** button. Select these
buttons to quickly build a custom Table view, showing only the fields that interest you.


![Add columns](search-events-add.png)
{scale="80%"}

In Table view, you can hover over field names in the sidebar or heading row to display a close box. Select the **x** to
remove their columns from the table.


![Remove columns](search-events-remove.png)
{scale="80%"}


> Columns that you hide using these shortcuts are not permanently hidden. They will reappear when you refresh the page
> or reload this search.
{.box .info}

#### Histogram {#histogram}

You can select bars in the histogram to view results for only the selected times. Use shift+select to select multiple
bars.


![Histogram bar selection](search-events-histogram-selection.png)
{scale="55%"}


> ###### Approximate Versus Precise Events Counts {#counts}
> 
> In the **Events** [field browser](#browser)'s left column, and on the [Fields tab](#fields), both covered below: The displayed event counts are approximations, and are not expected to be exact. Top‑*N* lists are generated using probabilistic analysis, and are similarly not exact. However, you can compute precise results for a given field using a query of this form: 
>```
>dataset=<DatasetName> | summarize count() by <fieldName>
>```
>
{.box .warning}

#### Field Browser {#browser}

The field browser on the **Events** tab allows you to easily identify information about the fields returned from your
search. Use the search bar to filter the returned fields. The browser gives you the data type, the unique count of
values the field has, and the percentage of log events returned with the field.


![Event field browser](search-event-field-browser.png)
{scale="100%"}

The options in the **Quick Searches** panel automatically generate and run a new search:

| Quick Search              | Query Example                                                                    |
| ------------------------- | -------------------------------------------------------------------------------- |
| New search with field     | `response_time="*"`                                                              |
| New search without field  | `response_time!="*"`                                                             |
| Add field to search       | `dataset="cribl_search_sample" response_time="*" \| limit 1000`                  |
| Exclude field from search | `dataset="cribl_search_sample" response_time!="*" \| limit 1000`                 |
| Top 10 values             | `dataset="cribl_search_sample" \| limit 1000 \| top-hitters 10 of response_time` |
| Distinct values over time | `dataset="cribl_search_sample" \| limit 1000 \| timestats dcount(response_time)` |
| Min over time             | `dataset="cribl_search_sample" \| limit 1000 \| timestats min(response_time)`    |
| Max over time             | `dataset="cribl_search_sample" \| limit 1000 \| timestats max(response_time)`    |
| Avg over time             | `dataset="cribl_search_sample" \| limit 1000 \| timestats avg(response_time)`    |
| Stdev over time           | `dataset="cribl_search_sample" \| limit 1000 \| timestats stdev(response_time)`  |

See the [Fields](#fields) tab for a table of all of the returned fields.

#### Lookaround {#lookaround}

**Lookaround** allows you to filter search results by adding or subtracting seconds, minutes, hours, or days, enabling
quick exploration of surrounding events.

Select an event's `Time` field or expand a row and select **+/- Lookaround** to view the modal.


![Lookaround UI](search-lookaround-ui.png)
{scale="70%"}

#### Export Search Results as NDJSON {#export}

You can export the raw results of any search in the Newline Delimited JSON (NDJSON) format.

1. [Run a search](build-a-search).
1. At the bottom right of the query box, select [**Actions**](#the-search-page).
1. Select **Export results as NDJSON**. The results are downloaded to your default download location.

### Fields Tab {#fields}

The **Fields** tab displays all of the returned fields on a table by the following dimensions:

- **Type**: Data type.
- **Uniques**: Number of unique values.
- **Nulls**: Number of `null` values.
- **Top Value Distribution**: How often values occur using the standard cumulative beta distribution
  [function](https://www.npmjs.com/package/@stdlib/stats-base-dists-beta-cdf).
- **Presence**: Percentage of results that contain the field.


![Fields tab](search-fields-tab.png)
{scale="100%"}

The table supports sorting and filtering, and allows you to adjust the order of columns.

- Select a column heading to change its sorting order.
- Hover over a column heading and select the funnel icon to define a filter.
- Select the triangle to the left of a **Field name** to expand and collapse the list of returned values.


> See the note above about [Approximate Versus Precise Events Counts](#counts).
>
{.box .warning}

### Chart Tab {#charts}

When you run the [`summarize`](summarize), [`eventstats`](eventstats), or [`timestats`](timestats) operator along with
an aggregation function, the **Chart** tab automatically displays your results in a Chart, along with a corresponding
results table. You can select from various Chart types and color palettes, manipulate how your results are plotted, and
customize the results table display.


> To [display results in the **Chart** tab](common-examples#display-results-as-events-or-tables) regardless of the search's type,
> append your query with [`| render table`](render).
{.box .info}


![Aggregate results area on a Bar Chart](search-chart-tab2.png)
{scale="100%"}

For detailed information about manipulating data and visualizations in aggregate search results, see
[Charting](charting).
