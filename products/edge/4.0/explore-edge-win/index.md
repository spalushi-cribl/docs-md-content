# Exploring Cribl Edge on Windows


Cribl Edge on Windows offers easy-to-use tools for exploring and collecting Windows events. You can run Cribl Edge on Windows Server 2016, 2019, or 2022, to collect events via the Windows Events API. 

## Limitations 

Cribl Edge on Windows is currently subject to the following limitations. We will update this page as we enable more features. 

#### Modes 

Cribl Edge on Windows supports only the following modes:

- `Edge: Single`
- `Edge: Managed Edge (managed by Leader)`

This means you can't switch Cribl Edge on Windows into Cribl Stream mode (Single-instance, Worker, or Leader).

You can, however, switch between the Cribl Edge supported modes via the UI, in  **Settings** (top nav) > **Distributed Settings** > **Mode**.

> Do not select an unsupported mode from this drop-down! Doing so will cause the Cribl service to fail. 
>
{.box .warning}

#### Sources and Destinations  

Cribl Edge on Windows supports the same [Sources](sources) and [Destinations](destinations) as Cribl Edge on Linux, with the following exceptions:
- The [AppScope](sources-appscope) and [System Metrics](sources-system-metrics) Sources are unavailable on Windows.
- The [Kubernetes Logs](sources-kubernetes-logs) and [Kubernetes Metrics](sources-kubernetes-metrics) Sources. Cribl Edge on Windows does not support Kubernetes deployments.
- The [Kafka](destinations-kafka) Destination does not support Kerberos authentication on Windows.  


#### Sources on Windows Only 

The following Sources are available **only** when running Cribl Edge on Windows (not on Linux): 

- [Windows Event Logs](sources-windows-event-logs)
- [Window Metrics](sources-windows-metrics) 

#### Functions

The [Grok Function](grok-function) is unavailable on Windows. If you include it in a Pipeline, Cribl Edge processing will skip over it. 

#### Data Formats 

Cribl Edge on Windows does not support reading or writing Parquet files. This is a limitation on the following Destinations: Amazon S3, Azure Blob Storage, MinIO, and FileSystem/NFS. 

## Accessing Cribl Edge on Windows {#home}

When you first log into Cribl Edge on Windows (single-instance or managed node), you'll land on the **Home** tab where you can explore the metrics and log data that the Node has auto-discovered, and can manually discover and explore other data of interest. 

![Edge on Windows](ed_windows_home.c02a5ca2a8.png)
{border="true"}

