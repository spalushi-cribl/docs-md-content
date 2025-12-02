# Destination Architecture

Cribl sends processed data to external systems using three primary Destination types: Streaming, Non-Streaming (Batch), and Internal. Understanding the architectural considerations for each, including deployment, scaling, and security, is essential for building a resilient and scalable data pipeline.

## Streaming and Non-Streaming Destinations

The fundamental distinction between destination types in Cribl is whether they handle data in a continuous stream or in discrete batches.

**Streaming Destinations** are systems that are always available to receive data in real-time. Think of message queues like Kafka or analytics platforms like Splunk. When you send data to a streaming destination, Cribl expects an immediate, persistent connection.
* **Architectural Use Case:** Use for any workflow that requires low-latency data delivery, such as real-time alerting, security monitoring, or live dashboarding.

**Non-Streaming (Batch) Destinations** are systems that ingest data in chunks or objects, such as cloud object stores (Amazon S3, Azure Blob). These collect data over a period of time, aggregated into a file, and then written to the destination.
* **Architectural Use Case:** Use for archiving data in long-term storage, compliance, or future analysis. This is the most cost-effective way to store high-fidelity data.

## Streaming Destinations

Streaming Destinations are systems that require persistent network connectivity to receive data in real-time. Architecting for these destinations involves ensuring network reliability and minimizing latency between Cribl Nodes and the downstream endpoints.

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Deployment** | Architect for reliable, low-latency network paths. |
| **Scaling** | Scale horizontally by increasing the number of destination endpoints and distributing the load via built-in load balancing. Use multiple Nodes to parallelize output.<br /><br />Cribl.Cloud automatically manages Node scaling. |
| **Security** | Enforce TLS for all external destinations. Use mutual TLS or API tokens for authentication where supported. Restrict destination ports using firewall rules and configure egress allowlists for outbound traffic. |
| **System Reqs** | Ensure sufficient CPU and memory for high-throughput serialization and encryption. Enable persistent queues (PQ) for critical destinations and allocate local disk for PQ storage to buffer during outages.<br /><br />Cribl-managed Nodes handle resource sizing and PQ disk management. |
| **Other** | Monitor PQ depth and output error metrics to detect backpressure from the destination. Tune batch sizes and flush intervals to optimize the balance between throughput and latency. |

## Non-Streaming Destinations

Non-Streaming Destinations use a local staging directory on each Node to batch events into files before uploading them. This model is highly scalable and cost-effective for archiving data.

### Architectural Considerations

| Consideration | Description |
| --- | --- |
| **Deployment** | The staging directory must be on a reliable, high-throughput local disk. Ensure network egress bandwidth is sufficient for batch uploads to cloud object stores.<br /><br />Cribl-managed Nodes handle their own staging directory. |
| **Scaling** | Scale by sharding data across multiple buckets or storage accounts. Use multiple Nodes with separate staging directories to avoid disk I/O contention. Monitor disk I/O and free space to prevent data loss. |
| **Security** | Use IAM roles or scoped credentials for cloud storage access. Enforce TLS for all uploads. For on-prem NFS, restrict access to trusted hosts and use network segmentation. |
| **System Reqs** | Provision sufficient disk space for both the staging directory and temporary files, sized based on batch size, event volume, and upload frequency. Optimize disks to reduce batch flush latency.<br /><br />Cribl-managed Nodes handle their own disks. |
| **Resilience** | These Destinations do not use persistent queues (PQ). Resilience depends entirely on the staging disk capacity. Upon restart, a Node will pace the upload of any leftover files to avoid overwhelming the destination. The staging directory must be persistent across restarts to allow Cribl to resume any incomplete batches. |
| **Other** | Tune batching conditions (size, open time) for your data profile. |

## Internal Destinations

Internal Destinations move data Node to Node within Cribl and serve as the backbone for distributed, hybrid, and tiered architectures. They enable precise routing, low-latency intra-cluster transport, and secure cross-site delivery. You can absorb downstream pauses or outages with persistent queues (PQ), while multi-endpoint targets provide load distribution and failover. This section focuses on architectural planning across deployment types, resource requirements, and performance tradeoffs.

Cribl-managed Nodes scale resources and handle Node communication, persistent queues, and load balancing automatically.

#### Data Transport Destinations: Cribl HTTP and Cribl TCP

These Destinations manage the physical transport of [data between Cribl Nodes](stream/usecase-transfer-data). The choice between them is a primary architectural decision based on network topology and security requirements.

