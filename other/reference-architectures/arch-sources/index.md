# Source Architecture


Cribl organizes receiving data into five Source types, each designed for a distinct ingestion pattern: Collector, Pull, Push, System, and Internal. The choice of Source dictates the architectural considerations for deployment, scaling, security, and integration within your environment.

## Collector Sources

 Intermittent data ingestion is done by **Collector Sources**. Think of them as your tool for on-demand data retrieval or for replaying historical data from a storage location. Instead of continuously listening for data, Collectors run as jobs that you can trigger manually or on a schedule for batch-oriented workflows like replay, backfill, or compliance checks. Only Cribl Stream provides Collector Sources.

### Use Cases and Examples

Collector Sources are useful where you need to pull batch data on demand.

* **Targeted Investigations and Cost Optimization:** Imagine your security team needs to investigate an incident that occurred three months ago. An Amazon S3 bucket archives all of your logs. Instead of ingesting months of data into your expensive SIEM, you can use an S3 Collector for a [targeted replay](stream/usecase-replay-s3). By pulling only the specific logs from the incident's timeframe, you dramatically accelerate the investigation and avoid high ingest costs, paying only for the data that provides immediate value.

* **Data Enrichment:** To add valuable context to your real-time data, you can use a REST API Collector. For example, set up a scheduled job to run every hour, pulling the latest indicators of compromise (IOCs), such as malicious IPs or file hashes, from a third-party threat intelligence API. This collected data can then be used in a lookup to enrich your live firewall logs, allowing you to identify potential threats as they happen.

* **Backfill and Compliance:** When you onboard a new analytics tool or need to satisfy an audit, Collectors are essential for backfilling data. If an auditor requires 90 days of security logs in a specific system, you can run a one-time collection job to pull that exact data from your archive and deliver it, fulfilling the compliance requirement without disturbing your production data flows.

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Deployment** | The Leader orchestrates Collector jobs to run on Workers, and those Workers require network access to the data source. Cribl.Cloud simplifies networking by abstracting some complexity, though you still configure access to private resources. |
| **Scaling** | Scale horizontally by adding Workers and partitioning data. Cribl.Cloud simplifies this by automatically managing the scaling of its Workers, though you still manage hybrid Workers. |
| **Security** | Requires user management of credentials (IAM roles, API keys) and enforcement of TLS. Cribl.Cloud offers a UI to assist with some credential management, but you are still responsible for securing access to private resources. |
| **Tradeoffs** | Ideal for high-throughput batch workloads but not low-latency streaming. Involves operational overhead for monitoring job outcomes. Cribl.Cloud reduces some of this overhead for its managed Workers. |

> Learn more about [Collector Sources](stream/collectors)
{.box .info}

## Pull Sources

**Pull Sources** actively retrieve data from an external system that holds data in a queue or stream, such as Kafka or Amazon SQS. In this model, Cribl initiates a persistent connection to the source and "pulls" the data in as it becomes available, acting as a consumer in a data-sharing architecture. This is ideal for integrating into existing event-driven architectures where applications publish data to a central message bus for consumption by multiple applications.

### Example Workflow – Amazon SQS

**Consuming Cloud Service Events:** You can configure many AWS services to send notifications and logs to an Amazon SQS queue. You can configure a Cribl Stream Amazon SQS Source to connect to that queue, pulling messages as they arrive. This provides a durable and scalable way to ingest data from services like AWS CloudTrail, VPC Flow Logs (via Kinesis Firehose to SQS), or custom applications.

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Deployment** | You configure Pull Sources per Worker Group (Cribl Stream) or Fleet (Cribl Edge), and Workers/Edge Nodes require direct network access to the source endpoint. Cribl.Cloud can abstract some networking, but you still need to configure access to private sources via VPC peering or similar methods. |
| **Scaling** | To scale, increase Worker/Edge Node count and partition the data source. Cribl.Cloud simplifies this by automatically managing the scaling of its Workers/Edge Nodes, while you manage any hybrid Worker Group or Fleet. |
| **Security** | Requires management of all credentials, network ACLs, and TLS certificates. Cribl.Cloud offers a UI to assist with some credential management, but you remain responsible for securing connections to private resources. |
| **Tradeoffs** | Pull Sources are flexible for near-real-time ingestion, but polling can introduce lag compared to Push Sources. While the same tradeoffs apply, Cribl.Cloud reduces some operational overhead for its managed Workers/Edge Nodes. |

