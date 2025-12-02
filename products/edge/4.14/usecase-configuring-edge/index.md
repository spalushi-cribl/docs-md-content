# Configuring Cribl Edge


Look through a checklist of steps to configure a complete Cribl Edge deployment.

---

This guide offers practical guidance on configuring your new deployment to efficiently collect, process, and forward your Cribl Edge data. We understand that every environment and requirement is unique, so we are offering flexible recommendations to help you build a tailored solution. Topics include:

* **Customize your Cribl Edge environment** by configuring Global Settings, certificates, and metrics limits.
* **Design effective Fleets and explore Fleet mapping** to group together Edge Nodes based on the data you are collecting. 
* **Configure essential Sources and Destinations** to meet your data collection needs, including considerations for specific Sources and Destinations.
* **Determine the best approach for routing data**, by using either Routes or QuickConnect.
* **Optimize data flow** with Pipelines.
* **Use Pre-built Packs** to streamline configuration and accelerate deployment.

### Customize Your Cribl Edge Environment

Begin by customizing your Cribl Edge instance in Global Settings. This includes:

* **Branding**: Adjust the UI display, including [custom banners ](https://docs.cribl.io/edge/settings-banners/)and  [login page](https://docs.cribl.io/edge/settings-login-page/), to match your organization's branding. 
* **Security**: Implement SSL/TLS certificates to secure communication between the Leader and its Edge Nodes. For details, see  the [Secure Leader/Edge Nodes Communication](https://docs.cribl.io/edge/securing-communications/) guide. In Cribl.Cloud, SSL/TLS certificates automatically secure communication between the Leader and Nodes, simplifying deployment and ensuring data protection.
* **Optimize Cribl Metrics on the Leader and Edge Nodes, respectively**: Review [performance considerations ](https://docs.cribl.io/edge/deploy-planning/#performance-considerations)and [set metrics limits ](https://docs.cribl.io/edge/internal-metrics/#controlling-metrics)to ensure optimal performance and resource utilization. See [Controlling Metrics in Cribl Edge](https://docs.cribl.io/edge/internal-metrics/#edge-metrics-controls) for a description of each option.


### Effective Fleet Design

Cribl Edge enables you to create flexible groups of Edge Nodes, called Fleets and Subfleets. Design your Fleets **based on the specific data being collected** by each Edge Node. 

When migrating from other agents to Cribl Edge, it's essential to avoid the common pitfalls of configuring a single Fleet for all your Edge Nodes, or creating Fleets to replicate other agent configurations. These approaches can lead to inefficient data collection and management. 

By instead creating targeted Fleets, you can optimize your data collection, processing, and flow. The Leader's platform-agnostic nature ensures seamless integration of your Fleets. Use Fleet Mapping, a feature on the Leader Node, to streamline the management of heterogeneous Edge Nodes by organizing them into Fleets. For detailed guidance beyond the scope of this guide, see [Design Fleet Hierarchy](https://docs.cribl.io/edge/fleets-design/). 

> Configuring multiple Fleets is available with an Enterprise plan. For details, go to the [Edge - Cloud Ingest](https://cribl.io/pricing/edge/?product=cloud-ingest) guide.
>
{.box .success}

#### Fleet Mapping 

**Cribl Edge Mapping Rulesets** enable you to effectively organize and classify Edge Nodes based on various criteria, such as tags, operating systems, environment variables, and more. This helps you create granular Fleet and Subfleet structures for optimized management and scaling.

Common mapping criteria include the following: 

* **Operating System (OS):** Use the platform field to filter by OS – for example: `platform == 'linux'`.
* **Environment Variables:** Leverage environment variables to identify specific environments or applications – for example: `env['ENVIRONMENT'] == 'production'`.
* **Tags:** Utilize tags to categorize Nodes based on custom attributes – for example: `instance.tags.some(tag => tag.key == 'Environment' && tag.value == 'Production')`.
* **Cloud-Specific Attributes:** For cloud deployments, use attributes like instance IDs, VPC IDs, or bucket regions.
* **Server Name:** Use the `host` field to filter by Server name, if you have well-defined server naming conventions in your company. 

For example, you can create a Fleet named `Prod-Windows-us-east` containing all Windows Nodes in a specific availability zone within your production environment. Use a filter like `platform==="windows" && aws.region==="us-east-2"` and assign it to your Fleet.

Refer to [Create a Mapping Ruleset](mapping-edge-nodes#create-a-mapping-ruleset) for details on how to map Edge Nodes. 

### Configure Essential Sources and Destinations

With your Fleets and Subfleets defined, it's time to configure Sources to collect the data you need, and to configure Destinations to receive the processed data. Sources are responsible for ingesting data from various systems, while Destinations send the processed data to your desired storage or analytics platforms.

#### Configure Sources to Collect Data 

Choose appropriate Sources based on your data collection needs. Commonly used Sources on Cribl Edge include:

##### Internal/System Sources

* [System State](https://docs.cribl.io/edge/sources-system-state/)
* [Exec](https://docs.cribl.io/edge/sources-exec/)

##### Kubernetes-Specific Sources

* [Kubernetes Metrics](https://docs.cribl.io/edge/sources-kubernetes-metrics/)
* [Kubernetes Logs](https://docs.cribl.io/edge/sources-kubernetes-events/)

##### Linux Sources
* [System Metrics ](https://docs.cribl.io/edge/sources-system-metrics/)
* [Syslog ](https://docs.cribl.io/edge/sources-syslog/)

##### Windows Sources
* [Windows Events Logs ](https://docs.cribl.io/edge/sources-windows-event-logs/)
* [Windows Metrics](https://docs.cribl.io/edge/sources-windows-metrics/) 

##### Platform-agnostic

* [File Monitor](https://docs.cribl.io/edge/sources-file-monitor/)
* [TCP](https://docs.cribl.io/edge/sources-tcp-raw/)

> While Cribl Edge can receive inbound data via Syslog or TCP, we generally discourage using Edge to ingest large volumes of data from external sources, due to its single-process nature and focus on running as an agent. Excessive network traffic can lead to performance bottlenecks or data loss. 
> 
> This applies to any external data feed that is sent to the node hosting a Cribl Edge instance. If you require more performance, or need to scale your data collection, consider using Cribl Stream instead. 
> 
> Cribl Stream is designed for larger-scale data ingestion and processing, and our [Sizing and Scaling](https://docs.cribl.io/stream/scaling/) guidance can help you determine the optimal configuration for your specific needs.
{.box .warning}

#### Considerations for the File Monitor Source

The File Monitor Source in Cribl Edge  is a powerful tool for collecting file-based data, but it can also be challenging to configure effectively. 

To optimize Cribl Edge's performance when monitoring systems with a large number of files (tens of thousands or more), consider the following guidelines for each phase: 

* **Discovery**: Involves scanning files to obtain their modification times, and can be time-consuming for extensive file systems. If you already know the specific files to monitor, you can optimize discovery speed by setting the **Discovery mode** to **Manual.** This bypasses the automatic file scanning process, reducing performance overhead and focusing resources on the files of interest. Limiting the scope of the File Monitor by using **Manual** mode's **Search path** and **Max depth** settings can significantly improve discovery speed. 
* **File Monitoring**: Enhance the efficiency of the file monitoring phase, by filtering out older, irrelevant files. Adjust the **Age duration limit** and **Check file modification** **times** settings. 

###### Discovery Phase
 To use the File Monitor Source effectively, it’s important to understand the interplay among the **Search path**, **Max depth**, and **Filename allowlist** settings. The following settings only appear when the **Discovery mode** is set to `Manual`:

* **Search path** specifies the root directory from which the File Monitor will start searching for files.
* **Max depth** defines how deep the File Monitor will recurse into subdirectories. A value of `0` limits the search to the specified search path only, excluding subdirectories.
* **Filename allowlist** is a list of file patterns to include in the monitoring. The allowlist field accepts only file patterns, not directory paths. 

The File Monitor Source first identifies potential files based on the **Search path** and **Max depth** settings. Then, it applies the **Filename allowlist** patterns to filter the list of files. Only files matching both the **Search path** and **Filename allowlist** criteria will be monitored. 

Some guidelines to keep in mind:

* Specify the full path in the **Filename allowlist**, for precise control over which files are monitored. For example: `/var/log/messages`. To optimize performance, avoid using excessive wildcards in the Cribl Search path, as this can significantly increase the number of files scanned.
* Use ` max depth=0 ` for specific directories: If you want to monitor files only within a specific directory, set **Max depth** to` 0` and specify the directory in the search path.
* Take advantage of the Explore UI: The Fleet's **Explore** > **Files** tab  can help you visualize the file structure and test different allowlist patterns before deployment.

For details and examples, refer to [Using Allowlists Effectively](https://docs.cribl.io/edge/sources-file-monitor/#using-allowlists). 
 
> Cribl Edge does not support **Auto** mode on Windows hosts. While the option will still appear in the UI for Edge Fleets that include Windows hosts, selecting it will have no effect. This is because the Leader does not adjust the user interface based on the operating systems of the connected clients.
{.box .warning}

###### Monitoring Phase

The  **Age duration limit** and **Check file modification** **times** settings apply to both **Manual** and **Auto** discovery modes in the File Monitor.

Some guidelines to consider when adjusting these settings are: 

* **Setting Check file modification times alone is not effective**: If you only enable **Check file modification times** without specifying an **Age duration limit,** the File Monitor will still process all files that match the filter criteria. To effectively filter files based on age, you must specify both settings.
* **Setting age duration alone reads all files:**  Without enabling **Check file modification times**, the File Monitor will read all files that match the specified **Filename allowlist**. No age criteria will be applied to the entire file. However, the **Age duration limit** will still be used to filter events within the files that have a timestamp older than the specified duration.
* **Setting both ensures efficient filtering:** To achieve optimal filtering, set both **Age duration limit** and **Check file modification times**. This configuration allows the File Monitor to skip files that are older than the specified duration, based on their modification time. And within the remaining files, it will skip events that are older than the specified duration.

#### Considerations for the Kubernetes Logs Source

Cribl Edge leverages the Kubernetes API to interact with the cluster and collect logs. When deployed using Helm charts, Edge operates as a DaemonSet within a Kubernetes cluster. This deployment model ensures that a copy of Edge runs on each Kubernetes Node, enabling efficient log collection from that specific Node using the Kubernetes API. Here are a few considerations to optimize your Kubernetes Logs Source, including:

* **Selective Data Collection**: Use filter expressions to match specific patterns in log messages, labels, or namespaces. The filter expressions are applied to all Edge Nodes, so carefully configure these expressions to match your specific requirements. See [Filter Expression](sources-kubernetes-logs#filters) for examples.

* **Adjust Log Retention and Rotation Settings**: For high-volume applications, log rotation can occur frequently. To ensure consistent data collection, configure cluster variables to adjust log rotations and retention policies in your Kubernetes deployment.

#### Considerations for the Windows Events Logs Source

Our [Windows Event Logs](https://docs.cribl.io/edge/sources-windows-event-logs/) Source directly interacts with Windows operating systems, eliminating the need for helper tools like PowerShell. This results in:

* **Faster collection**: Direct polling ensures more efficient data retrieval.
* **Flexible data formats**: Choose between XML or JSON to suit your specific needs. XML offers faster data collection than JSON, but this efficiency can sometimes cause some loss of event detail.
* **Message Rendering:** Choose to render message strings in the events for context. 

### Data Routing from Edge Nodes

Edge Nodes can be configured to route collected data in several ways:

* **To Cribl Stream:** For further processing and scalable distribution.
* **Directly to Destinations:** Such as cheap storage, SIEMs, or other security and IT tools.
* **Selectively:** Combining direct delivery plus routing to Cribl Stream, based on use cases.

Most customers prefer to offload as much workload from their agents as possible. To achieve this, use Cribl Edge to:

1. Collect data locally.
2. Filter out unwanted data, using the Drop Function.
3. Route the remaining data to Cribl Stream Workers for further processing and delivery to your desired Destinations. 

Cribl Edge supports a variety of Destinations for your data. Some of the most common options are: 


* [Cribl TCP](https://docs.cribl.io/edge/destinations-cribl-tcp/)
* [Cribl HTTP](https://docs.cribl.io/edge/destinations-cribl-http/)
* [S3](https://docs.cribl.io/edge/destinations-s3/)
* [Splunk](https://docs.cribl.io/edge/destinations-splunk/)

#### Considerations for  Cribl TCP vs. Cribl HTTP Destinations 

**Cribl HTTP** is generally recommended over **Cribl TCP** for data transmission between Edge and Worker Nodes connected to the same Leader, especially in environments with proxies or firewalls. Cribl HTTP provides greater flexibility and ease of integration with various systems. 

The choice between the two ultimately depends on specific use-case requirements, such as the need for low latency, high throughput, or compatibility with external systems. To optimize performance, consider advanced configuration options like throttling, load balancing, and connection pooling.

### QuickConnect vs. Routes in Cribl Edge

**QuickConnect** and **Data Routes** are both interfaces for managing data flow in Cribl Edge, but they serve distinct purposes, based on the complexity of your routing needs. 

QuickConnect is ideal for simple, straightforward scenarios with a given  Source sending all data to Destination(s). It offers ease of use and speedy configuration. 

Data Routes, on the other hand, are better suited for complex data routing involving multiple Sources, Destinations, and conditional processing. They provide flexibility, advanced logic, and scalability for handling intricate data flows. 

Choose QuickConnect for rapid, minimal configuration setups, and opt for Routes when your data requires more sophisticated routing and processing. For details, see [Routes](https://docs.cribl.io/edge/routes/).

### Processing Cribl Edge  Data with Pipelines

Once you have collected your data, you have options to transform it before sending it to your Destinations. 

You can transform data within pre-processing, processing, or post-processing Pipelines. In this section, we will highlight a few Functions/Pipelines to help you transform and optimize your data. 

> Limit Pipelines on Edge Nodes to basic Cribl Functions. Avoid complex operations, to prevent bottlenecks and ensure optimal performance. You can check the [Pipeline Profiler ](https://docs.cribl.io/edge/data-preview/#pipeline-profiling)to access statistics on each Pipeline’s performance.
{.box .warning}

#### Drop Events to Lower Ingest Cost 

By using the [Drop Function](https://docs.cribl.io/edge/drop-function/#main), you can effectively eliminate data from your ingestion pipeline without incurring additional costs.

 There are few ways you can take advantage of this Function: 

* Use pre-processing Pipelines to filter and drop specific events, based on criteria like event codes.
* Use processing Pipelines to drop data before cloning events to send them to multiple Destinations. This reduces data volume.
* Safely experiment with configurations, without incurring costs, by routing data to the DevNull Destination.

For details, refer to the [Get More Out of Cribl Edge by Dropping Events](https://cribl.io/blog/get-more-out-of-cribl-edge-by-dropping-events/) blog post. 

> The number of dropped events that lower the ingestion cost
> is calculated based on average event size.
> This means it may not represent the exact number of bytes you drop.
{.box .info}

#### Enrich Events by Adding Metadata 

To enrich and refine your data within Cribl Edge, effectively use the **Eval** Function's **Add Field** option. This feature enables you to dynamically create new fields based on existing data elements – such as tags from mapping rules, or event metadata. By promoting tags and metadata to fields, you can enhance data analysis and manipulation capabilities.  

For instance, a File Monitor might be collecting IIS logs from a Windows server. To ensure that these events are stored correctly in your final analysis system, you could specify a value of `index = 'IIS'`. This creates a new field named `index` with the value `IIS`, providing essential context for your data. 

For details, see [Host Info](explore-edge-linux#host-info).  

#### Reduce Your Data with Lookups 

Use lookup tables on Edge Nodes to filter out events that meet predefined conditions. This can significantly improve processing efficiency and reduce costs.

For more complex lookup scenarios, consider performing them on Cribl Stream instead of Cribl Edge, to offload the processing burden. This approach enables you to leverage the capabilities of Cribl Stream for advanced data manipulation and analysis, while maintaining optimal performance on Edge Nodes. 

For details, refer to the[ Lookup](https://docs.cribl.io/edge/lookup-function/#main) topic. 

#### Transform Logs into Metrics

Aggregating individual log events into metrics reduces throughput volume, reducing your infrastructure requirements and optimizing your data ingest utilization on downstream services.

For details, refer to our [Working with Metrics](https://sandbox.cribl.io/course/metrics?_gl=1*qrleds*_gcl_au*MTAwODA5NzE1Mi4xNzI2NjczNjM5) sandbox,  which walks you through extracting metrics from logs and publishing them.

## Packs for Cribl Edge

Cribl Edge supports various [Packs](https://packs.cribl.io/) that can enhance Edge functionality and provide pre-built solutions for common use cases. While the specific Packs available can vary, and may be updated over time, here are some example Packs that are often used in conjunction with Cribl Edge:

* [Cribl Edge for Windows](https://packs.cribl.io/packs/cribl-edge-for-windows)
* [Cribl Edge Windows for Common Schema](https://packs.cribl.io/packs/cribl-edge-windows-source)
* [Microservices Logs](https://packs.cribl.io/packs/cribl-microservices-logs-preprocessing)

When selecting Packs, prioritize features that align with your specific needs, while considering the potential impact on Edge Node performance. Some Packs might generate significant CPU load, especially those with large lookup tables or complex processing Pipelines. So it's generally recommended to keep Edge agents as streamlined as possible. By carefully evaluating the benefits and trade-offs of each Pack, you can optimize your Cribl Edge deployment for both functionality and efficiency.