| Aspect | **Cribl HTTP Destination** | **Cribl TCP Destination** |
| --- | --- | --- |
| **Primary Use Case** | Secure, proxy-aware transport across network boundaries (cross-site, hybrid, on-prem to cloud). | High-throughput, low-latency transport within a trusted, low round-trip-time (RTT) network (intra-site, same AZ). |
| **Security** | Supports **TLS and mTLS**, enabling strong, certificate-based authentication. Ideal for zero-trust environments and traversing proxies. | Supports **TLS and mTLS**, enabling strong, certificate-based authentication. |
| **Resilience** | **PQ** is highly recommended to absorb WAN latency and transient failures. | **PQ** is supported and effective for handling brief downstream pauses or processing spikes. |
| **Performance** | Network proxies can impact throughput. | Higher potential throughput, making it ideal for performance-sensitive hot paths. |

#### Routing Logic Destinations: Output Router, Default, and DevNull

These Destinations provide logical control over data flow rather than handling data transport themselves. They delegate their work to other configured components.

| Type | Architectural Function | Key Considerations |
| --- | --- | --- |
| **Output Router** | Provides **conditional fan-out** to multiple downstream Destinations based on event content. Enables A/B testing, data sharding, and targeted routing. | It inherits all behaviors (PQ, backpressure, billing) from the selected downstream Destination. **Monitor each target independently** to prevent hidden hot spots. |
| **Default** | Acts as a **deterministic fallback** for events that do not match any explicit route in a Pipeline, preventing silent data loss. | This component delegates all behavior to its configured target. **Use carefully**, as over-reliance can mask routing logic errors. Monitor its usage to detect misconfigurations. |
| **DevNull** | **Explicitly drops events**, serving as a terminal sink. Used for load shedding during incidents, testing, or filtering non-critical data. | It immediately removes backpressure but also causes **permanent data loss**. Manage its use by operational procedures and change controls. |

### Common Scenarios and Architectural Guidance

The following table outlines architectural patterns for common challenges, combining Destination types with best practices for deployment, scaling, and resilience.

| Scenario | Recommended Components & Configuration | Architectural Rationale & Guidance |
| --- | --- | --- |
| **High-Performance Intra-Site Forwarding** | **Cribl TCP Destination → Cribl TCP Source** | **Why:** This pairing offers the lowest overhead for high-volume flows within a data center or availability zone. <br /><br />**Guidance:** Ensure Nodes are in a low-RTT network (<10ms). Verify both ends are in Distributed mode and on compatible versions. Align TLS settings precisely. See [Transfer Data Between Workspaces or Environments](stream/usecase-transfer-data) for more information. |
| **Secure Cross-Site & Hybrid Transport** | **Cribl HTTP Destination → Cribl HTTP Source** | **Why:** HTTP/S with mTLS provides robust security for traversing untrusted networks or proxies. PQ is critical for resilience over the WAN. <br /><br />**Guidance:** Size Node with sufficient CPU for TLS operations. Place PQ on fast local storage and size it to cover the expected duration of a network outage. See [Transfer Data Between Workspaces or Environments](stream/usecase-transfer-data) for more information. |
| **Cribl Edge Fleet to Cribl Stream Handoff** | **Cribl Edge (Cribl HTTP/TCP) → Cribl Stream (Cribl HTTP/TCP)** | **Why:** This standard pattern for tiered processing supports single-ingest billing. <br /><br />**Guidance:** Use **Cribl HTTP** for geographically dispersed Cribl Edge Fleets. Use **Cribl TCP** if Edge Nodes and the Cribl Stream environment share a private, low-latency network. See [Cribl Edge to Cribl Stream](stream/usecase-edge-stream) for more information. |
| **Load Distribution & High Availability** | **Output Router → Multiple Cribl HTTP/TCP Destinations** | **Why:** The Output Router enables sharding by data attributes, while multi-endpoint Destinations handle failover. <br /><br />**Guidance:** Configure at least two endpoints per failure domain. Use health checks to ensure traffic is only sent to healthy Nodes. See [Output Router](stream/destinations-output-router) and [Load Balancing](stream/load-balancing) for more information. |
| **Resilience & Failure Domain Isolation** | **All Destinations with persistent queues (PQ)** | **Why:** PQ decouples upstream processes from downstream failures and buffers data locally during an outage. <br /><br />**Guidance:** Use site or region boundaries as failure domains. Deploy PQ on any Destination that crosses these boundaries to prevent a localized failure from causing a cascading, system wide outage. See [Optimize Destination Persistent Queues](stream/persistent-queues-destinations) for more information. |

> Learn more about [Destinations](stream/destinations)
{.box .info}