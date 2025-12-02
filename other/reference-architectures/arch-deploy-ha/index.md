# High Availability Architecture

Maximizing high availability across the Cribl control plane and data plane requires specific architectural strategies for each component to ensure resilience across your environments.

## Control Plane: Leader High Availability (HA)

The control plane is the command-and-control communication fabric that flows from the active Cribl Leader to all other components in the environment, including Stream Workers and Edge Nodes. It manages configuration, orchestration, and state, signaling nodes to collect new configurations and perform upgrades. 

For distributed environments (on-prem, hybrid, or cloud), Cribl strongly recommends Leader High Availability (HA) to ensure resilience and minimal service interruption.

### Cribl.Cloud Leader HA and Management

In Cribl.Cloud, the Leader is provisioned automatically with a highly available architecture as a managed service. Cribl handles all infrastructure, redundancy, and failover operations, reducing your operational overhead cost.

### How On-Prem Leader HA Works

In a customer-managed HA setup, only one Leader Node is active at any given time, while a second Leader remains in standby.

* **State Consistency:** The primary and standby Leaders use a shared, NFS-backed volume for consistent state and configuration. This ensures the standby node has the most current configuration immediately upon failover. 
* **Seamless Failover:** If the primary Leader fails, the standby Leader automatically takes over. This seamless transition is facilitated by a load balancer placed in front of the Leader Nodes, which actively polls the `/health` endpoint. The load balancer ensures that Worker/Edge Nodes and other clients are always routed to the active and healthy Leader, making the switchover virtually seamless for the rest of the Cribl environment.

### Proxy Routing for Simplified Leader Access

To simplify load balancer routing, both the primary and standby Leader Nodes are configured to accept incoming traffic and use a proxy function. 

Since both Leader Nodes (primary and standby) report the same healthy status (a `200` HTTP response code) to the load balancer health check, the load balancer is simplified and can route client requests to either node. The standby Leader then automatically proxies any incoming requests—UI/API traffic on port `9000` and control plane connections on port `4200` to the elected primary.

This architecture maintains crucial single-writer semantics, as only the primary Leader owns the state.

> For proxy mode to work and for the control plane to function, ensure firewall rules allow Leader-to-Leader communication on ports `9000` and `4200` to forward UI/API and control plane traffic, respectively.
>
{.box .info}

For key settings and configurations, see details in [Leader High Availability/Failover](/stream/deploy-add-second-leader/).

### Consequences of Unplanned Leader Outage Without HA

If the Leader for Cribl Stream or Cribl Edge goes down without HA configured, the control plane managing all configurations and job orchestration is immediately lost, causing service issues. 

Loss of the Leader causes these critical failures:

* **Configuration control (including licensing):**  Stream Workers and Edge Nodes stop receiving new configurations or orchestration commands. The system operates on its last known state, but no changes can be applied. The Leader also stops distributing license information. While nodes continue to run with their last known license, if a node is restarted, it will lose this state (including the license) and revert to a free license.
* **Collector jobs fail:** Collector and pull-based data sources that rely on the Leader for scheduling and instruction will stall or fail, leading to data gaps or loss for those specific workloads.  
* **Management access lost:** The Leader UI and REST API become inaccessible, preventing administrators from viewing status, troubleshooting, or making changes.  
* **Ad hoc tasks blocked:** Any operator-initiated actions, such as ad hoc collection jobs or on-demand fetches, cannot be started.

The outage also impacts system visibility and requires manual recovery:

* **Metrics interrupted:** Operational metrics (health, throughput, errors) from Stream Workers and Edge Nodes cannot be sent to or processed by the Leader. These metrics will be delayed or lost until the Leader is restored.  
* **Manual recovery required:** Without HA, there is no automatic failover. Recovery requires manual intervention (restarting or replacing the Leader), leading to extended service interruption and increased operational effort.  
* **Risk escalates:** The outage creates the risk of data loss (for pull-based Sources), compliance gaps, and service-level breaches due to the loss of monitoring and control.

### Expected Behavior During Leader Failover

The impact of a Leader failover on data flow varies based on the Source type. These effects might be seen on Stream Workers and on Edge Nodes.  

