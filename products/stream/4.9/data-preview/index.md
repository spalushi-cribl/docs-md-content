# Data Preview


Cribl Stream's Sample Data and Data Preview features enable you to visually inspect events as they flow into and out of a Pipeline. Preview helps you shape and control events before they're delivered to a Destination, and helps you troubleshoot Pipeline Functions. 

Preview works by taking a set of sample events and passing them through the Pipeline, while displaying the inbound and outbound results in a separate pane. Any time a Function is modified, added, or removed, the Pipeline changes, and so does its displayed output.

The Preview pane is shown below, to the right of the Pipelines pane.

![Preview options](se-data-preview-4.0.4295aa5537.png)
{scale="70%" border="true"}

> You can press the `]` (right-bracket) shortcut key to toggle the visibility of the Preview pane. (This shortcut works when no field has focus.)
>
{.box .info}

## Check Pipeline Status {#status}

From the [Pipelines page](pipelines), select any Pipeline at left to display a **Status** tab in the right Preview pane. Select this tab to reveal graphs of the Pipeline's **Events In**, **Events Out**, and **Events Dropped** over time, along with summary throughput statistics (in events per second).

![Pipeline status tab](se-pipeline-status-tab.bbb0ac66ae.png)
{scale="70%" border="true"}

## Add Sample Data {#add}

When you're on the Pipelines or Routes page, you can add samples through any of the Preview pane's supported options: 

- **Import Data**: Opens a modal where you can paste data from your clipboard or use the **Upload File** button
- **Edge Data**: Lets you import data from any [Edge Nodes](/edge/managing-edge-nodes/) deployed on your account
-  **Capture New**

The **Import Data** options work with content that needs to be broken into events, while the **Capture New** option works with events only.

> The **Edge Nodes** option requires at least one working Edge Node, and is not available when you've teleported to the Node.
>
{.box .success}

### Paste Area

Once you've selected **Import Data** or imported Edge data, you'll see an **Import Sample Data** modal, where you can paste data or upload a file.

![Import Sample Data modal](import-sample-data-modal.82fd0c7ba3.png)
{scale="70%"}

Once your data is onboard, you have options to modify it using Event Breakers, to add and drop fields, and to save it to a file.

![Working with imported data](preview-paste-dns.a5ff29d0c9.png)
{scale="70%"}

### Edge Data

To upload data from a file on an Edge node:

1. Select **Edge Data** and navigate to the Edge node where the file is stored.

   Selecting the Edge Node opens the **Select a file** modal.

2. In **Manual** mode, use the available filters to narrow the results:
    - **Path**: Sets the location from which to discover files.
    - **Allowlist**: This filter supports wildcard syntax (as shown in the screenshot above), and supports the exclamation mark (`!`) for negation.
    - **Max depth**: Sets which layers of files to return highlighted in bold typeface. By default, this field is empty, which implicitly specifies `0`. This default will boldface only the top-level files within the **Path**.

3. Once you find the file you want, select its name to add its contents to the **Add Sample Data** modal, where you'll finish configuring the data sample.

### Event Breaker Settings

Cribl [Event Breakers](event-breakers) are regular expressions that tell Cribl Stream how to break the file or pasted content into events. Breaking will occur at the **start** of the match. Cribl Stream ships with several common breaker patterns out of the box, but you can also configure custom breakers. The UI here is interactive, and you can iterate until you find the exact pattern.

#### Troubleshoot Event Breakers

 If you notice fragmented events, check whether Cribl Stream has added a `__timeoutFlush` internal field to them. This diagnostic field's presence indicates that the events were flushed because the Event Breaker buffer timed out while processing them. These timeouts can be due to large incoming events, backpressure, or other causes.

## Capture Sample Data

The **Capture Data** button opens a slightly different modal – it does not
require event breaking. 

> In order to capture live data, you must have Worker Nodes registered to the Worker Group
for which you're viewing events. You can view registered Worker Nodes from the
**Status** tab in the Source.
>
{.box .info}

In the composite screenshot below, we've already captured some events using the **Capture** drop-down. 

![Capture Data > Capture Sample Data modal](capture-new-modal-composite.1f8ab76ccf.png)
{border="true"}

### Capture from a Single Source or Destination {#live}

To capture data from a single enabled [Source](sources) or [Destination](destinations), it's fastest to use the Sources or Destinations UI instead of the Preview pane. You can initiate an immediate capture by selecting **Live** on the Source's or Destination's configuration row.

![Source > Live button](source-live-button.a7be7d842e.png)
{border="true"}

You can similarly start an immediate capture from within an enabled Source's or Destination's configuration modal, by selecting the modal's **Live Data** tab. 

