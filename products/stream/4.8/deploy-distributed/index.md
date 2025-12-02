# Distributed Deployment

 
Getting started with a distributed Cribl Stream deployment 

---

For larger data volumes and increased processing, you can scale up to a multiple-instance Distributed deployment. In a Distributed deployment, a single Leader Node manages all Worker Nodes and is responsible for tracking and monitoring their activity metrics, keeping configurations in sync, and handling version control.

Read the following docs for more high-level concepts related to Distributed deployment:

- To facilitate authoring and management of configuration settings for a particular set of Worker Nodes as a group, see [Worker Groups](worker-groups).
- To plan for unexpected outages in Distributed deployments, see [Leader High Availability/Failover](deploy-add-second-leader).

> For detailed deployment requirements, see [OS and System Requirements](requirements).
>
{.box .info}


## Concepts

Review the following basic concepts as a first step toward understanding how a Distributed deployment works. 

**Single instance**: A single Cribl Stream instance, running as a standalone (not distributed) installation on one server.
{#concept-single-instance}

**Leader Node**: A Cribl Stream instance that runs in **Leader** mode. You use the Leader Node to centrally monitor and author configurations for the Worker Nodes in a Distributed deployment.
{#concept-leader-node}

**Worker Node**: A Cribl Stream instance that runs as a **managed Worker**. The Leader Node fully manages the configuration of each Worker Node. (By default, the Worker Nodes poll the Leader for configuration changes every 10 seconds.)
{#concept-worker-node}

**Worker Group**: A collection of Worker Nodes that share the same configuration. You map Workers to a Worker Group using a Mapping Ruleset.

- Use multiple Worker Groups to make your configuration reflect organizational or geographic constraints. For example, you might have a US Worker Group, an APAC Worker Group, and an EMEA Worker Group, each with their own distinct certifications and settings.

**Worker Process**: A Linux process within a Single Instance, or within Worker Nodes, that handles data inputs, processing, and output. The process count is constrained by the number of physical or virtual CPUs available; for details, see [Sizing and Scaling](scaling).
{#concept-worker-process}

**Mapping Ruleset**: An ordered list of filters used to map Workers Nodes into Worker Groups.

> ##### Options and Constraints
>
> You can manually change the local configuration on a Worker Node, but changes won't persist on the filesystem. To permanently modify the configuration of a Worker Node, save and commit it, and then deploy it from the Leader Node. See [Deploy Stream Configurations](deploying-configurations).
> 
> With an Enterprise license, you can configure [Permissions‑based](members#group) or [Role‑based](roles) access control at the Worker Group level. Non-administrator users can access Workers only within the Worker Groups on which they're authorized.
>
{.box .warning}

## Architecture

Here’s a visual overview of a distributed Cribl Stream deployment.

![Distributed deployment architecture](load-balancing-3.0a.24cca707c7.png)
{border="true"}

The division of labor among components of the Leader Node and Worker Nodes in a Distributed deployment is as follows:

**Leader Node**
 - API Process: Handles all the API interactions.
 - Config Helpers: One process per Worker Group. Helps with maintaining configurations, previews, etc.

**Worker Nodes**
 - API Process: Handles communication with the Leader Node (that is, with its API Process) and handles other API requests.
 - Worker Processes: Handle all the data processing.

For comparison, the division of labor on a Single-instance deployment is simpler, because the Leader Node and Worker Nodes are essentially condensed into one stack:

 - API Process: Handles all the API interactions.
 - Worker Processes: Handle all data processing.
  	- One of the Worker Processes is called the leader Worker Process (not to be confused with the Leader Node) and is responsible for writing configurations to disk, in addition to data processing. The API Process handles the same responsibilities as a Leader Node's API Process, while the Worker Processes correspond to the Worker Nodes' Worker Processes. The exception is that one Worker Process does double duty, also filling in for one of the Leader Node's Config Helpers.

##  How Worker Nodes and the Leader Node Work Together {#interaction}

The Leader Node has two primary roles:  

- It serves as a central location for the operational metrics of Worker Nodes. The Leader Node ships with a monitoring console that includes dashboards that cover almost every operational aspect of the deployment.
 
- It authors, validates, deploys, and synchronizes configurations across Worker Groups. 

![Leader Node/Worker Nodes relationship](master-node-with-work-groups-3.0.5359efe5ac.png)
{border="true"}

### Network Port Requirements (Defaults)

The following are the default network port requirements.

- UI access to Leader Node: TCP 9000.
- Worker Node to Leader Node: TCP 4200. Used for Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on).
- Worker Node to Leader Node: HTTPS 4200. Used for config bundle downloads.

### Communication

Worker Nodes periodically (every 10 seconds) send a heartbeat to the Leader Node. This heartbeat includes information about the Worker Nodes and a set of current system metrics. The heartbeat payload includes facts – such as hostname, IP address, GUID, tags, environment variables, current software/configuration version, etc. – that the Leader Node tracks with the connection. If a Worker Node fails to successfully send two consecutive heartbeats, it is removed from the Workers page until the next time Leader Node receives a heartbeat message from it.

When a Worker Node checks in with the Leader Node:
 
* The Worker Node sends heartbeat to Leader Node.
* The Leader Node uses the Worker Node's configuration, tags (if any), and Mapping Rules to map it to a Worker Group.
* The Worker Node pulls its Worker Group's updated configuration bundle, if necessary.

### In-Flight Data Safeguarding

If a Worker Node goes down, it loses any data that it's actively processing. With streaming senders ([Push Sources](sources#push)), Cribl Stream normally handles that data only in-memory. So, to minimize the chance of data loss, configure [persistent queues](persistent-queues) on Sources and/or Destinations that support it.

### Leader/Worker Dependency {#independence}

If the Leader Node goes down, Worker Groups can continue autonomously receiving and processing incoming data from most Sources. They can also continue executing Collection tasks already in process. 

However, if the Leader Node fails, future scheduled [Collection](collectors) jobs will also fail. So will data collection on the Amazon Kinesis Data Streams, Prometheus Scraper, and all Office 365 Sources, which function as Collectors.

## Commit and Deploy Controls {#commit-deploy}

Distributed mode includes a Worker Group-specific **Commit & Deploy** control. Whereas the global **Commit** button commits (but does not deploy) configurations for *all* Worker Groups and for the Leader Node, the **Commit & Deploy** button commits and deploys configurations for the currently selected Worker Group. 

These controls will appear slightly differently depending on whether you have changes committed but not yet deployed, and on whether you've enabled the [Collapse actions](version-control#collapse-actions) setting.

![Commit and Deploy controls per Worker Group, at upper right](se-commit-deploy-x2.d2279a564e.png)
{border="true"}

## Manage Worker Nodes {#workers}

To manage Worker Nodes, select **Manage** in the top bar. This opens the Groups landing page with these upper tabs: **Groups**, **Workers**, and **Mappings**. (With a paid license, you will also see a [Notifications](notifications) tab.)

### Workers Tab {#workers-tab}

The **Workers** tab provides status information for each Worker Node in the selected [Worker Group](worker-groups). An **Auto-refresh** toggle (on by default) ensures that the **Config Version**  automatically refreshes when you commit or deploy. (This option is available in Cribl Stream 4.4 and newer and appears only in Worker Groups that contain fewer than 100 Worker Nodes.)

![Worker Nodes status/controls](workers-status-tab.png)
{border="true"}

If you see unexpected results here, keep in mind that:

- Worker Nodes that miss 5 heartbeats, or whose connections have closed for more than 30 seconds, will be removed from the list.
- For a newly created Worker Group, the **Config Version** column can show an indefinitely spinning progress spinner. This happens because the Worker Nodes are polling for a [config bundle](config-bundle-management) that has not yet been deployed. To resolve this, select **Deploy** to force a deploy.

To export the list of Worker Nodes, select the **Export list as** drop-down.
Select `JSON` or `CSV` as the export file format. If a list has any filters
applied, the exported list will also be filtered. To export an entire list,
remove any applied filters.

To reveal more details about a Worker Node, click anywhere on its row.

![Worker Node details](st-workers-status-details.0f5518f8e8.png)
{border="true"}

### Mappings Tab {#mappings-tab}

The **Mappings** tab displays status and controls for the active [Mapping Ruleset](mapping-workers-to-worker-groups).

![Mappings status/controls](workers-mappings-tab.35baf3993d.png)
{border="true"}

Select a Mapping Ruleset ID to manage and preview its rules.

![Editing the active Mapping Ruleset](mappings-default.0d076420c6.png)
{border="true"}

## Auto-Scale Workers and Load-Balance Incoming Data

If data flows in via load balancers, make sure to register all instances. Each Cribl Stream Node exposes a health endpoint that your load balancer can check to make a data/connection routing decision. 

Health Check Endpoint | Healthy Response
--- | ---
`curl http://<host>:<port>/api/v1/health` | `{"status":"healthy"}`

## Workers GUID

When you install and first run the software, Cribl Stream generates a GUID that it stores in a `.dat` file located in `CRIBL_HOME/local/cribl/auth`, e.g.:

```shell
$ cat /opt/cribl/local/cribl/auth/676f6174733432.dat
{"it":1647910738,"phf":0,"guid":"e0e48340-f961-4f50-956a-5153224b34e3","lics":["license-free-3.4.0-XYZ98765"]}
```

When deploying Cribl Stream as part of a host image or VM, be sure to remove this file, so that you don't end up with duplicate GUIDs. Cribl Stream will regenerate the file on the next run.

## Host Metadata {#host-metadata}

Cribl Stream can add a `__metadata` property to every event produced from every enabled Source. You can view the metadata collected for a Worker Node by selecting the Worker Node on the **Workers** tab. 

The metadata surfaced by a Worker Node can be used to:
- Enrich events (with an internal `__metadata` field).
- Display metadata to users as a part of instance exploration.

You can customize the type of metadata collected at **Group Settings** > **Limits** > **Metadata**. Add and select metadata sources by selecting **Add source** and choosing an option from the list.

> In Cribl Stream, all the **Event metadata sources** are disabled by default.
>
{.box .success}

The sources that you can select here include:
- `os`: Reports details for the host OS and host machine, like OS version, kernel version, CPU and memory resources, hostname, network addresses, etc.
- `cribl`: Reports the Cribl Stream version, distributed mode, Worker Nodes (for managed instances), and configuration version.
- `aws`: Reports details on an EC2 instance, including the instance type, hostname, network addresses, tags, and IAM roles. For security reasons, we report only IAM role **names**.
- `env`: Reports environment variables.
- `kube`: Reports details on a Kubernetes environment, including the node, Pod, and container. For details, see [__metadata.kube Property](#kube).

When these metadata sources are enabled (and can get data), Cribl Stream will add the corresponding property to events, with a nested property for each enabled source.

> Some metadata sources work only in configured environments. For example, the `aws` source is available only when running on an AWS EC2 instance. And `kube` applies only to on-prem deployments. 
> 
> If your security tools report denied outbound traffic to IP addresses like `169.254.169.168` or `169.254.169.254`, you can suppress these by removing `aws` from the metadata sources described above. If you have a proxy setup, Cribl recommends adding these IP addresses to your `no_proxy` environment variable.
>
{.box .info}

#### `__metadata.kube` Property {#kube}

For the  `__metadata.kube` property (displayed as `kube` in the list of event metadata sources) to report details on a Kubernetes environment, Cribl Stream needs to know where it is running. Set the `KUBE_K8S_POD` environment variable to the name of the Pod in which Cribl Stream is running. This gives the `__metadata.kube` property the information it needs to report on the `node` and `pod` properties. 
  
If `/proc/self/cgroup` is working, the `container` property information will be available, too. 

