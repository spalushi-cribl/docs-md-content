# Explore Cribl Edge on Linux


The Cribl Edge UI offers a centralized view to manage, configure, and version–control your Edge Nodes. It also endows you with [teleport–to–the–edge](#teleport) superpowers for locally previewing and validating your configurations. Here's a quick tour of the Cribl Edge UI in distributed mode.

## Access Cribl Edge  

When you first log into Cribl Edge (single-instance or distributed), you'll see
tiles that prompt you to choose between the Cribl Stream or Cribl Edge UI. The
Edge tile displays basic configuration details, including the number of Fleets,
Subfleets, Edge Nodes, and events and bytes over time. 

Select **Manage** to jump into Cribl Edge.

![Manage Edge](managed_edge_landing.4f11c1cdf5.png)
{border="true"}

## Edge Home

Once you open Cribl Edge, you'll be on the **Edge Home** landing page. This page
gives you an overview of configured Fleets, Monitoring information for Fleets,
Subfleets, Edge Nodes, and Events and Bytes in and out. At the bottom, you'll
see Recent Actions that users have taken in the environment as well as any
configured Fleet Mappings.

![The Edge Home page, showing an overview of the configured resources](edge-home-ui.png)
{border="true" scale="75%"}

From the sidebar, you can quickly access more Cribl Edge goodies:
 
- [**Fleets and Subfleets**](fleets)
- [**Edge Nodes**](managing-edge-nodes)
- [**Mappings**](mapping-edge-nodes)
- [**Logs**](internal-logs)
- [**Notifications**](notifications)

## Fleets   

Select **Fleets** from the sidebar to access your configured Fleets.

![Fleets page](fleets-sidebar.png)
{border="true" scale="75%"}

From here, you can select a Fleet **Name** to isolate individual Fleets, or use
the **Search** bar to locate your Fleet. 

- See [Exploring Fleets](fleets-access) for more information about the Fleets interface.
- For more information about creating Fleets, see [Creating and Managing Fleets and Subfleets](fleets).

## Edge Nodes

Select **Edge Nodes** to view all of your Edge Nodes in one list.

![View the Edge Nodes page](edge-nodes.png)
{border="true" scale="75%"}

The list provides status information for each Edge Node in the selected Fleet, gives
you the opportunity to [filter Edge Nodes](edge-nodes-filter) based on selected criteria,
as well as a UI for adding and updating Nodes. For more information, see
[Manage Edge Nodes](managing-edge-nodes).

## Mappings

You can use Mappings Rulesets to map Edge Nodes to Fleets by defining rules.
For more information, see [Mapping Edge Nodes to Fleets](mapping-edge-nodes).

## Notifications

Notifications alert Cribl Edge admins about issues that require their immediate
attention. For information on types of Notifications, Notification targets, and managing
Notifications, see our [Managing Notifications](notifications#managing) docs. 

## Logs

Cribl Edge generates internal application logs that monitor its own operations
and health. They provide valuable insights into the system's behavior,
performance, and potential issues. For more information, see [Internal Logs](internal-logs).  

## Explore Tab

Select **Explore** to view more details on a particular
Edge Node. On the **Node to explore** drop-down, select one of your hosts to
display the following tabs:

- [Processes](#processes)
- [Containers](#containers)
- [Files](#files)
- [System State](#host-metadata)

![Explore an Edge Node](edge-explore-tab.png)
{border="true" scale="75%"}

>The **Node to explore** drop-down lists up to 50 Edge Nodes, ordered by hostname.
>To view the details for a specific Edge Node, enter the hostname or GUID into
>the **Node to explore** field.
{.box .success}

Let's explore each tab.  

#### Processes {#processes}

The **Processes** tab lists all the processes running on the Edge Node. 

![View Processes](ed-system-processes.417154eac3.png)
{border="true"}

Select on any of the rows to open the **Process: <process_name>** drawer. In the drawer's default **Overview** tab, you'll find basic information on the process, including CPU, Memory, and IO graphs, along with tables for active **Listening**, **Inbound**, and **Outbound** connections.

![The Overview tab](ed-processes-listening-ports.65200195fe.png)
{border="true"}

In this tab, select **All details** to see the selected process' information out of `/proc`, expressed as key-value pairs. This information would normally require SSH'ing to the machine; this view makes troubleshooting across multiple systems much easier.

![Showing all details of a process](ed-process-all-details.f368eae873.png)
{border="true"}

Open the **AppScope** tab if you want to "scope" the process (i.e., use [AppScope](https://appscope.dev/docs/overview/) to monitor it). Once in the tab, you'll choose an an AppScope configuration that says what events and metrics to obtain, and an AppScope Source to receive them. See [Scoping by PID](https://appscope.dev/docs/edgescope-host/#scoping-by-pid) in the AppScope docs. Note that your Cribl Edge instance must be running as root to do process monitoring with AppScope. 

![The AppScope tab for a process](ed-appscope-tab.4fb6580a9a.png)
{border="true"}

The **AppScope** column indicates when a process is being scoped, which you can
view from the **Processes** tab.

#### Containers {#containers} 

The **Containers** tab lists all the running containers and container metrics including information about images, volumes, status, ports, and so on. 

> Cribl Edge supports both Docker and `containerd` runtimes. 
>
{.box .info}

`containerd` containers have less info than Docker containers, so `Ports`, `IPs`, and `Logs` won't populate. 

![Containers overview](ed-containers-overview.4ff1956a03.png)
{border="true"}

Select any container to view more details:  

![Container details](edge-containers-tab.png)
{border="true"}

Select the **Logs** tab to view container logs. Optionally, use the search bar to filter displayed logs by arbitrary strings. 

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

- **View**: Displays a representation of the lines this column contains. You can also select any file row. To restrict how much data is displayed, use the search field or time picker on the **Search** tab. 

- **Inspect**: Opens the **Inspect File** tab to show file metadata including details like permissions, file size, user, and modified date.

 When inspected, compressed files (such as `foo.bar.gz`) include a **File preview** that shows the beginning of the file contents.

 Archived files (such as `.zip`, `.tgz`, and `.tar.gz`) include a **File listing** that shows the files within it.

 If a file appears suspicious, select **VirusTotal** or **OpSwat** at the bottom of the modal to see if the file is flagged as compromised. 

![Inspect File tab](ed-file-inspect.ba13d60ab7.png)
{border="true"}

- **Monitor**: Displays the [File Monitor's](#monitor) configuration modal. 

- **Ingest**: Opens the [Ingest file](#ingest) modal to send the file content to Routes/Pipelines for further processing or downstream to any destination you have configured. This is useful for testing and troubleshooting your configurations.

The **Files** tab provides the following options.

##### File Discovery Modes 

Select a button at the top to select a discovery mode:

 - **Auto**: Tells Cribl Edge to automatically discover files that are open for writing on currently running processes.
 - **Manual**: Tells Cribl Edge to discover the files within the **Path** (directory) and **Allowlist** that you specify, down to the **Max depth**. 
 - **Browse**: Displays a tree view of all of your directories and files. 

![Manual discovery mode](ed-specify-file-list.5b2dcc1cce.png)
{scale="75%" border="true"}

##### Path 
  
The **Path** field tells Cribl Edge to discover the files within the path (a directory) that you specify, down to the **Max depth**. 

##### Allowlist

The **Allowlist** field, available with Auto and Manual discovery, supports wildcard syntax, and supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Select any file to see a representation of the lines it contains. To restrict how much data is displayed, you can use the search field or time picker on the **Search** tab. 

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

##### Monitor a File {#monitor}

Select a file's **Monitor** or **Actions** option to configure your [**File Monitor**](sources-file-monitor) Source to generate events from the file's lines or records. 

![New File Monitor modal](ed-file-monitor-discover.2dcccc616b.png)
{border="true"}

The **Monitor** feature automatically populates the modal with the following settings configured on the **Files** tab:
- **Discovery mode**
- **Search path**
- **Max depth**
- **Filename allowlist**

In addition, the **Connected Destinations** section defaults to **QuickConnect**. In the **Connected Destinations** section, you can select a Pipeline or Pack and a Destination. Otherwise, when you save, you'll be routed to the **Collect** page to set up your connections via QuickConnect.

![Collect page](ed-file-monitor-quickconnect.5e536e7e78.png)
{border="true"}

For further details, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

##### Ingest a File {#ingest}

To configure options for how and where to send file contents, use the **Ingest file** modal.

You have two options:

- Send directly to a configured Destination via **QuickConnect** (the default).

![Ingest file via QuickConnect](ed-file-ingest.58e24d6fd7.png)
{border="true"}

- Send to Routes through (an optional) Pre-Processing Pipeline. 

![Ingest file via Routes](ed-file-ingest-route.9d669cbb1a.png)
{border="true"}

You can configure Event Breakers and rulesets for both options. 

##### Explore Files with Event Breakers {#exploring-files}

When you select a file in the **Files** tab, and Cribl Edge shows a representation of the lines that the file contains, how does that work? What's happening is that Cribl Edge is applying a default Event Breaker to the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:

* To apply a different (existing) Event Breaker, select **Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.

* To create a new ruleset, select **Create New** to open the **New Ruleset** modal. Proceed as described [here](event-breakers#add). Later, you can persist the new Event Breaker as part of a Source or a Collector. While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content, and the content as modified by the Event Breaker you're creating. 

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
