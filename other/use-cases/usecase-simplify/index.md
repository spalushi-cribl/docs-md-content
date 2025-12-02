# Simplify Data Collection, Processing, and Routing with Cribl Stream


Cribl Stream is designed to make management of machine data easier while offering powerful features that improve efficiency and reduce complexity. Here’s a high-level approach to simplifying data collection, processing, and routing in your organization.

**1. Centralize data collection:**
* Identify all data sources that are generating logs, metrics, or other relevant data (applications, firewalls, databases, and so on).
* Configure **Cribl Stream Sources** for each data source, specifying the data format (such as syslog or JSON) and connection details.
* Provision Workers to handle your data flow. See [Managing Cribl.Cloud Worker Groups](/stream/cloud-workers/) to learn how.

To generate sample log data using Cribl Stream's built-in datagen, see [Add a Source](/stream/getting-started-guide/#get-data-flowing). For a complete list of data sources, see [Sources Overview](/stream/sources/). 

**2. Process and enrich data:**
* Use **parsing Functions** (for example, Regex Extract) to extract meaningful information from raw log data. Define patterns to identify specific fields within diverse log messages.
* Employ **normalization Functions** to standardize data formats across different sources. This ensures consistent data structures for simplified analysis.
* Enhance data context with **Lookup Functions**. These functions dynamically retrieve additional information (such as user details) from external databases and enrich your log events.
* Implement **redaction and obfuscation Functions** to mask sensitive data (like credit card numbers or PII) before storage or analysis. This ensures compliance with privacy regulations.
* Leverage **sampling Functions** to reduce data volume sent to analytics tools. Sample only a portion of low-priority messages while ensuring that all critical events are captured. This optimizes bandwidth and storage usage.
  
To learn the basics of data refinement, see the [Getting Started Guide: Refine Data with Functions](/stream/getting-started-guide/#refine-data-with-functions). Then, get hands-on practice by trying out the [Data Shaping in Cribl Stream Sandbox](https://sandbox.cribl.io/course/data-shaping). 

**3. Route and manage data flow:**
* **Define data routing rules** to direct data streams to appropriate destinations. Cribl Stream allows you to clone and route data to real-time platforms, long-term storage solutions (like Cribl Lake), or both.
* **Implement workflow automation** to automatically reroute data based on content. This ensures that specific logs are sent to dedicated destinations (for example, SIEM for security logs or performance platforms for application logs).
* **Configure conditional routing** to handle rare or critical events (such as system failures or security breaches) separately. Route such events to designated response teams for immediate action.
* **Use format conversion Functions** to transform data formats (for example, XML to JSON) before forwarding them to specific tools. This ensures compatibility with various analysis platforms.

To learn the basics of routing, see the [Getting Started Guide: Add and Attach a Route](/stream/getting-started-guide/#route). For real-world practice, take the Cribl U course [Routing Security Data](https://cribl-trough.myabsorb.com/#/online-courses/9eb5cacd-1ebf-4930-aa7f-86780cb45f97) to see how conditional routes are applied.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}