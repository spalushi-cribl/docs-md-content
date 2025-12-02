# Grand Omnivalent Deployment Guide [DRAFT]


> This was Dritan's DRAFT expanded Deployment Guide (a.k.a. Reference Architecture kernel), as of late 2020. It's resurrected here (as a hidden doc) only for reference purposes, toward publishing our real Reference Architectures.
> 
> Many of the referenced requirements, product/feature names, quantities, and feature details are obsolete. Be warned!
>
{.box .warning}

---

There are at least **three** key factors that will determine the type of Cribl LogStream deployment in your environment: 

* Amount of Incoming Data: This is defined as the amount of data planned to be ingested per unit of time. E.g. How many MB/s or GB/day? 

* Amount of Data Processing: This is defined as the amount of processing that will happen on incoming data. E.g., are there a lot of transformations, regex extractions, parsing functions, field obfuscations, etc.? 

* Routing and/or Cloning: Is most data going to a single destination, or is it being cloned and routed to multiple places? This is important because destination-specific serialization tends to be relatively expensive.

## Type of Deployment
----
* Use [Single-Instance Deployment](deploy-single-instance) when incoming data volume is low and/or amount of processing is light.

* Use [Distributed Deployment](deploy-distributed) to accommodate increased load. See [Sizing and Scaling](scaling) for detailed guidance.


## OS and System Requirements
----
Master and Worker Nodes should have sufficient CPU, RAM, network, and storage capacity to handle your **specific** workload. It's very important to test this before deploying to production.
[block:callout]
{
  "type": "info",
  "body": "On the table below, we assume that **1 physical core is equivalent to 2 virtual/hyperthreaded CPUs (vCPUs)**."
}
[/block]

[block:parameters]
{
  "data": {
    "h-1": "Requirements Details",
    "h-0": "Requirement Type",
    "0-0": "**Minimum**\n\nMaster and Worker Nodes.",
    "0-1": "**OS**: \nLinux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)\nmacOS 10.13 or 10.14\n**System**: \n+4 physical cores, +8GB RAM, 5GB free disk space (more if [persistent queuing](persistent-queues) is enabled on Workers)",
    "1-0": "**Recommended**\n**Master Node**",
    "1-1": "**OS**: \nLinux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)\n**System**: \n+4 physical cores, +8GB RAM, 5GB free disk space",
    "2-0": "**Recommended**\n**Worker Nodes**",
    "2-1": "**OS**: \nLinux: RedHat, CentOS, Ubuntu, AWS Linux, Suse (64bit)\n**System**: \n+8 physical cores, +32GB RAM, 5GB free disk space.\n\nSee [Sizing and Scaling](scaling) for more details."
  },
  "cols": 2,
  "rows": 3
}
[/block]
CPU - 
RAM - 
Disk/FS - 

[block:html]
{
  "html": ""
}
[/block]

## Browser Requirements
----
Most modern browsers will work, but here are the minimum supported versions as of this publication date: Firefox 65+, Chrome 70+, Safari 12+, Microsoft Edge.

## <span id="cluster-checklist">Cluster Installation/Configuration Checklist</span>
----

This section compiles basic checkpoints for successfully launching a [distributed](deploy-distributed)  cluster.

### 1. Provision Hardware

  * 1 Master Node (recommended: 8 vCPU/16 GB RAM).
  * 4 Worker Nodes (recommended: 8 vCPU/16 GB RAM) across two locations – see further [system requirements here](deploy-single-instance#requirements).
  * Acquire an evaluation (Sales Trial) License from the Cribl Sales Team.

### 2. Configure Master Node

  * Install `git` if not present (e.g., `yum install git`).
  * Open the initial ports listed below:

[block:parameters]
{
  "data": {
    "0-0": "TCP:4200 or TCP:9000",
    "1-0": "TCP:9000",
    "0-1": "Heartbeat/Metrics",
    "1-1": "Cribl UI",
    "h-0": "Default Port",
    "h-1": "Purpose"
  },
  "cols": 2,
  "rows": 2
}
[/block]
  * [Download, Install, and Launch Cribl](deploy-single-instance#section-installing-on-linux-mac).
  * [Enable Start at Boot](deploy-single-instance#section-enabling-start-on-boot).
  * [Configure as a Master](deploy-distributed#section-1-configuring-a-master-node).
  * [Confirm](scaling#section-scale-up) Worker Processes Settings at `-2` (via **Settings** > **System** > **Manage Processes**).
  * [Install License](licensing).

### 3. Configure Worker Nodes

* Enable GUI Access. Administrators will need to connect to the following port on each node:
[block:parameters]
{
  "data": {
    "h-0": "Default Port",
    "h-1": "Purpose",
    "0-0": "TCP:9000",
    "0-1": "Cribl UI"
  },
  "cols": 2,
  "rows": 1
}
[/block]
  * [Download, Install, and Launch Cribl](deploy-single-instance#section-installing-on-linux-mac).
  * [Enable Start at Boot](deploy-single-instance#section-enabling-start-on-boot).
  * [Configure as a Worker](deploy-distributed#section-2-configuring-a-worker-node).
    * Tag each worker `POV`.
  * [Confirm](scaling#section-scale-up)  Worker Processes Settings at `-2` (via **Settings** > **System** > **Manage Processes**).
  * [Install License](licensing).

### 4. Map Workers to Groups

  * On the Master Node, [create a Worker Group](deploy-distributed#section-worker-groups).
    * Name the Worker Group `POV`.
  * On the Master Node, confirm that workers are connecting.
    * From the Master Node's top menu, select **Workers**.
  * [Map Workers](deploy-distributed#section-mapping-workers-to-worker-groups) to `dev` Worker Groups.
    * Use the Filter: `cribl.tags.includes('POV')`.

### 5. Other

If you will be using LogStream's GeoIP enrichment feature, install the [MaxMind database](https://dev.maxmind.com/geoip/geoip2/geolite2/) onto the LogStream Master and all Worker Nodes.
