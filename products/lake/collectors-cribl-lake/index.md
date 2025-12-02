# Cribl Lake Collector


Send your data from Cribl Lake to Cribl Stream, using the **Cribl Lake Collector**.

---



## Requirements for Cribl Lake Collector

Cribl Lake Collector can gather data from both Cribl-managed [Cloud](/stream/cloud-workers) and customer-managed [hybrid](/stream/worker-groups) Stream Worker Groups.
Hybrid Worker Groups must be running version 4.8 or higher.

## Configure a Cribl Lake Collector

1. On the top bar, select **Products**, and then select **Stream**.
1. Under **Worker Groups**, select a Worker Group.
1. On the **Worker Groups** submenu, select **Data**, then **Sources**.
1. In the **Sources** tiles, under **Collectors**, select **Cribl Lake**.
1. Select **Add Collector** to open the **New Collector** modal
1. In the Collector modal, configure the following under **Collector Settings**:
    - **Collector ID**: Unique ID for this Collector. For example: `myLakeCollector`.
    - **Lake dataset**: Lake Dataset to collect data from.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Optionally, configure any [Result](#result-settings) and [Advanced](#advanced-settings) settings outlined in the below sections.
1. Select **Save**, then **Commit & Deploy**.

> You can't use QuickConnect to configure Cribl Lake Collector Sources.
{.box .info}

### Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields

In this section, you can add Fields to each event, using [Eval](/stream/eval-function/)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: Toggle on (default) if you want Cribl Stream to send events to normal routing and event processing. Toggle off to select a specific Pipeline/Destination combination. Toggling off exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

Toggling on (default) exposes this field:
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, or `GB`. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might toggle **Send to Routes** off when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
{.box .info}

### Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle on to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Verify Data Flow {#verify}

To verify that the Collector actually collects data, you can start a single run
in the [Preview](/stream/collectors-schedule-run#mode/) mode.

1. Select your Collector's **Run** action.
1. Make sure mode **Preview** is selected and accept other default settings.
1. Confirm with **Run**.
1. Look at the preview screen to check that data is being collected from Cribl Lake.
