# Cribl Edge-Specific Considerations

When you're planning a Cribl Edge deployment, there are several additional factors to consider. These considerations will help you optimize data collection, processing, and routing to meet your organization's needs effectively.

To successfully deploy and manage Cribl Edge, consider the following key aspects related to its unique architecture and operational model.

## Cribl Edge Deployment and Management Considerations

| Consideration | Description |
| :---- | :---- |
| **Leader Scalability** | A single Leader Node can manage up to 10,000 Edge Nodes.  If you're planning a large-scale deployment with over 10,000 Edge Nodes, see [Planning Large-Scale Deployments of Cribl Edge](/edge/how-to-scale-edge/). |
| **Version Compatibility** | To ensure compatibility, make sure your Leader and Edge Node versions match.  For details, see [Better Practices for Upgrading Cribl Edge.](/edge/upgrading-better-practices/) |
| **Air-Gapped Environments** | If you're deploying Edge Nodes on servers without internet access, they must be able to communicate with an on-prem Leader Node. Communication with a Cribl.Cloud Leader will not work in this scenario. For details, see [Deployment Planning.](/edge/deploy-planning/) |
| **Fleet Management** | Use **Fleets** to centrally group your Edge Nodes and apply configurations. This simplifies management and ensures consistency across a large number of nodes. For details, see [Design Fleet Hierarchy](/edge/fleets-design/). |
| **Metrics Optimization** | Carefully choose the metrics each Fleet sends to its Leader Node. For high-cardinality deployments, we recommend using the **Minimal** metrics set to reduce the load on the Leader. For details, see [Performance Considerations](/edge/deploy-planning/#performance-considerations) and [Control Metrics in Cribl Edge](/edge/internal-metrics/#edge-metrics-controls). |

For additional guidance, see [Configuring Cribl Edge](/edge/usecase-configuring-edge/).

## Data Routing

Cribl Edge offers flexible data routing, but your strategy profoundly impacts performance, cost, and scalability. The goal is to offload complex workloads from your agents whenever possible. A best-practice architectural pattern uses Edge Nodes for local collection and basic filtering, then routes the remaining data to Cribl Stream for more advanced processing and delivery.

### The Recommended Architecture

For environments with many data sources, the standard and most effective architecture is distributed. In this setup, Cribl Edge Nodes collect data locally and perform initial filtering—dropping unwanted events at the Source, before routing the remaining data to a central Cribl Stream instance. This approach offloads complex collection and preprocessing from agents and centralizes the heavier, more complex workloads within the scalable Stream environment.

You also have to be mindful of how Cribl Edge can impact other applications and workloads, especially those that are business-critical. It's essential to plan your deployment to ensure that these Edge Nodes don't negatively affect your core systems.

### Direct Routing Considerations

While Cribl Edge can route data directly to destinations like cheap storage (such as, S3 or Azure Blob) or security and IT tools (such as, SIEMs), this approach is generally **not recommended**. Bypassing Cribl Stream means you lose the ability to perform valuable processing, enrichment, and filtering at scale. Direct routing can also increase the load on your final destinations, limiting your ability to optimize data before it's stored or analyzed.

For details, see [Data Routing from Edge Nodes](/edge/usecase-configuring-edge/#data-routing-from-edge-nodes).

## Determining the Best Location for Relaying Data

The placement of your Edge Nodes and their routing strategy should align with your organization's architecture and data flow requirements.

* **Deploying a Stream Worker Group On-Prem:** If you have strict data sovereignty requirements or high data volumes, deploying a Cribl Stream Worker Group on-premises is often the best choice. This allows Edge Nodes to relay data to the on-prem Stream Worker Group for further processing and routing.  
* **Routing Edge to Cribl.Cloud:** For hybrid environments, Edge Nodes can route data directly to a Stream Worker Group in Cribl.Cloud. This is ideal if you want to offload infrastructure management. Just be sure to verify that your network latency and bandwidth are sufficient for this setup.

## Optimizing Data on Edge Nodes

Even with a distributed architecture, you can perform basic but powerful optimizations on the Edge node itself. Limit these operations to basic functions to avoid bottlenecks.

| <div style="width: 75px">Function</div> | Description | Recommended Use Case |
| :---- | :---- | :---- |
| **Drop** | Removes unwanted events based on specific criteria. | Use in a pre-processing pipeline to filter events at the source, reducing data volume and saving on downstream ingest costs. |
| **Eval** | Adds new fields to events using existing data or metadata. | Enrich events with crucial context, such as adding an `index` field to logs, to aid in downstream analysis. |
| **Lookups** | Filters events by referencing a lookup table. | Reduce data volume and improve efficiency by dropping events that match predefined conditions on the Edge Node. For complex lookups, offload to Cribl Stream. |