| Data Flow Type | Expected Outcome | Risk & Mitigation |
| :---- | :---- | :---- |
| **Push-based Sources** | Data flow generally continues uninterrupted. Worker/Edge Nodes continue to receive and process events. | A brief delay in config updates might occur until the standby Leader is active. |
| **Pull-based Sources** | Persistent connections and collection jobs that originate from Worker/Edge Nodes typically resume after the standby Leader becomes active.  | Pull-based collection jobs (which include scheduled tasks) might be interrupted and either delayed or fail, depending on the source type and the job's configuration settings. |
| **Collectors** | Collector-based sources (REST, API, etc.) might temporarily stop data collection, because Workers rely on the Leader to issue collection tasks.  When the new Leader becomes primary, it will resume the job based on its state and configuration. | The new Leader resumes the jobs, by either: <ul><li> Restarting jobs currently in the middle of discovery. </li><li> Resuming jobs if collection was running and the job is marked to resume on boot. </li><li> Skipping missed jobs if the job is marked skippable.  |
| **Notifications** | Notifications will stop working entirely because the Leader, which is responsible for generating and sending them, is offline. Any alert that would have fired will be, at best, delayed until the Leader returns, or at worst, skipped completely. | While the Leader outage prevents notification generation, configure secondary alert Destinations for general redundancy of your alerting targets, as a general best practice. |

 
> Data loss is rare but possible during large-scale migrations or with a misconfigured shared volume (NFS). If a pull job is running during a failover, some data batches might be skipped or delayed depending on job timing. However, you can mitigate the risk of data loss for Sources through the use of persistent queues, and you can mitigate collection risks by monitoring your jobs and systems and configuring job-specific behavior. For details, see [About Persistent Queues.](/stream/persistent-queues/)
>
{.box .info}

### Data Plane: Group/Fleet Design

The data plane, composed of Worker and Edge Nodes, is responsible for processing data. These components are designed for horizontal scalability but maintain necessary temporary local state, including the persistent queue, S3 source checkpoints, and staged data prior to cloud storage uploads (S3, Azure, GCP).

### Best Practice: Scale for N+1 Redundancy

For resilient deployments, Worker Groups should be designed to support N+1 redundancy. This means running one Worker more than necessary to handle peak operational demands, ensuring uninterrupted service during maintenance or unexpected outages.

A minimum of three Worker Nodes is recommended to provide necessary capacity headroom for traffic spikes and recovery after an outage.

For details, see [How Many Worker Nodes?](/stream/deploy-architecture/#how-many-worker-nodes)

For Cribl Edge recommendations on scaling, see [Planning Large-Scale Deployments of Cribl Edge](/edge/how-to-scale-edge/).

### Load Balancing for Push-Based Senders

All push-based data Sources (such as syslog or forwarders) must send data to Worker Nodes through a load balancer with health checks configured.

* Load balancers route traffic only to healthy Workers, ensuring events are processed immediately.  
* The load balancer should poll Worker health endpoints (for example, every 60 seconds) and only route events to Workers returning a 200 status code.    
*  Cribl-Managed Worker Groups in Cribl.Cloud deploy this architecture by default, using Network Load Balancers (NLBs) or equivalents.

For details, see [Load Balancer Health Checks.](/stream/deploy-add-second-leader/#load-balancer-health-checks)

#### Source-Level Health Checks

For HTTP-based Sources, you can enable a dedicated Source-level health check endpoint for use by load balancers. A 200 OK response confirms the Source's health. For details, see [Source-Level Health Checks](/stream/deploy-add-second-leader/#source-level-health-checks).


### Expected Behavior During Worker Failure

The data plane is designed to tolerate individual Worker failures with minimal disruption:

| Data Flow Type | Expected Outcome | Risk & Mitigation |
| :---- | :---- | :---- |
| **Push-based Sources** | Traffic is immediately routed to remaining healthy Workers by the load balancer. | Data loss is highly unlikely unless all Workers fail simultaneously. Peristent queues is the primary mechanism to ensure data in transit is not lost if a Worker/Edge Node fails or is restarted, minimizing all risk.|
| **Pull-based Sources** | Scheduled jobs are redistributed among remaining Workers by the Leader. | Jobs might be delayed. Focus on job configuration (such as `resume on boot`) and monitoring to prevent collection gaps. |
| **Collectors** | Collector load and jobs are rebalanced to healthy Workers by the Leader after a Worker failure. | If too few Workers remain, the reassignment and execution of failed tasks may be significantly delayed. This delay can lead to collection gaps or loss, particularly for time-sensitive jobs. |


### Cribl Outposts (Preview Feature) 

Cribl Outpost allows you to shift the control plane connection point into your own environment. It simplifies network architecture by using one or more Outpost Nodes as the local relay for control-plane traffic.

Once you establish an Outpost Node and configure it to accept traffic from Edge Nodes, control plane communication between those Nodes and the Leader will pass through the Outpost. This design simplifies network maintenance. Instead of configuring firewall exceptions for potentially thousands of individual Edge Nodes, you only need to establish the communication lines to and from the Outpost Node(s).

#### Outpost Node Resilience

A single Cribl Outpost Node is a single point of failure. For high availability (HA), you must deploy multiple Outposts with a load balancer or round-robin DNS setup to ensure better availability for the upstream Leader.

#### Data Plane 
While the Outpost centralizes control-plane traffic, in highly restricted networks, third-party proxies might still be required to manage the separate connections and firewall rules for the data plane (the flow of events from the Edge Node to its final destination).

For details, see [Connect Edge Nodes to Leader Through Cribl Outpost](/edge/outpost/).