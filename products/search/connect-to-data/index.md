# Connect Cribl Search to Your Data


Link Cribl Search to your data by setting up Dataset Providers and Datasets.

---

Cribl Search offers native support for a [wide array of data sources](#list). To start searching any of them, set up a
Dataset Provider and a Dataset. For that, you need to be a Cribl Search **Editor** or **Admin**.

## Add a Dataset Provider {#provider}

A [Dataset Provider](basic-concepts#dataset-providers) tells Cribl Search where to query and contains access
credentials. To start adding a new one, go to **Data** > **Dataset Providers** > **Add Provider**.

For detailed instructions on how to set up each Dataset Provider type, follow the relevant link from the
[list of supported data sources](#list).

## Add a Dataset {#dataset}

A [Dataset](basic-concepts#datasets) tells Cribl Search what data to search from the Dataset Provider. To start adding a
new one, go to **Data** > **Datasets** > **Add Dataset**.

For detailed instructions on how to set up each Dataset type, follow the relevant link from the
[list of supported data sources](#list).

## List of Supported Data Sources {#list}

Cribl Search offers native support for the data sources listed below. Follow the links to learn how to set up each one:

- [Amazon S3](set-up-s3)
- [Amazon Security Lake](set-up-amazon-security-lake)
- [AWS API](set-up-aws)
- [Azure API](set-up-azure-api)
- [Azure Blob Storage](set-up-azure-blob)
- [Azure Data Explorer](set-up-azure-data-explorer)
- [ClickHouse](set-up-clickhouse)
- [Cribl Edge](set-up-edge)
- [Cribl Lake](set-up-cribl-lake) / [Lakehouses](set-up-cribl-lake#search-lakehouse)
- [Cribl Stream's "Data Lake Amazon S3" Destination](set-up-data-lake-amazon-s3)
- [Elasticsearch](set-up-elasticsearch)
- [Google Cloud Platform API](set-up-google-cloud-platform)
- [Google Cloud Storage](set-up-google-cloud-storage)
- [Google Workspace API](set-up-google-workspace-api)
- [Generic HTTP API](set-up-generic-http-api)
- [Microsoft Graph API](set-up-microsoft-graph-api)
- [Okta API](set-up-okta)
- [OpenSearch](set-up-opensearch)
- [Prometheus](set-up-prometheus)
- [Snowflake](set-up-snowflake)
- [Tailscale API](set-up-tailscale)
- [Zoom API](set-up-zoom)

If there's no native support for your data source type, there still might be ways to connect to your data. You can set
up a [Generic HTTP API](set-up-generic-http-api) Dataset, use the [`externaldata`](externaldata) operator, or reach out
to [Cribl Support](/support/) for finding a solution.

### Connect Cribl Search to Other Cribl Products

Cribl Search seamlessly integrates with [Cribl Stream](/stream/), [Cribl Edge](/edge/), and [Cribl Lake](/lake/) (with
or without [Lakehouses](/lake/lakehouse)), as they're all part of your [Cribl.Cloud](cloud) Organization. In most cases,
you can start searching your data right away. For more information, see the pages listed below.

Cribl Edge:

- [Connect to Cribl Edge](set-up-edge)
- [Built-In Cribl Edge Datasets](edge-datasets)
- [Searching Cribl Edge Data](search-edge)

Cribl Lake:

- [Connect to Cribl Lake](set-up-cribl-lake)
- [Searching Cribl Lake](search-cribl-lake)
- [Search a Lakehouse](set-up-cribl-lake#search-lakehouse)
