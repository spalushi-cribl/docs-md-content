# Windows Event Logs Source


Cribl Edge supports collecting local Windows Event Logs. 

For guidance on workflow, see our better practices doc: [Windows Observability Using Cribl Edge](usecase-windows-observability).

> Type: **Pull**  |  TLS Support: **N/A** | Event Breaker Support: **No** | Uses Key-Value Store
> 
> Cribl Edge Workers support Windows Event Logs only when running on Windows, not on Linux.
>
{.box .info}

You can collect standard event logs with the Windows Event Logs Source –
Application, Security, and System – and any other event logs on the machine.

## Configuring Cribl Edge to Collect Windows Event Logs

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
    - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
1. Configure the following under **General Settings**:
     - **Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.
     - **Description**: Optionally, enter a description.
     - **Event logs**: Enter one or more event logs to collect. `Security` is prefilled
   as a default. Click **Add log** to specify more event logs (for example, `Application`
or `System`). To identify the name of the log to collect, you can use the Windows Server Event Viewer.
For more information, see [Locate Windows Event Logs in the Server](usecase-windows-observability#locate-windows-event-logs-in-the-server).
1. Next, you can configure the following **Optional Settings**: 
     - **Read mode**: Select **From last entry** (the default) to read only new events.
     Select **Entire Log** to read all of the historical events and new events.
     - **Event format**: Select JSON or XML as the event format. JSON is the default. You can change this setting at any time.
     - Events include the `__winEvent` property in both XML and JSON format. This
  property is one of the default **Excluded fields** in the [Cribl TCP Destination](destinations-cribl-tcp#advanced-settings). For details, see  [`__winEvent` Property](#win).
     - **Render event message strings**: Toggle on to display rendered message strings in the XML and JSON event output.
    - This toggle only appears when you toggle **Use Windows Tools** off. 
      - Defaults to toggled on when JSON is the selected event format and off when XML 
        is the event format.
      - Events will not contain a `Message` property when toggled off.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Optionally, you can adjust the [Processing](#processing) and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
1. Select **Save**, then **Commit & Deploy**. 

### Processing Settings {#processing}

#### Fields 

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-tab}

**Use Windows Tools**: Toggled off by default. This legacy setting will be removed in a future release.
We highly recommend keeping this option disabled and using the newer native capabilities which are faster and more reliable.

- If toggled off:

  - The Windows Event Logs Source interacts directly with the Windows Event Log
  API, resulting in faster event log processing.
  - **Render event message strings** is available and allows you to include the
  `Message` property in JSON and XML events.
  - The event collector will drain the entire Windows event log (upon startup
  for the first time and when configured to read fully), or drain fully from the
  last place it read (upon subsequent reads and when Windows writes new events
  to the log). It will read as much as possible rather than in batches.

- If toggled on:
  
  - The Windows Event Logs Source will collect events using external tools like
Powershell cmdlets or `wevtutil.exe`.
  - **Render event message strings** is not an option.
  - JSON events will contain rendered message strings, XML will not.

**Polling interval**: Specify how often, in seconds, to check for new event
logs. Defaults to `10` seconds, and must be at least `1` second. Polling will
read each log, up to the **Batch Size**, in their specified order. This Source
will not make a polling request if it’s still reading events from the previous
request.

**Batch size**: Set the maximum number of events to read per polling request. By
default, each request reads up to `500` events. Set as high as you need to;
however, if you configure the Source to pull from multiple event logs, be aware
that setting **Batch Size** higher can keep one log waiting longer while the
Source collects a batch from another log. 

>Batch size and Polling interval only apply when **Use Windows Tools** is toggled on.
{.box .info} 

**Max event bytes**: The maximum number of bytes that can be in an event before Cribl Edge sends it to the Pipelines. Defaults to `51200`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

**Send to Routes**: Enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

**QuickConnect**: Send this Source’s data to one or more Destinations via independent, direct connections.

> This Source defaults to [QuickConnect](quickconnect).  
>
{.box .info}

###  `__winEvent` Property {#win}

The `__winEvent` property contains a JSON representation of the native XML data obtained from the Windows Event API. The Windows Event Logs Source attempts to convert the JSON representation to the JSON schema that Windows Tools uses – however, there are some cases where the automatic conversion process doesn't perfectly match the JSON schema. The `__winEvent` property provides the raw JSON representation, allowing you to extract any necessary data if the conversion process isn't perfect.
{.box .info}

## Configure Event Log Behavior When Full

In [Windows Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer), 
you can configure how Windows handles Event Logs when the event log reaches maximum size.

To configure Windows Event Logs in **Event Viewer**, right-click on an event log and
select **Properties**.

There are three available modes:

- **Overwrite events as needed**: When the log is full, Microsoft overwrites
  oldest events as new events enter.
- **Archive the log when full, do not overwrite events**: When the log is full,
  Microsoft rotates the file and starts a new file.
- **Do not overwrite events (Clear logs manually)**: When the log is full,
  Microsoft drops events until you clear the log manually.

In Cribl Edge, we designed the Windows Event Logs Source primarily for use with
Event Logs in **Overwrite** mode, where it can collect events continuously. The
WEL source may drop events in Event Logs when rotation occurs or when the log
clears.

## Collect From the Forwarded Events Logs

You can collect XML and JSON events from the Windows Forwarded Events log, as
long as **Use Windows Tools** is toggled off in [Advanced Settings](#advanced-settings).

## Collect ETL-Formatted Logs

Microsoft Debug, Analytic, and Tracing logs use the ETL format. These logs are
intended for short-term recording of detailed logging for troubleshooting and
are not suitable for use with continuous collection. They cannot be read when
set to Overwrite mode and they don't support seeking to skip past previously
collected events. 

The WEL Source in Edge supports reading events from ETL logs,
but using them for continuous event collection may result in dropped events and
higher resource usage.

## Key-Value Store

The Windows Event Logs Source uses key-value store for cursors for each Windows Event Log.
Log names are used as keys.

## Resources {#resources}

For processing Pipelines that you can import and adapt to your needs, examine these resources on the Cribl Packs Dispensary:

- [Cribl Edge for Windows](https://packs.cribl.io/packs/cribl-edge-for-windows) – Transform Windows logs collected by Cribl Edge to forward to various downstream services.

- [Microsoft Windows Events](https://packs.cribl.io/packs/cribl-windows-events) – Shape events into JSON or Key=Value format, and dramatically reduce event sizes.

- [Cribl Edge Windows for Common Schema](https://packs.cribl.io/packs/cribl-edge-windows-source) – Transform Windows events into a Common Schema.

- [Exabeam Pack for Windows](https://packs.cribl.io/packs/cribl_exabeam_windows) – Forward specific Windows Event IDs to Exabeam.

- [QRadar Pack for Windows Events](https://packs.cribl.io/packs/cribl-qradar-windows-events) – Optimize Windows events for delivery to QRadar.

## Troubleshooting

If you are running this Source to collect events on a Windows Node without admin access, change the permission on the registry entry for the `EventLog` key: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog` to `Read.` 