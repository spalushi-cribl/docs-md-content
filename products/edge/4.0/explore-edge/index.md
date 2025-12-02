# Exploring Cribl Edge on Linux


The Cribl Edge UI offers a centralized view to manage, configure, and version–control your Edge Nodes. It also endows you with [teleport–to–the–edge](#teleport) superpowers for locally previewing and validating your configurations. Here's a quick tour of the Cribl Edge UI in distributed mode.

## Accessing Cribl Edge  

When you first log into Cribl Stream/Edge (single-instance or distributed), you'll see tiles that prompt you to choose between ~~two roads diverging in a yellow wood~~ the Stream versus Edge UIs. The Edge tile displays basic configuration details, including the number of Fleets, Subfleets, Edge Nodes, and events and bytes over time. Click **Manage** to start.

![Manage Edge](managed_edge_landing.4f11c1cdf5.png)
{border="true"}

## Fleets Overview  

On the Cribl Edge **Home** tab, you can access your configured Fleets and a summary of your Cribl Edge environment, highlighting the aggregate data for all Fleets, Subfleets, Edge Nodes, and Mappings. The charts display information about traffic in and out of the system. 

![Cribl Edge Homepage](ed-edge_home.5e781321cd.png)
{border="true"}

Select **Manage** from the top nav to view the **Fleets Landing** page. Here you can access the tabs for more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

![Manage Fleets](ed-fleet_all.22be2dc69f.png)
{border="true"}

The **Manage Fleets** page gives you access to more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

You can click a **Fleet** link to isolate individual Fleets, or use the **Search** bar to locate your Fleet.   

## Fleet Landing Page 

![Fleet Landing page](ed_edge_fleet_landing.a9181fa26d.png)
{border="true"}

The Fleet's landing page highlights information about your configured Edge Nodes. The following information is displayed across the top.

**Number of Edge Nodes**: How many Edge Nodes are configured in this Fleet. 

**Events In**: Total number of Events in the last 5 minutes of data collected. You can change the display's granularity from the default ` last 5 min`, selecting a variety of time ranges from 1 min up to 1 day. (The latter covers the preceding 24 hours, and this maximum window is not configurable.)

**Bytes In**: The uncompressed amount of data in the last 5 minutes of data collected. You can change the display's granularity from the default ` last 5 min`, selecting from a variety of time ranges from 1 min up to 1 day. (The latter covers the preceding 24 hours, and this maximum window is not configurable.)

**Sources**: List of configured Sources.

**Destinations**: List of configured Destinations.

Select the **Fleet** dropdown (top right) to see a hierarchical list of all your Fleets and Subfleets.

![Fleets and SubFleets list](ed-fleet-dropdown.d86c028c5f.png)
{border="true"}

### Fleet Map View 

**Map View**: Here, a query builder allows you to display metrics from the Edge Nodes in the Fleet. You can select from different aggregations in the **Chart** field, different metrics in the **Measure** field, and the time window in the **During** field. A hexagon-based map view displays the resulting metric combination for each of your Edge Nodes. 

Hovering over on one of the hexagons displays the Edge Node's **GUID**.

![View Edge Node](ed-hex-nodes.e93b8753cf.png)
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

### Fleet List View 

The **List View** tab displays a list of all the systems in the Fleet. This also serves as the "transporter room," allowing you to [teleport](#teleport) into each of the Node's interfaces. 

![Edge Node list view](ed-edge-list-view.9555a2c437.png)
{border="true"}

Click anywhere on a row to display a quick snapshot of the Edge Node's details, with an option to **Restart** the host.


### Teleport into an Edge Node {#teleport}

Click the **Edge Node GUID** link to teleport from the Leader into the Edge Node. Here, you can explore the metrics and log data that the Node has autodiscovered, and can manually discover and explore other data of interest. You can use the discovered data to perform root-cause analysis, to troubleshoot, and to restart the host. 

The page displays metadata for the Node, and below it, graphs of system activity. A magenta border indicates you are remotely viewing a host, and identifies the host's name. 

Click **Restart Edge** to restart the Node. To return to the **Manage Edge Nodes** page, click the **X** close button on the upper right.

![Teleporting into an Edge Node](ed-teleport-node.4cf3e55509.png)
{border="true"}

> Changes that you make on an Edge Node will not propagate to the Leader. Also, the Leader will override any changes that you make directly on a Node. 
>
{.box .warning}

From the top nav, select **Explore** to view more details on a particular Edge Node. On the **Node to explore** drop-down, select one of your hosts to display the following tabs: 
- [Processes](#processes)
- [Containers](#containers)
- [Files](#files)
- [System State](#host-metadata)

![Explore an Edge Node](ed-edge-explore.c3fd2579a4.png)
{border="true"}

Let's explore each tab.  

### Processes {#processes}

The **Processes** tab lists all the processes running on the Edge Node. Click on any of the rows to display a modal with basic information on the process, including CPU, Memory, and IO graphs. 

In this modal, click **All details** to get a table view of processes' information out of `/proc`. This information would normally require SSH'ing to the machine; this view makes troubleshooting across multiple systems much easier.

![View Processes](ed-system-processes.61cb799cfa.png)

For active listening ports, the **Processes** tab displays a table of all active **Listening**, **Inbound**, and **Outbound** connections. 

![Active listening ports](ed-processes-listening-ports.ace7cfec2a.png)
{border="true"}

### Containers {#containers}

The **Containers** tab lists all the running containers and container metrics including information about images, volumes, status, ports, etc. 

> Currently, Cribl Edge supports only the Docker runtime, not `containerd` or other runtimes. 
>
{.box .info}

![Containers overview](ed-containers-overview.ec156963dc.png)
{border="true"}

Click any container to view more details:  

![Container details](ed-containers-details.f20e28cb8f.png)
{border="true"}

Click the **Logs** tab to view container logs. Optionally, use the search bar to filter displayed logs by arbitrary strings. 

![View Container logs](ed-containers-details2.704567cea9.png)
{border="true"}

### Files {#files}

The **Files** tab lists all the log files being actively written to by running applications that Cribl Edge has auto-discovered. You can also specify a list of directories and files to actively monitor.

This tab provides the following options.

#### Discover File Options 

![Explore files](ed-explore-files.dc0e144605.png)
{border="true"}

Use the buttons to select one of these options:
 - **Auto**: Tells Cribl Edge to automatically discover files that are open for writing on currently running processes.
 - **Manual**: Tells Cribl Edge to discover the files within the **Allowlist** and **Search path** (i.e., a directory) that you specify, down to the **Max depth**. 
 - **Browse**: Displays a tree view of all of your directories and files to explore. 

![Manual discovery mode](ed-specify-file-list.ab576c847d.png)
{border="true"}

#### Max Depth

The **Max depth** field is empty by default. This means that Cribl Edge will search subdirectories, and their subdirectories, downward without limit. 

If you specify `0`, Cribl Edge will discover only the top-level files within the **Search path**. If you specify `1`, Cribl Edge will discover files one level down. Follow this pattern to specify the depth you want.

#### Filename Allowlist

The **Filename allowlist** field supports wildcard syntax, and supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Click on any of the files to see a representation of the lines it contains. 

If the representation doesn't seem ideally suited to the file's content, you can use the **Event Breakers** tab to [change it](#exploring-files).

![Event Breakers tab](ed-file-details.22d395eae1.png)
{border="true"}

Click **Monitor this file** to display the [File Monitor's](#monitor) configuration modal. 

To restrict how much data is displayed, you can use the **Search** tab's search bar and/or time picker.

![Search tab](ed-search-file.20e739d118.png)
{border="true"}

Click the **Inspect** tab to view file metadata, including details like permissions, file size, user, modified date, etc. If the file appears suspicious, click the links at the bottom of the modal – **VirusTotal** or **OpSwat** – to check whether the file is flagged as compromised. 

![Specify files and directories](ed-file-inspect.eccf97c3ab.png)
{border="true"}

#### Monitor {#monitor}

Click a file's **Monitor** button to select the file for monitoring, and to generate events from its lines or records. In the resulting modal, configure your [**File Monitor**](sources-file-monitor) Source. 

![File Monitor configuration modal](ed-file-monitor-discover.2dcccc616b.png)
{border="true"}

The **Monitor** feature automatically prepopulates the modal with the settings configured on the **Files** tab:
- **Search path**
- **Discovery mode**
- **Max depth**
- **Filename allowlist**

In addition, the **Connected Destinations** section defaults to **QuickConnect**. 

![Specify files and directories](ed-file-monitor-quickconnect.5e536e7e78.png)
{border="true"}

When you **Save**, a dialog confirms that you will be routed to the **Collect** page to set up your connections.

For further details, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

#### Exploring Files with Event Breakers {#exploring-files}

When you click a file in the **Files** tab, and Cribl Edge shows a representation of the lines that the file contains, how does that work? What's happening is that Cribl Edge is applying a default Event Breaker to the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:

* To apply a different (existing) Event Breaker, click **+ Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.

* To create a new ruleset, click **+ Create New** to open the **New Ruleset** modal. Proceed as described [here](event-breakers#add). Later, you can persist the new Event Breaker as part of a Source or a Collector. While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content, and the content as modified by the Event Breaker you're creating. 

Now return to the **Search** tab – the contents of your chosen file will appear with the new Event Breaker applied.

### System State {#host-metadata}

The **System State** upper tab provides access to these left tabs:

- [Host Info](#host-info)
- [Hosts File](#hosts-file)
- [Routes](#host-routes)

#### Host Info {#host-info}

Cribl Edge can add a `__metadata` roperty to every event emitted from every enabled Source. The **System State** tab displays the metadata collected for each Edge Node under **Host Info**. 

![Host info/metadata collected](ed-host-info.b5b965a37c.png)
{border="true"}

The metadata surfaced by an Edge Node can be used to:
- Enrich events (with an internal `__metadata` field).
- Display to users as a part of instance exploration.

You can customize the type of metadata collected at **Fleet Settings** > **Limits** > **Metadata**. Use the **Event metadata sources** drop-down list (and/or the **+ Add source** button) to add and select metadata sources.

> In Edge mode, all the **Event metadata sources** are enabled by default.
>
{.box .success}

![Set limits](ed-settings-limits.bad032ab90.png)
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

#### Hosts File {#hosts-file}

The **Hosts File** tab displays the host system's current state, based on what's configured on the [System State Source](sources-system-state):

![Hosts File tab](ed-hosts-file.8fc5dbefe5.png)
{border="true"}

The display includes a time picker to select different snapshots of the hosts file. The filter provides a paginated view of files for the selected time bucket.

> For the **Hosts File** tab to appear, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that Source's [Hosts File](sources-system-state#hosts) field is enabled. 
>
{.box .info}

#### Routes {#host-routes}

The **Routes** tab displays entries from host's network routes, based on what's configured on the [System State Source](sources-system-state). 

![Hosts Routes tab](ed-routes-tab.9798c59ffe.png)
{border="true"}

The display includes a time picker to select different snapshots of the host's network routes. The filter provides a paginated view of network routes for the selected time bucket.

> For the **Hosts File** tab to appear, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that Source's [Routes](sources-system-state#routes) field is enabled. 
>
{.box .info}

#### `__metadata.kube` Property {#kube}

For the  `__metadata.kube` property (`kube` in the UI) to report details on a Kubernetes environment, Cribl Edge needs to figure out where it is running. Set the `KUBE_K8S_POD` environment variable to the name of the Pod in which Cribl Edge is running. At this point, the `__metadata.kube` property will have information to report on the `node` and `pod` properties. 
  
If the `/proc/self/cgroup`is working, then the `container` property information will be available, too. 
  
>   If you leave the `KUBE_K8S_POD` environment variable unset, and `/proc/self/cgroup` is not working, then Cribl Edge will not know what Pod it is running in. This state has multiple implications:
>   - The [Kubernetes Metrics Source](sources-kubernetes-metrics) will be unable to identify whether or not it is in a DaemonSet. The result will be redundant metrics from each node in the cluster.
>   - The Kubernetes Metadata collector will not add the `__metadata.kube property`. 
>  - The [Kubernetes Logs Source](sources-kubernetes-logs) will also duplicate data. Every node in the cluster will collect logs for every container in the cluster.
>
{.box .warning} 
