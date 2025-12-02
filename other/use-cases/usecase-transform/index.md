# Use Cribl Products to Change the Shape, Size, or Quality of Data


Cribl provides powerful data transformation capabilities, allowing you to accommodate analysis tools or convert data types as needed. Transforming data usually involves altering the structure, format, and content of the data to fit specific requirements. You can transform data at [any stage](/stream/event-processing-order/) where Cribl is involved. Here, we’re focusing on processing data through a [Pipeline](/stream/pipelines/).
Use Functions to manipulate data, such as filtering, enriching, extracting, and aggregating. Here are just a few of the Functions you can use to transform data:
* **Regex Extract:** Extracts a portion of the raw event, and places it into a specified field. It’s useful for reorganizing and capturing data that matches a regular expression pattern. See [regex filtering](/stream/usecase-regex-filtering/) and [lookups with regex](/stream/usecase-lookups-regex/).
* **Lookup:** Enriches event data by adding fields like hostname, name, id, and type using a key such as the host IP. It is typically used to add context or additional information to events from external databases or tables. For details, refer to the [Lookup Function](/stream/lookup-function/#lookup) and [Search Lookups](/search/lookups/).  If your data feed changes regularly or is larger than a few million rows, we recommend using [Redis](/stream/redis-function/) to host the lookup file.
* **Eval:** Executes arbitrary expressions to [generate new fields or modify existing ones](/stream/usecase-ingest-time-fields/). This can be used to perform calculations, manipulate strings, or format and transform event data. 
* **GeoIP:** Correlates source IPs to a [geographic](/stream/geoip-function/) database, thereby enriching IP address data with their corresponding geographical information like country, city, and lat/long coordinates. Check out our [GeoIP and Threat Feed Enrichment sandbox](https://sandbox.cribl.io/course/enrichment).
* **Aggregation:** Use the [Aggregations Function](/stream/aggregations-function/) to group multiple records into summarized data, which is beneficial for presenting high-level trends without the overhead of every single transactional detail. Cribl Search provides many [aggregation functions](/search/search-overview/#aggregate).
* **Sampling:** Processes a [percentage](/stream/usecase-sampling/) of incoming events. This is particularly useful for maintaining a manageable Dataset for analyses in environments with a vast data inflow.
* For more complex transformations that are not covered by built-in functions, use the [Code Function](/stream/code-function/), where custom JavaScript code can be written to manipulate and cast data as needed.
  
> Note that to test and enable data flow through your configured Pipeline, you'll need to provision Workers – see [Managing Cribl.Cloud Worker Groups](/stream/cloud-workers/).
>
{.box .info}

Check out this [conceptual video](https://vimeo.com/466972075) on transforming data. 
By using these powerful features, you can ensure that vast amounts of machine-generated data are effectively utilized and fit for varied analytical needs.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}