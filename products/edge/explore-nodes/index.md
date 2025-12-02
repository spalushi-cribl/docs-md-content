# Explore Cribl Edge Nodes


Examine Edge Nodes details in the Explore tab.

---

You can view details of each individual Edge Node by using the **Explore** tab.

To access it, in the sidebar, select **Fleets**, then choose a Fleet and select **Explore**.

In the **Node** drop-down, select one of your hosts to display the following tabs:

- [Processes](#processes)
- [Containers](#containers)
- [Files](#files)
- [System State](#host-metadata)
- [Kubernetes](#kubernetes)

> The available tabs will depend on your deployment environment.
{.box .info}

![Explore view of a single Edge Node, with Processes tab open, listing all the processes running on the Node](edge-explore-tab-4.14.png)
{border="true" scale="75%" caption="Explore an Edge Node"}

> The **Node** drop-down lists up to 100 Edge Nodes, ordered by hostname.
> To view the details for a specific Edge Node, enter the hostname or GUID into the **Node** field.
{.box .success}

Let's explore each tab in the **Explore** view.  

### Processes {#processes}

The **Processes** tab lists all the processes running on the Edge Node. 

Select any of the rows to display a drawer with basic information on the process.
In the drawer's default **Overview** tab, you'll find information such as CPU, Memory, and IO graphs,
along with tables for active **Listening**, **Inbound**, and **Outbound** connections, if applicable.

Select **All details** to see the selected process' information out of `/proc`, expressed as key-value pairs.
To access this information, you would normally need to SSH to the machine.
This view makes troubleshooting across multiple systems much easier.

#### Process Metrics on Windows

In Windows-based Fleets, you can control the collection method for Process Metrics using a configuration option
in the **Fleet Settings** > **Limits** > **Other** section.
This setting allows you to choose between using PowerShell cmdlets or the more efficient native module.

> When using PowerShell cmdlets on Windows, the environment variables displayed in this tab might be inaccurate.
> For details, see [Other](fleet-settings#other) Limits.
{.box .warning}

#### Monitor Processes with Appscope

Open the **AppScope** tab if you want to "scope" the process
(that is, use [AppScope](https://appscope.dev/docs/overview/) to monitor it).
Once in the tab, you'll choose an an AppScope configuration that says what events and metrics to obtain,
and an AppScope Source to receive them.
See [Scoping by PID](https://appscope.dev/docs/edgescope-host/#scoping-by-pid) in the AppScope docs.
Note that your Cribl Edge instance must be running as root to do process monitoring with AppScope. 

The **AppScope** column indicates when a process is being scoped,
which you can view from the **Processes** tab.

### Containers {#containers}

The **Containers** tab, available for Edge Nodes running in containers,
lists all the running containers and container metrics
including information about images, volumes, status, ports, and so on.

> Cribl Edge supports both Docker and `containerd` runtimes. 
>
> `containerd` containers have less info than Docker containers, so `Ports`, `IPs`, and `Logs` won't populate. 
{.box .info}

![Containers overview](edge-containers-tab-4.14.png)
{border="true" scale="75%" caption="Containers overview"}

Select any container to open a drawer with more details, including container logs.

If you run Edge as an unprivileged user, see [Making Docker Containers Visible to Edge](deploy-runtime-user#make-containers-visible).

### Files {#files}

The **Files** tab lists all the log files being actively written to
by running applications that Cribl Edge has auto-discovered.
You can also specify a list of directories and files to actively monitor.

The **Actions** column allows you to:

- **View**: Displays a representation of the lines this column contains.
  You can also select any file row.
  To restrict how much data is displayed, use the search field or time picker on the **Search** tab. 

- **Inspect**: Opens the **Inspect File** tab to show file metadata,
  including details like permissions, file size, user, and modified date.

  When inspected, compressed files (such as `foo.bar.gz`) include a **File preview**
  that shows the beginning of the file contents.

  Archived files (such as `.zip`, `.tgz`, and `.tar.gz`) include a **File listing**
  that shows the files within it.

  If a file appears suspicious, select **VirusTotal** or **OpSwat** at the bottom of the modal
  to see whether the file is flagged as compromised. 

- **Monitor**: Displays the [File Monitor's](#monitor) configuration modal. 

- **Ingest**: Opens the [Ingest file](#ingest) modal to send the file content to Routes/Pipelines for further processing or downstream to any destination you have configured. This is useful for testing and troubleshooting your configurations.

The **Files** tab provides the following options.

#### File Discovery Modes

Select a button at the top to select a discovery mode:

 - **Auto**: Tells Cribl Edge to automatically discover files that are open for writing on currently running processes.
   The **Auto** discovery mode is unavailable on Windows and macOS.
 - **Manual**: Tells Cribl Edge to discover the files within the **Path** (directory) and **Allowlist** that you specify, down to the **Max depth**. 
 - **Browse**: Displays a tree view of all of your directories and files. 

#### Path 
  
The **Path** field tells Cribl Edge to discover the files within the path
(a directory) that you specify, down to the **Max depth**.

#### Allowlist

The **Allowlist** field, available with Auto and Manual discovery, supports wildcard syntax, and
supports the exclamation mark (`!`) for negation. For example, you can use `!*cribl*access.log` to prevent Cribl Edge from discovering its own access log. The default filters are `*/log/*` and `*log`. 

Select any file to see a representation of the lines it contains.
To restrict how much data is displayed, you can use the search field or time picker on the **Search** tab. 

If the representation of events shown on the **Search** tab isn't ideally suited to the file's contents,
you can use the **Event Breakers** tab to [refine it](#exploring-files).

#### Monitor Files

The **Monitor Files** button, available with Auto and Manual discovery, opens a new [File Monitor](sources-file-monitor) Source.
The Source is prefilled with the discovery mode and anything else you specified on the **Files** tab, such as allowlist entries, path, or max depth.

#### Max Depth

The **Max depth** field, available with Manual discovery, is empty by default.
Cribl Edge will search subdirectories, and their subdirectories, downward without limit.

If you enter `0`, Cribl Edge will discover only the top-level files within the specified path. If you specify `1`, Cribl Edge will discover files one level down from the top. Follow this pattern to specify the depth you want.

#### Monitor a File {#monitor}

Select a file's **Monitor** or **Actions** option
to configure your [File Monitor](sources-file-monitor) Source to generate events from the file's lines or records. 

The **Monitor** feature automatically populates the modal with the following settings configured on the **Files** tab:
- **Discovery mode**. Defaults to **Manual** for Windows and macOS.
- **Search path**
- **Max depth**
- **Filename allowlist**

In addition, the **Connected Destinations** section defaults to **QuickConnect**.
In the **Connected Destinations** section, you can select a Pipeline or Pack and a Destination. Otherwise, when you save, you'll be routed to the **Collect** page to set up your connections via QuickConnect.
For details about making these connections, see [File Monitor](sources-file-monitor) and [QuickConnect](quickconnect).  

#### Ingest a File {#ingest}

To configure options for how and where to send file contents, use the **Ingest file** modal.

You have two options:

- Send directly to a configured Destination via **QuickConnect** (the default).
- Send to Routes through (an optional) Pre-Processing Pipeline. 

You can configure Event Breakers and rulesets for both options. 

#### Explore Files with Event Breakers {#exploring-files}

When you select a file in the **Files** tab,
and Cribl Edge shows a representation of the lines that the file contains, how does that work?
What's happening is that Cribl Edge is applying a default Event Breaker to the file.

You are not limited to the default Event Breaker, though. Select the **Event Breakers** tab, then:

- To apply a different (existing) Event Breaker, select **Add ruleset**, then select the desired ruleset from the **Event Breaker rulesets** drop-down.
- To create a new Event Breaker Ruleset, click **Create New**. In the resulting **New Ruleset** modal, proceed as described [here](event-breakers#add). Later, you can reuse the new Event Breaker as part of a Source or a Collector.

  While you create the new ruleset, Cribl Edge pulls the contents of the open file into the Sample File area. Toggle between the **In** and **Out** tabs to compare, respectively, the original content and the content as modified by the Event Breaker you're creating. 

Now return to the **Search** tab – the contents of your chosen file will appear with the new Event Breaker applied.

### System State {#host-metadata}

The **System State** upper tab provides access to a collection of information gathered by the System State Source.

> To display any of the information, you need to configure and enable the [System State Source](sources-system-state). Also, make sure that the Source's [Collector Settings](sources-system-state#collectorsettings) fields are enabled. 
>
{.box .info}

![An Edge Node's Explore view, with System State open to the Host Info tab](edge-explore-system-state-4.14.png)
{border="true" scale="75%" caption="System State for the selected Edge Node"}

#### Host Info {#host-info}

Cribl Edge can add a `__metadata` property to every event emitted from every enabled Source.
The **System State** tab displays the metadata collected for each Edge Node under **Host Info**. 

You can use the metadata surfaced by an Edge Node to:
- Enrich events (with an internal `__metadata` field).
- Display to users as a part of instance exploration.

To customize the type of metadata collected, select **Fleet Settings > Limits > Metadata**.
Use the **Event metadata sources** drop-down (and/or the **Add source** button) to add and select metadata sources.

> In Edge mode, all the **Event metadata sources** are enabled by default.
>
{.box .success}

The metadata sources that you can select here include:
- `os`: Reports details for the host OS and host machine, like OS version, kernel version, CPU, and memory resources, hostname, network addresses, and so on.
- `cribl`: Reports the Cribl Edge version, mode, Fleet for managed instances, tags defined on the instance, and config version.
- `aws`: Reports details for an EC2 instance, including the instance type, hostname, network addresses, tags, and IAM roles. For security reasons, we report only IAM role names.
- `env`: Reports environment variables.
- `kube`: Reports details on a Kubernetes environment, including the node, Pod, and container. For details, see [__metadata.kube Property](#kube).

When these metadata sources are enabled (and can get data), Cribl Edge will add the corresponding property to events, with a nested property for each enabled source.

> Some metadata sources work only in configured environments. For example, the `aws` source is available only when running on an AWS EC2 instance.
> 
> If your security tools report denied outbound traffic to IP addresses like `169.254.169.168` or `169.254.169.254`, you can suppress these by removing `aws` from the metadata sources described above. If you have a proxy setup, Cribl recommends adding these IP addresses to your `no_proxy` environment variable.
>
{.box .info}

##### `__metadata.kube` Property {#kube}

For the  `__metadata.kube` property (`kube` in the UI) to report details on a Kubernetes environment, Cribl Edge needs to figure out where it is running. Set the `KUBE_K8S_POD` environment variable to the name of the Pod in which Cribl Edge is running. At this point, the `__metadata.kube` property will have information to report on the `node` and `pod` properties. 
  
If the `/proc/self/cgroup`is working, then the `container` property information will be available, too. 
  
> If you leave the `KUBE_K8S_POD` environment variable unset, and `/proc/self/cgroup` is not working, then Cribl Edge will not know what Pod it is running in. This state has multiple implications:
>   - The [Kubernetes Metrics Source](sources-kubernetes-metrics) will be unable to identify whether or not it is in a DaemonSet. The result will be redundant metrics from each node in the cluster.
>   - The Kubernetes Metadata Collector will not add the `__metadata.kube property`. 
>   - The [Kubernetes Logs Source](sources-kubernetes-logs) will also duplicate data. Every node in the cluster will collect logs for every container in the cluster.
>
{.box .warning} 

#### Other System State Information

The remaining information in the System State tab is organized into the following tabs:

| Tab | Description |
| --- | --- |
| Disks | The **Disks** tab displays the inventory of physical disks and their partitions on the host system. |
| DNS | The **DNS** tab lists the host system's DNS resolvers and search entries. |
| File Systems | The **File Systems** tab displays an inventory of the mounted file systems on the host system. |
| Firewall | The **Firewall** tab displays a list of the host’s defined firewall rules. |
| Groups | The **Groups** tab displays a list of the local groups including their names, descriptions, and members on the host system. |
| Hosts File | The **Hosts File** tab displays the host system's current state. |
| Interfaces | The **Interfaces** tab displays a list of each of the network interfaces on the host system. |
| Listening Ports | The **Listening Ports** tab displays a list of listening ports and their associated process identifier (pid). |
| Logged-in Users | The **Logged-In Users** tab displays a list of currently logged-in users on the host. |
| Routes | The **Routes** tab displays entries from the network routes on the host system. |
| Services | The **Services** tab displays a list of each configured service (such as `systemd` and `initd`) along with their running status. |
| Users | The **Users** tab displays a list of local users on the host system. |

### Kubernetes {#kubernetes}

The **Kubernetes** tab, available only for Kubernetes-based Edge Nodes,
lists all the Kubernetes clusters, including nodes, pods, and containers.

![An Edge Node's Explore view, with Kubernetes tab open](explore-kubernetes-tab-4.14.png)
{border="true" scale="85%" caption="Kubernetes tab"}

Select any row to open a summary table for the pod, including:

| Tab | Description |
| --- | --- |
| **Overview** | Basic node information, including creation time, labels, taints, lease, and annotations. |
| **Host** | Detailed information about the node, including its IP address, capacity, system information, and the number of pods it's currently hosting. |
| **Pods** | List of pods running on the node, along with their resource usage and age. |

Select **Filter Rules** to view/add more filters. The default filter ignores the `kube-*` namespace, which is typically used by Kubernetes system components and may contain a large volume of irrelevant events.

**Filter Rules** can be applied to the [Kubernetes Logs](sources-kubernetes-logs) Source to refine the data ingested into Cribl Edge.

> To preview the effects of a new or modified [Event Breaker Ruleset](event-breakers#event-breaker-rulesets), you must commit and deploy the configuration changes.
>
{.box .info}

#### Kubernetes Namespaces

In the sidebar, select **Namespaces** to view the list of active namespaces in the Kubernetes cluster, along with their status and age.

Select any namespace to view detailed information, including creation time, labels, annotations, resource quotas, and resource limits.

#### Kubernetes Pods

In the sidebar under **Workloads**, select **Pods** to view a list of pods running in the cluster.
For each pod, the list shows its associated name, namespace, readiness, status, restart count, age, IP address, and the node it's running on.

Select any row to view pod information with the following tabs available:

| Tab | Description |
| --- | --- |
| **Overview** | General information about the pod, including its name, namespace, status, IP address, labels, annotations, current health conditions, and tolerations. |
| **Containers** | Details about the containers within the selected pod, including their name, image, ports used, and their container ID. |
| **Logs** | Real-time logs from the container(s), providing insights into its activity and any potential issues. |

On the **Logs** tab, you can add timestamps to container logs that lack them, by toggling **Enable timestamps** on.
This prefixes each line with a timestamp.
After the timestamps are extracted, you can remove them from the events using the Kubernetes Logs Source [Event Breaker](sources-kubernetes-logs#event-breakers)
and the [Pre-processing Pipeline](sources-kubernetes-logs#pre-processing).

Select **Events** to apply and preview Event Breaker Rulesets to your logs and preview them.
