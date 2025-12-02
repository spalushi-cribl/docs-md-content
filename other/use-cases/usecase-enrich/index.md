# Use Cribl to Enrich Data Events with Context


To enrich events, you can leverage the powerful capabilities of [Pipelines](/stream/pipelines/), which serve as a processing layer for incoming data streams. Within a Pipeline, you can add Functions to append additional data or context to events, allowing you to tailor enrichment to your unique needs.

**Enrich data with context:**
* **Lookups:** Use the [Lookup Function](/stream/lookup-function/#lookup) and [operator](/search/lookups/) to add information and metadata to your data. In Cribl Edge, configure lookups to enrich data at the source. In Cribl Stream, set up lookups to further enrich and route data. In Cribl Search, use lookups to add context during analysis. If your data feed changes regularly or is larger than a few million rows, we recommend using [Redis](https://cribl.io/blog/enrichment-better-data-in-for-better-response-times-out/) to host the lookup file.
* **Eval Function:** This Function executes arbitrary expressions to [generate new fields or modify existing ones](/stream/usecase-ingest-time-fields/). It can be used to perform calculations, manipulate strings, or format and transform event data.
* **GeoIP:** Correlate source IPs to a [geographic](/stream/geoip-function/) database, thereby enriching IP address data with their corresponding geographical information like country, city, and lat/long coordinates. This allows you to make informed security decisions. Check out our [GeoIP and Threat Feed Enrichment](https://sandbox.cribl.io/course/enrichment) sandbox.
* **External data sources:** Integrate with a variety of external data repositories, including SQL databases, NoSQL databases, or static CSV files. Use Cribl Edge to perform initial enrichment at the source, reducing the load on central processing. Cribl Stream can further enhance the data as it flows through the Pipeline. Cribl Search can query external APIs or databases to fetch additional context. To learn more about integrating external data sources, see [Database Connections](/stream/database-connections/) and the [Lookups Library](/stream/lookups-library/).
* For more complex enrichments that are not covered by built-in Functions, use the [Code Function](/stream/code-function/), where you can use custom JavaScript code to enrich data as needed. Check out our [enrichment](https://sandbox.cribl.io/coursedocs/uf-to-s3/docs/enrich) sandbox.

> To test and enable data flow through your configured Pipeline, you'll need to provision Workers – see [Managing Cribl.Cloud Worker Groups](/stream/cloud-workers/).
>
{.box .info}

By enriching your data, you establish a more effective data processing workflow that enhances data analysis.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}