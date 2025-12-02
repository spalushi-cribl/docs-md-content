# Exploring Cribl Edge on Linux


The Cribl Edge UI offers a centralized view to manage, configure, and version–control your Edge Nodes. It also endows you with [teleport–to–the–edge](#teleport) superpowers for locally previewing and validating your configurations. Here's a quick tour of the Cribl Edge UI in distributed mode.

## Accessing Cribl Edge  

When you first log into Cribl Edge (single-instance or distributed), you'll see tiles that prompt you to choose between the Cribl Stream or Cribl Edge UI. The Edge tile displays basic configuration details, including the number of Fleets, Subfleets, Edge Nodes, and events and bytes over time. Click **Manage** to jump into Cribl Edge.

![Manage Edge](managed_edge_landing.4f11c1cdf5.png)
{border="true"}

## Fleets Overview  

On the Cribl Edge **Home** tab, you can access your configured Fleets and a summary of your Cribl Edge environment, highlighting the aggregate data for all Fleets, Subfleets, Edge Nodes, and Mappings. The charts display information about traffic in and out of the system. 

![Cribl Edge Homepage](ed-edge_home.5e781321cd.png)
{border="true"}

Select **Manage** from the top nav to view the **Fleets Landing** page. Here you can access the tabs for more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

![Manage Fleets](ed-fleet_all.eceaf7be87.png)
{border="true"}

The **Manage Fleets** page gives you access to more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

You can click a **Fleet** link to isolate individual Fleets, or use the **Search** bar to locate your Fleet.   

## Fleet Landing Page 

![Fleet Landing page](ed_edge_fleet_landing.6d95cbcc21.png)
{border="true"}

The Fleet's landing page highlights information about your configured Edge Nodes. The following information is displayed across the top.

**Number of Edge Nodes**: How many Edge Nodes are configured in this Fleet. 

**Events In**: Total number of Events in the last 5 minutes of data collected. You can change the display's granularity from the default ` last 5 min`, selecting a variety of time ranges from 1 min up to 1 day. (The latter covers the preceding 24 hours, and this maximum window is not configurable.)

**Bytes In**: The uncompressed amount of data in the last 5 minutes of data collected. You can change the display's granularity from the default ` last 5 min`, selecting from a variety of time ranges from 1 min up to 1 day. (The latter covers the preceding 24 hours, and this maximum window is not configurable.)

**Sources**: List of configured Sources.

**Destinations**: List of configured Destinations.

Select the **Fleet** dropdown (top right) to see a hierarchical list of all your Fleets and Subfleets.

![Fleets and SubFleets list](ed-fleet-dropdown.cc48a59293.png)
{border="true"}

### Fleet Map View 

**Map View**: Here, a query builder allows you to display metrics from the Edge Nodes in the Fleet. You can select from different aggregations in the **Chart** field, different metrics in the **Measure** field, and the time window in the **During** field. 

The metrics that appear in the **Measure** list depend on the option selected in **Fleet Settings** > **Limits** > **Metrics** under **Metrics to send from Edge Nodes**. The metrics `total.in_bytes`, `total.in_events`, `total.out_bytes`, and `total.out_events` always appear in the list; the display of any other metrics depends on what the Edge Nodes are sending to the Leader. To learn more about these metrics options, see [Controlling Metrics](internal-metrics#controlling-metrics).

A hexagon-based map view displays the resulting metric combination for each of your Edge Nodes. Hovering over on one of the hexagons displays the Edge Node's **GUID**.

![View Edge Node](ed-hex-nodes.png)
{border="true"}

Clicking any of the hexagons displays a modal providing details on the Edge Node, similar to [teleporting](#teleport) into it, with the option to **Restart** the host. The **System Activity** tab displays details about the host's CPU, memory, network, and disk operations.

![Edge Node System Activity](ed-edge-system-activity.b6ad751e51.png)
{border="true"}

The **Data Activity** tab offers a view into the data flowing through the Edge Node.

![Edge Node Data Activity](ed-edge-data-activity.1b3b944f50.png)
{border="true"}

The **Node Info** tab offers Host/OS level information with a snapshot of the latest Heartbeat captured.

![Edge Node info](ed-edge-node-info.9df224e5cb.png)
{border="true"}

### View Edge Nodes in a Fleet 
 
The **List View** tab displays a list of all the systems and their statuses in the Fleet.

A few highlights on some of the columns:

- **Health**: This column displays an icon indicating the overall health of the Edge Node:
  - Green checkmark: Everything is good! All Sources and Destinations are healthy.
  - Yellow warning icon: Attention needed. One or more Sources or Destinations are in a warning state.
  - Red exclamation point: Critical issue! One or more Sources or Destinations have encountered an error.
  - Indeterminate: No data available yet for Sources or Destinations.

- **Sources**: Shows the individual health statuses of the Edge Node's Sources.
- **Destinations**: Shows the individual health statuses of the Edge Node's Destinations.
- **Edge Version**: Displays a status icon next to the version number. This icon can provide information about
  the upgrade status for a Node. For more information on upgrade status, read this section on [Upgrading Edge Nodes](upgrading-ui#view-the-status-of-fleet-upgrades).

While not displayed by default, the list also includes these columns accessible through the column selector:

- **CPU**: Shows the CPU utilization for the Edge Node.
- **RAM**: Displays the memory usage of the Edge Node.

This also serves as the "transporter room," allowing you to [teleport](#teleport) into each of the Node's interfaces. 

![The Edge Node list view shows a list of all the Nodes in a Fleet](ed-edge-list-view.png)
{border="true"}
 
### Teleport into an Edge Node {#teleport}

Click the **Edge Node GUID** link to teleport from the Leader into the Edge Node. Here, you can explore the metrics and log data that the Node has autodiscovered, and can manually discover and explore other data of interest. You can use the discovered data to perform root-cause analysis, to troubleshoot, and to restart the host. 

The page displays metadata for the Node, and below it, graphs of system activity. A magenta border indicates you are remotely viewing a host, and identifies the host's name. 

Click **Restart Edge** to restart the Node. To return to the **Manage Edge Nodes** page, click the **X** close button on the upper right.

![Teleporting into an Edge Node](ed-teleport-node.4cf3e55509.png)
{border="true"}

> Changes that you make on an Edge Node will not propagate to the Leader. Also, the Leader will override any changes that you make directly on a Node. 
>
{.box .warning}

#### Adding Tags to Edge Nodes in Teleport View

You can add tags to an Edge Node you've teleported into. Tags help the Leader map
an Edge Node to a Fleet. Note that changing the tags on a Node can cause
the Leader to map the Node into a new Fleet.

1. Teleport into an Edge Node.
1. Navigate to **Node Settings** then **Distributed Settings**.
1. Enter tags for this Node and select **Save**.

>In Cribl.Cloud you must accept the confirmation modal, as changing the tags on
>an Edge Node can affect the Fleet that the Leader maps it into. {.box .cloud}


### Explore Tab

From the top nav, select **Explore** to view more details on a particular
Edge Node. On the **Node to explore** drop-down, select one of your hosts to
display the following tabs:

- [Processes](#processes)
- [Containers](#containers)
- [Files](#files)
- [System State](#host-metadata)

![Explore an Edge Node](edge-node-to-explore.png)
{border="true"}

>The **Node to explore** drop-down lists up to 50 Edge Nodes, ordered by hostname.
>To view the details for a specific Edge Node, enter the hostname or GUID into
>the **Node to explore** field.
{.box .success}

Let's explore each tab.  

#### Processes {#processes}

The **Processes** tab lists all the processes running on the Edge Node. 

![View Processes](ed-system-processes.417154eac3.png)
{border="true"}

Click on any of the rows to open the **Process: <process_name>** drawer. In the drawer's default **Overview** tab, you'll find basic information on the process, including CPU, Memory, and IO graphs, along with tables for active **Listening**, **Inbound**, and **Outbound** connections.

![The Overview tab](ed-processes-listening-ports.65200195fe.png)
{border="true"}

In this tab, click **All details** to see the selected process' information out of `/proc`, expressed as key-value pairs. This information would normally require SSH'ing to the machine; this view makes troubleshooting across multiple systems much easier.

![Showing all details of a process](ed-process-all-details.f368eae873.png)
{border="true"}

Open the **AppScope** tab if you want to "scope" the process (i.e., use [AppScope](https://appscope.dev/docs/overview/) to monitor it). Once in the tab, you'll choose an an AppScope configuration that says what events and metrics to obtain, and an AppScope Source to receive them. See [Scoping by PID](https://appscope.dev/docs/edgescope-host/#scoping-by-pid) in the AppScope docs. Note that your Cribl Edge instance must be running as root to do process monitoring with AppScope. 

![The AppScope tab for a process](ed-appscope-tab.4fb6580a9a.png)
{border="true"}

When a process is being scoped, back in the **Processes** tab you'll see that indicated in the process' entry in the **AppScope** column.

#### Containers {#containers} 

The **Containers** tab lists all the running containers and container metrics including information about images, volumes, status, ports, etc. 

> Cribl Edge supports both Docker and `containerd` runtimes. 
>
{.box .info}

`containerd` containers have less info than Docker containers, so `Ports`, `IPs`, and `Logs` won't populate. 

![Containers overview](ed-containers-overview.4ff1956a03.png)
{border="true"}

Click any container to view more details:  

![Container details](ed-containers-details.f20e28cb8f.png)
{border="true"}

Click the **Logs** tab to view container logs. Optionally, use the search bar to filter displayed logs by arbitrary strings. 

![View Docker Container logs](ed-containers-details2.704567cea9.png)
{border="true"}

The screenshot below shows `containerd` details, which don't include charts or logs. 

![View containerd logs](ed-containers-details3.4a4582b21b.png)
{border="true"}

If you run Edge as an unprivileged user, see [Making Docker Containers Visible to Edge](deploy-runtime-user#make-containers-visible).

#### Files {#files}

The **Files** tab lists all the log files being actively written to by running applications that Cribl Edge has auto-discovered. You can also specify a list of directories and files to actively monitor.

![Explore files](ed-explore-files.d880d70a5d.png)
{border="true"}

The **Actions** column allows you to:

- **View**: Displays a representation of the lines this column contains. You can also click any file row. To restrict how much data is displayed, use the search field or time picker on the **Search** tab. 

- **Inspect**: Opens the **Inspect File** tab to show file metadata including details like permissions, file size, user, and modified date.

 When inspected, compressed files (such as `foo.bar.gz`) include a **File preview** that shows the beginning of the file contents.

 Archived files (such as `.zip`, `.tgz`, and `.tar.gz`) include a **File listing** that shows the files within it.

 If a file appears suspicious, click **VirusTotal** or **OpSwat** at the bottom of the modal to see if the file is flagged as compromised. 

![Inspect File tab](ed-file-inspect.ba13d60ab7.png)
{border="true"}

- **Monitor**: Displays the [File Monitor's](#monitor) configuration modal. 

- **Ingest**: Opens the [Ingest file](#ingest) modal to send the file content to Routes/Pipelines for further processing or downstream to any destination you have configured. This is useful for testing and troubleshooting your configurations.

The **Files** tab provides the following options.

##### File Discovery Modes 

Click a button at the top to select a discovery mode:
 - **Auto**: Tells Cribl Edge to automatically discover files that are open for writing on currently running processes.
 - **Manual**: Tells Cribl Edge to discover the files within the **Path** (directory) and **Allowlist** that you specify, down to the **Max depth**. 
 - **Browse**: Displays a tree view of all of your directories and files. 

![Manual discovery mode](ed-specify-file-list.5b2dcc1cce.png)
{scale="75%" border="true"}

##### Path 
  
The **Path** field tells Cribl Edge to discover the files within the path (a directory) that you specify, down to the **Max depth**. 

##### Allowlist

The **Allowlist** field, available with Auto and Manual discovery, supports wildcard syntax, and supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Click any file to see a representation of the lines it contains. To restrict how much data is displayed, you can use the search field or time picker on the **Search** tab. 

![Search tab](ed-search-file.c44c19bf50.png)
{border="true"}

If the representation of events shown on the **Search** tab isn't ideally suited to the file's content, you can use the **Event Breakers** tab to [change it](#exploring-files).

![Event Breakers tab](ed-file-details.d5e6ad7fae.png)
{border="true"}

##### Monitor Files

The **Monitor Files** button, available with Auto and Manual discovery, opens a new [File Monitor](sources-file-monitor) Source prefilled with the discovery mode and anything else you specified on the **Files** tab, such as allowlist entries, path, or max depth.

##### Max Depth

The **Max depth** field, available with Manual discovery, is empty by default. Cribl Edge will search subdirectories, and their subdirectories, downward without limit. 

If you enter `0`, Cribl Edge will discover only the top-level files within the specified path. If you specify `1`, Cribl Edge will discover files one level down from the top. Follow this pattern to specify the depth you want.

##### Monitoring a File {#monitor}

Click a file's **Monitor** button or **Actions** option to configure your [**File Monitor**](sources-file-monitor) Source to generate events from the file's lines or records. 

![New File Monitor modal](ed-file-monitor-discover.2dcccc616b.png)
{border="true"}

The **Monitor** feature automatically prepopulates the modal with the following settings configured on the **Files** tab:
- **Discovery mode**
- **Search path**
- **Max depth**
- **Filename allowlist**

In addition, the **Connected Destinations** section defaults to **QuickConnect**. In the **Connected Destinations** section, you can select a Pipeline or Pack and a Destination. Otherwise, when you save, you'll be routed to the **Collect** page to set up your connections via QuickConnect.
 

![Collect page](ed-file-monitor-quickconnect.5e536e7e78.png)
{border="true"}

For further details, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

##### Ingesting a File {#ingest}

To configure options for how and where to send file contents, use the **Ingest file** modal.

You have two options:

- Send directly to a configured Destination via **QuickConnect** (the default).

![Ingest file via QuickConnect](ed-file-ingest.58e24d6fd7.png)
{border="true"}

- Send to Routes through (an optional) Pre-Processing Pipeline. 

![Ingest file via Routes](ed-file-ingest-route.9d669cbb1a.png)
{border="true"}

You can configure Event Breakers and rulesets for both options. 

##### Exploring Files with Event Breakers {#exploring-files}

When you click a file in the **Files** tab, and Cribl Edge shows a representation of the lines that the file contains, how does that work? What's happening is that Cribl Edge is applying a default Event Breaker to the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:

* To apply a different (existing) Event Breaker, click **Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.

* To create a new ruleset, click **Create New** to open the **New Ruleset** modal. Proceed as described [here](event-breakers#add). Later, you can persist the new Event Breaker as part of a Source or a Collector. While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content, and the content as modified by the Event Breaker you're creating. 

Now return to the **Search** tab – the contents of your chosen file will appear with the new Event Breaker applied.

#### System State {#host-metadata}

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

##### Host Info {#host-info}

Cribl Edge can add a `__metadata` property to every event emitted from every enabled Source. The **System State** tab displays the metadata collected for each Edge Node under **Host Info**. 

![Host info/metadata collected](ed-sysstate-linux-hostinfo.png)
{border="true"}

The metadata surfaced by an Edge Node can be used to:
- Enrich events (with an internal `__metadata` field).
- Display to users as a part of instance exploration.

You can customize the type of metadata collected at **Fleet Settings** > **Limits** > **Metadata**. Use the **Event metadata sources** drop-down (and/or the **Add source** button) to add and select metadata sources.

> In Edge mode, all the **Event metadata sources** are enabled by default.
>
{.box .success}

![Set limits](ed-settings-limits.476db83cde.png)
{border="true"}

The metadata sources that you can select here include:
- `os`: Reports details for the host OS and host machine, like OS version, kernel version, CPU and memory resources, hostname, network addresses, etc.
- `cribl`: Reports the Cribl Edge version, mode, Fleet for managed instances, and config version.
- `aws`: Reports details for an EC2 instance, including the instance type, hostname, network addresses, tags, and IAM roles. For security reasons, we report only IAM role names.
- `env`: Reports environment variables.
- `kube`: Reports details on a Kubernetes environment, including the node, Pod, and container. For details, see [__metadata.kube Property](#kube).

When these metadata sources are enabled (and can get data), Cribl Edge will add the corresponding property to events, with a nested property for each enabled source.

> Some metadata sources work only in configured environments. For example, the `aws` source is available only when running on an AWS EC2 instance.
> 
> If your security tools report denied outbound traffic to IP addresses like `169.254.169.168` or `169.254.169.254`, you can suppress these by removing `aws` from the metadata sources described above. If you have a proxy setup, Cribl recommends adding these IP addresses to your `no_proxy` environment variable.
>
{.box .info}

##### Disks {#disks} 

The **Disks** tab displays the inventory of physical disks and their partitions on the host system.

![Disks tab](ed-sysstate-linux-disks.png)
{border="true"}


##### DNS {#dns}

The **DNS** tab lists the host system's DNS resolvers and search entries.

![DNS tab](ed-sysstate-linux-dns.png)
{border="true"}


##### File Systems {#filesystems}

The **File Systems** tab displays an inventory of the mounted file systems on the host system.

![File Systems tab](ed-sysstate-linux-filesystems.png)
{border="true"}


##### Firewall {#firewall}

The **Firewall** tab displays a list of the host’s defined firewall rules.

![Firewall tab](ed-sysstate-linux-firewall.png)
{border="true"}


##### Groups {#groups}

The **Groups** tab displays a list of the local groups including their names, descriptions, and members on the host system.

![Groups tab](ed-sysstate-linux-groups.png)
{border="true"}


##### Hosts File {#hosts-file}

The **Hosts File** tab displays the host system's current state.

![Hosts File tab](ed-sysstate-linux-hostsfile.png)
{border="true"}


##### Interfaces {#interfaces}

The **Interfaces** tab displays a list of each of the network interfaces on the host system.

![Interfaces tab](ed-sysstate-linux-interfaces.png)
{border="true"}

 
##### Listening Ports {#listening}

The **Listening Ports** tab displays a list of listening ports and their associated process identifier (pid).

![Listening Ports tab](ed-sysstate-linux-listeningports.png)
{border="true"}


##### Logged-In Users {#logged}

The **Logged-In Users** tab displays a list of currently logged-in users on the host.

![Logged-In Users tab](ed-sysstate-linux-loggedinusers.png)
{border="true"}


##### Routes {#host-routes}

The **Routes** tab displays entries from the network routes on the host system.

![Routes tab](ed-sysstate-linux-routes.png)
{border="true"}


##### Services {#services}

The **Services** tab displays a list of each configured service (such as `systemd` and `initd`) along with their running status.

![Services tab](ed-sysstate-linux-services.png)
{border="true"}


##### Users {#users}

The **Users** tab displays a list of local users on the host system.

![Users tab](ed-sysstate-linux-users.png)
{border="true"}


##### `__metadata.kube` Property {#kube}

For the  `__metadata.kube` property (`kube` in the UI) to report details on a Kubernetes environment, Cribl Edge needs to figure out where it is running. Set the `KUBE_K8S_POD` environment variable to the name of the Pod in which Cribl Edge is running. At this point, the `__metadata.kube` property will have information to report on the `node` and `pod` properties. 
  
If the `/proc/self/cgroup`is working, then the `container` property information will be available, too. 
  
>   If you leave the `KUBE_K8S_POD` environment variable unset, and `/proc/self/cgroup` is not working, then Cribl Edge will not know what Pod it is running in. This state has multiple implications:
>   - The [Kubernetes Metrics Source](sources-kubernetes-metrics) will be unable to identify whether or not it is in a DaemonSet. The result will be redundant metrics from each node in the cluster.
>   - The Kubernetes Metadata collector will not add the `__metadata.kube property`. 
>  - The [Kubernetes Logs Source](sources-kubernetes-logs) will also duplicate data. Every node in the cluster will collect logs for every container in the cluster.
>
{.box .warning} 
