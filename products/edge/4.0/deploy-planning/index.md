# Deployment Planning


Key points to factor in when planning an Edge deployment: 

* Amount of data to be collected at the endpoint: This is defined as the amount of data planned to be ingested per unit of time for your metrics, logs, custom command outputs, etc. E.g., how many MB/s or GB/day?
* Amount of processing: This is defined as the amount of processing that will happen on incoming data. E.g., are there a lot of transformations, regex extractions, parsing functions, field obfuscations, etc.?
* Routing and/or cloning: Is most data going to a single destination, or is it being cloned and routed to multiple places? This is important, because destination-specific serialization tends to be relatively expensive.
* Deploying onto servers with no internet access: If you plan to deploy Edge Nodes on air-gapped on-prem servers (i.e., servers with no internet access), you must ensure that every Edge Node can communicate with a Leader that is on-prem (i.e., not in Cribl.Cloud).

## Type of Deployment

* Use [Cribl Cloud](/stream/deploy-cloud) to quickly launch a Cribl-hosted deployment of the combined Cribl applications suite (Edge, [Stream](/stream/), and [Search](/search/)). With this option, Cribl assumes responsibility for provisioning and managing all infrastructure, on your behalf.

* Use [Single-Instance Deployment](deploy-single-instance#run-edge) when incoming data volume is low, and/or amount of processing is light.

* Use [Fleet Management](fleet-management) to accommodate increased load. See [Add/Update Edge Nodes](managing-edge-nodes#add-node) to streamline Edge Nodes' deployment via scripting.

## System Requirements {#requirements}

Edge Nodes should have sufficient CPU, RAM, network, and storage capacity to handle your **specific** workload. It's very important to test this before deploying to production.

Requirement Type | Requirements Details
--- | ---
**Minimum: Edge Nodes** | **OS**: Linux: RedHat, CentOS, Ubuntu, AWS Linux, Suse, Windows Server (64bit)<br/>**System**: ~1Ghz processor, 512MB RAM, 5GB of free disk space (more if [persistent queuing](persistent-queues) is enabled on Edge Nodes)
**Recommended: Leader Node** | **OS**: Linux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)<br/>**System**: +4 physical cores, +8GB RAM, 5GB free disk space

> Edge Nodes can’t be installed on Windows laptops/desktops. 
>
{.box .warning}

## Leader Scalability {#scaling}

Our above [System Requirements](#requirements) suffice for Leaders handling up to 3,000 Edge Nodes. To support more than 3,000 Nodes, consider the following additional guidelines for the Leader's host:

- Deploy one CPU for every 3,000 Edge Nodes.
- Configure one [Connection Process](#connection) for every 3,000 Edge Nodes.
- Configure a separate [Fleet hierarchy](fleets) (Top-level Fleet including all SubFleets) for a maximum of 3,000 Edge Nodes. Do not push new configurations to more than one Fleet at a time.  
- Increase memory by 2GB RAM for every 3,000 Edge Nodes.
- Increase memory by 2MB RAM per Edge Fleet.
- In the current release, each Leader can support a hard maximum of 15,000 Edge Nodes.

## About Licensing 

* **Free**: Entitles users to manage up to 100 Edge Nodes, in a single Fleet, and ingest up to 1TB/day. 
* **Enterprise**: Entitles users to manage unlimited Edge Nodes, across unlimited Fleets and ingest up to its rated volume. 

## Permissions and Rights

You do **not** need elevated privileges to install Edge. However, you will need:
* Administrator (or equivalent) rights in order to enable Edge to start on boot.
* Sufficient rights to access the resources that need monitoring – files, metrics, etc.

##  Performance Considerations

Edge Nodes are limited to a single Worker Process, and generally will use no more than 1 CPU core.

As with most data-collection and -processing applications, Cribl Edge's expected resource utilization will be proportional to the data volume and type of processing. For instance, a Function that adds a static field on an event will perform faster than one that applies a regex to find and replace a string. 

* Processing performance is proportional to CPU clock speed.
* All processing happens in-memory.
* Processing does not require significant disk allocation. 

## Fleet Checklist

This section compiles basic checkpoints for successfully launching a [Fleet](fleet-management).

### 1. System 

  * 1 Leader Node (recommended: 8 vCPU/16 GB RAM).
  * N Edge Nodes (depending on your environment).
  * Free license, or acquire an Enterprise/Trial License from the Cribl Sales Team.

### 2. Configure Leader Node

  * Install `git`, if not present (e.g., `yum install git`).
  * Open the necessary [ports](ports).
  * Download, install, and launch Cribl Edge ([Linux](deploy-single-instance), [Windows](deploy-windows)).
  * [Configure as a Leader](setting-up-leader-and-edge-nodes#config-leader).
  * Enable [Start on Boot](deploy-boot-start).
  * [Install License](licensing).

### 3. Configure Edge Nodes {#step-3}


* Enable GUI Access. Administrators will need to connect to the TCP:9420 port on each Node.
* Download, install, and launch Cribl Edge ([Linux](deploy-single-instance), [Windows](deploy-windows)).
* Configure as a [Managed Edge Node](setting-up-leader-and-edge-nodes#config-edge).
  * Point to the Leader address (optionally, use the configured access token).
  * Give each Edge Node an arbitrary tag like `POV`.
* Enable [Start on Boot](deploy-boot-start).
    
### 4. Map Edge Nodes to Fleets

  * On the Leader Node, [create a Fleet](fleet-management##configuring).
    * Name the Fleet (abitrarily) `POV`.
  * On the Leader Node, confirm that Edge Nodes are connecting.
    * From the Leader Node's top nav, click **Manage** and select **Edge Nodes**.
  * [Map Nodes](fleet-management#mapping) to the `dev` Fleet.
    * Use the Filter to select the tag you applied when configuring Nodes: `cribl.tags.includes('POV')`.

## Configure Connection Processes {#connection}

To configure the connection processes for your Edge Nodes:

1. From the top nav, select **Settings** > **Global Settings** > **Service Processes** > **Services**.
1. Confirm that **Connections listener number of processes** is set to its default `1`. 
