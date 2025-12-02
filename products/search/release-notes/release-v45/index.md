# Cribl Search 4.5


Starting with Cribl Search 4.5, you can explore data from a variety of new sources, send
email notifications, create more powerful visualizations, and more.

### Generic HTTP API Dataset Provider

You can now quickly configure Cribl Search to explore data coming from any HTTP API, by
setting up generic HTTP API [data providers](set-up-generic-http-api#provider) and
[Datasets](set-up-generic-http-api#dataset).

### Explore Azure Data Explorer Logs

You can now search
[Azure Data Explorer](https://azure.microsoft.com/en-us/products/data-explorer) logs, by
setting up Azure Data Explorer [data providers](set-up-azure-data-explorer#provider) and
[Datasets](set-up-azure-data-explorer#dataset).

### Explore Cribl Edge Disk Spool Data

You can now search data that has been spooled with Cribl Edge's
[Disk Spool Destination](/edge/destinations-disk-spool). For this, use the new built-in
[`cribl_edge_spool`](edge-datasets) Dataset.

### Send Email Notifications

You can now send email notifications, by creating an
[email notification target](notification-targets#email) and configuring
[notifications](notifications#configure) for a scheduled search.

### Generate Metadata

Although you can point Cribl Search at any source and start searching immediately, now you can also choose to improve search performance by analyzing selected portions of your data beforehand. For this, use the new `.generate metadata`command, along with the two new virtual tables: `$vt_object_list` and `$vt_object_list_summary`. 

>The three resources mentioned above were later deprecated and removed, as of Cribl Search 4.13.1, in favor of the newer [Lakehouses](/lake/lakehouse/) option to speed up searches on designated Datasets.
{.box .info}

### Automatically Reuse Previous Search Results

When writing a query, you can now use a new optional [set](set) statement:
`allow_previous_results`. This allows Cribl Search to
[automatically reuse](search-results#automatically-reuse-search-results) the results of analogous searches run
recently in your Organization, which may reduce costs and improve performance.

### Visualize Results in a Trellis Chart

The [Area](chart-area), [Bar](chart-bar), and [Line](chart-line) charts now support the
Trellis view, which enables you to divide the chart into subsets of data based on fields
in your search.

### Rotate Data, Using the New `pivot` Operator

You can now turn field **values** into field **names**, using the new [`pivot`](pivot)
operator.

### Force a Query to Render Its Results as Events

You can now force your search to render its results as a
[list of events](search-overview#events) rather than a [chart](search-overview#charts).
For that, add [`| render event`](render) at the end of your query.
