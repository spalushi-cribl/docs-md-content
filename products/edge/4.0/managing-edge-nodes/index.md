# Managing Edge Nodes


If you have an Enterprise or Standard [license](licensing), clicking the left nav's **Manage** tab opens a **Manage Fleets** page with three upper tabs: **Fleets**, **Edge Nodes** and **Mappings**.

## Fleets Tab {#fleets-tab}

The **Manage Fleets** page provides a list of all configured Fleets and Subfleets in the instance. 

![Manage Fleets](ed-fleet_all.22be2dc69f.png)
{border="true"}

The top header now shows you the number of configured Fleets (2), along with other statistics. Clicking on the Fleet's **Name** redirects you to its [Fleet Landing Page](explore-edge#fleet-landing-page) where you can explore more information for each of your configured Edge Nodes. 

## Edge Nodes Tab {#workers-tab}

The **Edge Nodes** tab (**Manage Edge Nodes** page) provides status information for each Edge Node in the selected Fleet. 

![Edge Node tab](ed-edge-node-tab.10d30ea68d.png)
{border="true"}

You can click each Edge Node's row to display additional details and controls. 

![Edge Node tab](ed-edgenode-details.10eb556af9.png)
{border="true"}

Click the **Edge Node GUID** link to [teleport](explore-edge#teleport) from the Leader into the Edge Node.

### Add/Update Edge Nodes {#add-node}

You can use the **Add/Update Edge Node** control at upper right to update an existing Node, or to add a new Node by generating a [bootstrap script](/stream/deploy-workers#ui). You also have the option of generating a bootstrap script for a Kubernetes node. 

![Options to add or update an Edge Node](ed-add-new-node.cab0d17255.png)
{border="true"}
 
In the resulting **Add Edge Node** modal, you then generate the bootstrap script to copy, paste, and execute.

![Generating a script to bootstrap an Edge Node](ed-bootstrap-new.a146058e18.png)
{border="true"}

> **Cribl Edge 4.7.2 and newer includes a Powershell-specific bootstrap script.**
>
> The provided Cribl Edge bootstrap command might not work directly in
> PowerShell due to unescaped quotes in the `APPLICATIONROOTDIRECTORY` variable.
> When running the bootstrap command in PowerShell, escape the double quotes
> within the `APPLICATIONROOTDIRECTORY` variable. Use backticks as escape
> characters.
{.box .warning}

### Troubleshooting the Display {#trouble}

If you see unexpected results on the **Edge Nodes** tab, keep in mind that:
- Edge Nodes that miss 5 heartbeats, or whose connections have closed for more than 30 seconds, will be removed from the list.
- For a newly created Fleet, the **Config Version** column can show an indefinitely spinning progress spinner for that Fleet. This happens because the Edge Nodes are polling for a config bundle that has not yet been deployed. To resolve this, click the **Deploy** option to force a deploy.

## Mappings Tab {#mappings-tab}

Click the **Mappings** tab (**Manage Mappings** page) to display status and controls for the active [Mapping Ruleset](fleets#mapping).

![Mappings status/controls](ed-mappings-tab.2705357ae0.png)
{border="true"}

Click into a Ruleset to manage and preview its contained Rules.

![Managing Ruleset page](ed-default-mapping-rule.c3f80c45b5.png)
{border="true"}

## How Do Edge Nodes and Leader Work Together  {#interaction}

The Leader Node has two primary roles:  

1. Serves as a central location for Edge Nodes' operational metrics. The Leader ships with a monitoring console that has a number of dashboards, covering almost every operational aspect of the deployment.
 
2. Serves as a central location for authoring, validating, deploying, and synchronizing configurations across Fleets. 

![Leader Node/Edge Nodes relationship](edge-master-node-with-work-groups.c850974436.jpg)
{border="true"}

## Network Port Requirements (Defaults)

- UI access to Leader Node: TCP 9000.
- Edge Node to Leader Node: TCP 4200. Used for Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on).
- Edge Node to Leader Node: HTTPS 4200. Used for config bundle downloads.

## Leader/Edge Node Communication

Edge Nodes will periodically (every 10 seconds) send a heartbeat to the Leader. This heartbeat includes information about themselves, and a set of current system metrics. The heartbeat payload includes facts – such as hostname, IP address, GUID, tags, environment variables, current software/configuration version, etc. – that the Leader tracks with the connection. 

When an Edge Node checks in with the Leader:
 
* It sends a heartbeat to Leader with "facts" about itself.
* The Leader uses Edge Node's facts and Mapping Rules to map it to a Fleet.
* The Edge Node pulls its Fleet's updated configuration bundle, if necessary.

Edge Nodes that don’t maintain their connection to the Leader will eventually disappear from the Edge Nodes page.

> The Leader is unaware of Edge Nodes' platforms (i.e, Linux or Windows) within a Fleet. So the ConfigHelper omits platform-specific limitations. Therefore, when you manage Edge Nodes on heterogeneous platforms, create a Windows-specific Fleet and mapping. See [Managing Edge Nodes on Multiple Platforms](fleets#hplatforms). 
>
{.box .warning}

# Config Bundle Management

Config bundles are compressed archives of all config files and associated data that an Edge Node needs to operate. The Leader creates bundles upon Deploy, and manages them as follows: 
- Bundles are wiped clean on startup.
- While running, at most 5 bundles per group are kept.
- Bundle cleanup is invoked when a new bundle is created.

The Edge Node pulls bundles from the Leader and manages them as follows:
- Last 5 bundles and backup files are kept.
- At any point in time, all files created in the last 10 minutes are kept.
- Bundle cleanup is invoked after a reconfigure.
