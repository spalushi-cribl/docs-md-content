# Cribl Search 4.7.2


Starting with Cribl Search 4.7.2, you can query multiple paths using a single Dataset, embellish your Dashboards with
Markdown, quickly traverse between a search and its related Datasets or Dashboards, and more.

### Multi-Path Datasets

Object store Datasets now support searching multiple paths per bucket or container. This way, you can cover the same
amount of data by using fewer Datasets. The following provider types support this feature:

- [S3](set-up-s3)
- [Azure Blob Storage](set-up-azure-blob)
- [Google Cloud Storage](set-up-google-cloud-storage)
- [Amazon Security Lake](set-up-amazon-security-lake)

### Markdown in Dashboards

[Dashboards](dashboards) can now include [Markdown](editing-dashboards#add-markdown) panels. Here, you can enhance your
Dashboards with text and images to provide explanations, captions, or additional information.

### Snapshots for All API Datasets

We recently introduced [Snapshots](set-up-apis#snapshots) for [Generic HTTP API](set-up-generic-http-api) Datasets. Now,
you can use Snapshots with all the other API Dataset types as well, including:

- [AWS API](set-up-aws)
- [Azure API](set-up-azure-api)
- [Google Cloud Platform API](set-up-google-cloud-platform)
- [Google Workspace API](set-up-google-workspace-api)
- [Microsoft Graph API](set-up-microsoft-graph-api)
- [Okta API](set-up-okta)
- [Tailscale API](set-up-tailscale)
- [Zoom API](set-up-zoom)

### Grafana Plugin

You can now quickly visualize your search results in [Grafana](grafana), using the official
[Cribl Search plugin](https://grafana.com/grafana/plugins/criblcloud-search-datasource/).

### New Set-Statement Option: `job_visibility`

You can now set a search's default visibility right from the query box, using the new [`set`](set)-statement option:
[`job_visibility`](set#job_visibility).

### Interactions for Single-value Charts

[Single-value charts](chart-single) now support [interactions](editing-dashboards#add-interactions), including adding
values to Dashboard inputs and running a new search.

### UI Improvements

UI improvements include:

- Streamlined UI of the search [History](search-overview#history).
- A dedicated button that [loads](common-examples#button) the results of a previous search.
- Two-way links between a search and its related Datasets or Dashboards.

### Prometheus Enhancements

Searching [Prometheus](set-up-prometheus) got easier. Instead of using `metric=...`, pass the metric name alone:

```kusto
// before (no longer supported)
dataset="prometheus_dataset_ID" metric=node_cpu_seconds_total

// now
dataset="prometheus_dataset_ID" node_cpu_seconds_total
```

You can also search with wildcards, the Prometheus `__name__` field, and more. See the examples:
[Search a Prometheus Dataset](set-up-prometheus#search-a-prometheus-dataset).
