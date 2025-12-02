# Exec


The Exec Source enables you to periodically execute a command and collect its `stdout` output. This is typically used in cases when Cribl Edge cannot accomplish collection with native Collectors or other Sources.

> Available in Cribl.Cloud: **No** | Type: **System**  |  TLS Support: **N/A** | Event Breaker Support: **Yes**
>
{.box .info}

> This Source allows you to run **almost anything** on the host system. Make sure you understand the security and other impacts of commands you plan to execute, before proceeding.
>
{.box .warning}

Especially for monitoring or polling, you'll need to receive the command or script's output periodically. For this reason, the Exec Source lets you specify when the command or script should run, by time interval or by cron-style schedule.

Here are a few examples of what you can run from an Exec Source:

* Database queries.
* Custom commands to poll databases or apps.
* `ping` to check latency.
* `ps -ax` to get a list of running processes.
* `SNMP GET` requests to query an SNMP agent about network entities.
* `netstat -natp` to get lists of sockets and TCP ports, and what they're doing.
* `mtr -c 1 --report --json 8.8.8.8` to run a traceroute on a location (here, `8.8.8.8`), and packaging up a report in JSON format.

## Configuring an Exec Source

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **Exec**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **Exec**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings {#general-settings}

**Enabled**: Defaults to `Yes`.

**Input ID**: Enter a unique name to identify this Exec Source definition. 

**Command**: Command to execute; supports [Bourne shell](https://www.grymoire.com/Unix/Sh.html) syntax.

**Schedule type**: Use the buttons to select either `Interval` or `Cron`. Customize the behavior in the corresponding field below the button.

* **Interval**: Specify how often, in seconds, the command should run. Defaults to `60`.

* **Schedule**: Enter a [cron expression](https://crontab.cronhub.io/). (You enter the cron expression in UTC time, but the resulting **Estimated Schedule** displays in local time.) 

### Optional Settings {#optional}

**Max retries**: Maximum number of retry attempts in the event that the command fails.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Processing Settings

#### Event Breakers

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

### Internal Fields

Cribl Edge uses internal fields to assist in forwarding data to a Destination. 

You can find the following event fields on the **Live Data** tab of the selected
Exec Source, including the hostname and source of the event:
 
- `_raw`
- `_time`
- `cribl_breaker`
- `host`
- `source`

![Event fields](exec-source-event-fields.6ff20079f9.png)
{border="true"}

### Troubleshooting

If a command isn't running as expected, the **Logs** tab can help you identify
issues by displaying the command executed, its elapsed time, and its exit code.

![Event fields](exec-source-log-fields.ceca539402.png)
{scale="75%" border="true"}