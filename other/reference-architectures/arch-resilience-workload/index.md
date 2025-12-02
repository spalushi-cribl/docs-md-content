# Data Resilience and Workload Architecture


The resilience of a Cribl deployment depends on architectural patterns that manage workloads and prevent data loss. Key controls for achieving this include load balancing, rate limits, backpressure, and persistent queues.

**Deployment Model Considerations:**
  * **Cribl.Cloud:** Handles the control plane (Leader) high availability, scaling, upgrades, and patching for you. You do not provision or maintain Leader HA, load balancers, or infrastructure. For Cribl-managed Nodes, persistent queue (PQ) sizing, disk provisioning, and upgrades are also managed automatically. You're' responsible for Pipeline logic, resilience settings, and managing backpressure or fallback at the data plane level (Nodes).
  * **On-prem/Hybrid:** You are responsible for all aspects of deployment: Leader HA, load balancers, disk provisioning for PQ, upgrades, and patching. For hybrid workflows (on-prem to cloud), compress data early in your Pipelines to optimize bandwidth.

## Sizing for Pull-Heavy Workflows

When designing your environment, account for the resource consumption of pull Sources. Unlike push Sources, which are driven by external clients, pull Sources actively poll external systems, consuming CPU, memory, and network resources on Worker Nodes.

**Key Considerations:**

* **Concurrency:** The number of concurrent polling tasks can significantly impact resource utilization. Sizing should account for the number of configured pull Sources and their polling frequency. Each concurrent task will consume a dedicated thread and a share of the available CPU, memory, and network resources. The more tasks you run, the more resources you'll consume.
* **Payload Size:** The amount of data retrieved in each poll affects memory and network bandwidth.
* **API Limits:** Be mindful of rate limits on the source APIs. Aggressive polling can lead to throttling, impacting data collection.
* **Control-Plane Dependency:** The Leader Node manages Pull Sources. To avoid a single point of failure, consider a High Availability (HA) setup for the Leader. 
  > In Cribl.Cloud Enterprise, Leader HA is already provided.
  {.box .cloud}
* **Workload Isolation:** It's a good practice to separate your pull Sources from your push Sources by using different Worker Groups for each Source type (pull vs. push). This prevents a problem with one type of Source from impacting the other.

