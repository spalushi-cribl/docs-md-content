# About Lookups


Lookup files are reference tables that you can apply to raw data events in your Pipeline. Typically, you use lookup files to enrich data by adding new fields or replacing the value of an existing field using the values contained in the lookup file.

There are many common use cases for lookup files, including:

- **Add fields to control how data is routed or filtered:** Use values from your raw events to look up related metadata (such as priority level, geographic region, or risk classification) and add it to the event as a new field. You can use these enriched fields later in your Pipeline to filter events, route them for additional processing, or send them to specific downstream Destinations. For example, you could enrich logs from `service_id="auth-prod"` with `priority="high"` and route them to a dedicated alerting Pipeline, while routing `priority="low"` events to long-term storage.
- **Make data more useful by human-readable business or operational fields:** Use lookup files to translate machine-readable values (such as user IDs, product SKUs, or error codes) into human-readable or business-relevant fields. This helps make logs and metrics easier to interpret and act on. For example, instead of just storing `product_id=8924`, you could enrich the event with additional fields: `product_name="Honeycrisp Apple"`, `product_category="Produce"`, and `region="Northwest"`.
- **Normalize inconsistent values across different sources:** Use lookup files to map inconsistent or vendor-specific field values (such as region codes, platform names, or status labels) to a standardized, canonical format. This helps ensure reliable aggregation, filtering, and reporting across diverse data sources. For example, you could map `region="US_WEST"`, `region="uswest"`, and `region="us-west-1"` to the same, consistent data format: `region="us-west"`.
- **Enrich events with time-based values:** Use lookup files that include time windows to enrich events with the correct value based on when the event occurred. This is useful for applying dynamic metadata such as pricing tiers, active campaigns, or software versions that change over time. For example, you could enrich a transaction event with the correct `discount_percentage` value by looking up the active promotion based on the `transaction_time` field.
- **Add geographic context to events using IP addresses:** Use lookup files or binary IP address databases to convert raw IP addresses in your event data into geographic fields such as country, region, city, or autonomous system. This added context makes it easier for downstream receivers to detect unusual activity, group traffic by region, or apply compliance policies. For example, you could enrich an event containing `client_ip="192.0.2.0"` with additional fields like `geo_country="US"`, `geo_city="Chicago"`, and `geo_org="ExampleNet"`. You could then route traffic from unexpected regions to a separate review path or generate alerts for geo anomalies. See [Lookup Geographic Data from IP Addresses](usecase-lookups-ip-addresses) and the [GeoIP Function](geoip-function) documentation for information about this specific use case.

## Overview of the Workflow for Using Lookups

To use lookups in your Cribl Stream deployment:

