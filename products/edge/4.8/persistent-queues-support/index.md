# Persistent Queues Support


This page identifies Cribl integrations that do, and don't, support persistent queues.

## Persistent Queues Support by Source Type {#support-src}

The dividing line here is simple:
- Cribl [Push Sources](/stream/sources#push), which ingest streaming data, support PQ. 
- Pull and Collector Sources, which ingest data intermittently and/or from static files, omit PQ support.

## Persistent Queues Support by Destination Type {#support}

Persistent Queues support, behavior, and triggers vary by Destination type, as summarized below.

### HTTP-Based Destinations

HTTP-based Destinations handle backpressure based on HTTP response codes. Backpressure alone is not sufficient to trigger Destination PQ. That is by design, because if the Destination is unable to send data as fast as it should, it is desirable for the backpressure to propagate upstream to the source of the data. Engaging PQ would interfere with that desired result.

The following conditions will trigger PQ:

1. Connection failure.
2. Retryable HTTP requests failing repeatedly – for example, those with `5xx` responses.

As a general rule, HTTP `4xx` response errors will not engage PQ, and Cribl Edge will simply drop corresponding events. Cribl Edge cannot retry these requests, because they have been flagged as "bad," and would just fail again. If you see `4xx` errors, these often indicate a need to correct your Destination's configuration.

This rule has an exception, though: `429 Too Many Requests` responses mean that the client is simply sending data too fast and the data should be resent at a later time rather than dropped. Therefore, depending on the downstream receiver, you may wish to configure Cribl Edge to retry events that come back with a `429` response code.

HTTP-based Destinations include: 

* Amazon Cloudwatch Logs
* Amazon SQS
* Amazon Kinesis Data Streams
* Azure Data Explorer (Streaming mode)
* Azure Monitor Logs
* Microsoft Sentinel
* Cribl Lake
* Cribl HTTP
* CrowdStrike Falcon LogScale
* Datadog
* Elasticsearch
* Elastic Cloud
* Google Chronicle
* Google Logging
* Google Pub/Sub
* Grafana Cloud
* Honeycomb
* InfluxDB
* Loki (Logs)
* New Relic Ingest: Logs & Metrics
* New Relic Ingest: Events
* OpenTelemetry
* Prometheus
* SentinelOne DataSet
* SignalFx
* Splunk HEC
* Sumo Logic
* WaveFront
* Webhook

### TCP Load-Balanced Destinations

TCP load-balanced Destinations can be configured with one or multiple receivers. If one or more receivers go down, Cribl Edge will continue sending data to any healthy receivers. Source PQ is designed to help buffer for occasional backpressure; unlike Destination PQ, its role is not to propagate backpressure upstream.

The condition that will trigger PQ is connection errors on **all** receivers – typically because sockets close or become unavailable.

> As long as even one of the Destination's receivers is healthy, Cribl Edge will redirect data to that receiver, 
> and will **not** engage PQ.
{.box .info}

TCP load-balanced Destinations include: 

* Splunk Load Balanced
* TCP JSON
* Syslog 

### Destinations Without PQ Support

Filesystem-based Destinations, and Destinations that use the UDP protocol, do not support PQ. These include:

* Amazon S3 Compatible Stores
* Data Lakes > Amazon S3
* Data Lakes > Amazon Security Lake
* Azure Blob Storage 
* Azure Data Explorer when in batching mode
* Filesystem/NFS
* Google Cloud Storage
* MinIO
* SNMP Trap
* StatsD (with UDP)
* StatsD Extended (with UDP)
* Syslog (with UDP)

Filesystem-based Destinations do not support PQ because they already persist events to disk, before sending them to their final destinations.

UDP-based Destinations do not support PQ because the protocol is not reliable. Cribl Edge gets no indication whether the receiver received an event.

### Other Destinations

Other Destinations that write to a single receiver (without load balancing enabled) generally engage PQ based on these trigger conditions:

1. Connection errors.
2. Fail to send an event (for any reason).
