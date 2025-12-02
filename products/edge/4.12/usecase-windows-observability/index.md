# Windows Observability Using Cribl Edge


Cribl Edge offers holistic support for Windows observability, allowing comprehensive data collection across your Windows servers to provide reliable monitoring and analysis. You can run Cribl Edge on any [supported Windows version](#supported-windows-versions) to collect observability data (both metrics and logs) and send them to your desired destinations. By combining Cribl Edge with your favorite system of analysis, or data visualization tool, you can identify and investigate issues faster and monitor applications more effectively.

In this guide, we will walk you through the following:

- Deploy Cribl Edge on Windows by using the bootstrap script to add an Edge Node. 
- Create a Windows-specific Fleet, and modify Mapping Rulesets to assign Nodes to Fleets. 
- Explore the Windows Node and get a snapshot of the host system's current state. 
- Locate and collect custom Windows event logs and send them to your desired destination. 
- Collect Windows metrics and send them to your visualization tool of choice (or any supported destination). 
- Collect logs using the File Monitor Source.

As a bonus, we'll walk you through how to use Cribl's [Exec](sources-exec) Source to execute Powershell commands and process them in Cribl. 

## Supported Windows Versions

[Snippet not found: content/edge/4.12/snippets/_windows-supported-versions.md]

## Deploy Cribl Edge on Windows

This section covers creating a Windows-specific Fleet, adding Windows Nodes via bootstrap scripts, and assigning Nodes to Fleets or Subfleets via mapping rules.

### Create a Windows-Specific Fleet 

Before we deploy the Edge Node, let’s configure a Fleet. [Fleets](fleets) and their corresponding Subfleets allow you to author and manage configuration settings for a particular set of Edge Nodes. We recommend separating Linux and Windows machines into different Fleets per OS.

The first step is to [configure a Fleet](fleets#configuring) and name it: `Windows`. 

For our [mapping](#map) illustration later, let’s also create a Subfleet in your `Windows` Fleet. In our example, we named it `windows_logs`.

![Create a Subfleet](usecasewindows-subfleet-4101.png)
{scale="80%" border="true"}

> We use the name `Windows` for demonstration purposes and simplicity. Since Fleet names must be unique across your deployment, you should select a name that makes sense in practice. For better practices on designing and naming your Fleets, see the [Fleet Hierarchy and Design](fleets-design) doc.
>
{.box .info}

### Add a Windows Edge Node Using the Bootstrap Script

Next, add a new Windows Edge Node in your Fleet with the built-in [bootstrap](managing-edge-nodes#bootstrap) script.

1. Navigate into the Windows Fleet you just configured.
1. On the Fleet page, select **Add/Update Edge Node** and select **Windows > Add**.
1. Copy the **Command Line** Script if you're using CMD or **Powershell** if you're using Powershell. 
1. Paste the script into your command line shell and execute it. 

> You must run your choice of shell as a Windows Administrator for the bootstrap script to work. For details, see
> Windows resources:
>- [Start Command Prompt as an Administrator](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj717276(v=ws.11)).
>- [Starting Windows Powershell with Administrative Privileges](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell).
{.box .warning}

![Pasting the script to the Command Prompt](usecasewindows-install-2.9902ec4102.png)
{border="true"}

> Starting in Cribl Edge 4.1, we introduced the following changes for Windows deployments:
>
>- The MSI installer uses the `CRIBL_VOLUME_DIR`. Cribl Edge stores logs,
>  configs, state, and so on in `ProgramData\Cribl`.
>- The default installation type is an Edge single-instance deployment.
>- The bootstrap script includes TLS when you deploy from a Cribl.Cloud Leader. 
{.box .info}

#### Editing the `instance.yml` Files

You can edit the `instance.yml` files (located in `ProgramData\Cribl`) to change options like the following:

- Reassign the Edge Node to a different Fleet.
- Enable TLS after installation (for versions of Cribl Edge older than 4.1). For details, see [Enabling TLS After Installation](deploy-windows#tls).

For details, see [Using YAML Config File](setting-up-leader-and-edge-nodes#using-yaml-config-file-1) to configure an Edge Node.

### Use a Mapping Ruleset to Switch Fleets {#map}

In this step, we will show you how to create a [Mapping Ruleset]mapping-edge-nodes to switch the Windows Edge Node to the Subfleet `windows_logs` you created earlier.

First, we’ll add a new rule in the existing `default` ruleset.

![Map to the SubFleet](usecasewindows-mapping-4101.png)
{border="true"}

> Only one Mapping Ruleset can be active at a time, although a ruleset can contain multiple rules. It's important to keep your Fleet assignments within one ruleset. This example uses the `default` ruleset.
>
{.box .info}

Make sure you reorder the rules, so the new rule is first. 

![Reorder the Rulesets](usecasewindows-mapping2-4101.png)
{border="true"}

Your Windows Edge Node will now be assigned to the new SubFleet.

![View Edge Nodes](usecasewindows-mapping3.66dc631bc7.png)
{border="true"}

For details on mapping, see [Mapping Edge Nodes to Fleets]mapping-edge-nodes.

## Explore Your Windows Node {#source}

When you first log in to Cribl Edge on Windows (single-instance or managed node), you'll land on the **Home** tab where you can explore the metrics and log data that the Node has auto-discovered and manually discover and explore other data of interest. For details, see [Explore Cribl Edge on Windows](explore-edge-win).

From the Fleet's **Explore** menu, select the **System State** tab to see snapshots of the host system's current state in configurable time intervals. For details on what's displayed, see [System State](explore-edge-win#host-metadata).

You can specify which collectors are enabled and the polling interval, by configuring the [System State Source](sources-system-state).

## Configure the Windows Event Logs Source on Cribl Edge {#source}

Next, we'll look at how to get Windows Event Logs into Edge, and on to your chosen destination. We must first identify the logs we want, and then configure Edge's Source to match.

### Locate Windows Event Logs in the Server 

In order to configure the [Windows Event Logs](sources-windows-event-logs) Source on Cribl Edge, you need to figure out which event logs you want to collect from your Windows Server. 

Event logs in Windows Servers are classified into the following broad categories:
- **System**: Events related to the system and its components. Failure to load the boot-start driver is an example of a system-level event.
- **Applications and Services**: Events related to software or an application hosted on a Windows computer get logged under the application event log. For example, if a user encounters a problem in loading the app, it will be logged. 
- **Security**: Events related to the safety of the system. The event gets recorded via the Windows auditing process. Examples include failed authentication and valid logins. 
- **Setup**: Events occur during the installation of the Windows operating system. On domain controllers, this log will also record events related to Active Directory.
- **Forwarded Events**: Contains event logs forwarded from other computers in the same network. 

To find out what other event logs might be of interest, go to your Windows Server Event Viewer. 

![Event Viewer](usecasewindows-eventviewer1.2017138a1e.png)
{scale="25%" border="true"}

To get the accurate name of the event log, right-click and select **Properties**.

![Log properties](usecasewindows-eventviewer2.44ec5ba8f9.png)
{scale="40%" border="true"}

From the **Properties** modal, copy the **Full Name** value to your clipboard. In the next section, when you populate the Edge Windows Event Log Source's [configuration modal](#configure), you'll paste this into that modal's **Event Logs** field.

![Log properties: Full name from the UI](usecasewindows-eventviewer3.8893b1150b.png)
{scale="60%" border="true"}

Alternatively, you can execute the following PowerShell command to list a particular log's name. In this example, we are using a `*` wildcard. 

`powershell Get-WinEvent -ListLog Hardware*`

![Log Name from PowerShell](usecasewindows-eventviewer4.8b45324ba7.png)
{scale="60%" border="true"}

From the powershell output, the **LogName** field is what you would specify in the Cribl UI. In the above screenshot, for example, you would enter `Active Directory Web Services` in the Cribl UI.


### Configure the Windows Event Logs Source {#configure}

Once you have the accurate names of the event logs you want to collect, you can configure and enable the [Windows Event Logs](sources-windows-event-logs) Source in Cribl Edge.

![Configuring Windows Event Logs](usecasewindows-eventviewer5.18a87935c2.png)
{scale="60%" border="true"}

When done, **Commit** and **Deploy** your changes. Before moving on to the next step, check the **Live Data** tab to make sure the logs are generated. 

![Live Data](usecasewindows-eventviewer6.e5ae615f2e.png)
{scale="60%" border="true"}

You can now send the event logs to your visualization tool of choice or any of Cribl Edge’s supported Destinations.

## Configure the Windows Metrics Source on Cribl Edge

To collect Windows metrics rather than log events, configure and enable Cribl Edge's [Windows Metrics](sources-windows-metrics) Source.

The key requirement here is to decide on the level of granularity for the metrics you want to collect. In the **Windows Metrics** Source, go to the **Host Metrics** left tab and select its **Custom** button. Here, we recommend setting the following granularity options for three of the configurable types:
- **CPU**: Enable **Per CPU** metrics.
- **Network**: Enable **Per Interface** metrics.
- **Disk**: Enable **Per Volume** metrics.

On the **Pre-Processing** tab, select the `prometheus_metrics` Pipeline. This will format the metrics for export to Prometheus-compatible services like Grafana.

When done, **Commit** and **Deploy** your changes. Before moving on to the next step, check the **Live Data** tab to make sure the metrics are generated.

To see an example of how to connect to your Windows Metrics Source to a Grafana Cloud Destination and visualize the data in Grafana, see [Monitoring your Infrastructure with Cribl Edge](usecase-infrastructure-monitoring). 

## Configure the File Monitor Source on Cribl Edge {#fm-source} 

To round out this walk-through, we'll take a quick look at enabling the File Monitor Source to gather application logs. 

The Leader deploys with two File Monitor Sources preconfigured, respectively to operate in **Auto** and **Manual** modes. **Manual** mode enables you to specify a single path from which Edge will collect log files. You define this path (with no wildcards) in the **Search path** field shown below. 

**Manual** mode also provides a **Max depth** setting where you can limit collection to a specified depth of subdirectories. To limit collection to the specified **Search path** only, set **Max depth** to `0`. A value of `1` will include only the next level of subdirectories, and higher values will dig deeper.

![File Monitor configuration in Manual mode](usecasewindows-filemonitor2-4101.png)
{scale="60%" border="true"}

Update the **Filename allowlist** with the specific file to monitor; this field supports wildcards. For guidance on how to use allowlists without creating duplicates, see [Using Allowlists Effectively](sources-file-monitor#using-allowlists).

## Bonus Section: Executing PowerShell Commands 

The [Exec](sources-exec) Source enables you to periodically execute a command and collect its `stdout` output. This is typically used in cases when Cribl Edge cannot accomplish collection with native Sources.

The Exec Source command is actually executed from a Windows command prompt, not from a PowerShell prompt.

> The execution of PowerShell is resource-intensive and should be used with caution. On limited resource systems (e.g., domain controllers) it is best to run Exec with PowerShell carefully as the Source uses the host's CPU.
>
{.box .warning}

The main thing to keep in mind when you configure Cribl's [Exec](sources-exec) Source is that what you enter in the **Command** field is slightly different from a PowerShell prompt command. 

PowerShell Prompt Command | Cribl Exec Source configuration
--- | ---
`Get-Process`| `powershell Get-Process`
`# cd c:\mydir` <br/> `# .\myps-script.ps1` |  `powershell c:\mydir\myps-script.ps1`

You can handle a PowerShell command with parameters as follows:

To process the PowerShell command for this script: `Get-CimInstance win32_process | select ProcessId,Name, commandline,HandleCount,WorkingSetSize,VirtualSize`

...enter the following into the Exec Source's **Command** field:

`powershell [-command] "Get-CimInstance win32_process | select ProcessId, Name, commandline, HandleCount, WorkingSetSize,VirtualSize"`

(Include the `-command` parameter if your PowerShell version requires it.)

![Exec Source's Command](usecasewindows-exec1.82777fff14.png)
{scale="60%" border="true"}

The output of the command without using an Event Breaker:

![Default Output](usecasewindows-exec2.6278065ff2.png)
{scale="60%" border="true"}

The output of the command with an Event Breaker:

![Output with an Event Breaker](usecasewindows-exec3.1c8f4a94ba.png)
{scale="60%" border="true"}