1. [Plan your lookup strategy](#plan): Begin by making strategic decisions about how and where to use lookups in your deployment. This includes evaluating whether ingest-time or search-time lookup files are appropriate for your use case. You also need to estimate hardware needs and consider strategies for optimizing your system for better performance. The rest of this page will guide you through the planning process.

1. [Upload a lookup file](lookups-configure#upload-lookups): Add your lookup files to either the global [Knowledge Library](lookups-library) or to a specific [Pack](packs), depending on your use case.

1. [Add indexes to the lookup file](lookups-configure#add-indexes): If you're using disk-based lookups, index the fields you'll use for lookups. Cribl Stream supports a maximum of four indexes per file.

1. [Reference the lookup file in a Pipeline or Route](lookups-configure#reference): Use the Lookup Function or the `C.Lookup()` expression to enrich, route, or filter data based on lookup results.

1. [Update lookup files as needed](lookups-configure#update-lookups): You can update lookup files manually through the UI or programmatically using the API, which is the ideal method for automated or frequent lookup file updates.

## Plan Your Lookup Strategy {#plan}

Before adding lookup files to your deployment, it’s important to define a strategy that aligns with your performance goals, data flow design, and operational needs. This includes:

- Deciding when to enrich your data, such as at ingest-time or search-time.
- Choosing your lookup storage mode based on the size of your files and system performance concerns.
- Preparing your lookup files for optimal performance.
- Understanding system resource requirements.
- Ensuring your implementation scales reliably over time.

### Decide When to Enrich Data

One important consideration when using lookups in Cribl Stream is *when* to apply them to your data. The timing of enrichment can affect system performance, data freshness, and data consistency.

Cribl Stream performs lookup enrichment at ingest-time or transform-time, depending on where you use lookups in your Pipeline. This means Cribl Stream applies lookup values either at the moment data first enters the system or during a specific data processing stage. Once enriched, the new fields become part of the event and you can then use those fields for routing, filtering, or forwarding to downstream Destinations. Because this enrichment happens early, the enriched values remain consistent and available throughout the rest of your Pipeline.

Whether to enrich data at ingest-time or search-time depends on your broader data processing goals. As a general rule, use ingest-time lookups for stable reference data, and use search-time lookups for dynamic of frequently changing values.

- Choose ingest-time enrichment when the context is fixed and should reflect the moment in time when the event was processed.
- Choose search-time when the context is time-sensitive and needs to reflect the most current data at the time you execute the query.

The following table summarizes the tradeoffs:

| Time | Description | Advantages | Disadvantages |
| ---- | ----------- | ---------- | ------------- |
| Ingest-time or transform-time | Enrichment happens as the data is first received or as part of a processing stage. | <li><strong>Fast query performance:</strong> Since the data is pre-enriched, downstream systems don't redo lookups.</li><li><strong>Immutable enrichment:</strong> Enriched values are part of the stored record.</li><li><strong>Consistency:</strong> All downstream consumers use the same enrichment snapshot.</li> | <li><strong>Stale enrichment:</strong> If the lookup source changes, the already-enriched events won't update.</li><li><strong>Higher ingest latency:</strong> Lookups add work during processing, potentially impacting system performance.</li><li><strong>Larger storage footprint:</strong> Enriched fields are stored with each event.</li><li><strong>Harder to fix mistakes:</strong> Errors in enrichment logic persist with the data.</li> |
| Search-time or query-time | Enrichment happens only when the system queries or analyzes the data. | <li><strong>Always fresh:</strong> The data reflects lookup changes immediately.</li><li><strong>Lower ingest cost:</strong> Faster ingestion and smaller payloads.</li><li><strong>Easier to improve:</strong> Fix enrichment logic without reprocessing past data.</li> | <li><strong>Slower query performance:</strong> Lookups happen at query time, potentially impacting search performance.</li><li><strong>Inconsistent results:</strong> Results may vary over time as lookup data changes.</li><li><strong>More complex logic:</strong> You must apply enrichment logic during each query.</li> |

Because Cribl Stream applies lookup files during ingest or transformation, the enriched values become part of the event. These values won't reflect changes to the lookup file after data processing. This behavior ensures fast and consistent downstream behavior, but it also means you should carefully manage your lookup files and validate their accuracy before deploying it to production.

> If you need enrichment to reflect current values at query time, consider using a downstream tool such as Cribl Search. Cribl Search supports search-time lookups against global lookup files in the Cribl Search Knowledge Library.
>
> Cribl Search can also dynamically update lookup tables based on search results, which is useful for scenarios where enrichment conditions change after the event occurred or after initial processing. See [Cribl Search Lookups](/search/lookups/) for more information.
>
{.box .info}

### Choose a Lookup File Storage Mode {#storage-mode}

When uploading lookup files, Cribl Stream gives you two storage mode options:

- **Store in Memory (in-memory lookup):** The lookup file is stored in memory and packaged with your deployment. This option offers fast lookup access and is ideal for small lookup files that can be stored entirely in memory.
- **Store on Disk (disk-based lookup):** The lookup file is streamed to the Worker separately and accessed from disk at runtime. This option is suitable for larger files and more flexible deployments.

You can select storage modes on a file-by-file basis, and it's safe to mix both approaches in the same deployment.

If you are unsure which method to use, use in-memory lookups (Store in Memory) for better performance. However, there are times when it's better to use disk-based lookup storage (Store on Disk).

Use disk-based lookups when:

- Your lookup file is large (generally 25 MB or more) and when storing completely in memory is not feasible.
- You are deploying lookups to a large number of Worker Processes, even if the lookup file size is small.
- You want to scale CPU cores independently from memory and reduce overall memory allocation per Worker.
- You manage lookup files using an external system outside of Cribl, and you want to migrate to an internal Cribl-based solution to reduce maintenance overhead.

Disk-based lookups have some limitations:

- **No in-place editing:** To update the lookup file, you must download, delete, and re-upload it.
- **Exact Match only:** When referencing disk-based lookups in Pipelines or Routes, the lookups are limited to Exact Match Mode only. They do not have CIDR or Regex support.
- **No Reload Period support:** The **Reload Period** setting in the [Lookup Function](lookup-function) is not supported for disk-based files.
- **Lookup file type restrictions:** Cribl Stream requires `.csv` format for disk-based lookup files. Use in-memory lookups instead if you need to use a different lookup file type, such as an external Redis database or binary IP address databases from either [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/) (GeoLite2 or GeoIP2 `.mmdb` files) or [IPinfo](https://ipinfo.io/products/ip-database-download).

> You can convert in-memory lookups to disk-based lookups, but not vice versa. Choose carefully before migrating, as the conversion is permanent.
>
{.box .info}

### Optimize Your Lookup Files {#optimize-lookup-files}

To improve system performance and minimize processing overhead, follow these general best practices when creating lookup files:

- **Keep file sizes minimal:** Include only the fields necessary for data processing. Avoid storing extra metadata or unused columns.
- **Place your primary key in the first column:** Cribl Stream automatically indexes the left-most column by default. Organizing your primary lookup key in this position improves default performance and avoids the need for additional configuration. If your most important lookup field isn’t in the left-most column, you'll miss out on automatic indexing and may experience slower performance until indexes are manually configured.
- **Avoid indexing low-cardinality fields:** Avoid indexing fields with very few unique values. These indexes offer little performance benefit and may increase overhead.

For disk-based lookups:

- **Index all fields you need to reference:** Cribl Stream requires you to index the field you use in the [Lookup Function](lookup-function). If you reference a field that you have not indexed, the lookup will fail. For disk-based lookup files, you can define a maximum of four indexes to accelerate lookups on frequently queried fields. Choose fields that are used in routing, filtering, or enrichment logic to maximize performance benefits.
- **Avoid over-indexing:** While Cribl Stream supports a maximum of four indexes, indexing too many fields can increase memory usage and impact performance. Only index fields that are frequently queried or matched against.

### Plan Your File Update Strategy

If you expect lookup values to change regularly over time, develop a clear strategy for keeping your lookup files accurate and up-to-date. An effective update strategy helps ensure consistent enrichment and prevents stale or incorrect data from propagating through your Pipelines. See [Update a Lookup File](lookups-configure#update-lookups) for more information.

Consider using these strategies:

- Automate updates using the API if lookup files change frequently or if another system maintains the files.
- Version your files to track changes and allow rollbacks if needed.
- Validate file content before uploading to production to avoid enrichment errors.
- Coordinate updates across environments (such as staging and production) to ensure consistent behavior.

## Supported System and File Requirements

This section outlines the supported file types, size limits, and hardware recommendations for using lookup files in Cribl Stream.

### Supported Lookup File Types {#supported-lookup-file-types}

Cribl Stream supports and recommends using lookup files in `.csv` format for compatibility, performance, and ease of management. Disk-based lookups require `.csv` format.

Additional supported formats for in-memory lookups only:

- External Redis databases, which are supported using the [Redis](redis-function) Function.
- Binary IP address databases from either [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/) (GeoLite2 or GeoIP2 `.mmdb` files) or [IPinfo](https://ipinfo.io/products/ip-database-download), which are supported using the [GeoIP Function](geoip-function). See [Lookup Geographic Data from IP Addresses](usecase-lookups-ip-addresses) for information about this specific use case.

### Supported Lookup File Sizes

Cribl Stream supports:

- **Maximum file size (per file):** 500 MB
- **Total storage limit (across all files):** Up to 2 GB (including both in-memory and disk-based lookup files)

Cribl.Cloud enforces these limits and does not allow changes. In hybrid and self-managed deployments, you can modify the default limits. However, increasing the limits is not officially supported, so make sure your deployment can safely handle this change before doing so.

> For best performance and reliability, keep individual lookup files as small and focused as possible. Use multiple smaller files instead of one large one when feasible.
>
{.box .info}

### Hardware Recommendations

To optimize performance, ensure that you store disk-based lookups on high-performance storage. To avoid performance degradation, store disk-based lookup files on high-performance storage volumes.

Avoid:

- Network file systems (NFS)
- Non-solid state drives (HDDs)
- Any low IOPS environments

Use:

- Local SSD storage or similarly fast media for optimal throughput and lookup performance

In addition to storage needs, in-memory large lookup files can also require additional RAM on each Worker Node that processes the lookups. See [Memory Sizing for Large Lookups](lookups-library#sizing) for more information.
