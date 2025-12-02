# Persistent Queues Support


This page identifies Cribl integrations that do, and don't, support persistent queues.

## Persistent Queues Support by Source Type {#support-src}

The dividing line here is simple:
- Cribl [Push Sources](/stream/sources#push), which ingest streaming data, support PQ. 
- Pull and Collector Sources, which ingest data intermittently and/or from static files, omit PQ support.

## Persistent Queues Support by Destination Type {#support}

Persistent Queues support, behavior, and triggers vary by Destination type, as summarized below.

### HTTP-Based Destinations

HTTP-based Destinations handle backpressure based on HTTP response codes. The following conditions will trigger PQ:

1. Connection failure.
2. HTTP `500` responses.
3. Data overload – sending more data than the Destination will accept. 

HTTP `400` response errors will not engage PQ, and Cribl Stream will simply drop corresponding events. Cribl Stream cannot retry these requests, because they have been flagged as "bad," and would just fail again. If you see `400` errors, these often indicate a need to correct your Destination's configuration.

HTTP-based  Destinations include: 

* Amazon Cloudwatch Logs
* Amazon S3
* Amazon SQS
* Azure Blob Storage
* Azure Monitor Logs
* Cribl HTTP
* CrowdStrike Falcon LogScale
* Datadog
* Data Lake > S3
* Elasticsearch
* Google Chronicle
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

TCP load-balanced Destinations can be configured with one or multiple receivers. If one or more receivers go down, Cribl Stream will continue sending data to any healthy receivers. The following conditions will trigger PQ:

1. Connection errors on **all** receivers.

  > As long as even one of the Destination's receivers is healthy, Cribl Stream will redirect data to that receiver, and will **not** engage PQ.
  {.box .info}

2. Data overload – sending more data than the Destination will accept. 

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

UDP-based Destinations do not support PQ because the protocol is not reliable. Cribl Stream gets no indication whether the receiver received an event.

### Other Destinations

Other Destinations that write to a single receiver (without load balancing enabled) generally engage PQ based on these trigger conditions:

1. Connection errors.
2. Fail to send an event (for any reason).