From the  **Explore** page, you can view more details on your node via the following tabs: 
- [Processes](#processes)
- [Files](#files)
- [Host Info/Metadata](#host-metadata)

### Processes {#processes}
The **Processes** tab lists all the processes running on the Edge Node. 

![Processes tab](ed-processess-windows.eb36b2d3f1.png)
{border="true"}

Click on any of the rows to display a modal with basic information on the process, including CPU usage, Memory usage, and I/O graphs.

![Process details](ed-windows-processes-cpu.2f2e69115f.png)
{border="true"}

In this modal, click **All details** to get a table view of processes' information. To access this information, you would normally need to SSH to the machine; this view makes troubleshooting across multiple systems much easier.

![All detail link](ed-windows-all-deets.0eeaba6a96.png)
{border="true"}

### Files {#files}

The **Files** tab enables you to specify a list of directories and files to actively monitor. There are two options:

- **Manual**
- **Browse** 

(./_img/ed-file-manual-windows.png "File tab -Manual mode" {"scale": "100%", "border":true})

![File tab -Browse mode](ed-file-browse-win.9f12e0232a.png)
{border="true"}

This **Manual** mode provides the following options:

#### Path 
  
Tells Cribl Edge to discover the files within the **Path** (i.e., a directory) that you specify, down to the **Max depth**. 

#### Max Depth

The **Max depth** field is empty by default. This means that Cribl Edge will search subdirectories, and their subdirectories, downward without limit. 

If you specify `0`, Cribl Edge will discover only the top-level files within the **Path**. If you specify `1`, Cribl Edge will discover files one level down. Follow this pattern to specify the depth you want.

#### Allowlist

The **Allowlist** field supports wildcard syntax, and supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Click on any of the files to see a representation of the lines that it contains. 

If the representation doesn't seem ideally suited to the file's contents, you can use the **Event Breakers** tab to [refine it](#exploring-files).

![Event Breakers tab](ed-file-details-win.bb2ec0a121.png)
{border="true"}

In both **Manual** and **Browse** mode, you can click **Monitor this file** to display the [File Monitor](#monitor)'s configuration modal. 

To restrict how much data is displayed, you can use the **Search** tab's search bar and/or time picker.

![Search tab](ed-search-file-win.f8ec2dfa52.png)
{border="true"}

Cribl Edge on Windows does not currently support the  **Inspect** feature to view file metadata, including permissions, file size, user, and modified date.

#### Monitor {#monitor}

Click a file's **Monitor** button to select a text file for monitoring, and to generate events from its lines or records. In the resulting modal, configure your [**File Monitor**](sources-file-monitor) Source. 

![File Monitor configuration modal](ed-file-monitor-discover-win.4a533314d7.png)
{border="true"}

The **Monitor** feature automatically prepopulates the modal with the settings configured on the **Files** tab:
- **Search path**
- **Discovery mode**: Defaults to **Manual** for Windows. 
- **Max depth**
- **Filename allowlist**

Note that the **Connected Destinations** section defaults to **QuickConnect**, Cribl Edge's graphical UI.

![Connected Destinations](ed-file-connected-dest.61ff0a1e42.png)
{border="true"}

When you **Save**, a dialog confirms that you will be routed to the **Collect** page to set up your connections. For details about making these connections, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

![QuickConnect page](ed-file-monitor-quickconnect.5e536e7e78.png)
{border="true"}

#### Exploring Files with Event Breakers {#exploring-files}

When you click a file in the **Files** tab, and Cribl Edge shows a representation of the lines that the file contains – how does that work? Cribl Edge is applying a default Event Breaker to format the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:
- To apply a different (existing) Event Breaker, click **+ Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.
- To create a new Event Breaker ruleset, click **+ Create New**. In the resulting **New Ruleset** modal, proceed as described [here](event-breakers#add). Later, you can persist the new Event Breaker as part of a Source or a Collector.

   While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content and the content as modified by the Event Breaker you're creating. 

Now return to the **Search** tab – the contents of your chosen file will appear with the new Event Breaker applied.

### System State {#host-metadata}

The **System State** upper tab provides access to these left tabs:

- [Host Info](#host-info)
- [Hosts File](#hosts-file)
- [Routes](#host-routes)

### Host Info/Metadata {#host-info}

Cribl Edge can add a ` __metadata ` property to every event emitted from every enabled Source. The **Host Info** tab displays the metadata collected for each Edge Node. 

![Host info / Metadata collected](ed-host-win-info.abfc3b7850.png)

The metadata surfaced by an Edge Node can be used to:
- Enrich events (with an internal `__metadata` field).
- Display to users as a part of instance exploration.

To customize the type of metadata collected, select **Settings** > **General Settings** > **Limits** > **Metadata**. Use the **Event metadata sources** drop-down list (and/or the **+ Add source** button) to add and select metadata sources.

> In Edge mode, all the **Event metadata sources** are enabled by default.
>
{.box .success}

![Set limits](ed-settings-limits.bad032ab90.png)

The metadata sources that you can select here include:
- `os`: Reports details for the host OS and host machine, like OS version, kernel version, CPU and memory resources, hostname, network addresses, etc.
- `cribl`: Reports the Cribl Edge version, mode, Fleet for managed instances, and config version.
- `aws`: Reports details for an EC2 instance, including the instance type, hostname, network addresses, tags, and IAM roles. For security reasons, we report only IAM role names.
- `env`: Reports environment variables.

When these metadata sources are enabled (and can get data), Cribl Edge will add the corresponding property to events, with a nested property for each enabled source.

> Some metadata sources work only in configured environments. For example, the `aws` source is available only when running on an AWS EC2 instance.
> 
> If your security tools report denied outbound traffic to IP addresses like `169.254.169.168` or `169.254.169.254`, you can suppress these by removing `aws` from the metadata sources described above. If you have a proxy setup, Cribl recommends adding these IP addresses to your `no_proxy` environment variable.
>
{.box .info}

#### Hosts File {#hosts-file} 

The **Hosts File** tab displays the host system's current state, based on what's configured on the [System State Source](sources-system-state):

![Hosts File tab](ed-hosts-win-file.f67ba54a49.png)
{border="true"}

The display includes a time picker to select different snapshots of the hosts file. The filter provides a paginated view of files for the selected time bucket.

> For the **Hosts File** tab to appear, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that Source's [Hosts File](sources-system-state#hosts) field is enabled. 
>
{.box .info}

#### Routes {#host-routes}

The **Routes** tab displays entries from host's network routes, based on what's configured on the [System State Source](sources-system-state). 

![Hosts Routes tab](ed-routes-win-tab.0c9d0a6362.png)
{border="true"} 

The display includes a time picker to select different snapshots of the host's network routes. The filter provides a paginated view of network routes for the selected time bucket.

> For the **Hosts File** tab to appear, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that Source's [Routes](sources-system-state#routes) field is enabled. 
>
{.box .info}
