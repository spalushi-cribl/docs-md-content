# Deployment Planning


There are at least **three** key factors that will determine the type of Cribl Stream deployment in your environment: 

* Amount of Incoming Data: This is defined as the amount of data planned to be ingested per unit of time. E.g., how many MB/s or GB/day? 

* Amount of Data Processing: This is defined as the amount of processing that will happen on incoming data. E.g., are there a lot of transformations, regex extractions, parsing functions, field obfuscations, etc.? 

* Routing and/or Cloning: Is most data going to a single destination, or is it being cloned and routed to multiple places? This is important because destination-specific serialization tends to be relatively expensive.

These factors are covered in detail in [Sizing and Scaling](scaling), and in our [Architectural Considerations](/stream/deploy-architecture) introduction to reference architectures.

## Type of Deployment

* Use [Cribl.Cloud](/stream/deploy-cloud) to quickly launch a Cribl-managed deployment of the combined Cribl applications suite (Stream, [Edge](/edge/), and [Search](/search/)). With this option, Cribl assumes responsibility for provisioning and managing all infrastructure, on your behalf.

* Use [Single-Instance/​Basic Deployment](/stream/deploy-single-instance) when incoming data volume is low, and/or amount of processing is light.

* Use [Distributed Deployment](/stream/deploy-distributed) to accommodate increased load. (See [Sizing and Scaling](scaling) for detailed guidance. See [Bootstrap Workers from Leader](/stream/deploy-workers) to streamline 
Workers' deployment via scripting.)

## OS and System Requirements {#requirements}

Leader and Worker Nodes should have sufficient CPU, RAM, network, and storage capacity to handle your **specific** workload. It's very important to test this before deploying to production. For details, see [OS and System Requirements](/stream/requirements/).

For large on-prem deployments handling high traffic, review the recommended OS-level tuning detailed in [OS Tuning for Large Deployments](os-tuning).


> ##### Network Requirement (DPI/IPS Exclusion)
> Deep Packet Inspection (DPI) and Intrusion Prevention Systems (IPS) on network devices situated between your Cribl Worker Groups can cause high resource utilization and connection instability in all environments (both Cribl.Cloud and on-prem).
> Explicitly exclude all Cribl data streams from DPI/IPS inspection for stable operations and optimal performance.
{.box .warning}

## Cluster Installation/Configuration Checklist {#cluster-checklist}

This section compiles basic checkpoints for successfully launching a [distributed](deploy-distributed)  cluster.

### 1. Provision Hardware

* 1 Leader Node (see specs/requirements in [OS and System Requirements](#requirements) above).
* 4 Worker Nodes (see specs/requirements in [OS and System Requirements](#requirements) above).
* Acquire an evaluation (Sales Trial) License from the Cribl Sales Team.

### 2. Configure Leader Node

* Install `git` if not present (for example, `yum install git`).
* Open the necessary [ports](ports).
* [Download, Install, and Launch Cribl](deploy-single-instance#install).
* [Enable Start at Boot](run-stream#boot-start).
* [Configure as a Leader](/stream/setting-up-leader-and-worker-nodes).
* [Confirm](scaling#section-scale-up) Worker Processes Settings at `-2` (via **Settings** > **Global** > **System** > **Manage Processes**).
* [Install License](licensing).

### 3. Configure Worker Nodes

  * Enable GUI Access. Administrators will need to connect to the TCP:9000 port on each Node.
  * [Download, Install, and Launch Cribl](/stream/deploy-single-instance#install).
  * [Enable Start at Boot](/stream/deploy-workers).
  * [Configure as a Worker](/stream/setting-up-leader-and-worker-nodes). (Give each Worker the (arbitrary) tag `POV`.)
  * [Confirm](scaling#section-scale-up)  Worker Processes Settings at `-2` (via **Settings** > **Global** > **System** > **Manage Processes**).
  * [Install License](licensing).

### 4. Map Workers to Groups

  * On the Leader Node, [create a Worker Group](/stream/worker-groups).
    * Name the Worker Group (arbitrarily) `POV`.
  * On the Leader Node, confirm that workers are connecting.
    * In the sidebar, select **Workers**.
  * [Map Workers](/stream/mapping-workers-to-worker-groups) to `dev` Worker Groups.
    * Use the Filter: `cribl.tags.includes('POV')`.

### 5. Other

If you will be using Cribl Stream's GeoIP enrichment feature, install the [MaxMind database](https://dev.maxmind.com/geoip/geoip2/geolite2/) onto the Cribl Stream Leader and all Worker Nodes.
