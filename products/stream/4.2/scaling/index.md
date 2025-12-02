# Sizing and Scaling


A Cribl Stream installation can be scaled **up** within a single instance and/or scaled **out** across multiple instances. Scaling allows for: 

* Increased data volumes of any size.
* Increased processing complexity.
* Increased deployment availability. 
* Increased number of destinations.

We offer specific examples below, and introduce reference architectures in [Architectural Considerations](/stream/deploy-architecture).

## Scale Up {#worker-processes}

A Cribl Stream installation can be configured to scale up and utilize as many resources on the host as required. In a [single-instance deployment](deploy-single-instance), you govern resource allocation through the **Settings** > **Global Settings** > **System** >  **Service Processes** > **Processes** section.

In a [distributed deployment](deploy-distributed), you allocate resources per Worker Group. Navigate to **Manage** > `group-name` > **Group Settings** > **Worker Processes**.

Either way, these controls are available:

- **Process count**: Indicates the number of Worker Processes to spawn. Positive numbers specify an absolute number of Workers. Negative numbers specify a number of Workers relative to the number of CPUs in the system. like this: {`<number of CPUs available>` minus `<this setting>`}. The default is `-2`.  

   Cribl Stream will correct for an excessive + or - offset, or a `0` entry, or an empty field. Here, it will guarantee at least the **Minimum process count** set below, but no more than the host's number of CPUs available.

- **Minimum process count**: Indicates the minimum number of Worker Processes to spawn. Overrides the **Process count**'s lowest result. A `0` entry is interpreted as "default," which here yields `2` Processes.