![Destination modal > Live Data tab](destination-live-data-tab.7e5d2058ca.png)
{border="true"}

Beside the **Live Data** tab's **Fields** selectors is a Copy button, which enables you to copy the field names to the clipboard in CSV format. The **Logs** tab also provides this copy button.

![Destination modal > Live Data - Copy Fields Icon](se-copy-fields-csv.ef239f505f.png)
{border="true"}

## Control Sample Size

To prevent in-memory samples from getting unreasonably large, samples that you input by any means (Import Data, Edge Data, or Capture Data) are constrained by a limit set as follows:
- On a single instance at **Settings** > **General Settings** > **Limits** > **Storage** > **Max sample size**.
- On distributed Worker Groups (both Cribl-managed and customer-managed) at **Group Settings** > **General Settings** > **Limits** > **Storage** > **Max sample size**. 

In each location, the default limit is `256 KB`. You can adjust this upward (to a maximum of `3 MB`) or downward.

## Very Large Integer Values

Cribl Stream's JavaScript implementation can safely represent integers only up to the [Number.MAX_SAFE_INTEGER](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) constant of about 9 quadrillion (precisely, {2^53}‑1). Data Preview will round down any integer larger than this, and trailing 0’s might indicate such rounding.

## Fields

In the **Capture Data** and **Live Data** modals, use the **Fields** sidebar (at left) to streamline how events are displayed. You can toggle among **All** fields, **None** (to reset the display), and check boxes that enable/disable individual fields by name.

### Field Type Symbols

Within the right Preview pane, each field's type is indicated by one of these leading symbols:

| Symbol | Meaning |
| --- | --- |
|α | string
|\# | numeric
|b | boolean
|m | metric
|{} | JSON object
|[] | array

On JSON objects and arrays, you'll also see:

| Symbol | Meaning |
| --- | --- |
|+ | expandable
|- | collapsible

##  Save Sample Data  {#save}

The Preview pane's **Import Data** or **Capture Data** modal, once you've successfully populated it with data, provides options to save the data as a sample and/or datagen file. Select the appropriate button, accept or modify the default/generated file name and other options, and confirm the save.

![Saving sample data](sample-data-save-options.b68b0bf877.png)
{border="true"}

## Access and Manage Data Files

As you add more samples to your system, you can easily access them via the **Sample data file** drop-down. You can also manage and modify sample files via the **Samples** tab highlighted below.

![Managing sample files](se-samples-mgmt-tab-4.0.c251ec45a1.png)
{scale="70%" border="true"}

### Simple Versus Full Preview

Select **Simple** or **Full** beside a file name to display its events in the Preview pane. The **Simple Preview** option enables you to view events on either the **IN** or the **OUT** (processed) side of a single Pipeline.

![Simple Preview schematic](preview-simple-schematic.9296415585.png)
{scale="70%" border="true"}

