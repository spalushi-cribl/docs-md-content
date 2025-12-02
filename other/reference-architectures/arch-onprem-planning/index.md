# On-Prem Architecture Planning

**Cribl on-prem deployment** is the best choice when you need to maintain control over your data, leverage existing infrastructure, or meet strict data sovereignty requirements. This guide will walk you through the key considerations and best practices for designing a robust, scalable, and resilient Cribl on-prem architecture. Let's dive into the core decisions that will shape your deployment, starting with how to choose the right architecture for your needs.

## Choosing Your On-Prem Deployment Architecture

When planning an on-prem deployment, your choice of architecture will depend on two key factors:

* **Incoming Data:** The total volume of data you need to ingest daily.
* **Processing Complexity:** The complexity of your processing, such as heavy transformations, Regex extractions, and serialization.  
* **Data Type:** ​​The type of data you're handling, such as SQS, also significantly influences processing needs.

For Cribl Edge, you'll need to consider a few additional points:

* **Routing and Cloning:** Determine if data is going to a single destination or being routed to multiple places. This is important because destination-specific serialization can be expensive. 
* **Air-Gapped Deployment:**  If deploying Edge Nodes on servers without internet access, ensure each node can communicate with an on-prem Leader Node (not a Cribl.Cloud Leader).

For details, see Deployment Planning for [Cribl Stream](/stream/deploy-planning/) and [Cribl Edge](/edge/deploy-planning).

### Deployment Types

Once you've assessed your architectural needs, the next step is to select a deployment model that aligns with your environment and goals. Here's a breakdown of the supported deployment types.

| Deployment Type | Ideal For | Architecture And Setup | Use Case |
| :---- | :---- | :---- | :---- |
| **Single-Instance** | Small environments, proof-of-concept testing, or lightweight workloads. | **Cribl Stream:** All functions (inputs, processing, outputs) on one node. <br> <br> **Cribl Edge:** Operates independently without a Leader Node. <br> <br>  **Setup:** Ensure /opt/cribl or /opt/cribl-edge and its subdirectories are on the same device. | Processing and routing data in a single node; local management for Cribl Edge. |
| **Distributed** | Large environments needing scalability, resilience, and centralized management. | **Cribl Stream:** A Leader Node manages multiple Worker Nodes. Worker Groups can be scaled horizontally.  <br> <br> **Cribl Edge:** Managed Edge Nodes are controlled by a Leader Node.  <br> <br>**Load Balancing:** Essential for Push Sources that don't support native balancing. | Centralized processing and routing of data from multiple sources; Edge Nodes relay data to Stream Worker Groups. |
| **Containerized** | Flexible, scalable environments using Docker or Kubernetes. | **Cribl Stream:** Deploy with Docker images or Helm charts. Manual upgrades are required as pods revert to their Helm chart image on restart.  <br>  <br>**Cribl Edge:** Bootstrap Docker nodes from the UI; use Kubernetes for large-scale deployments. | Integration into existing containerized platforms; uniform management for both Cribl Edge and Cribl Stream. |

#### General Sizing Considerations {#sizing}

Regardless of the deployment type you choose, proper sizing is a foundational step. A stable, efficient deployment starts with a solid foundation – and that means properly sizing your infrastructure.

| Resource | Recommendation | Notes |
| :---- | :---- | :---- |
| **Processors** | 1 physical core for every 400 GB/day of IN+OUT throughput. | Adjust for processor type (such as Graviton ARM processors offer \~20% higher throughput than x86-64). |
| **Memory** | Start with 2 GB per Worker Process. | Track memory utilization. As your data processing becomes more complex with heavy transformations, Regex extractions, or serialization, your memory usage will increase. |
| **Disk Space** | Plan for persistent queues based on peak load and burst tolerance. | 100 GB of disk space can handle \~4 hours of data at a peak load of 25 GB/hour. |


