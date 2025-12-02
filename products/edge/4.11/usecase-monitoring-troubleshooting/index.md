# Monitor and Troubleshoot Cribl Edge


Cribl Edge is a data collection agent for Windows, Linux, and Kubernetes. Even robust agents like Cribl Edge can encounter issues that disrupt data flow. To maintain optimal performance and reliability, it's essential to implement effective monitoring and troubleshooting strategies.

In this topic, we explore key metrics to monitor for optimal Cribl Edge operation, discuss how to gather this information, and provide effective monitoring strategies and general troubleshooting guidelines.

Additionally, we highlight the importance of integrating Cribl Edge with your existing IT monitoring and alerting tools to gain comprehensive visibility into its health and performance.

## Cribl Edge and Resource Constraints

Cribl Edge is designed to be a lightweight agent that often runs on machines that have limited resources. Unlike Cribl Stream, Edge uses a single Worker Process to limit resource usage and avoid affecting other functions of that machine.

While this is beneficial for resource-constrained environments, it's important to remember two things when implementing Edge:

* **Data consumption:** A single Worker Process can handle a limited amount data. If you try to ingest a massive amount of data, the process might become overloaded. For guidelines, see [Performance Considerations](deploy-planning#performance-considerations) and [Sizing and Scaling](/stream/scaling/).
* **Data manipulation:** Extensive manipulations within Pipelines can also strain the single Worker Process. Be strategic about the transformations you apply to the data.  

## Key Metrics to Monitor

Cribl Edge offers you multiple ways to monitor your deployments in order to get an operational view of the health of your Fleets, Edge Nodes, Sources and Destinations, and more. 

### Edge Node Status

The [Edge Nodes](managing-edge-nodes#view-edge-node-status-information) page provides status information for each Edge Node in the selected Fleet. You can use this page to quickly answer the following questions:

* Is the agent running?  
* Is it connected to Cribl Leader?

#### Edge Node Connectivity 

To verify the connectivity of your Edge Nodes, check the **Health** column in the **Edge Nodes** page. A status of "alive" indicates that the Edge Node is running and successfully communicating with the Cribl Leader.

For details, see [View Edge Node Status Information](managing-edge-nodes#view-edge-node-status-information) and [Tracking Data Activity for an Edge Node](node-data-activity).

#### Set Up Continuous Monitoring for Your Edge Nodes

​​While the Edge Nodes page provides essential information, it's important to note that disconnected Edge Nodes may be automatically removed after a certain period. To ensure continuous monitoring of your Edge Node health and performance, consider implementing centralized monitoring with a third-party solution like Grafana. By configuring your Edge Nodes to send [internal metrics](sources-cribl-internal) to a centralized monitoring solution, you can gain real-time insights into their statuses and troubleshoot potential issues proactively. For details and resources, see the [Centralized Monitoring](#centralized-monitoring) section below.

### Sources and Destinations Health

To maintain optimal performance and data reliability, monitor the health of your Sources and Destinations at both the Edge Node and Fleet levels. Monitoring at the Edge Node level provides granular insights into the performance of individual Nodes, while Fleet-level monitoring offers a broader view of overall system health and potential bottlenecks.

You can monitor and track the health of Sources and Destinations associated with an Edge Node on the **Edge Nodes** page. By filtering the list to display only Edge Nodes with "Warning" or "Error" statuses for their Sources or Destinations, you can quickly identify and troubleshoot potential issues. For details, see [Monitor Source and Destination Health on Edge Nodes](managing-edge-nodes#monitor-source-and-destination-health-on-edge-nodes) and [Filter Edge Nodes](edge-nodes-filter).

Alternatively, you can monitor the health of Sources and Destinations within a specific Fleet. To do this, navigate to the desired Fleet, select **Health**, and then choose either **Sources**  or **Destinations**. This will display a color-coded health status for each Source or Destination, providing a quick visual overview of their operational status. For details, see [Overall Source and Destination Health.](monitoring#overall-source-and-destination-health)

### Data Throughput 

There are several pages  you can use to monitor the rate at which data is collected and transmitted (data throughput) across your Edge Nodes. This information can help you determine if an Edge Node is capable of handling the current data generation rate and if data is being successfully transmitted to Destinations.

* **Cribl Edge home page:**  The Cribl Edge home page displays the aggregate data for all Fleets, Subfleets, and Edge Nodes. The charts display information about traffic in and out of the system. For details, see [Monitor Health on the Edge Home Page](monitoring#monitor-health-on-the-edge-home-page).  
* **Fleets page:** The **Fleets** page gives you access to more information about each of your Fleets and the health of Sources and Destinations associated with the Fleets. You can view the **Monitor** tab of a selected Fleet to analyze the traffic flowing through your Fleet. For details, see [Monitor Fleet Information](monitoring#monitor-fleet-information).   
* **Edge Nodes page:** To monitor the activity of your Edge Node and identify potential issues with Sources, Destinations, Routes, Pipelines, or Packs: Navigate to the **Edge Nodes** page, pick a specific Node and click anywhere on the row to launch the **Node Info** modal. Then, select **Data Activity**. This will provide detailed insights into the Node's data flow without teleporting into the Node itself. For details, see [Viewing the Data Activity for a Node](node-data-activity#viewing-the-data-activity-for-a-node).

### Resource Utilization

To effectively monitor the health and performance of your Edge Nodes, use node and process information as well as details from logs.

Sources of node and process information include:

- **System Activity** tab: Gain a detailed view of your Edge Node's health, including CPU usage, memory consumption, network traffic, and disk operations. Please note that these metrics represent the total system usage and may not solely reflect the resource utilization of the Edge Node itself. To view **System Activity**, click the row for an Edge Node. For details, see [Viewing the System Activity for a Node](node-data-activity#viewing-the-system-activity-for-a-node).
- **Process-Level Monitoring**: Delve deeper into specific processes, such as `cribl` processes, to pinpoint resource bottlenecks and optimize performance. For details, see [Processes](explore-edge#processes).

Logs to analyze include:

- **API Server Logs**: Examine the `cribl.log` file at `$CRIBL_HOME/log/cribl.log` for high-level system telemetry and license validation information.
- **Worker/Edge Node Process Logs**: Analyze the `cribl.log` files at `$CRIBL_HOME/log/worker/N/cribl.log` to scrutinize detailed resource utilization metrics for each Worker/Edge Node Process at a granular one-minute interval.

Combine these approaches to proactively identify and address potential performance issues and ensure optimal data processing and system stability.


## Effective Monitoring Strategies

This section explores various strategies for effectively monitoring your Cribl Edge deployments. We'll delve into Edge Node logging, centralized monitoring, and network monitoring techniques to ensure optimal performance and data flow.

### Edge Node Logging

Cribl Edge logs at the default `Info` level, providing a good balance between detail and performance for most environments. Analyze logs regularly to identify trends and potential issues.

If you encounter a specific issue or need more detailed information, Cribl Edge settings allows you to adjust the log level for individual channels. However, frequently changing log levels from the default `Info` might indicate an underlying issue requiring further investigation.

By carefully selecting the appropriate log level, you can obtain valuable troubleshooting information without compromising performance.  For details, see [Logging Settings](monitoring#log_settings).


### Centralized Monitoring

Centralized monitoring allows you to efficiently collect and analyze data from multiple Edge Nodes. This approach helps you proactively detect issues by enabling you to:

* **Implement alerts:** Set alerts for critical metrics, such as agent failures or performance degradation.  
* **Create dashboards**:  Visualize key metrics and gain valuable insights into the overall system health.

Combine Cribl Edge with your existing IT monitoring tools to gain a unified view of your deployment's health. This allows you to identify and address issues before they impact performance. By sending Cribl Edge logs to platforms like Elastic, Grafana, or Loki, you can:

* **Centralize log management:** Consolidate logs from various Sources for efficient analysis and troubleshooting.  
* **Create advanced dashboards:** Visualize complex metrics and correlations to gain deeper insights.  
* **Implement sophisticated alerts:** Set up alerts based on specific conditions and triggers to proactively address issues.

For details, see  [Monitoring your Infrastructure with Cribl Edge](usecase-infrastructure-monitoring). Additionally, you can use [CriblVision](https://github.com/criblio/criblvision/tree/main) to troubleshoot specific issues in CriblSearch, Grafana, and Splunk. 

### Network Monitoring

Monitor network traffic between the Edge Node and the Cribl Leader to identify and resolve network-related issues. Use third-party network monitoring tools to track network performance and troubleshoot connectivity problems efficiently.

### Troubleshooting Edge Nodes

To troubleshoot potential issues with your Edge Nodes, consider the following:

* Start by verifying the Edge Node’s configuration, including connection settings, data collection intervals, and permissions. For details, see [Configuring Cribl Edge](usecase-configuring-edge).  
* Analyze the logs in Cribl Edge. For details, see [Search Internal Logs](monitoring#log_search). To isolate issues related to a specific Edge Node, filter the logs by the Edge Node's IP address. This will help you identify any error messages or exceptions that may require attention.  
* Check network connectivity between the Edge Node and the Cribl Leader by reviewing the Health column on the Edge Nodes page.  
* To monitor Edge resource utilization, examine the API server logs and Worker Process logs. These logs provide detailed information on CPU usage, memory consumption, network traffic, and other relevant metrics. You can also monitor the System activity for the entire host by [Viewing the System Activity for a Node](node-data-activity#viewing-the-system-activity-for-a-node).

Refer to [Common Challenges on the Edge](edge-common-challenges) topic for other troubleshooting tips.