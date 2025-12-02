# Cribl Search 4.11.0


With version 4.11.0, Cribl Search offers a new decimal conversion function, streamlined views of search results and
event details, automatic query and visualization suggestions for your Datasets, improved UX for visualizations and
Packs, and more.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl is deprecating [Dataset Acceleration](/search/acceleration) (a Preview feature) in preparation for a different
solution. This feature will be removed in a future release. Please continue to report issues through normal Cribl
[support channels](/support/), but assistance for this deprecated feature might be limited.

### New Cribl.Cloud Domain

With this release, Cribl.Cloud Organizations start moving under a new, unified domain. You will be able to log in to
Cribl.Cloud through https://app.cribl.cloud, and you'll find your portal under
https://app.cribl.cloud/organizations/<organizationId>. The changes will roll out gradually, and the old domain will
still be functional, redirecting to the new one as it is introduced.

The changes do not affect the communication between Stream Workers or Edge Nodes and the Leader Node.
You do not need to take any action to ensure the communication keeps working.

## New Features

This release includes the following new features:

### New Event Details Viewer

On the Search Home [**Events**](search-overview#events) tab, a new drawer provides a streamlined view of event details.
Here, you can easily traverse among events using your keyboard's up/down arrow keys.

### New `todecimal` Function

A new scalar conversion function, [`todecimal`](todecimal), converts numeric values to type [`decimal`](decimal)
([`double`](double), [`real`](real)).

### Suggested KQL Queries

On the Search Home page, [Cribl Copilot](/copilot/) (where enabled) now suggests potentially useful KQL queries, based on analyzing your most recently used Dataset and recent search history. You can select and immediately execute these queries, modify them,
dismiss them, or hide the whole feature if you find it distracting.

### Suggested Visualizations per Dataset

On the Search Home page, hovering over a Dataset now displays a Visualize button. Select this button to prompt Cribl
Copilot to directly [suggest visualizations](copilot-dashboards) that might be useful in displaying this data. You can
then select, modify, or replace these suggestions, and add your chosen visualization to a Dashboard.

### Suggested Git Commits

In the Git Changes modal, [Cribl Copilot](/copilot/) (where enabled) can now automatically suggest commit
messages by analyzing changes against the target branch. You can enable or disable this feature at **Settings** >
**Global** > **Git Settings** > **Copilot**.

### Invoices and Plan Update

In Cribl.Cloud, you'll see a newly updated Plan and Invoices page. This update provides
a clearer picture of your invoice, and provides a monthly breakdown of your
credits consumed by product and infrastructure. We've updated the [Understand Your Cribl.Cloud Bill and Plan](cloud-billing) docs to match.

## UI/UX Improvements

This release includes the following improvements to the Cribl Search UI/UX.

### Streamlined Search Results Display

We've streamlined the [search results status bar](search-overview#track-the-progress-of-your-search) with a new
expand/collapse button, enabling you to control how much detail is displayed here. Switch from Annotated to Zen, and
back.

### Select and Copy Chart Results

The [**Chart** tab](search-overview#charts) now enables selecting and copying results by row.

### Create a Collection When Adding a Dashboard

The **New Dashboard** modal now provides a button to directly create a new Collection along with the Dashboard.

### Reset Visualization Settings

When adding or editing a visualization, you can now select **Reset** to quickly clear the query and all chart settings.

### Return Pack Focus to Last-Used Tab

Reopening a [Pack](packs) (Preview feature) now returns focus to the child tab that you last displayed, rather than
always defaulting to the **Dashboards** tab.

### Pack Dashboard Indicator Column

[Pack](packs) (Preview feature) [Dashboards](packs#dashboards) now identify their parent Pack in a new column on the
global **Dashboards** tab.

### Dropped Data Warning on Send

If any events are dropped during a [send](send) operation, the [search logs](search-details#logs) now display an
appropriate error message.

## Corrections

This release includes several fixes to various areas of Cribl Search, most notably:

| ID                                     | Description                                                                                                                                                                                                                                 |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px"> SEARCH-8741 | The [`extract`](extract-operator) operator can now pass `$` as the `JSONPath` argument, in expressions of the form: `extract_json('$', ...)`.                                                                                               |
| SEARCH-8746                            | The [`project`](project) operator's results now show the correct total count of events in the UI.                                                                                                                                           |
| SEARCH-8650                            | The [`dayofweek`](dayofweek) function now returns an integer between 0 and 6, representing the day of the week beginning on Sunday. (Previously, this function returned an integer representing the current Unix epoch time.)               |
| SEARCH-8535                            | The [`startofweek`](startofweek) and [`endofweek`](endofweek) functions now correctly treat Sunday at 00:00 UTC as the start of the week, as documented. Previously, they incorrectly treated Monday at 00:00 UTC as the start of the week. |
| SEARCH-6684<br>SEARCH-8135             | Visualization [inputs](editing-dashboards#add-inputs) now accept and respond to 0 values, and to empty-string values, rather than blocking the panel search execution.                                                                      |
| SEARCH-9061                            | Using a [Generic HTTP API](set-up-generic-http-api) Dataset Provider or the [`externaldata`](externaldata) operator no longer displays headers in every event.                                                                              |
| SEARCH-7657                            | On Dashboards, the visualization type drop-down now includes the [Map Chart](chart-map) type.                                                                                                                                               |
| SEARCH-7012                            | Datasets' **Path Filter**s are once again correctly parsing expressions that contain `\|\|` (the OR operator). <br>For a time, expressions  of the following form incorrectly ignored the `id=='bar'` part:<br>`id=='foo' \|\| id=='bar'`               |
