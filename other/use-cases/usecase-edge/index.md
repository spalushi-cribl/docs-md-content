# Use Cribl Edge to Collect Logs and Metrics on a Host Device


With Cribl Edge, you can collect logs, metrics, and application data in real time from your Linux and Windows machines, apps, and microservices, and then process and deliver them to Cribl Stream or any supported destination. All data processing in Cribl Edge is based on discrete data entities known as **events**. 

**Collect distributed data from endpoints:**

When you need to collect events from numerous endpoints like IoT devices, workstations, or remote servers, you can create a tailored scenario in Edge to accomplish this task.

You can enable local data processing at the edge (that is, on an individual server or workstation) before sending it to a central location. This reduces network load and latency.

**Discover and collect events in real-time:**

You can configure endpoints and automatically discover logs, metrics, and application data in real-time. Then, use the real-time data capture functionality to ensure you don’t miss any events.

**Process data on the Edge:**

Cribl Edge can help you streamline the data it sends to your central system by processing events directly at the source. This reduces the amount of transferred raw data. This strategy is useful for scenarios requiring low-latency processing and lightweight processing directly on devices.

**Deploy Edge with no internet access:**

If your organization’s servers are restricted from accessing the internet, you can deploy Cribl Edge locally using a command-line interface. This is called an on-prem environment. Edge Nodes can communicate with a local Leader which is also on-prem.

Follow these steps to leverage Cribl Edge to enhance your data handling capabilities:
## Define Your Data Sources in Cribl Edge
For each data source, specify which logs, metrics, and other application data you wish to collect.
### Set up Sources for Different Data Types
To configure Cribl Edge to collect different types of data events, you’ll need to set up different Sources:
* For logs, point to log directories or specify log collection methods, such as listening on a syslog port. You can use the [Syslog Source](/edge/sources-syslog/#syslog-source).
* For metrics, choose and configure the appropriate metrics collection modules, such as the [System Metrics Source](/edge/sources-system-metrics/#system-metrics-source) for Linux or the [Windows Metrics Source](/edge/sources-windows-metrics/#windowsmetrics-source) for Windows.
* For application data, define custom data sources as needed, ensuring that the Edge agent has the necessary access to collect this data. 
To see all of the Sources that Cribl Edge offers, see the [Sources documentation](/edge/sources/). 
### Define Processing Rules
Next, create processing rules that will transform your data before it’s sent to a Destination. Define any data transformation or enrichment that must be applied at the edge level before sending the data upstream.
You can create data processing Pipelines and add Functions that parse, structure, and cleanse the incoming data.
You can also use various tools to configure field extraction to transform unstructured data into structured data formats. These tools can be broadly categorized as:
* **Regular expressions (regexes):** Using [regex](/edge/regex-library/) allows you to define patterns within the data. Cribl Edge can then extract specific parts of the data that match the pattern and assign them to designated fields.
* **Parsers:** Cribl Edge offers built-in [parsers](/edge/parsers-library/) for common data formats like JSON, CSV, and syslog. These parsers automatically recognize the structure of the data and extract relevant fields based on predefined rules.
* **Grok patterns:** Similar to regular expressions, [Grok patterns](/edge/grok-patterns-library/) are specifically designed for parsing log data. They offer a more concise and readable way to define patterns for common log formats.
To mask or remove sensitive information, set up redaction rules. Cribl Edge redacts certain log fields by default, and you can define additional custom redaction rules.
## Configure Destinations to Send Your Data
To specify where Cribl Edge will send your processed data, you’ll want to configure at least one [Destination](/edge/destinations/). This could be Cribl Stream, a centralized log management system, or analytics and monitoring tools.
### Integrate with Cribl Stream
Cribl Stream can help collect, reduce, enrich, transform, and route data from Cribl Edge to any destination. Using a Cribl TCP Source, you can collect and route data from Edge Nodes to Stream Worker Nodes connected to the same Leader, without incurring additional cost.
We offer a use case guide if you’re interested in configuring [Cribl Edge to Cribl Stream](/edge/usecase-edge-stream/).
### Validate Data Collection and Transmission
To validate your configurations and processing logic, use the built-in test and preview features on the Source. 
Verify that the data is reaching the intended Destinations correctly and confirm that the data structure and content meet the requirements of downstream systems.
### Monitor and Optimize
Once you have Sources and Destinations configured and data is flowing, stay on top of your events through monitoring and optimization:
* Regularly check for software updates, and upgrade Cribl Edge to ensure optimal security and performance.
* Monitor the health and performance of your Fleets, Edge Nodes, Sources, and Destinations through the Cribl Edge dashboard or through external monitoring tools, adjusting configurations as necessary.
* Review the efficiency of data processing rules and make improvements to continuously optimize data collection.
By following these steps, you should be able to successfully collect, process, and forward logs, metrics, and application data to the desired destinations, providing timely and relevant data to your organization's monitoring and analysis tools.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}