> Learn more about [Pull Sources](stream/sources#pull-sources)
{.box .info}

## Push Sources

**Push Sources** are the most common way to get data into Cribl. In this model, the data source initiates the connection and "pushes" data to a listening port on Cribl. This is the standard pattern for real-time data ingestion and forms the backbone of most observability pipelines, where the source sends data as soon as it generates it.

### Example Workflow – Splunk HEC

**Centralizing Splunk Data:** You might have hundreds of Splunk Universal Forwarders sending data from various servers. Instead of sending them directly to Splunk Indexers, you can point them to a Cribl Stream Splunk HEC (HTTP Event Collector) Source. Cribl Stream can then process, route, and enrich this data before forwarding it to Splunk, another SIEM, or an object store, giving you centralized control and reducing your Splunk license costs. For example, by masking PII, dropping noisy debug events, or routing specific logs to a cheaper S3 destination.

### Syslog Example Workflow – Network and Security Devices

**Aggregating Network Logs:** Your network team manages hundreds of firewalls, switches, and routers, all generating syslog. You can configure them to send their logs to a dedicated Cribl Stream Syslog Source. Cribl Stream can then parse the different vendor formats, normalize the data (for example, extracting IPs consistently across Cisco, Palo Alto, and Fortinet logs), enrich it with GeoIP data, and then route it to both a security analytics platform and a long-term archive.

**Syslog** is a standard protocol for sending log messages and a common Push Source in most enterprises. Cribl Stream has a dedicated Syslog Source that can receive syslog data over UDP or TCP. It is often the only way to get data from network devices and legacy appliances.

> Read the [Syslog to Cribl Stream Reference Architecture](reference-arch-syslog)
{.box .info}

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Deployment** | On-prem deployments with Push Sources need to configure Workers/Edge Nodes behind a load balancer and conduct health checks for high availability and scaling. Cribl.Cloud deployments manage load balancing and health checks for you. |
| **Scaling** | On-prem deployments scale by adding more Workers/Edge Nodes to increase ingestion capacity where you monitor for signs of backpressure and dropped connections. Cribl.Cloud handles this automatically for its managed Workers/Edge Nodes, though you still manage hybrid Worker Groups or Fleets and should monitor for backpressure. |
| **Security** | Requires user management of TLS termination, firewall rules, and IP allowlists. Cribl.Cloud manages ingress security for its managed Workers/Edge Nodes, simplifying this process. |
| **Tradeoffs** | Push Sources provide real-time ingestion but requires robust load balancing and backpressure management. While the same tradeoffs apply, Cribl.Cloud reduces the operational overhead for its managed Workers/Edge Nodes. |

> Learn more about [Push Sources](stream/sources#push-sources)
{.box .info}

## System Sources

Use **System Sources** to collect data from the underlying operating system where Cribl Stream or Cribl Edge is running. These are essential for monitoring the health of your hosts and collecting logs from local files. This provides crucial visability, ensuring that the components of your data pipeline are healthy and performant.

### Example Workflow – Tailing Application Logs with Cribl Edge

**Monitoring a Custom Application:** You have a critical application running on a fleet of servers that writes its logs to `/var/log/my-app.log`. You can deploy Cribl Edge to these servers and use the File Monitor Source to tail this log file. Cribl Edge can then forward these logs to Cribl Stream for central processing, providing real-time visibility into your application's health without needing a separate logging agent. 

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Purpose** | System Sources provide host-level observability (logs, CPU, memory, disk, and network metrics). In Cribl.Cloud, while some Sources might be unavailable on Cribl-managed Workers/Edge Nodes, you retain full observability on any hybrid nodes you manage. |
| **Security** | Requires you to secure endpoints and run agents with least-privilege permissions. For Cribl-managed Workers/Edge Nodes, Cribl restricts and manages access to system data, while you retain full security responsibility for your hybrid Nodes. |

> Learn more about [System Sources](stream/sources#system-sources)
{.box .info}

## Internal Sources

Internal Sources are a special category of Sources used for two distinct purposes: **receiving data sent from other Cribl instances** and **exposing the application's own operational data (telemetry)** for monitoring. They are not used for ingesting data from external systems but are critical for building a scalable and observable Cribl deployment.

### Types of Internal Sources

| Source Type | Primary Use Case | 
| :--- | :--- | 
| **Cribl HTTP** | Preferred for most use cases. It enables even distribution across Worker/Edge Node Processes, works with NLBs/proxies, and is generally more performant and easier to set up. |
| **Cribl TCP** | Supported but less recommended due to connection pinning and more complex setup. |
| **Cribl Internal** | Exposes the application's own logs (`CriblLogs`) and metrics (`CriblMetrics`). This Source does not receive external data, it generates data about Cribl performance.  |

### Example Workflow – Monitoring Cribl Health

* **Operational Visibility:** To ensure your Cribl deployment is running smoothly, you can use the **CriblMetrics** Internal Source. This Source emits detailed performance metrics about your Worker/Edge Nodes Processes, such as events in/out per second and backpressure buildup (as measured by the depth of the persistent queue). You can create a route to send these metrics to a time-series database and build dashboards to visualize data throughput, CPU usage, and queueing statistics, allowing you to proactively manage the health of your observability pipeline.

### Architectural Considerations

| Source | Deployment & Security |
| :--- | :--- |
| **Cribl TCP** | Direct TCP traffic between nodes is fully user-managed in on-prem deployments. In Cribl.Cloud, routing is pre-configured for managed Workers/Edge Nodes, but only specific ports (20000–20010) are available for custom sources. |
| **Cribl HTTP** | Use when network restrictions require HTTP/S. In on-prem deployments, you manage all endpoint and proxy configurations. Cribl.Cloud pre-enables this for many sources, though hybrid setups might still require you configure proxies. |
| **Cribl Internal** | Provides full access to internal telemetry on-prem. In Cribl.Cloud, Cribl limits this access on managed Workers/Edge Nodes to Source and Destination logs, and you manage RBAC through the UI. Hybrid Workers/Edge Nodes retain full observability. |

> Learn more about [Internal Sources](stream/sources-cribl-internal)
{.box .info}