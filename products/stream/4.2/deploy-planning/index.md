# Deployment Planning


There are at least **three** key factors that will determine the type of Cribl Stream deployment in your environment: 

* Amount of Incoming Data: This is defined as the amount of data planned to be ingested per unit of time. E.g., how many MB/s or GB/day? 

* Amount of Data Processing: This is defined as the amount of processing that will happen on incoming data. E.g., are there a lot of transformations, regex extractions, parsing functions, field obfuscations, etc.? 

* Routing and/or Cloning: Is most data going to a single destination, or is it being cloned and routed to multiple places? This is important because destination-specific serialization tends to be relatively expensive.

These factors are covered in detail in [Sizing and Scaling](scaling), and in our [Architectural Considerations](/stream/deploy-architecture) introduction to reference architectures.

## Type of Deployment

* Use [Cribl Cloud](/stream/deploy-cloud) to quickly launch a Cribl-hosted deployment of the combined Cribl applications suite (Stream, [Edge](/edge/), and [Search](/search/)). With this option, Cribl assumes responsibility for provisioning and managing all infrastructure, on your behalf.

* Use [Single-Instance Deployment](deploy-single-instance) when incoming data volume is low, and/or amount of processing is light.

* Use [Distributed Deployment](/stream/deploy-distributed) to accommodate increased load. (See [Sizing and Scaling](scaling) for detailed guidance. See [Bootstrap Workers from Leader](/stream/deploy-workers) to streamline 
Workers' deployment via scripting.)

## OS and System Requirements {#requirements}

Leader and Worker Nodes should have sufficient CPU, RAM, network, and storage capacity to handle your **specific** workload. It's very important to test this before deploying to production.

> In the table below, we assume that **1 physical core is equivalent to 2 virtual/hyperthreaded CPUs (vCPUs)**. This corresponds to Intel/Xeon or AMD processors. On Graviton2/ARM64 processors, where 1 core is equivalent to 1 vCPU – but with higher capacity – sizing can be slightly different. For details, see [Sizing and Scaling](scaling) and [Requirements](deploy-single-instance#requirements).
> 
> Although the table shows only tested distro's, Cribl Stream's general requirements are 64-bit Linux kernel >= 3.10 and glibc >= 2.17.
>
{.box .info}

Requirement Type | Requirements Details
--- | ---
**Minimum**<br/><br/>Leader and Worker Nodes. | **OS**: <br/>Linux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)<br/>**System**: <br/>+4 physical cores, +8GB RAM, 5GB free disk space (more if [persistent queuing](persistent-queues) is enabled on Workers)
**Recommended**<br/>**Leader Node** | **OS**: <br/>Linux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)<br/>**System**: <br/>+4 physical cores, +8GB RAM, 5GB free disk space
**Recommended**<br/>**Worker Nodes** | **OS**: <br/>Linux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)<br/>**System**: <br/>+8 physical cores, +32GB RAM, 5GB free disk space.



## Browser Requirements

Most modern browsers will work, but here are the minimum supported versions as of this publication date: Firefox 65+, Chrome 70+, Safari 12+, Microsoft Edge.

## Port Requirements

See [Ports](ports) for detailed information of ports which need to be open
for Cribl Stream and its intergrations to work.

## Cluster Installation/Configuration Checklist {#cluster-checklist}

This section compiles basic checkpoints for successfully launching a [distributed](/stream/deploy-distributed)  cluster.

### 1. Provision Hardware

  * 1 Leader Node (see specs/requirements in [OS and System Requirements](#requirements) above).
  * 4 Worker Nodes (see specs/requirements in [OS and System Requirements](#requirements) above).
  * Acquire an evaluation (Sales Trial) License from the Cribl Sales Team.

### 2. Configure Leader Node

  * Install `git` if not present (e.g., `yum install git`).
  * Open the necessary [ports](ports).
  * [Download, Install, and Launch Cribl](deploy-single-instance#section-installing-on-linux-mac).
  * [Enable Start at Boot](deploy-single-instance#section-enabling-start-on-boot).
  * [Configure as a Leader](/stream/deploy-distributed#section-1-configuring-a-master-node).
  * [Confirm](scaling#section-scale-up) Worker Processes Settings at `-2` (via **Settings** > **Global Settings** > **System** > **Manage Processes**).
  * [Install License](licensing).
  * [Change the default auth token value](securing-auth-token).

### 3. Configure Worker Nodes

  * Enable GUI Access. Administrators will need to connect to the TCP:9000 port on each Node.
  * [Download, Install, and Launch Cribl](/stream/deploy-single-instance#install).
  * [Enable Start at Boot](/stream/deploy-workers).
  * [Configure as a Worker](/stream/deploy-distributed#section-2-configuring-a-worker-node). (Give each Worker the (arbitrary) tag `POV`.)
  * [Confirm](scaling#section-scale-up)  Worker Processes Settings at `-2` (via **Settings** > **Global Settings** > **System** > **Manage Processes**).
  * [Install License](licensing).

### 4. Map Workers to Groups

  * On the Leader Node, [create a Worker Group](/stream/deploy-distributed#section-worker-groups).
    * Name the Worker Group (arbitrarily) `POV`.
  * On the Leader Node, confirm that workers are connecting.
    * From the Leader Node's top menu, select **Workers**.
  * [Map Workers](/stream/deploy-distributed#section-mapping-workers-to-worker-groups) to `dev` Worker Groups.
    * Use the Filter: `cribl.tags.includes('POV')`.

### 5. Other

If you will be using Cribl Stream's GeoIP enrichment feature, install the [MaxMind database](https://dev.maxmind.com/geoip/geoip2/geolite2/) onto the Cribl Stream Leader and all Worker Nodes.
