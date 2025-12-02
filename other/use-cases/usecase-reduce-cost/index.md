# Use Cribl Products to Reduce Data Storage Costs


Reducing the cost of your organization's data storage costs, such as the cost of your SIEM (Security Information and Event Management) license, can be a significant priority, especially if your storage solution uses a pricing model based on the volume of data ingested. Here are some strategies using Cribl products to help manage and potentially reduce these costs.

* **Reduce data volume with  [Cribl Stream](/stream/):**

    * **Routing:** Set up [routing](/stream/routes/) within Cribl Stream to direct data to alternative destinations and systems that may be more cost effective for certain types of data processing and long-term storage.
    * **Field reduction:** Use Cribl Stream's [Functions](/stream/functions/) such as [Drop](/stream/drop-function/) or [Eval](/stream/eval-function/) to exclude unnecessary fields from your data before sending it to storage, decreasing the overall data volume.
    * **Sampling:** Apply the [Sampling](/stream/sampling-function/) Function to send only a representative subset of events, significantly reducing the volume while retaining the ability to detect anomalies and trends.
    * **Aggregation:** Use the [Aggregations](/stream/aggregations-function/) Function to summarize data, sending computed results instead of raw logs, which leads to fewer events being ingested into data storage.

* **Preprocess edge data with [Cribl Edge](/edge/):**

    * **Local processing:** Deploy Cribl Edge to parse, structure, and clean data at the source, allowing for significant volume reduction before the data is ever transferred to your central SIEM system, thereby saving on volume-based licensing costs.
    * **Field extraction and transformation:** Use built-in capabilities in Cribl Edge to perform real-time processing and field extraction, sending only the most relevant and valuable information to your data storage for analysis.

* **Optimize data storage with [Cribl Lake](/lake/):**

    * **Data retention policies:** Configure [retention periods](/lake/#retention-periods/) in Cribl Lake to automatically expire and delete data that is beyond a certain age or out of compliance scope, preventing unnecessary storage and processing in the SIEM.
    * **Compression:** Benefit from the gzip-compressed JSON format used in Cribl Lake, reducing storage footprint and data transfer requirements, thus indirectly lowering costs related to SIEM data handling.

These strategies give you more control over the volume and quality of data being sent to your SIEM, which directly impacts license costs. Always consider which data is mission-critical for security analysis in your SIEM, and use Cribl solutions to ensure that is the data you are paying to analyze.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}