Learn more about [Sizing & Scaling Considerations](reference-architectures/reference-arch-full-suite#sizing).

## Load Balancing

Proper load balancing is essential for distributing data evenly across Worker Nodes, ensuring high availability and scalability.

**Source Load Balancing:**

* **Push Sources:** For Sources like Syslog or HTTP-based inputs, an external load balancer is typically placed in front of the Worker Group. This distributes the incoming connections across the available Workers.
* **Pull Sources:** Configure these Sources on a Worker Group and the Leader Node distributes the execution of these tasks across the available Workers. An external load balancer is not needed for the Sources.
* **Leader Traffic:** Use a network-level load balancer to control traffic between the Workers and the Leader Node. 
  > Cribl.Cloud manages Leader/Worker connectivity and HA. External LBs are only your concern for push Sources egressing from your network to Cribl.Cloud endpoints or for Destinations that require customer-managed LBs.
  {.box .cloud}

**Destination Load Balancing:**

* Many Destinations have built-in load balancing capabilities. For example, the Splunk Load Balanced Destination can send data to multiple indexers.
* For Destinations that don't have native load balancing, you can often place an external load balancer in front of the systems.

Learn more [About Load Balancing](stream/load-balancing).

## Latency

Latency, the delay between data generation and its availability in a Destination, is influenced by several factors:

* **Network:** The physical distance and network performance. On-prem to cloud communication will inherently have higher latency than communication within a single data center.
* **Processing Complexity:** Complex Pipelines with many Functions will introduce more processing latency.
* **Batching vs. Real-time:** Sources and Destinations often batch data to improve efficiency. For low-latency needs, you can reduce batch sizes, but this can increase CPU usage. Some Destinations, like Amazon S3, prioritize high throughput over low latency.
* **Persistent Queues (PQ):** When a Destination is down, an enabled PQ will write data to disk. This adds a disk-write and disk-read step, which increases latency. For latency-sensitive workflows, ensure you use high-performance disks for your persistent queues.

## Limits

Setting limits is a powerful way to protect your Cribl environment and downstream systems. The main goals are to:
* **Protect Resources:** Prevent bursty or misbehaving clients from overwhelming Worker CPU, memory, and network connections.
* **Prevent Overload:** Stop cascading failures where a problem in one system brings down others.
* **Prioritize Traffic:** Ensure critical data flows smoothly by slowing down or dropping less important data first.
* **Maintain Performance:** Keep latency and throughput predictable, even when things go wrong.

Limits are a tradeoff. Throttling traffic can increase latency or reduce throughput. Use them when they help you meet your performance goals and protect shared resources.

> **Cribl.Cloud** deployments manage the underlying infrastructure. These limits are the primary tools you use to control data flow, manage backpressure, and protect your downstream systems.
{.box .cloud}

| Limit Type | What it does | Benefits | Costs/Tradeoffs | Use when |
| --- | --- | --- | --- | --- |
| **Active Requests**<br /><br />Relevant Sources use **Active request limit** under **Advanced Settings**. | Caps the number of simultaneous client connections to a Source on each Worker. | Prevents a Worker from running out of memory or network sockets. Creates a predictable environment for sizing your infrastructure. | During traffic spikes, Nodes might refuse new clients, which increases latency as they wait to reconnect. Setting this too low will underutilize your Workers. | You have many clients with unpredictable connection behavior (like syslog forwarders) or you want to isolate different source types with different concurrency rules. |
| **Rate Limiting (Throughput Ceilings)**<br /><br />On Destinations, configure **Throttling** to cap send rate.<br /><br />On Sources, bound concurrency with **Requests-per-socket** and use **Sampling**/**Dynamic Sampling** in Pipelines to reduce event rate. | Caps the data rate (in bytes/sec or events/sec) for a Source or Destination. | Prevents traffic from overloading downstream systems. Smooths out traffic bursts to preserve latency for high-priority data. | When the limit is hit, data backs up, which increases latency and can fill up persistent queues. | Your downstream system has known ingestion limits, you need to ensure fairness between different data sources sharing a Worker Group, or you need to cap network costs. |
| **Payload Size Limits**<br /><br />Sources use **Max event bytes** on Event Breaker Rules.<br /><br />Destinations use **Body size limit** or **Batch/Record size**. | Rejects or truncates messages or batches that are too large. | Prevents large, unexpected payloads from causing memory spikes on Workers. Ensures predictable latency by keeping batch sizes consistent. | The system might drop rejected events unless you configure a Dead Letter Queue, which could impact compliance. | You receive data from untrusted clients that might send malformed or oversized events, or your destination has a hard limit on payload size. |
| **Retry Budget**<br /><br />Relevant Destinations have a **Retries** section. | Sets a boundary on how many times (or for how long) Cribl will try to resend data to a failing destination before giving up. | Prevents infinite retry loops that can clog up your Workers and network. | If the budget is too small, you might fail over during a brief, temporary network blip. If it’s too large, you can prolong an outage’s impact by tying up resources. | You have a clear failover plan and want deterministic, predictable behavior during outages. |

**Putting it together:**
* **Start with your goal:** Decide if your priority is low latency, zero data loss, or low cost, and tune your limits to support that goal.
* **Combine controls:** A balanced approach, like moderate connection limits, rate ceilings, and a bounded retry budget, usually produces the most stable results.
* **Observe and iterate:** Monitor your queue fill rates, end-to-end latency, and CPU usage to fine-tune your limits over time.

## Backpressure

Backpressure is a critical concept for resilience. It's the mechanism by which a downstream system can signal to an upstream system that it's unable to accept more data.

* **Mechanism:** When a Destination is slow or unavailable, it stops accepting new connections or data. The Worker detects this and stops sending data to that Destination.
* **Impact:** Backpressure propagates backward through the system. If a Destination is blocked, the Pipeline processing will halt for that Destination, and eventually, the Source will stop accepting new data. This prevents data loss by stopping ingress when egress is blocked.
> **Cribl.Cloud:** Cribl.Cloud has backpressure mechanisms built-in to ensure the stability of the service. Destination PQ on managed Workers is auto-capped to 1 GB and blocks when full. You still choose per-Destination backpressure behavior and configure routing or fallback.
{.box .cloud}

Learn more about [Managing Backpressure](stream/manage-backpressure) and [Data Delivery to Unreachable Destinations](stream/destinations#data-delivery-to-unreachable-destinations).

### Persistent Queue (PQ)

Persistent queuing (PQ) is a key feature for ensuring data durability when a Destination is unreachable. 

> In Cribl.Cloud deployments, PQ is always enabled and capped at 1 GB per process on Cribl-managed Workers. Sizing and drain strategy only relevant for hybrid/self-hosted Workers.
{.box .cloud}

* **How it Works:** When a Destination is down and the system engages backpressure, Cribl can write the outbound data to a disk-based queue on the Worker Node. When the Destination comes back online, the Worker will drain the queue, sending the stored data.
* **Placement Strategy:** You can enable PQ at the **Source** (to protect against data loss on ingest) or at the **Destination** (to protect delivery to the final system). The choice depends on what part of the flow you need to protect most.
* **Sizing:** You can estimate the required disk space with the formula: `Queue Size = Ingest Rate (in Bps) x Max Outage Time (in seconds)`.
* **Drain Strategy:** When a Destination recovers, configure the drain rate to be gradual. This prevents overwhelming the recovered system.
* **Load Shedding:** If a queue becomes completely full, you can configure a rule to drop less important data or send it to a `DevNull` Destination. This sacrifices some data to keep the most critical Pipelines flowing.

Learn more about [persistent queues](stream/persistent-queues).

## Processing Failure and Recovery

When transformation or transport fails, design for containment and observability.

### Dead Letter Queue

A dead-letter queue (DLQ) is a location for data that cannot be delivered to its intended Destination after a series of retries. DLQ is available on many filesystem-based/non‑streaming Destinations. For example: Amazon S3, S3‑compatible stores, Azure Blob Storage, Azure Data Explorer, Google Cloud Storage, Exabeam, and Minio.

* **Implementation:** In Cribl, you can **Enable dead-lettering** in the advanced settings of a Destination. If the primary Destination fails to send data after all retries, Cribl routes the data to the configured dead-letter location (for example, an S3 bucket, a local file system, or another SIEM).
* **Use Case:** This is critical for ensuring that no data is lost due to prolonged Destination outages or misconfigurations. It allows you to inspect and replay the failed data later.
* **Metadata:** When sending to a DLQ, include metadata like an `error_code` or `stage` to make troubleshooting easier.
* **Governance:** Be aware that a DLQ can contain sensitive raw data, so apply appropriate access controls.

### Retry/Backoff

When a Destination is temporarily unavailable, Cribl will automatically retry sending the data.

* **Configuration:** Most Destinations have configurable retry settings. You can define the number of retries and the backoff period (the time to wait between retries).
* **Exponential Backoff:** It's a best practice to use an exponential backoff strategy, where the delay between retries increases with each failed attempt. This prevents the Workers from overwhelming a struggling Destination with constant retry attempts.

### Operational Monitoring

Proactive monitoring is essential for maintaining a resilient and performant Cribl deployment. Learn more about [Operational Monitoring](arch-monitoring).