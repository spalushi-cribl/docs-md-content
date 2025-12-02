# Cribl Internal Source


The Cribl Internal Source enables you to capture and send Cribl Edge's own internal **logs** and **metrics** through Routes and Pipelines. 

> Type: **Internal** | TLS Support: **N/A** | Event Breaker Support: **No**
>
{.box .info}

## Scope and Purpose {#scope}

In [distributed](/stream/deploy-distributed) mode, this Source's CriblLogs option can process internal logs only from Worker Processes. (Logs on the Leader remain on the Leader, because the Leader Node is not part of any processing path.) 

In both distributed and [single-instance](deploy-single-instance) mode, this Source's CriblLogs option omits API Process logs, meaning that it omits telemetry/license-validation traffic. You can, however, use a [Script Collector](/stream/collectors-script#examples) to check for API Server (or Worker Group) events.

This Source's CriblMetrics option offers Cribl's most up-to-date and precise view of event throughput and transformation, aggregated every 2 seconds at the Worker Process level. The CriblLogs option is less granular – tracking logs that are written once per minute – so CriblLogs might fail to reflect Worker Process crashes or restarts.

> In Cribl.Cloud, the CriblLogs internal Source is only available on [hybrid](cloud-enterprise#hybrid), customer-managed Edge Nodes.
{.box .cloud}

## Configuring Cribl Internal Logs/Metrics as a Data Source {#configuring}

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **Cribl Internal**. Next, click **Select Existing**, and click either **Cribl Logs** or **Cribl Metrics** to access the configuration options listed below. When prompted, confirm that you want to switch the existing Source to QuickConnect.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **Cribl Internal**. Next, click either **Cribl Logs** or **Cribl Metrics** to open a modal that provides the configuration options listed below.

In either UI, after you've adjusted or confirmed configuration options: On the **CriblLogs** and/or the **CriblMetrics** row, toggle **Enabled** to `Yes`. Confirm your choice in the resulting message box. The screenshot below shows both options successfully enabled.

![Cribl Internal Sources – click either or both of these rows to configure](se-cribl-internal-src-01.6917f2ee52.png)
{border="true"}

### CriblLogs – General Settings {#logs}

**Enabled**: This duplicates the parent page's **Enabled** toggle. Keep it at `Yes` to enable Cribl logs as a Source.

**Input ID**: Enter a unique name to identify this CriblLogs Source definition. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**. 

### CriblLogs – Optional Settings

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### CriblMetrics – General Settings {#metrics}

**Enabled**: This duplicates the parent page's **Enabled** toggle. Keep it at `Yes` to enable Cribl metrics as a Source.

**Input ID**: Enter a unique name to identify this CriblMetrics Source definition. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**. 

### CriblMetrics – Optional Settings {#optional}

**Metric name prefix**: Enter an optional prefix that will be applied to metrics provided by Cribl Edge. The prefix defaults to `cribl.logstream.`

> If Cribl Edge detects `source` or `host` fields in metrics, it copies their values into new dimensions with added `event_` prefixes (e.g., `event_source`). This preserves the original fields' values if they're overwritten in downstream services. For details, see [Duplicated Fields/Dimensions](#2bl).
> 
> You can disable metric collection for these and other fields by specifying them in **Settings** > **System** > **General** > **Limits** > **Metrics** > **Disable field metrics**.
>
{.box .info}

**Full fidelity**: Toggle this to `No` to exclude granular metrics that can cause high CPU load. {{< id `full-fidelity` >}}

The `No` option will drop the following metrics events: 
- `cribl.logstream.host.(in_bytes,in_events,out_bytes,out_events)`
- `cribl.logstream.index.(in_bytes,in_events,out_bytes,out_events)`
- `cribl.logstream.source.(in_bytes,in_events,out_bytes,out_events)`
- `cribl.logstream.sourcetype.(in_bytes,in_events,out_bytes,out_events)`

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

#### Processing Settings

##### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. E.g., here you could specify adding an `index` field.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

##### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

#### Advanced Settings

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Reporting Metrics Less Frequently {#rollup}

By default, Cribl Edge generates internal metrics every 2 seconds. To consume metrics at longer intervals, you can use or adapt the `cribl_metrics_rollup` Pipeline that ships with Cribl Edge. 

Attach this Pipeline to your **Cribl Internal** Source as a [pre‑processing Pipeline](pipelines#input-conditioning-pipelines). The Pipeline's **Rollup Metrics** Function has a default **Time Window** of 30 seconds, which you can adjust to a different granularity as needed. This provides a second lever to reduce granularity, in addition to the [Full fidelity](#full-fidelity) toggle described above.


## Omitting `sourcetype` {#sourcetype}

You can easily drop the `sourcetype` attribute from metrics events, leaving only `event_sourcetype`. This will prevent duplicate `sourcetype` events from being routed to Destinations.

To do this: In the same `cribl_metrics_rollup` pre-processing Pipeline (or a clone) that you attach to your Source, enable the final Eval Function, which applies this **Filter** expression to remove the `sourcetype` field: 
`_metric && _metric.startsWith('cribl.logstream.sourcetype.')`

## Duplicated Fields/Dimensions {#2bl}

The CriblMetrics Source operates on metrics that Edge Nodes report to their Leader Nodes. Typically included are `source` and `host` fields.

Sending metrics from this Source to Splunk is one common use case. Because Splunk might overwrite these two fields, the Source copies their values into new dimensions with added `event_` prefixes: `event_source` and `event_host`. This way, if Splunk does overwrite `source` and/or `host`, their original values remain intact in the new dimensions with `event_` prefixes.

Here's an example of how the added dimensions look in the **Live Capture** window:

![Doubled fields](se-cribl-internal-src-02.d396de22ab.png)
{border="true"}

If you are not sending to a downstream service that overwrites `source` or `host` fields, you can use an appropriate Function to drop the added dimensions. (You also have the option to suppress these fields entirely, as covered above in [Optional Settings](#optional).)

## Internal Fields

The following fields will be added to all events/metrics:

* `source`: set to `cribl`.
* `host`: set to the hostname of the Cribl instance.

Use these fields to guide these events/metrics through Cribl Routes.

> All Cribl internal fields are subject to change and modification. Cribl provides them to assist with analytics and diagnostics, but does not guarantee that they will remain available.
>
{.box .warning}
