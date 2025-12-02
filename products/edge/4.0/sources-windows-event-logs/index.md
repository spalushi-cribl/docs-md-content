# Windows Event Logs


Cribl Edge supports collecting local Windows Event Logs in batches. You can collect the standard event logs – Application, Security, and System – and any other event logs on the machine.

> Type: **Pull**  |  TLS Support: **N/A** | Event Breaker Support: **No** | Uses Key-Value Store
> 
> Cribl Edge Workers support Windows Event Logs only when running on Windows, not on Linux.
>
{.box .info}

## Configuring Cribl Edge to Collect Windows Event Logs

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Windows Event Logs**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Windows Event Logs**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Source definition.

**Log Name**: Enter one or more event logs to collect. `Security` is prefilled as a default. Click **+ Add Name** to specify more event logs (e.g., `Application` or `System`).

### Optional Settings

**Read Mode**: Select **Entire Log** (the default) to read all of the historical events and new events. Select **From last entry** to read only new events.

**Polling Interval**: Specify how often, in seconds, to check for new event logs. Defaults to `10` seconds, and must be at least `1` second. Polling will read each log, up to the **Batch Size**, in their specified order. This Source will not make a polling request if it’s still reading events from the previous request.

**Batch Size**: Set the maximum number of events to read per polling request. By default, each request reads up to `500` events. Set as high as you need to; however, if you configure the Source to pull from multiple event logs, be aware that setting **Batch Size** higher can keep one log waiting longer while the Source collects a batch from another log.

**Tags**: Add tags to use for filtering and grouping in Cribl Edge. Use a tab, space, or hard return between (arbitrary) tag names.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

> This Source defaults to [QuickConnect](quickconnect).  
>
{.box .info}

## Key-Value Store

The Windows Event Logs Source uses key-value store for cursors for each Windows Event Log.
Log names are used as keys.
