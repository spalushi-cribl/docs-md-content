# Cribl Stream 4.4.4


## New Features

Rules are made to be broken, so this maintenance release contains the following
new features:

### FIPS Mode (Beta)

Cribl Stream can now run in FIPS (Federal Information Processing Standards)
mode. When running in FIPS mode, Cribl Stream will use only FIPS compliant
cryptography algorithms, and will require FIPS compliant passwords for user
login. See the [FIPS Mode](fips-mode) topic for details. In Cribl Stream version
4.4.4, FIPS mode is a beta feature available in Customer Managed deployments.

### Load Balanced Webhook Destination

We’ve added support for load balancing in the [Webhook Destination](destinations-webhook#general-settings).
You can enable load balancing and specify multiple webhook URLs and their load
weights, which sets relative traffic-handling capabilities for each connection.
You can also choose whether to exclude all IPs of the current host from the list
of any resolved hostnames.

### Exponential Backoff Retry for HTTP Destinations

In some [HTTP-based Destinations](destinations-backpressure-triggers#http-based-destinations),
you can now configure retry behavior for HTTP requests to use an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff),
with settings for initial retry delay, retry delay multiplier, and maximum retry
delay.

This works both for requests that time out (for example, because of Destination
latency), or fail with a non-200 HTTP response code. You can configure
retry behavior for the [Google Chronicle](destinations-google_chronicle), [Azure
Data Explorer](destinations-azure-data-explorer),
[Dataset](destinations-dataset), and [Elasticsearch](destinations-elastic)
Destinations.

### More Features

We added an auto-refresh toggle on the **Manage > Workers** page for lists with
fewer than 100 Workers. When disabled, a timestamp indicates the last time the
Workers list was updated. For more details, see [Workers Tab](deploy-distributed#workers-tab) docs. CRIBL-15651

Our [Google Chronicle Destination](destinations-google_chronicle#google-chronicle) now supports more regional endpoints: Dammam,
Europe Multi-Region, Frankfurt, London, Singapore, Sydney, Tel Aviv, United
States Multi-Region, and Zurich. CRIBL-21391

In Cribl.Cloud, you can now configure a [**Session idle time limit**](authentication#auth-controls)
for the GUI. This helps keep pages up longer – for data monitoring, for example.
To configure this option, go to **Settings > Global Settings > General Settings**.
CRIBL-20753

## Corrections

This release includes the following fixes:

### Sources

- The [OpenTelemetry Source](sources-otel) now correctly listens on the
  configured **Address field** instead of always listening on the default address
  regardless of configuration. CRIBL-21690

- OpenTelemetry now accurately reports when an HTTP server relinquishes
  listening on a port. Previously, Worker processes were failing to bind to a
  port when you swapped between two Sources that used the same port. CRIBL-21382

- [Splunk TCP Source](sources-splunk) can now properly send captures when the
  **Max S2S version** is set to `v4`. CRIBL-21378

- Cribl Stream now ensures that data from Windows clients is actually compressed
  before attempting decompression. This prevents Stream from displaying the
  error `Tried to interpret data bytes before a scheme was set` that previously
  occurred in the [Windows Event Forwarder](sources-wef) when uncompressed data
  was incorrectly flagged as compressed. CRIBL-21957

- The [Windows Event Forwarder Source](sources-wef) now correctly closes sockets upon
  receiving malformed requests from Windows clients. This avoids a situation where data flow stopped because the WEF Source had used up its maximum allowable active connections. CRIBL-21965

### Collectors

- When creating an [S3](collectors-s3), [Google Cloud Storage](collectors-google-cloud-storage),
  or [Azure Blob Collector](collectors-azure-blob), a warning will appear when
  both the earliest and latest fields are left blank. This helps prevent the
  Collector from excessively collecting data. CRIBL-17215

- The [REST Collector](collectors-rest) now adds the `User-Agent` header to HTTP
  requests by default, and should now be able to successfully access the
  Proofpoint API. CRIBL-21571

- When previewing REST Collector data, the source field is no
  longer incorrectly set on results when the Discover type is not an HTTP
  request. CRIBL-21833

- The REST Collector now supports JWT and text/plain authentication responses via
  the **Login** and **Login (credentials secret)** authentication method. CRIBL-21699

- When configuring the [Database Collector](collectors-database), you can now
  expand the SQL Query window to show the whole query, even if it’s quite long.
  CRIBL-21707

- The [Database Collector](collectors-database) now allows you to toggle strict
  query validation on and off. Proceed with caution, because this means that you
  can execute potentially destructive queries. CRIBL-21734

### Destinations

- The [Google Chronicle Destination](destinations-google_chronicle) now supports
  the [Chronicle Ingestion API](https://cloud.google.com/chronicle/docs/reference/ingestion-api)'s
  `labels` field. This enables you to see which data was sent through Cribl. CRIBL-20950

- The [Google Chronicle Destination](destinations-google_chronicle) now holds
  subsequent attempts to receive data for 30 seconds by default. This helps if
  you’re running into quota limits and errors when sending data to Google
  Chronicle. CRIBL-21238

- The [DataDog Destination](destinations-datadog) now correctly displays an
  error when an invalid token is added during configuration. CRIBL-21249

- For Worker Nodes in Cribl.Cloud with [Azure Data Explorer (ADX) Destinations](destinations-azure-data-explorer),
  the **Persistent Queue Settings** now no longer incorrectly exposes on-prem PQ
  settings. CRIBL-21335

- The [Elasticsearch Destination](destinations-elastic) no longer uses an
  authentication token when you toggle **Authentication enabled** off. CRIBL-21129

### Functions

- The use of a wildcard in the **Remove Fields** config of an Eval after the [Redis
  function](redis-function) no longer causes data loss in the deployed [Pipeline](pipelines). CRIBL-21355

### Packs

- Events are no longer dropped from Routes when a Pack contains the Suppress
  function and is used in a non-final Route. CRIBL-21577

### Roles

- Monitoring, Mappings, and Recent Actions sections are visible again on the
  Home page for all users with the **reader_all** Role. CRIBL-20994

- Users with the **admin** Role can once again access Mappings. CRIBL-21269

### Other Functional Fixes

- Live data capture is now available in Gitops read-only mode. CRIBL-21782

- Several [`C.Mask`](expressions-mask#cmask--data-masking-methods) functions (`md5`, `sha1`, `sha256`, and `sha512`) can now
  process input given as a hex string representing bytes, instead of always
  interpreting input as a UTF-8 string. To make use of this feature, set the new
  encoding optional parameter). The `C.Encode.base64` function also accepts this
  optional parameter. CRIBL-21240

- Logging in to Cribl using [LDAP](authentication#ldap) (like AzureAD) configured with uid in the
  Username field no longer displays the error `Failed to read current user`.
  CRIBL-21716

- Clicking **Export** from the [**Manage as JSON**](sources-system-metrics#using-advanced-mode) view for Sources,
  Destinations, and Collectors will redact sensitive information from the config
  before saving. CRIBL-15632

- [Persistent Queues](persistent-queues) (PQ) now drain when it is appropriate
  to do so. CRIBL-21298

- Cribl Stream was displaying an error when a user with the **stream_editor**
  role created a new Project. CRIBL-20167