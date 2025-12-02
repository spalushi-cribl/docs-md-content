# Data Preview


You can use the Data Preview tool to preview, test, and validate the Functions you add to a Pipeline or Route. It helps ensure that your Pipeline and Function configurations produce the expected output, giving you confidence that Cribl Stream processes your data as intended.

The Data Preview tool works by processing a set of sample events and passing them through the Pipeline. Then, it displays the inbound and outbound results in a dedicated pane. Whenever you modify, add, or remove a Function, the output updates instantly to reflect your changes.

For example, suppose you add a Parser Function to your Pipeline to convert raw, unstructured data into structured key-value pairs. After parsing, you may choose to promote these key-value pairs to the field level to enhance data processing and analysis. To verify these changes meet your expectations, you can use the **OUT** tab in the Data Preview pane to review the transformed data.

![Add Functions to your Pipeline to transform data, then preview the impact on sample data in the Data Preview](pipeline-data-preview-4101.png)
{scale="70%" border="true"}

In addition to previewing data, the Data Preview tool offers:

- **Metrics View**: This metrics-first view in the Data Preview pane automatically detects and visualizes metrics events in your sample data, organizing them by metric name for a more intuitive experience. Designed for metrics-heavy datasets, this view provides insights into key metrics, dimensions, and their unique values. It helps you identify high-cardinality data — datasets with a large number of uncommon or unique values — enabling you to make informed decisions about data processing before forwarding it to potentially costly downstream receivers. See [Metrics View](#metrics-view) for more information.
- **Pipeline diagnostics**: Includes status updates and summary throughput statistics to assess the health of the Pipeline and its Functions. See [Pipeline Diagnostics and Statistics](#pipeline-diagnostics-and-statistics) for more details.

> The [Get Started With Cribl Stream](getting-started-guide) tutorial provides hands-on experience with the Data Preview tool if you would like to learn more about this tool through personal experimentation and exploration.
>
{.box .info}

## How to Use the Data Preview Tool

To use the Data Preview tool:

1. Create or import some sample data. See [Add Sample Data](#add) for information about different methods to generate sample data.

1. Then, use the different Data Preview views to test and validate the changes you make in your Pipeline. See [Explore Data and Validate Pipelines with Different Views](#explore-validate) to learn about the different views you can apply to your data.

## Add Sample Data {#add}

To use the Data Preview tool, your Pipeline first needs sample data to work with. You can get sample data using a few methods:

- [Upload a sample data file](#upload)
- [Copy data from a clipboard](#clipboard)
- [Import data from Cribl Edge Nodes](#edge)
- [Capture new sample data](#capture)
- [Capture live data from a Single Source or Destination](#live)
- [Use a datagen or a sample data file provided by Cribl](#datagens)

When deciding which method to use, note that:

- The **Import Data** options work with content that needs to be broken into events, meaning it needs Event Breakers.
- The **Capture New** option works with events only.

Cribl [Event Breakers](event-breakers) are regular expressions that tell Cribl Stream how to break the file or pasted content into events. Breaking will occur at the **start** of the match. Cribl Stream ships with several common breaker patterns out of the box, but you can also configure custom breakers. The UI here is interactive, and you can iterate until you find the exact pattern.

If you notice fragmented events, check whether Cribl Stream has added a `__timeoutFlush` internal field to them. This diagnostic field's presence indicates that Cribl Stream flushed the events because the Event Breaker buffer timed out while processing them. These timeouts can be due to large incoming events, backpressure, or other causes. See [Event Model: Internal Fields](event-model#internal-fields) for more information about how to check whether Cribl Stream added internal fields to your data.

### Upload a Sample Data File {#upload}

To upload a sample data file:

1. In the sidebar, select **Worker Groups**, then select a Worker Group. On the **Worker Groups** submenu, select **Processing**, then **Pipelines**.

1. Select the **Sample Data** tab in the **Data Preview** pane. Select **Import Data**.

1. In the **Import Sample Data** modal, drag and drop your sample file or select **Upload** to navigate to the file on your computer.

1. Select an appropriate option from the **Event Breaker Settings** menu.

1. Fill in the **File name**, **Description**, and **Tags** fields as needed.

1. Add or drop fields as needed.

   ![Example of imported data](preview-paste-dns-4101.png)
   {scale="70%"}

1. Select **Save as a Sample File**.

After saving the file, verify that your new sample file appears in the list of available data in the **Sample Data** tab in the Data Preview pane.

### Copy Data from a Clipboard {#clipboard}

To copy sample data from a clipboard:

1. In the sidebar, select **Worker Groups**, then select a Worker Group. On the **Worker Groups** submenu, select **Processing**, then **Pipelines**.

1. Select the **Sample Data** tab in the **Data Preview** pane. Select **Import Data**.

1. In the **Import Sample Data** modal, paste data from your computer clipboard into the provided field.

   ![Import Sample Data modal](import-sample-data-modal.82fd0c7ba3.png)
   {scale="70%"}

1. Select an appropriate option from the **Event Breaker Settings** menu.

1. Fill in the **File name**, **Description**, and **Tags** fields as needed.

1. Add or drop fields as needed.

1. Select **Save as a Sample File**.

After saving the file, verify that your new sample file appears in the list of available data in the **Sample Data** tab in the Data Preview pane.

### Import Data from Cribl Edge Nodes {#edge}

To import data from Cribl Edge Nodes, you must have at least one working Edge Node. To upload data from a file on an Edge Node:

1. In the sidebar, select **Worker Groups**, then select a Worker Group. On the **Worker Groups** submenu, select **Processing**, then **Pipelines**.

1. Select the **Sample Data** tab in the **Data Preview** pane. Select **Import Data**.

1. In the **Import Sample Data** modal, select **Edge Data** and navigate to the Edge Node that has the stored file.

1. In **Manual** mode, use the available filters to narrow the results:

    - **Path**: The location from which to discover files.
    - **Allowlist**: Supports wildcard syntax and supports the exclamation mark (`!`) for negation.
    - **Max depth**: Sets which layers of files to return highlighted in bold typeface. By default, this field is empty, which implicitly specifies `0`. This default will boldface only the top-level files within the **Path**.

1. Once you find the file you want, select its name to add its contents to the **Add Sample Data** modal.

1. Select an appropriate option from the **Event Breaker Settings** menu.

1. Fill in the **File name**, **Description**, and **Tags** fields as needed.

1. Add or drop fields as needed.

1. Select **Save as a Sample File**.

After saving the file, verify that your new sample file appears in the list of available data in the **Sample Data** tab in the Data Preview pane.

### Capture New Sample Data {#capture}

The **Capture Data** mode does not require event breaking. To capture live data, you must have Worker Nodes registered to the Worker Group for which you're viewing events. You can view registered Worker Nodes from the **Status** tab in the Source.

To capture new sample data:

Worker Group, Data

1. In the sidebar, select **Worker Groups**, then select a Worker Group. On the **Worker Groups** submenu, select **Processing**, then **Pipelines**.

1. Select the **Sample Data** tab in the **Data Preview** pane. Select **Capture Data**.

1. Select **Capture**. Accept the default settings and select **Start**.

1. When the modal finishes populating with events, select **Save as Sample File**. In the sample file settings, change the generated file name to a name you'll recognize.

![Example captured data](capture-new-modal-composite.1f8ab76ccf.png)
{border="true"}

After saving the file, verify that your new sample file appears in the list of available data in the **Sample Data** tab in the Data Preview pane.

### Capture Live Data from a Single Source or Destination {#live}

To capture data from a single enabled [Source](sources) or [Destination](destinations), it's fastest to use the Sources or Destinations pages instead of the Preview pane.

To capture live data:

1. In the sidebar, select **Worker Groups**, then select a Worker Group. On the **Worker Groups** submenu, select **Data**, then **Sources** or **Destinations**.

1. Select **Live** on the Source or Destination configuration row to initiate a live capture.

   ![Live button on a Source](source-live-button-4101.png)
   {border="true"}

1. Select **Capture**. Accept or modify the default settings as needed and select **Start**.

1. When the modal finishes populating with events, select **Save as Sample File**. In the sample file settings, change the generated file name to a name you'll recognize.

After saving the file, verify that your new sample file appears in the list of available data in the **Sample Data** tab in the Data Preview pane.

Alternatively, you can start an immediate capture from within an enabled Source's or Destination's configuration modal, by selecting the modal's **Live Data** tab.

![The Live Data tab in the Destination modal](destination-live-data-tab.7e5d2058ca.png)
{border="true"}

You can also use the **Fields** selector in the **Live Data** is a Copy button, which enables you to copy the field names to the clipboard in CSV format. The **Logs** tab also provides this copy button.

![The Copy Fields Icon in the Live Data tab](se-copy-fields-csv.ef239f505f.png)
{border="true"}

### Use a Datagen or Sample Data File from Cribl {#datagens}

You can use the Cribl-provided datagens or sample data files to test and validate your data. See [Using Datagens](datagens) for more information.

## Modify Sample Files {#modify}

To modify existing sample files:

1. In the right Preview pane, select the **Samples** tab.

1. Hover over the file name you want to modify to show an edit (pencil) button next to the file name.

1. Select that button to open the modal shown below. It provides options to edit, clone, or delete the sample, save it as a [datagen](sources-datagens) Source, or modify its metadata (**File name**, **Description**, **Expiration time**, and **Tags**).

   ![Options for modifying a sample](se-samples-modal-3.5.515d06f261.png)
   {scale="70%" border="true"}

1. To make changes to the sample, select the modal's **Edit Sample** button. This opens the **Edit Sample** modal shown below, exposing the sample's raw data.

1. Edit the raw data as desired.

1. Select **Update Sample** to resave the modified sample, or select **Save as New Sample** to give the modified version a new name.

  ![Editing a sample file](se-edit-sample-modal-3.5.470d535a86.png)
  {scale="70%" border="true"}

## Troubleshoot Sample Data

This section describes some strategies for managing potential issues with sample data that you might encounter.

### Sample Sizes Are Too Large

To prevent in-memory samples from getting unreasonably large, Cribl Stream constrains all data samples by a limit set as follows:

- On a single instance at **Settings** > **General Settings** > **Limits** > **Storage** > **Sample size limit**.
- On distributed Worker Groups (both Cribl-managed and customer-managed) at **Group Settings** > **General Settings** > **Limits** > **Storage** > **Sample size limit**.

In each location, the default limit is `256 KB`. You can adjust this down or up to a maximum of `3 MB`.

### Integer Values Are Too Large

The JavaScript implementation in @Cribl Stream can safely represent integers only up to the [Number.MAX_SAFE_INTEGER](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) constant of about 9 quadrillion (precisely, {2^53}-1). Data Preview will round down any integer larger than this, and trailing zeroes might indicate such rounding.

### Change Field Displays

In the **Capture Data** and **Live Data** modals, use the **Fields** sidebar to streamline how events display. You can toggle among **All** fields, **None** (to reset the display), and check boxes that enable or disable individual fields by name.

Within the right Preview pane, each field's type is indicated by one of these leading symbols:

| Symbol | Meaning |
| ------ | ------- |
| α | string |
| \# | numeric |
| b | boolean |
| m | metric |
| {} | JSON object |
| [] | array |

On JSON objects and arrays:

| Symbol | Meaning |
| ----- | -------- |
| + | expandable |
| - | collapsible |

## Explore Data and Validate Pipelines with Different Views {#explore-validate}

Once you have created or imported sample data for your Pipeline, you can explore and analyze it using the various views available in the Data Preview pane. These views allow you to preview and tesst changes in real time, helping ensure that your data processing Functions produce the expected results. By leveraging the Data Preview tool, you can confidently validate that your Pipeline transformations align with your intended outcomes.

### Simple or Full Previews

Select **Simple Preview** or **Full Preview** beside a file name to display its events in the Preview pane:

- **Simple Preview**: View events on either the **IN** or the **OUT** (processed) side of a single Pipeline.
- **Full Preview**: View events on the **OUT** side of either the [processing or post-processing Pipeline](pipelines#types-of-pipelines). Selecting this option expands the Preview pane's upper controls to include an **Exit Point** drop-down, where you make this choice.

### Before and After Views

The Data Preview pane offers two time-based display options for previewing sample data:

- The **IN** tab displays samples as they appear on the way *in* to the Pipeline, before data processing.
- The **OUT** tab displays the same information, but shows the sample data as it will appear on way *out* of the Pipeline after the data processing. Use the **Select Fields** drop-down menu to control which fields the **OUT** tab displays.

In most cases, you will typically use the **OUT** tab to preview, test, and validate your Pipeline.

### Event and Table Views

The Data Preview toolbar has several available views. Each format can be useful, depending on the type of data you are previewing.

The Event View displays data in JSON format for both the **IN** and **OUT** tabs.

![Example Event View](data-preview-event-view.png)
{scale="70%" border="true"}

The Table View in displays data as a table for both the **IN** and **OUT** tabs.

![Example Table View](data-preview-table-view.png)
{scale="70%" border="true"}

### Metrics View {#metrics-view}

If your data sample contains metrics events, the Data Preview tool automatically switches to the Metrics view. It intelligently detects the underlying data structure and switches to this view when needed to display relevant insights. This metrics-first view allows you to visualize metrics events as you build your Pipeline. It allows you to analyze dimensions, the number of unique values, and the percentage each metric contributes to the total series.

The Metrics view is especially useful for datasets with a high volume of unique (and potentially costly) metric data points. By highlighting metric events with *high cardinality* — datasets with a large number of uncommon or unique values — you can make more informed decisions about processing the data before forwarding it to downstream receivers, such as high-cost data processing or storage solutions.

![Example Metrics View](data-preview-metrics-view.png)
{scale="70%" border="true"}

The Metrics view provides the following high-level summary statistics to help you understand the structure of your metrics event data:

- **Total Metrics**: The total number of metrics in the sample data. If a metric event contains multiple metrics, each one is counted separately.
- **Total Series**: The total number of series in the metric event, where a series represents a unique combination of a metric and a dimension pair.
- **Unique Metrics**: The total number of distinct metrics within the sample data.
- **Total Dimensions**: The total number of dimensions across all metric events.

Beneath the summary, the Metrics view presents a table with the following columns:

- **Metric name**: The name of a unique metric.
- **Type**: The type of metric, such as a gauge, histogram, and more.
- **No. of series**: Displays the number of series in which the metric appears, calculated as the unique combination of a metric and a dimension pair.
- **% of total series**: The proportion of series represented by this metric, shown as a percentage of the total series.

Each row represents a single metric, which you can expand to show its associated dimensions. The expanded view displays additional details:

- **Dimension**: The name of the dimension.
- **Unique value count**: The number of unique values for this dimension, also known as dimensionality.
- **Example values**: The first five recorded values in the sample data, providing insight into typical values.

## Pipeline Diagnostics and Statistics {#pipeline-diagnostics-and-statistics}

This section explains the available tools in the Data Preview tool that you can use to get information about your Pipeline and Functions.

### Check Pipeline Status {#status}

From the [Pipelines page](pipelines), select any Pipeline to display a **Status** tab in the Data Preview pane. Select this tab to show graphs of the Pipeline **Events In**, **Events Out**, and **Events Dropped** over time, along with summary throughput statistics (in events per second).

![Pipeline status tab](se-pipeline-status-tab.bbb0ac66ae.png)
{scale="70%" border="true"}

### Pipeline Profiling

At top of the Preview pane, you can select a sample data file and Pipeline, then select the Pipeline diagnostics (bar-graph) button to access statistics on the selected Pipeline's performance.

![Pipeline diagnostics](pipeline-diagnostics-button.b6eceeb30a.png)
{scale="75%" border="true"}

The **Statistics** tab in the resulting modal displays the processing impact of the Pipeline on field length and counts.

![Pipeline diagnostics > Statistics tab](pipeline-statistics.81b5c74aa4.png)
{scale="70%" border="true"}

The **Pipeline Profile** tab, available from the **Simple Preview** tab, helps you debug your Pipeline by showing the contribution of individual Functions to data volume, events count, and the Pipeline processing time. Every Function has a minimum processing time, including Functions that make no changes to event data, such as the Comment Function.

![Pipeline diagnostics > Pipeline Profile tab](pipeline-profile.c54c6049f8.png)
{scale="70%" border="true"}

> When you profile a [Chain Function](chain-function), this tab condenses all of the chained Pipeline's processing into a single **Chain** row. To see individual statistics on the aggregated Functions, profile the chained Pipeline separately.
>
{.box .info}

The **Advanced CPU Profile** tab, available from the **Simple Preview** tab, displays richer details on individual Function stacks.

![Pipeline diagnostics > Advanced CPU Profile tab](advanced-cpu-profile.06137993a6.png)
{scale="70%" border="true"}

You can disable the profiling features to regain their own (minimal) resources. To disable profiling features, select the drop-down list beside the **Run** button and clear the **Run with Profiler** check box.

![Disabling Pipeline profiling](pipeline-profile-deselect.8733e08bc5.png)
{scale="75%" border="true"}

## Advanced Settings

The **Advanced Settings** menu ![](advanced-menu-light.png) includes **Show Dropped Events**, **Show Internal Fields**, and **Enable Diff** toggles, which control how the **OUT** tab displays changes to event data.

If you enable the **Show Dropped Events** and **Enable Diff** toggles, the **OUT** tab displays event data as follows:

- Modified fields are highlighted in amber.
- Added fields are highlighted in green.
- Deleted fields are highlighted in red and displayed in strikethrough text.
- Dropped events are displayed in strikethrough text.

![Highlighted fields in Pipeline output](preview-OUT-changes-4.9.png)
{border="true"}

The rest of this section describes the additional fields available in **Advanced Settings**.

**Show Internal Fields**: Display fields that Cribl Stream adds to events, as well as Source-specific fields that Cribl Stream forwards from upstream senders.

![Internal fields highlighted in Pipeline output](preview-OUT-internal-events-4.9.png)
{border="true"}

**Render Whitespace**: This toggles between displaying carriage returns, newlines, tabs, and spaces as white space, versus as (respectively) the symbols `␍`, `↵`, `→`, and `·`.

**Enhance Metric Output**: Controls how the **OUT** tab presents the **Metric name expression** resulting from the [Publish Metrics](publish-metrics-function#metric-name-expression) Function. When you disable this setting (default), Cribl Stream represents the **Metric name expression** as literal expression string. Enable the setting to see the expression evaluated in the **OUT** tab.
{#enhance-metric-output}

**Timeout (Sec)**: If large sample files time out before they fully load, you can adjust this setting to increase the default value of `10` seconds. Cribl Stream interprets a blank field as the minimum allowed timeout value of `1` second.

**Save as New Sample**: Enables you to save your captured data to a file, using either the **Download as JSON** or the **Download as NDJSON** (Newline-Delimited JSON) option.

![Saving sample data as JSON](preview-save-options.08149c59e9.png)
{scale="75%" border="true"}

**Preview Log**: Opens a modal where you can preview Cribl Stream's internal logs summarizing how this data sample was processed and captured.