The **Full Preview** option gives you a choice of viewing events on the **OUT** side of either the [processing or post-processing Pipeline](pipelines#types-of-pipelines). Selecting this option expands the Preview pane's upper controls to include an **Exit Point** drop-down, where you make this choice.

![Full Preview schematic](preview-full-schematic.2097c9aa3c.png)
{scale="70%" border="true"}

### Modify Sample Files {#modify}

1. In the right Preview pane, select the **Samples** tab.
2. Hover over the file name you want to modify. This displays an edit (pencil) button to its left.
3. Select that button to open the modal shown below. It provides options to edit, clone, or delete the sample, save it as a [datagen](sources-datagens) Source, or modify its metadata (**File name**, **Description**, **Expiration time**, and **Tags**).

![Options for modifying a sample](se-samples-modal-3.5.515d06f261.png)
{scale="70%" border="true"}

4. To make changes to the sample, select the modal's **Edit Sample** button. This opens the **Edit Sample** modal shown below, exposing the sample's raw data. 
5. Edit the raw data as desired. 
6. Select **Update Sample** to resave the modified sample, or select **Save as New Sample** to give the modified version a new name.

![Editing a sample file](se-edit-sample-modal-3.5.470d535a86.png)
{scale="70%" border="true"}

## **IN** Tab: Display Samples on the Way IN to the Pipeline {#in}

The Preview pane offers two display options for events: Event and Table. Each format can be useful, depending on the type of data you are previewing. This screenshot shows Event view:

![Event, Table, and Advanced options (composite screenshot)](preview-INJSON.51b8546ef5.png)
{scale="70%" border="true"}

In the **Advanced Settings** menu ![](advanced-menu-light.png) at the upper right, the first few toggles are self-explanatory, and are used primarily to filter the [OUT tab](#out)'s display of processed data. The following subsections cover the less-obvious controls at the menu's bottom.

### Render Whitespace

This toggles between displaying carriage returns, newlines, tabs, and spaces as white space, versus as(respectively) the symbols `␍`, `↵`, `→`, and `·`.

### Timeout (Sec)

If large sample files time out before they fully load, increase this field's default value of `10` seconds. A blank field is interpreted as the minimum allowed timeout value of `1` second.

### Pipeline Profiling

At the Preview pane's top, you can select a sample data file and Pipeline, then select the Pipeline diagnostics (bar-graph) button to access statistics on the selected Pipeline's performance. (The same button is available on Pipelines within a Pack.)

![Pipeline diagnostics](pipeline-diagnostics-button.b6eceeb30a.png)
{scale="75%" border="true"}

The resulting modal's **Statistics** tab displays basic stats about the Pipeline's processing impact on field and event length and counts.

![Pipeline diagnostics > Statistics tab](pipeline-statistics.81b5c74aa4.png)
{scale="70%" border="true"}

The **Pipeline Profile** tab, available from the **Simple Preview** tab, helps you debug your Pipeline by showing individual Functions' contributions to data volume, events count, and the Pipeline's processing time. Every Function has a minimum processing time, including Functions that make no changes to event data, such as the Comment Function.

![Pipeline diagnostics > Pipeline Profile tab](pipeline-profile.c54c6049f8.png)
{scale="70%" border="true"}

> When you profile a [Chain Function](chain-function), this tab condenses all of the chained Pipeline's processing into a single **Chain** row. To see individual statistics on the aggregated Functions, profile the chained Pipeline separately.
>
{.box .info}

The **Advanced CPU Profile** tab, available from the **Simple Preview** tab, displays richer details on individual Function stacks.

![Pipeline diagnostics > Advanced CPU Profile tab](advanced-cpu-profile.06137993a6.png)
{scale="70%" border="true"}

You can disable the profiling features – and regain their own (minimal) resources – by selecting the drop-down list beside the **Run** button and clearing the **Run with Profiler** check box.

![Disabling Pipeline profiling](pipeline-profile-deselect.8733e08bc5.png)
{scale="75%" border="true"}



### Save {#save-submenu}

The **Save** submenu enables you to save your captured data to a file, using either the **Download as JSON** or the **Download as NDJSON** (Newline-Delimited JSON) option.

![Saving sample data as JSON](preview-save-options.08149c59e9.png)
{scale="75%" border="true"}

### Preview Log

The final option on the **Advanced Settings** menu ![](advanced-menu-light.png) opens a modal where you can preview Cribl Stream's internal logs summarizing how this data sample was processed and captured.

## **OUT** Tab: Display Samples on the Way OUT of the Pipeline {#out}

As data traverses through a Pipeline's Functions, events can be modified or even dropped altogether. The **OUT** tab displays the same Event and Table buttons as the [IN tab](#in). Use the **Select Fields** drop-down menu to control which fields the **OUT** tab displays.

The **Advanced Settings** menu ![](advanced-menu-light.png) includes **Show Dropped Events**, **Show Internal Fields**, and **Enable Diff** toggles, which control how the **OUT** tab displays changes to event data.

If you enable the **Show Dropped Events** and **Enable Diff** toggles, the **OUT** tab displays event data as follows:

- Modified fields are highlighted in amber.
- Added fields are highlighted in green.
- Deleted fields are highlighted in red and displayed in strikethrough text.
- Dropped events are displayed in strikethrough text.

![Highlighted fields in Pipeline output](preview-OUT-changes-4.9.png)
{border="true"}

Enable **Show Internal Fields** in the **Advanced Settings** menu ![](advanced-menu-light.png) to display fields that Cribl Stream adds to events, as well as Source-specific fields that Cribl Stream forwards from upstream senders.

![Internal fields highlighted in Pipeline output](preview-OUT-internal-events-4.9.png)
{border="true"}

Use **Enhance Metric Output** in the **Advanced Settings** menu ![](advanced-menu-light.png) to control how the **OUT** tab presents the **Metric name expression** resulting from the [Publish Metrics](publish-metrics-function#metric-name-expression) Function. When the setting is disabled (default), the **Metric name expression** is represented as literal expression string. Enable the setting to see the expression evaluated in the **OUT** tab.
{#enhance-metric-output}

## Manage the Preview Pane

On the pane divider between Route or Pipeline and the preview, select Collapse to hide the Preview pane. This allows the Route or Pipeline configuration to expand to your browser's full width. (The Preview pane collapses automatically on narrow viewports.)

![Collapse / Expand toggle](preview-toggle.af5e855797.png)
{border="true"}

Select Expand at your browser's right edge to restore the split view. The pane divider will snap back to wherever you last dragged it.