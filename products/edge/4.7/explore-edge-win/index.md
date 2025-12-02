# Exploring Cribl Edge on Windows


Cribl Edge on Windows offers easy-to-use tools for exploring and collecting Windows events. You can run Cribl Edge on a supported Windows environment and collect events via the Windows Events API. 

## Limitations 

Cribl Edge on Windows is currently subject to the following limitations:

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

The **Files** tab enables you to specify a list of directories and files to actively monitor. 

![File tab -Manual mode](ed-file-manual-windows.1ded09774f.png)
{border="true"}

The **Actions** column allows you to:
- **View**: Displays a representation of the lines this column contains. You can also click any file row. To restrict how much data is displayed, use the search field or time picker on the **Search** tab. 

- **Inspect**: Opens the **Inspect File** tab to show file metadata, including details like permissions, file size, user, and modified date.

![Inspect File tab](ed-file-inspectwin.9894059b54.png)
{border="true"}

- **Monitor**: Displays the [File Monitor's](#monitor) configuration modal. 

- **Ingest**: Opens the [Ingest file](#ingest) modal to send the file content to Routes/Pipelines for further processing or downstream to any destination you have configured. This is useful for testing and troubleshooting your configurations.

The **Files** tab provides the following options.

#### File Discovery Modes 

There are two discovery modes:

- **Manual**
- **Browse** 

The **Manual** mode provides the following options:

#### Path 
  
The **Path** field tells Cribl Edge to discover the files within the path (a directory) that you specify, down to the **Max depth**.  

#### Allowlist

The **Allowlist** field supports wildcard syntax, and supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Click any file to see a representation of the lines it contains. To restrict how much data is displayed, you can use the search field or time picker on the **Search** tab. 

![Search tab](ed-search-file-win.f8ec2dfa52.png)
{border="true"}

If the representation of events shown on the **Search** tab isn't ideally suited to the file's contents, you can use the **Event Breakers** tab to [refine it](#exploring-files).

![Event Breakers tab](ed-file-details-win.bb2ec0a121.png)
{border="true"}


#### Max Depth

The **Max depth** field is empty by default. Cribl Edge will search subdirectories, and their subdirectories, downward without limit.  

If you enter `0`, Cribl Edge will discover only the top-level files within the specified path. If you specify `1`, Cribl Edge will discover files one level down from the top. Follow this pattern to specify the depth you want.

#### Monitor Files

The **Monitor Files** button opens a **New File Monitor** modal prefilled with the discovery mode, path, max depth, and allowlist entries specified on the **Files** tab.

#### Monitor {#monitor}

Click a file's **Monitor** button to configure your [**File Monitor**](sources-file-monitor) Source to generate events from the file's lines or records. 

![New File Monitor modal](ed-file-monitor-discover-win.4a533314d7.png)
{border="true"}

The **Monitor** feature automatically prepopulates the modal with the following settings configured on the **Files** tab:
- **Search path**
- **Discovery mode**: Defaults to **Manual** for Windows. 
- **Max depth**
- **Filename allowlist**

Note that the **Connected Destinations** section defaults to **QuickConnect**, Cribl Edge's graphical UI. In the **Connected Destinations** section, you can select a Pipeline or Pack and a Destination. Otherwise, when you save, you'll be routed to the **Collect** page to set up your connections via QuickConnect.

![Connected Destinations](ed-file-connected-dest.61ff0a1e42.png)
{border="true"}

For details about making these connections, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

![Collect page](ed-file-monitor-quickconnect.5e536e7e78.png)
{border="true"}

#### Ingesting a File {#ingest}

To configure options for how and where to send file contents, use the **Ingest file** modal.

You have two options:

- Send directly to a configured Destination via **QuickConnect** (the default).

![Ingest file via QuickConnect](ed-file-ingestwin.94d4c76e94.png)
{border="true"}

- Send to Routes through (an optional) Pre-Processing Pipeline. 

![Ingest file via Routes](ed-file-ingest-routewin.431b2e8f39.png)
{border="true"}

You can configure Event Breakers and rulesets for both options. 

#### Exploring Files with Event Breakers {#exploring-files}

When you click a file in the **Files** tab, and Cribl Edge shows a representation of the lines that the file contains – how does that work? Cribl Edge is applying a default Event Breaker to format the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:
- To apply a different (existing) Event Breaker, click **Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.
- To create a new Event Breaker ruleset, click **Create New**. In the resulting **New Ruleset** modal, proceed as described [here](event-breakers#add). Later, you can reuse the new Event Breaker as part of a Source or a Collector.

   While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content and the content as modified by the Event Breaker you're creating. 

Now return to the **Search** tab – the contents of your chosen file will appear with the new Event Breaker applied.

### System State {#host-metadata}

The **System State** upper tab provides access to these left tabs:

- [Host Info](#host-info)
- [Disks](#disks)
- [DNS](#dns)
- [File Systems](#filesystems)
- [Firewall](#firewall)
- [Groups](#groups)
- [Hosts File](#hosts-file)
- [Interfaces](#interfaces)
- [Listening Ports](#listening)
- [Logged-In Users](#logged) 
- [Routes](#host-routes)
- [Services](#services)
- [Users](#users) 

> To display any of the tabs above, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that the Source's [Collector Settings](sources-system-state#collectorsettings) fields are enabled. 
>
{.box .info}


#### Host Info/Metadata {#host-info}

Cribl Edge can add a ` __metadata ` property to every event emitted from every enabled Source. The **Host Info** tab displays the metadata collected for each Edge Node. 

![Host info / Metadata collected](ed-sysstate-win-hostinfo.png)
{border="true"}

The metadata surfaced by an Edge Node can be used to:
- Enrich events (with an internal `__metadata` field).
- Display to users as a part of instance exploration.

To customize the type of metadata collected, select **Settings** > **General Settings** > **Limits** > **Metadata**. Use the **Event metadata sources** drop-down (and/or the **Add source** button) to add and select metadata sources.

> In Edge mode, all the **Event metadata sources** are enabled by default.
>
{.box .success}

![Set limits](ed-settings-limits.476db83cde.png)
{border="true"}

The metadata sources that you can select here include:
- `os`: Reports details for the host OS and host machine, like OS version, kernel version, CPU and memory resources, hostname, network addresses, etc.
- `cribl`: Reports the Cribl Edge version, mode, Fleet for managed instances, tags defined on the instance, and config version.
- `aws`: Reports details for an EC2 instance, including the instance type, hostname, network addresses, tags, and IAM roles. For security reasons, we report only IAM role names.
- `env`: Reports environment variables.

When these metadata sources are enabled (and can get data), Cribl Edge will add the corresponding property to events, with a nested property for each enabled source.

> Some metadata sources work only in configured environments. For example, the `aws` source is available only when running on an AWS EC2 instance.
> 
> If your security tools report denied outbound traffic to IP addresses like `169.254.169.168` or `169.254.169.254`, you can suppress these by removing `aws` from the metadata sources described above. If you have a proxy setup, Cribl recommends adding these IP addresses to your `no_proxy` environment variable.
>
{.box .info}

#### Disks {#disks}

The **Disks** tab displays the inventory of physical disks and their partitions on the host system.

![Disks tab](ed-sysstate-win-disks.png)
{border="true"}


#### DNS {#dns}

The **DNS** tab lists the DNS resolvers and search entries on the host system.

![DNS tab](ed-sysstate-win-dns.png)
{border="true"}


#### File Systems {#filesystems} 

The **File Systems** tab displays an inventory of the mounted file systems on the host system.

![File Systems tab](ed-sysstate-win-filesystems.png)
{border="true"}


#### Firewall {#firewall}

The **Firewall** tab displays a list of the host’s defined firewall rules.

![Firewall tab](ed-sysstate-win-firewall.png)
{border="true"} 


#### Groups {#groups}

The **Groups** tab displays a list of local groups including their names, descriptions, and members on the host system.

![Groups tab](ed-sysstate-win-groups.png)
{border="true"}


#### Hosts File {#hosts-file}

The **Hosts File** tab displays the current state on the host system.

![Hosts File tab](ed-sysstate-win-hostsfile.png)
{border="true"}


#### Interfaces {#interfaces} 

The **Interfaces** tab displays a list of each of the network interfaces on the host system.

![Interfaces tab](ed-sysstate-win-interfaces.png)
{border="true"}


#### Listening Ports {#listening}

The **Listening Ports** tab displays a list of listening ports and their associated process identifier (pid).

![Listening Ports tab](ed-sysstate-win-listening.png)
{border="true"}


#### Logged-In Users {#logged}

The **Logged-In Users** tab displays a list of currently logged-in users on the host.

![Logged-In Users tab](ed-sysstate-win-loggedinusers.png)
{border="true"}


#### Services {#services}

The **Services** tab displays a list of each configured service along with their running status.

![Services tab](ed-sysstate-win-services.png)
{border="true"}


#### Routes {#host-routes}

The **Routes** tab displays entries from network routes on the host system.

![Routes tab](ed-sysstate-win-routes.png)
{border="true"}

#### Users {#users}

The **Users** tab displays a list of local users on the host system.

![Users tab](ed-sysstate-win-users.png)
{border="true"}
