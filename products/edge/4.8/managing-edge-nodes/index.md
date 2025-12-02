# Managing Edge Nodes


You can access the **Edge Nodes** page if you have an Enterprise or Standard [license](licensing). 

In Cribl Edge, click the **Manage** tab, then click **Edge Nodes**. This page contains five upper tabs: 

- **[Fleets](fleets)**
- **Edge Nodes**
- **[Mappings](#creating-a-mapping-ruleset)**
- **[Notifications](notifications-targets)**
- **[Logs](internal-logs)**

## Edge Nodes Tab {#workers-tab}

The **Edge Nodes** tab on the **Manage Edge Nodes** page provides status
information for each Edge Node in the selected Fleet, as well as a UI for adding
and updating Nodes.

![Edge Node tab](edge-node-tab.png)
{border="true"}

### Monitor Source and Destination Health on Edge Nodes

You can monitor and track the health of Sources and Destinations associated with
an Edge Node from the **Edge Nodes** tab. This is useful when you have a large
number of Nodes, and want to quickly identify and troubleshoot issues with
Sources and Destinations.

To sort and filter by health status, select the **Sources** or **Destinations**
column header.

To open the Node Info drawer and view specific monitoring
information, select a health icon. For more information, see [Track Data Activity for an Edge Node](node-data-activity).

### Export a List of Edge Nodes

To export the list of Edge Nodes, click the **Export list as** drop-down.
Select `JSON` or `CSV` as the export file format. If you filter the list
before exporting, the exported list will maintain that filter. You can export the
entire list by removing all filters.

### Teleport into an Edge Node

Click the **Edge Node GUID** link to teleport from the Leader into the
Edge Node. Here, you can explore the metrics and log data that the Node has
discovered automatically, and can manually discover and explore other data of
interest. 

You can use the discovered data to perform root-cause analysis, to troubleshoot,
and to restart the host. If you want to monitor and troubleshoot the Node data
without teleporting, check out the [Node Info Panel](node-data-activity).

The page displays metadata for the Node, and below it, graphs of system
activity. A magenta border indicates you are remotely viewing a host, and
identifies the host's name. 

Click **Restart Edge** to restart the Node. To return to the **Manage Edge Nodes** page, click the **X** close button on the upper right.

![Teleporting into an Edge Node](ed-teleport-node.4cf3e55509.png)
{border="true"}

> Changes that you make on an Edge Node will not propagate to the Leader. Also,
> the Leader will override any changes that you make directly on a Node.
{.box .warning}

#### Add Tags to Edge Nodes in Teleport View

You can add tags to an Edge Node you've teleported into. Tags help the Leader map
an Edge Node to a Fleet. Note that changing the tags on a Node can cause
the Leader to map the Node into a new Fleet.

1. Teleport into an Edge Node.
1. Navigate to **Node Settings** then **Distributed Settings**.
1. Enter tags for this Node and select **Save**.

> In Cribl.Cloud, you must accept the confirmation modal. Changing the tags on
> an Edge Node can affect the Fleet that the Leader maps it into.
{.box .cloud}

### Add/Update Edge Nodes {#add-node}

You can use the **Add/Update Edge Node** control at upper right to update an
existing Node, or to add a new Node by generating a bootstrap script. Cribl Edge
admins can use the UI to concatenate and copy/paste the bootstrap script,
automating several steps below. You can use an adjacent option to grab a script
that updates an Edge Node's Fleet assignment. You also have the option of
generating a bootstrap script for the following Edge Nodes:

- Docker: For details, see [Running in a Docker Container](deploy-running-docker).
- Kubernetes: For details, see [Deploying via Kubernetes](deploy-running-kubernetes).
- Linux: For details, see [Installing Cribl Edge on Linux](deploy-single-instance).
- Windows: For details, see [Installing Cribl Edge on Windows](deploy-windows).

![Options to add or update an Edge Node](edge-node-add-update.png)
{border="true"}

> The **Update** option in the UI adjusts the Leader connection details
> for the currently installed software. To upgrade the Edge Node – that is,
> install a newer version of the software – see [Upgrading](upgrading). 
>
{.box .info}

#### Edge Node Updates Using Fleet Version Settings {#fleets-target}

The **Add/Update Edge Node** command ensures accurate updates by using the specified [**Target Software Version**](upgrading-ui-nodes#upgrading-edge-nodes) for your Fleet, enhancing the reliability and consistency of updates across all supported environments: Docker, Kubernetes, Linux, and Windows. 

For Docker and Kubernetes:

- If you don’t specify a target version, the command uses the Leader version’s image and version flag respectively, by default.

- If you specify a target version, the command parses the Fleet version and maps it to the appropriate CDN URL or version flag.

For Linux environments:

- The existing script behavior from the Leader manages the updates unless you specify a target version, in which case the script uses that version.

For Windows: 

- The default behavior uses the Leader Version from the CDN, while specifying a target version directs the command to parse the Fleet version and map it for MSI download via the CDN. 

This mechanism ensures a more consistent and reliable deployment process, maintaining version control and improving update accuracy across all environments.

### Add/Bootstrap a New Edge Node {#bootstrap}

All Edge Nodes' hosts must enable ongoing outbound communication to the Leader's port 4200,
to enable the Leader to manage the Nodes.
While the bootstrap script runs, firewalls on each Nodes's host must also allow outbound communication on the following ports:
- Port 443 to `https://cdn.cribl.io`.
- Port 443 to a Cribl.Cloud Leader.
- Port 9000 to an on-prem Leader.

> The details below differ slightly depending on which deployment option you
> select.
>
{.box .info} 

1. From Cribl Edges's top nav, select Manage > Edge Nodes.

1. On the resulting Edge Nodes tab, click **Add/Update Edge Node** at the upper right.

1. Select the deployment option as shown in the composite screenshots below. 

1. In the resulting modal, the **Install package location** defaults to `Cribl CDN`. If desired, change this to `Download URL`. 

1. As needed, correct the target **Fleet**, as well as the **Leader
   hostname/IP** (URI).

1. As needed, correct the **User** and **User Group** to run Cribl as. Defaults to `cribl`.

1. As needed, correct the **Installation directory**. Defaults to `/opt/cribl`. 

1. Optionally, add **Tags** that you can use for filtering and grouping in
   Cribl Edge. Use a tab or hard return between (arbitrary) tag names.

1. Copy the resulting script to your clipboard.

1. Click either **Done** or **X** to close the modal.

Paste the script onto your Edge Node's command line and execute it, to add the
Edge Node.

### Troubleshooting the Display {#trouble}

If you see unexpected results on the **Edge Nodes** tab, keep in mind that:
- Edge Nodes that miss 5 heartbeats, or whose connections have closed for more
  than 30 seconds, will be removed from the list.
- For a newly created Fleet, the **Config Version** column can show an
  indefinitely spinning progress spinner for that Fleet. This happens because
  the Edge Nodes are polling for a config bundle that has not yet been deployed.
  To resolve this, click the **Deploy** option to force a deploy.
- For details on collocating multiple Cribl products, see [Cribl Edge and Cribl Stream on the Same Host](https://docs.cribl.io/edge/4.1/deploy-single-instance#collocated).
- For details on overriding default ports, see [Overriding Default Ports](https://docs.cribl.io/edge/4.1/deploy-running-docker#ports-override).

## How Edge Nodes and Leader Nodes Work Together  {#interaction}

The Leader Node has two primary roles:  

1. Serves as a central location for Edge Nodes' operational metrics. The Leader
   ships with a monitoring console that has a number of dashboards, covering
   almost every operational aspect of the deployment.
 
1. Serves as a central location for authoring, validating, deploying, and
   synchronizing configurations across Fleets. 

![Leader Node/Edge Nodes relationship](edge-master-node-with-work-groups.c850974436.jpg)
{border="true"}

### Network Port Requirements (Defaults)

- UI access to Leader Node: TCP 9000.
- Edge Node to Leader Node: TCP 4200. Used for Heartbeat/Metrics/Leader requests/notifications to clients (such as live captures, teleporting, status updates, and config bundle notifications).
- Edge Node to Leader Node: HTTPS 4200. Used for config bundle downloads.
- Edge Node bootstrapping/update: TCP 443.

### Leader/Edge Node Communication

Edge Nodes will send a heartbeat to the Leader every 60 seconds.
This heartbeat includes information about the Nodes and a set of current
system metrics. The heartbeat payload includes facts – such as hostname, IP
address, GUID, tags, environment variables, and current software/configuration
version – that the Leader tracks with the connection. 

When an Edge Node checks in with the Leader:
 
- It sends a heartbeat to Leader with "facts" about itself.
- The Leader uses Edge Node's facts and Mapping Rules to map it to a Fleet.
- The Edge Node pulls its Fleet's updated configuration bundle, if necessary.

> The Leader is unaware of Edge Nodes' platforms (such as Linux or Windows) within a Fleet. So the ConfigHelper omits platform-specific limitations. Therefore, when you manage Edge Nodes on heterogeneous platforms, create a Windows-specific Fleet and mapping. See [Managing Edge Nodes on Multiple Platforms](fleets#hplatforms). 
>
{.box .warning}

## Config Bundle Management

Config bundles are compressed archives of all config files and associated data
that an Edge Node needs to operate. The Leader creates bundles upon Deploy, and
manages them as follows: 
- Bundles are wiped clean on startup.
- While running, at most 5 bundles per group are kept.
- Bundle cleanup is invoked when a new bundle is created.

The Edge Node pulls bundles from the Leader and manages them as follows:
- Last 5 bundles and backup files are kept.
- At any point in time, all files created in the last 10 minutes are kept.
- Bundle cleanup is invoked after a reconfigure.

### Bundle in S3 {#remote-bundle}

You can store bundles in an AWS S3 bucket to reduce the Leader load. This offloads bundle distribution from the Leader to your designated S3 bucket. Edge Nodes will pull bundles directly from S3, minimizing Leader strain.

You can configure bundling in S3 through the following ways:

- **UI**: When [configuring](setting-up-leader-and-edge-nodes#config-leader) **Leader Settings** on a Leader Node, specify an S3 bucket (format: `s3://${bucket}`) for remote bundle storage in the **S3 Bundle Bucket URL** field.
- **YAML**: In the [`instance.yml` config](instanceyml) file, specify the S3 bucket in `master.configBundles.remoteUrl`.
- **Environment Variable**: Use the `CRIBL_DIST_LEADER_BUNDLE_URL` [environment variable](fleets#environment-variables).

#### Optimized Bundle Delivery from S3 {#optimized-S3bundle}

Cribl prioritizes efficient updates for Edge Nodes, regardless of their network environment. Here's how:

- **Direct S3 Downloads (Preferred)**: Cribl prioritizes downloading upgrade packages directly from Amazon S3 (S3) whenever possible, ensuring efficient updates for Edge Nodes connected to Cribl.Cloud or an on-premise Leader. The source is specified in the Leader's **Global Settings** > **Upgrades** > **Package source** setting.
- **Automatic Failover**: If the S3 download fails, a built-in failover automatically retrieves the bundle from the Leader itself (port `4200`). This redundancy guarantees updates even in challenging network environments, including private networks with restricted internet access. For these networks, Cribl prioritizes a CDN download before reverting to the Leader Node as a final resort.
- **Allowlist S3 Access (Optional)**: To further optimize downloads for Edge Nodes with internet access, consider adding the S3 access (`*.s3.amazonaws.com`) to the allowlist in your firewalls or proxies (due to non-static S3 addresses). This proactive step helps avoid disruptions.
  
This multi-step approach ensures efficient and reliable Edge Node updates in various network configurations.