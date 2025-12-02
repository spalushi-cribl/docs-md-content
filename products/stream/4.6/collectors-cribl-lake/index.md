# Cribl Lake Collector




The Cribl Lake Collector gathers data from [Cribl Lake](/lake/).

> The Cribl Lake Collector is available only in Cribl.Cloud,
> and only in cloud-based Worker Groups.
{.box .cloud}

## Configure a Cribl Lake Collector

1. From the top nav, click **Manage**, then select a Worker Group to configure.
2. Select **Data**, then **Sources**.
3. In the **Manage Sources** page’s tiles or left nav, select **Collectors**, then **Cribl Lake**.
4. Click **Add Collector** to open the **Cribl Lake**, then **New Collector** modal, which provides the following options and fields.
5. The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs.
6. Click **Save** when you’ve configured your Collector.

> You can’t use QuickConnect to configure Cribl Lake Collector Sources.
{.box .info}

7. In the Source modal, configure the following under **General Settings**:
    - **Collector ID**: Unique ID for this Collector. For example: `myLakeCollector`.
    - **Lake dataset**: Lake Dataset to collect data from.

8. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

9. Optionally, configure any [Result](#result-settings) and [Advanced](#advanced-settings) settings outlined in the below sections.

10. Click **Save**, then **Commit & Deploy**.

11. Verify that data is being collected. See the [Verify Data Flow](#verify) section below.

### Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination. The `No` setting exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, or `GB`. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
{.box .info}

### Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Verify Data Flow {#verify}

To verify that the Collector actually collects data, you can start a single run
in the [Preview](collectors-schedule-run#mode) mode.

1. In the **Manage Sources** > **Collectors** > **Cribl Lake** screen, select your Collector's **Run** action.
1. Make sure mode **Preview** is selected and accept other default settings.
1. Confirm with **Run**.
1. Look at the preview screen to check that data is being collected from Cribl Lake.