For specific guidelines, see OS and System Requirements for [Cribl Stream](/stream/requirements/) and [Cribl Edge](/edge/requirements/) and [General Sizing Considerations](/stream/deploy-architecture/#general-sizing-considerations).

### Port Considerations: Enabling Communication

After you've determined your sizing, the next critical step is configuring your network. Correct port configuration is essential for inter-node communication and data flow.

| Node Type | Port(s) | Protocol | Purpose |
| :---- | :---- | :---- | :---- |
| **Leader Node** | 9000 | HTTP/S | Cribl UI and bootstrapping Fleets. |
|  | 4200 | TCP | Heartbeat, metrics, Leader requests, and software upgrades. |
|  | 443 | HTTP/S | Communication with Cribl.Cloud in hybrid deployments. |
| **Edge Node** | 9420 | TCP | Cribl Edge UI. |
|  | 9000 | HTTP/S | Communication with the Leader for bootstrapping. |
|  | 4200 | TCP | Heartbeat, metrics, and config bundle downloads. |
|  | 443 | HTTP/S | Communication with Cribl.Cloud and a CDN for updates. |
| **Integration Ports** | 8088 | TCP | Splunk HEC input/output. |
|  | 9092 | TCP | Kafka output. |
|  | 9200 | HTTP/S | Elasticsearch API Source. |
|  | 10080 | TCP | HTTP JSON Sources. |

**Custom Port Overrides:** Default ports can be overridden in the `cribl.yml` and `inputs.yml` configuration files located in `$CRIBL\_HOME/local/`.

For details, see Ports in [Cribl Stream](/stream/ports/) and [Cribl Edge](/edge/ports/).

### High Availability And Scaling Design {#ha}

Once the foundational elements of your architecture and networking are in place, you must plan for long-term stability. Designing for high availability (HA) and proper scaling is key to a resilient deployment that can withstand unexpected events and grow with your needs.

#### Leader Redundancy Architecture

For Leader redundancy, deploy at least one standby Leader with **NFS-backed shared storage** for the configuration and state. Use a load balancer in front of the Leader's UI and API ports (e.g., 9000, 4200) to ensure reliable access.

To build a comprehensive **failover and recovery plan**, you should monitor the health of your system, configure persistent queues for your most critical data sources, and establish regular backup and restore processes for all configurations and secrets. For details, see [Leader High Availability/Failover](/stream/deploy-add-second-leader/).

#### Worker Node Scaling

Beyond the Leader, a resilient deployment also relies on a robust Worker infrastructure. Proper scaling of your Worker Nodes is important for ensuring uninterrupted service and handling data spikes. For resilient deployments, we recommend a minimum of **three Worker Nodes**. This ensures the system can handle maintenance or an unplanned outage without a loss of service.

In Cribl, a Worker Process is an independent instance that handles data processing. Think of it as a separate child process. A single Worker Node can run multiple Worker Processes to provide workload distribution and increase throughput. 

| Deployment Type | Worker Processes | Total Capacity |
| :---- | :---- | :---- |
| **2 Worker Nodes** | 60 | 6 TB/day |
| **3 Worker Nodes** | 42 | 5.6 TB/day |
| **6 Worker Nodes** | 36 | 6 TB/day |

For details see, [More About Worker Groups, Nodes, and Processes – Definitions](/stream/distributed-guide/#worker-details) and [How Many Worker Nodes?](/stream/deploy-architecture/#how-many-worker-nodes)

As you plan your scaling strategy, consider the unpredictable nature of data flow.

**Throughput Variability:** Data flow is rarely constant. Plan for variability and spikes, and start with about **10% headroom** to handle unexpected demands. Use historical per-second throughput data to set thresholds and prevent system failures. For details, see [Throughput](/stream/deploy-architecture/#throughput).

**Risk vs. Cost Balance:** Also, plan for contingency capacity to handle resource demands during failure scenarios. The goal is to allocate enough contingency capacity to handle resource demands that arise during failures. Failure scenarios often require additional Worker Nodes to process backlogged data after recovery. If you don’t have enough spare capacity, this can overwhelm your existing nodes, leading to data loss or expanding delays. Your investment in contingency resources should align with your desired level of reliability. For details, see [Finding the Balance Between Risk and Cost.](/stream/deploy-architecture#finding-the-balance-between-risk-and-cost)

### Container Strategy

If you've chosen a containerized approach, a specific set of best practices will help you get the most out of your deployment. When deploying Cribl products in a containerized environment, following a few best practices ensures optimal performance, scalability, and resilience for both Cribl Edge and Cribl Stream.

#### Kubernetes Orchestration

Use Kubernetes for a scalable, managed deployment. This is ideal for large-scale environments that require automated orchestration and resource management.

| Best Practice | Description |
| :---- | :---- |
| **Probes** | Apply readiness and liveness probes to monitor pod health and ensure stable performance. |
| **Resource Management** | Set resource requests and limits appropriately for each pod to prevent resource contention and ensure consistent performance. |
| **Containerd Runtime** | For Cribl Edge, you can use the `containerd` runtime alongside Kubernetes for efficient management. |

For details, see [Orchestrated Deployment](/stream/orchestrated-deployment/)  in Cribl Stream and [Deploy Cribl Edge via Kubernetes](/edge/deploy-running-kubernetes/).

#### Docker Configuration And Container Lifecycle

Beyond orchestration, a robust strategy for container configuration and lifecycle management is essential for ensuring stability, performance, and resilience in Docker environments.

| Consideration | Details and Best Practices |
| :---- | :---- |
| **Resource Management** | Allocate specific CPU and memory resources to each container using Docker's `\--cpus` and `\--memory `constraints. This prevents resource contention and ensures predictable performance, especially in high-volume environments. <br><br>For details, see the [Docker’s Resource  Management Guide](https://docs.docker.com/config/containers/resource_constraints/). |
| **Networking** | Carefully map container ports to host ports to avoid conflicts. For example: <br><br>**Leader Node**: `http://localhost:19000`<br><br> **Worker Nodes**: Automatically assigned host ports (such as, `http://localhost:<port>`).<br><br> Use Docker's networking features to isolate containers or connect them to specific networks as needed. |
| **Persistent Data** | To prevent data loss, it's crucial to mount volumes that will persist critical data, such as configurations, logs, and Persistent Queues. Without proper volume mounting, scaling down pods can lead to data loss for anything in staging or in-flight, unless you've precisely tuned your timing. For example, when deploying: <br><br> **Cribl Edge**: `\-v /:/hostfs:ro` <br><br> **Cribl Stream**: `\-v /opt/cribl:/opt/cribl` <br><br>Ensure that mounted volumes are backed up regularly to prevent data loss. |
| **Bootstrap Configuration** | For **Cribl Edge**, use the UI to generate a bootstrap script.<br><br> For **Cribl Stream**, store configurations in a shared location (such as, S3) to support a distributed architecture. |
| **Security** | To enhance security, run containers as non-root users whenever possible. Regularly update Docker images to apply the latest security patches.  |

For configuration details, see [Docker Deployment](/stream/deploy-docker/) on Cribl Stream and [Run Cribl Edge in a Docker Container](/edge/deploy-running-docker/)

### Container-Specific Architectural Decisions

Before you finalize your plan, it's important to step back and make a few high-level architectural decisions that will guide your deployment. Making these key decisions up front will ensure a solid foundation for your on-prem architecture.

| Consideration | Description |
| :---- | :---- |
| **Platform Integration** | Integrate Cribl with your existing enterprise container management platforms, such as OpenShift or Docker Swarm, for uniform management and monitoring. |
| **Fleet Management** | Use **Fleets** and **Subfleets** to group your Cribl Edge Nodes and centrally apply configurations, simplifying management in containerized environments. |
| **Performance Tuning** | Optimize container resource allocation based on your workload's data volume, processing complexity, and routing needs. |
| **Version Compatibility** | Always ensure compatibility between your Leader Nodes and Worker/Edge Nodes versions. |

### On-Prem Planning

Making these key decisions up front will ensure a solid foundation for your on-prem architecture.

* **Infrastructure Isolation:** Isolate your production, development, and test environments using network segmentation or VLANs.  
* **Leader Placement and Sizing:** Place the Leader in a resilient part of the network with secure, high-performance storage and adequate CPU/RAM for configuration management. For details, see (General Sizing Considerations)[#sizing] and (High Availability And Scaling Design)[#ha].
* **Scaling Architecture:** Plan for both vertical and horizontal scaling. Isolate critical data flows in dedicated Worker Groups/Fleets for easier, safer scaling.  
* **Upgrade Coordination:** Upgrades are manual and require careful coordination. Always back up your configurations and secrets before making changes.

### On-Prem Deployment Trade-offs

Finally, it's crucial to understand the trade-offs that come with an on-prem deployment. While it offers control, it also requires you to take on specific responsibilities. The following table highlights the key areas where your on-prem choices have a direct impact.

| Area | Best Practice | Limitation |
| :---- | :---- | :---- |
| **Sizing** | You should plan for **peak throughput** to ensure your system can handle the highest volume of data. | Your system's physical resource limits may restrict its maximum capacity, and upgrades will need to be performed manually. |
| **HA And Scaling** | To ensure high availability and scalability, you should use redundant Leader and Worker Nodes along with external load balancers. | The level of **fault tolerance** you achieve is limited by the specific design of your deployment architecture. |
| **Container Strategy** | You must **size and monitor** your containers appropriately and **orchestrate upgrades** carefully to prevent issues. | You are constrained by resource and time limits per container, which can also impact state durability. |
| **Upgrade Considerations** | Isolate environments (such as, development, staging, and production) and schedule upgrades carefully. | You are solely responsible for implementing and managing security, backups, and auditing processes, as these tasks are all manual. |



