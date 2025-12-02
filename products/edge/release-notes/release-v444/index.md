# Cribl Edge 4.4.4


## Corrections

The [Kubernetes Logs Source](sources-kubernetes-logs) no longer emits duplicate events as a result of
container log rotation. CRIBL-21576

We now ensure that data from Windows clients is in fact compressed
before attempting to decompress it. This prevents the error `Tried to interpret
data bytes before a scheme was set` that previously occurred in the [Windows Event Forwarder](sources-wef)
when uncompressed data was incorrectly flagged as compressed. CRIBL-21957

The [Windows Event Forwarder Source](sources-wef) now correctly closes sockets upon receiving malformed requests from Windows clients. This avoids a situation where data flow stopped because the WEF Source had used up its maximum allowable active connections. CRIBL-21965

## Shared New Features

The following new features are shared by Stream and Edge:

### Load Balanced Webhook Destination

We’ve added support for load balancing in the [Webhook
Destination](destinations-webhook#general-settings). You can enable load
balancing and specify multiple webhook URLs and their load weights, which sets
relative traffic-handling capabilities for each connection. You can also choose
whether to exclude all IPs of the current host from the list of any resolved
hostnames.

### Exponential Backoff Retry for HTTP Destinations

In some [HTTP-based Destinations](destinations-backpressure-triggers#http-based-destinations), 
you can now configure retry behavior for HTTP requests to use an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) with
settings for initial retry delay, retry delay multiplier, and maximum retry
delay.

This works both for requests that time out (for example, because of Destination
latency), or fail with a non-200 HTTP response code. You can configure
retry behavior for the [Google Chronicle](destinations-google_chronicle), [Azure
Data Explorer](destinations-azure-data-explorer),
[DataSet](destinations-dataset), and [Elasticsearch](destinations-elastic)
Destinations.

## Shared Corrections

The following corrections are shared by Stream and Edge:

### Sources

- The [OpenTelemetry Source](sources-otel) now correctly listens on the
  configured **Address field** instead of always listening on the default address
  regardless of configuration. CRIBL-21690

- [Splunk TCP Source](sources-splunk) can now properly send captures when the
  **Max S2S** version is set to `v4`. CRIBL-21378

- OpenTelemetry now accurately reports when an HTTP server relinquishes
  listening on a port. Previously, Worker processes were failing to bind to a
  port when you swapped between two Sources that used the same port. CRIBL-21382

### Destinations

- We've added support for the `labels` field in the [Google Chronicle Destination](destinations-google-chronicle),
  so you can see which data was sent through Cribl. CRIBL-20950

- The Google Chronicle Destination now holds subsequent attempts to receive data
  for 30 seconds by default. This helps if you’re running into quota limits and
  errors when sending data to Google Chronicle. CRIBL-21238

- The [DataDog Destination](destinations-datadog) now correctly displays an
  error when an invalid token is added during configuration. CRIBL-21249