- **Memory (MB)**: Amount of heap memory available to each Worker Process, in MB. Defaults to `2048`. (See [Estimating Memory Requirements](#memory) below.) Heap memory is allocated dynamically from the OS as it is requested by the Process up to the amount dictated by this setting. If a Process needs more than the amount of heap memory allocated, it will encounter an `Out of memory` error, which is logged to `$CRIBL_HOME/log/cribl_stderr.log`. This error indicates either a memory leak or that the memory requirements are too low for the workload being performed by the Process. Cribl support can help you troubleshoot this situation.

For changes in any of the above controls to take effect, you must click the **Manage Processes** page's **Save** button, and then restart the Cribl Stream server via **Settings**  > **Global Settings** > **System** >  **Controls** > **Restart**. In a distributed deployment, also deploy your changes to the Groups.

For example, assuming a Cribl Stream system running on Intel or AMD processors with 6 physical cores hyperthreaded (12 vCPUs):  

* If ** Process count** is set to `4`, then the system will spawn exactly 4 processes. 
* If ** Process count** is set to `-2`, then the system will spawn 10 processes (12-2).

## Scale Out

When data volume, processing needs, or other requirements exceed what a single instance can sustain, a Cribl Stream deployment can span multiple Nodes. This is known as a [distributed deployment](deploy-distributed). Here, you can centrally configure and manage Nodes' operation using one administrator instance, called the Leader.

It's important to understand that Worker Processes operate in parallel, i.e., independently of each other. This means that: 

1. Data coming in on a single connection will be handled by a single Worker Process. **To get the full benefits of multiple Worker Processes, data should come in over multiple connections.** <br/>
E.g., it's better to have 5 connections to TCP 514, each bringing in 200GB/day, than one at 1TB/day.

2. Each Worker Process will maintain and manage its own outputs. E.g., if an instance with 2 Worker Processes is configured with a [Splunk](destinations-splunk) output, then the Splunk destination will see 2 inbound connections. <br/>
For further details, see [Shared-Nothing Architecture](deploy-types#shared-nothing).

## Capacity and Performance Considerations {#capacity}

As with most data processing applications, Cribl Stream's expected resource utilization will be proportional to the type of processing that is occurring. For instance, a Function that adds a static field on an event will likely perform faster than one that applies a regex to find and replace a string. Currently:

* A Worker Process will utilize up to 1 physical core (encompassing either 1 or 2 vCPUs, depending on the [processor type](#examples). 
* Processing performance is proportional to CPU clock speed.
* All processing happens in-memory.
* Processing does not require significant disk allocation. 

Throughout these guidelines, we assume that **1 physical core** is equivalent to: 

* 2 virtual/hyperthreaded CPUs (vCPUs) on Intel/Xeon or AMD processors.
* 1 vCPU on Graviton2/ARM64 processors.

> Overall guideline: Allocate 1 physical core for each 400GB/day of IN+OUT throughput.
>
{.box .info}

## Examples {#examples}

### Estimating Number of Cores {#estimating-requirements}

In estimating core requirements – per processor type, it's important to distinguish among vCPUs versus physical cores. 

#### Intel/Xeon/AMD Processors with Hyperthreading Enabled

To estimate the number of cores needed: Sum your expected input and output volume, then divide by 400GB per day per physical core.

* Example 1: 100GB IN ‑> 100GB out to each of 3 destinations = 400GB total = 1 physical core (or 2 vCPUs).
* Example 2: 3TB IN ‑> 1TB out = 4TB total = 10 physical cores (or 20 VCPUs).
* Example 3: 4 TB IN ‑> full 4TB to Destination A, plus 2 TB to Destination B = 10TB total = 25 physical cores (or 50 vCPUs).

#### Graviton2/ARM64 Processors

Here, 1 physical core = 1 vCPU, but overall throughput is ~20% higher than a corresponding Intel or AMD vCPU. So, to estimate the number of cores needed: Sum your expected input and output volume, then divide by 480GB per day per vCPU.

* Example 1: 100GB IN -> 100GB OUT to each of 3 destinations = 400GB total = 1 physical core. 
* Example 2: 3TB IN -> 1TB OUT = 4TB total = 8 physical cores. 
* Example 3: 4 TB IN -> full 4TB OUT to Destination A, plus 2 TB OUT to Destination B = 10TB total = 21 physical cores. 

Remember to also account for an additional core per Worker Node for OS/system overhead.  

###  Estimating Number of Nodes {#sizing}

When sizing the number of Stream Worker Nodes, there are two other factors to consider:

- Number of Nodes sized for peak workloads;
- Number of Nodes offline.

This will allow you to create a robust Cribl Stream environment that can handle peak workloads even during rolling restarts and maintenance downtime. 

General guidance includes:

- On each Node, Cribl recommends at least 4 ARM vCPUs, or at least 8 x86 vCPUs (i.e., 4 cores with hyperthreading). Below this theshold, the OS overhead from reserved threads claims an excessive percentage of your capacity.
- On each Node, Cribl recommends no more than 48 ARM vCPUs, or 48 x86 vCPUs (i.e., 24 cores with hyperthreading). This allocation handles disk I/O requirements when [persistent queueing](persistent-queues) engages.
- Plan to successfully handle peak workloads even when 20% of your Worker Nodes are down. This allows for OS patching, and for conducting rolling Stream upgrades and restarts.
- For customers in the 5–20 TB range, size for 4‑8 Worker Nodes per Worker Group.

### Sizing Example for a Deployment   

**Step 1:** Calculate the total inbound + outbound volume for a given deployment.

In this example, your deployment has 6TB/day streaming into Worker Nodes for a given site, and this data is being routed to both cheap mass storage (S3) and to your system of analysis – 10 TB/day going out.  

Stream Workers must have enough CPUs to handle a total of 16 TB per day IN+OUT.

**Step 2:** From the available server configurations, size the correct number of Nodes for your environment.

 You are considering an AWS Compute-optimized Graviton EC2 instance, with either 16-vCPU or 8-vCPU options. We'll walk you through how to calculate based on each option. 

**Using Graviton 16-vCPU instances**

- Each Node would have 15 vCPUs available to Cribl Stream, each at 480 GB per day per vCPU.  
- 15 vCPUs per Node at 480 GB per day per CPU = 7200 GB/day per Node.
- 7200 GB/day divided by 1024 GB/TB = 7 TB/day.
- To handle the workload of 16 TB/day, you need 3 Nodes. Make sure you always round up. 
- 20% of 3 Nodes ≈ 1 Node. Make sure you always round up. To provide for redundancy during patching, maintenance, or restarts, you need one additional server.

With 4x 16-vCPU Nodes, you have 28 TB/day capacity, and can support 21 TB/day with one Node down.

**Using Graviton 8-vCPU instances**

- Each Node would have 7 vCPUs available to Stream, each at 480 GB per day per vCPU.  
- 7 vCPUs per Node at 480 GB per day per CPU = 3360 GB/day per Node.  
- 3360 GB/day divided by 1024 GB/TB = 3.3 TB/day.
- To handle the workload of 16 TB/day, you need 5 Nodes. Make sure you always round up. 
- 20% of 5 Nodes = 1 Node. To provide for redundancy during patching, maintenance, or restarts, you need one additional server.

With 6x8‑vCPU Nodes, you have 19.8 TB/day capacity, and can support 16.5 TB/day with one Node down.

If you are optimizing for price, AWS pricing is linear based on the total number of cores. So six 8‑vCPU systems are roughly 75% the cost of four 16-vCPU systems.  

If you are optimizing for potential future expansion, the 4x16-vCPU systems would offer ideal additional capacity.

###  Estimating Memory Requirements  {#memory}

The general guideline for memory allocation is to start with the default 2048 MB (2 GB) per Worker Process, and then add more memory as you find that you're hitting limits.

Memory use is consumed per component, per Worker Process, as follows:

1. Lookups are loaded into memory.  
  (Large lookups require extra memory allocation – see [Memory Sizing for Large Lookups](lookups-library#sizing).)
2. Memory is allocated to in-memory buffers to hold data to be delivered to downstream services.
3. Stateful Functions ([Aggregations](aggregations-function) and [Suppress](suppress-function)) consume memory proportional to the rate of data throughput. 
4. The Aggregations Function's memory consumption further increases with the number of **Group by**'s. 
5. The Suppress Function's memory use further increases with the cardinality of events matching the **Key expression**. A higher rate of distinct event values will consume more memory.


## Recommended AWS, Azure, and GCP Instance Types {#vms}

You could meet the requirement above with multiples of the following instances:

**AWS** – Intel processors, [Compute Optimized Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/compute-optimized-instances.html). For other options, see [here](https://aws.amazon.com/ec2/physicalcores/).

Minimum | Recommended
--- | ---
c5d.2xlarge (4 physical cores, 8vCPUs)<br/>c5.2xlarge (4 physical cores, 8vCPUs) | c5d.4xlarge or higher (8 physical cores, 16vCPUs)<br/>c5.4xlarge or higher (8 physical cores, 16vCPUs)

**AWS** – Graviton2/ARM64 processors, [Compute Optimized Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/compute-optimized-instances.html). For other options, see [here](https://aws.amazon.com/ec2/instance-types/).

Minimum | Recommended
--- | ---
c6g.2xlarge (8 physical cores, 8vCPUs)<br/>c6gd.2xlarge (8 physical cores, 8vCPUs) | c6g.4xlarge or higher (16 physical cores, 16vCPUs)<br/>c6gd.4xlarge or higher (16 physical cores, 16vCPUs)

**Azure** – [Compute Optimized Instances](https://docs.microsoft.com/en-us/azure/virtual-machines/fsv2-series) 

Minimum | Recommended
--- | ---
Standard_F8s_v2 (4 physical cores, 8vCPUs) | Standard_F16s_v2 or higher (8 physical cores, 16vCPUs)


**GCP** – [Compute Optimized Instances](https://cloud.google.com/compute/all-pricing#compute-optimized_machine_types) 

Minimum | Recommended
--- | ---
c2-standard-8 (4 physical cores, 8vCPUs)<br/>n2-standard-8 (4 physical cores, 8vCPUs) | c2-standard-16 or higher (8 physical cores, 16vCPUs)<br/>n2-standard-16 or higher (8 physical cores, 16vCPUs)

In all cases, reserve at least 5GB disk storage per instance, and more if [persistent queuing](persistent-queues) is enabled.

## Measuring CPU or API Load  {#profiling}

You can profile CPU usage on individual Worker Processes. 

### Single-Instance Deployment

Go to **Settings** > **Global Settings** > **System** >  **Service Processes** > **Processes**, and click **Profile** on the desired row.

![Worker CPU profiling (single-instance)](cpu-profiling-1-instance.3ff9e0603e.png)
{border="true"}

### Distributed Deployment

This requires a few more steps: 

1. Enable [Worker UI Access](deploy-distributed#worker-access) if you haven't already.
1. From the top nav, select **Manage** > **Workers**.
1. Click on the **GUID** link of the Worker Node you want to profile.
  (You will now see that GUID in a **Worker** drop-down at the top left, above a purple header that confirms that you've tunneled through to the Worker Node's UI.)
1. Select **Settings** from that Worker Node's top nav.
1. Select **System** > **Worker Processes** from the resulting side nav.
1. Click **Profile** on the desired Worker Process. To profile the API usage, click **Profile** on the **api** row. 

![Worker CPU profiling (distributed)](cpu-profiling-distributed.abcc9fb4af.png)
{border="true"}

### Generating a CPU or API Profile

In either a single-instance or distributed deployment, you will now see a **Worker Process Profiler** modal. 

The default **Duration (sec)** of `10` seconds is typically enough to profile continuous issues, but you might need to adjust this – up to several minutes – to profile intermittent issues. (Longer durations can dramatically increase the lag before Cribl Stream formats and displays the profile data.)

Click **Start** to begin profiling. After the duration you've chosen (watch the progress bar), plus a lag to generate the display, you'll see a profile something like this:

![Worker CPU profile](cpu-profile.35e1e7c1e9.png)
{border="true"}

Profiling the API process can be a helpful troubleshooting tool if the Leader is slow.

![API profile](api-profile.32aca4cfb5.png)
{border="true"}

Below the graph, tabs enable you to select among **Summary**, **Bottom‑Up**, **Call Tree**, and **Event Log** table views.

To save the profile to a JSON file, click the **Save profile** (⬇︎) button we've highlighted at the modal's upper left. 

Whether you've saved or not, when you close the modal, you'll be prompted to confirm discarding the in-memory profile data.

See also: [Diagnosing Issues > Including CPU Profiles](diagnosing#diag-profile).
