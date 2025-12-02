# Use Cribl Products to Reduce the Size of Data for Cost Savings and Performance


The Cribl Product Suite can help you reduce the size of your data and optimize
performance by applying any or all of the following strategies:

## Route, Filter, and More in Cribl Stream

* **Routing:** Configure [routing](/stream/routes/) within [Cribl
  Stream](/stream/) to dynamically send data to the most suitable destinations
  based on content, type, or other data characteristics, optimizing for cost and
  performance.
* **Filtering:** Apply [filters](/stream/filter-and-transform-data) to exclude
  irrelevant or low-value data from being ingested into expensive storage or
  analytics systems, reducing resource use and improving the signal-to-noise
  ratio.
* **Reducing fields:** Use the [Eval](/stream/eval-function/) Function to strip
  out fields that aren't necessary for your analysis, which can drastically
  reduce the incoming data size.
* **Sampling:** Employ the [Sampling](/stream/sampling-function/) Function to
  process only a fraction of the incoming data streams, based on your specified
  sampling rate, to lower the volume while still getting a representative
  dataset.
* **Aggregation:** Use the [Aggregations](/stream/aggregations-function/)
  Function to summarize or roll up data, such as by counting occurrences rather
  than logging each event, thereby reducing the number of events transmitted and
  stored.
* **Drop:** Leverage the [Drop](/stream/drop-function/) Function to remove
  irrelevant events selected with precise filters and reduce the overall volume
  of data.

## Preprocess at the Source in Cribl Edge

With [Cribl Edge](/edge/), you can parse, structure, and
clean your data at the edge, where it originates. This allows for significant
volume reduction before it's ever transferred across the network, reducing
transmission and storage loads.

## Define Custom Datasets, Compress Data, and More in Cribl Lake

* **Custom Datasets:** Organize data in [Cribl Lake](/lake/datasets#create-a-new-dataset) using special-purpose Datasets, focusing on retaining valuable data and discarding what's unnecessary.
* **Data retention:** Set [data retention policies](/lake/datasets#create-a-new-dataset/) when you create a Dataset that automatically expires and deletes data that is beyond a certain age, thus avoiding excess storage consumption.
* **Compression:** Store data in [gzip-compressed formats](/lake/datasets/) to minimize storage space requirements, enabling cost savings.
* **Data format:** Store and interact with data in open, non-proprietary formats
  like JSON and Parquet that don't incur extra processing overhead or locked-in
  costs of proprietary formats.

## Conduct Precise Queries and Optimize Analysis in Cribl Search

* **Targeted searching:** Conduct precise queries with [Cribl Search](/search/)
  against the Datasets in Cribl Lake to extract just the needed information,
  reducing the time and computation power required for data sifting.
* **Optimized analysis:** When you use pinpointed searches, you analyze only the
  subset of data that's pertinent to your query, which conserves analysis
  resources and promotes faster insights.

Here’s a sample combined workflow that incorporates strategies from Cribl
Stream, Cribl Edge, and Cribl Lake, along with targeted searching using Cribl
Search:

1. **Pre-process data with Cribl Edge.**
    * At the very edge, where data originates, configure Cribl Edge Nodes to parse and structure the incoming raw data.
    * Use Cribl Edge to perform initial data transformations such as removing
      unnecessary fields and applying filters to reduce the volume and enhance
      the quality of data before Cribl Edge transmits it.
2. **Efficiently transport and process data with Cribl Stream.**
    * Ingest the preprocessed edge data into Cribl Stream for further refinement
      and optimization.
    * Apply the **Sampling** Function to process only a subset of the data,
      maintaining a representative sample while lowering the volume.
    * Employ the **Aggregations** Function to summarize data when possible,
      rather than storing each individual event.
    * Remove irrelevant events by using the **Drop** Function.
3. **Store optimized data with Cribl Lake.**
    * Send the optimized data from Cribl Stream to designated Datasets in Cribl
      Lake, focusing on valuable information and discarding the rest.
    * Configure data retention policies within Cribl Lake to automatically purge
      data that exceeds a certain age.
    * Store the data in Cribl Lake using gzip-compressed JSON or Parquet format
      to maximize storage efficiency and cost savings.
4. **Perform targeted searching and analysis with Cribl Search.**
    * Utilize Cribl Search for precise and targeted queries against the
      structured and compressed Datasets stored in Cribl Lake.
    * Leverage targeted searches to provide more efficient and faster insights
      by analyzing only the specific data pertinent to your queries, rather than
      the entire stored collection.

When you apply these strategies while working with the Cribl product suite, data
management becomes more efficient and cost-effective. You achieve leaner data
flows, reduced storage requirements, and quicker data retrieval, all of which
contribute to a faster and more economical data pipeline.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}