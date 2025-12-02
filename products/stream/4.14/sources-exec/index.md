# Exec Source


The Exec Source enables you to periodically execute a command and collect its `stdout` output. This is typically used in cases when Cribl Stream cannot accomplish collection with native Collectors or other Sources.

> Type: **System**  |  TLS Support: **N/A** | Event Breaker Support: **Yes**
>
> This Source is available in Cribl.Cloud on customer-managed [hybrid Workers](cloud-enterprise#hybrid), but not on Cribl-managed Workers in Cribl.Cloud.
> 
{.box .info}

> This Source allows you to run **almost anything** on the host system. Make sure you understand the security and other impacts of commands you plan to execute, before proceeding. To disable it on your Worker Nodes, see [Disable the Exec Source](#disable) below. 
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

## Configure an Exec Source

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
   - **Enabled**: Defaults to toggled on.
   - **Input ID**: Enter a unique name to identify this Exec Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**. 
   - **Command**: Command to execute; supports [Bourne shell](https://www.grymoire.com/Unix/Sh.html) syntax.
   - **Schedule type**: Use the buttons to select either `Interval` or `Cron`. For details, see [Schedule Type](#type) below.
3. Next, you can configure the following **Optional Settings**: 
   - **Retry limit**: Maximum number of retry attempts in the event that the command fails.
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Processing](#processing) and [Advanced](#advanced) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Schedule Type {#type}

Customize the behavior in the corresponding field below the button.
 - **Interval**: Specify how often, in seconds, the command should run. Defaults to `60`.
 - **Schedule**: Enter a [cron expression](https://crontab.cronhub.io/). (You enter the cron expression in UTC time, but the resulting **Estimated Schedule** displays in local time.) 
  
### Processing Settings {#processing}

#### Event Breakers

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

### Internal Fields

Cribl Stream uses internal fields to assist in forwarding data to a Destination. 

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

## Disable the Exec Source on a Node {#disable}

To disable the Exec Source on a specific Worker Node, set the `CRIBL_NOEXEC` environment variable. This will override any configuration pushed from the Leader Node, ensuring the Exec Source remains disabled on that particular node.

The `CRIBL_NOEXEC` environment variable must be set outside of the Cribl product to disable the Exec Source. This variable cannot be modified within the product's interface. 

### How to Disable the Exec Source on Different Platforms

To disable the Exec Source, follow the platform-specific instructions below. Once you've set the `CRIBL_NOEXEC` environment variable, the Exec Source will be disabled. To re-enable it, remove or unset the variable.

#### Disable on Linux Platform

To disable the Exec Source on a Linux system running as a Systemd service, edit the appropriate service definition file:

- Cribl Edge:  `/etc/systemd/system/(cribl|cribl-edge).service` 
- Cribl Stream: `/etc/systemd/system/cribl.service`

To ensure the `CRIBL_NOEXEC` environment variable is set when the Cribl Edge service starts, add `CRIBL_NOEXEC=1` to the service's configuration. If the service is currently running, a restart is required for the change to take effect.

#### Disable on Docker 

To disable the Exec Source on Docker, modify the `docker run` command (or equivalent) to include the `-e CRIBL_NOEXEC=1` flag.

#### Disable on Kubernetes/Helm

To disable the Exec Source on Kubernetes, add the ` --set env.CRIBL_NOEXEC=1` flag to your `Helm install` command or modify the `values.yml` file to include this setting.

#### Disable on Windows (For Cribl Edge)

To add the `CRIBL_NOEXEC` environment variable to Cribl Edge on Windows, use the command prompt to execute the following command:

`setx CRIBL_NOEXEC "1" /M`

This command sets the environment variable for the current user account. The Cribl Edge service runs under `LocalSystem` user account by default. 

