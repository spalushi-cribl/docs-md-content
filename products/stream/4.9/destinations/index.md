# Destinations


Cribl Stream can send transformed data to various Destinations, including Cribl HTTP, Cribl TCP, Cribl Lake, Elasticsearch, Amazon Kinesis, Amazon S3 and other object stores, Prometheus and compatible services, InfluxDB, Splunk, Snowflake, Databricks, TCP JSON, and many others. 

Destinations can write data to either IPv4 or IPv6 addresses.

Destination are grouped into [categories](#destination-categories)
that define how they handle unreachable outputs (backpressure events)
and their load-balancing capabilities.

![Diagram illustrating data flowing from Sources into Cribl Stream and then on to Destinations](sources-and-destinations-4.6.png)
{caption="Flow of data from Sources into Cribl Stream and then on to Destinations."}

## Destination Categories

Destinations can be divided into [streaming and non-streaming](#streaming-and-non-streaming-destinations):
those that accept events in real time, and those that batch them from a staging directory.

Other than that, each Destination can belong to one of the following (non-exclusive) categories:

- [Filesystem-based](#filesystem-based-destinations)
- [Load-balanced](#load-balanced-destinations)

### Streaming and Non-Streaming Destinations

Streaming and non-streaming Destination differ in the way they receive events:

- Streaming Destinations accept events in real time.
- Non-streaming Destinations receive events in batches from a staging directory.

#### Non-Streaming Destinations

Non-streaming Destinations make use of the [staging directory](#staging-directory),
and have specific behavior regarding [batching events into files](#batching-conditions).

##### Staging Directory

With non-streaming Destinations, Cribl Stream uses a staging directory in the local filesystem
to format and write outputted events before sending them to configured Destinations.
After a set of [conditions](#batching-conditions) is met, data is compressed and then moved to the final Destination. 

To reduce costs when the staging directory is also the final directory,
Cribl Stream avoids iterating through all the files within a directory
by keeping an inventory of open (in progress) files in the staging directory's root.
At startup, Cribl Stream will check for any leftover files in progress from prior sessions,
and will ensure that they're moved to their final Destination.
The process of moving to the final Destination is delayed after startup (default delay: 30 seconds).
Processing of these files is paced at one file per service period (which defaults to 1 second).

> In Cribl.Cloud, using a staging directory is only available on customer-managed [hybrid](cloud-enterprise#hybrid) Workers.
{.box .cloud}

##### Batching Conditions

In non-streaming delivery, a file is closed and rolled out when it reaches its configured maximum:

- Size
- Open time
- Idle time

If a new file needs to be open, Cribl Stream will enforce the maximum number of open files
by closing files in the order in which they were opened.

### Filesystem-based Destinations

Some Destinations are Filesystem-based, which means they receive files on disk from a staging directory and batch them in a queue.
When a batch of events is ready for transmission, Cribl Stream closes the file,
optionally compresses it, and transmits the file to the downstream service.
Filesystem-based Destinations do not support [persistent queues](persistent-queues).

### Load-balanced Destinations

Certain Destinations offer built-in [load-balancing](load-balancing) capabilities.

## Available Destinations

Cribl Stream supports the following Destinations. You can configure [proxy servers](proxy-config) for all HTTP-based Destinations.

| Destination | Protocol | Streaming | Filesystem-Based | Load-Balanced
| --- | --- | --- | --- | --- |
| [Amazon S3 Compatible Stores](destinations-s3) | HTTP/S | Non-streaming | ✓ |
| [Amazon CloudWatch Logs](destinations-cloudwatch-logs) | HTTP/S | Streaming 
| [Data Lakes > Cribl Lake](destinations-cribl-lake) | HTTP/S | Non-Streaming
| [Data Lakes > Amazon S3](destinations-data-lake-s3) | HTTPS only | Non-Streaming | ✓
| [Data Lakes > Amazon Security Lake](destinations-security-lake) | HTTP/S | Non-Streaming | ✓
| [Amazon Kinesis Data Streams](destinations-kinesis-streams) | HTTP/S | Streaming
| [Amazon MSK](destinations-msk) | TCP | Streaming
| [Amazon SQS](sources-sqs) | HTTP/S | Streaming
| [Azure Blob Storage](destinations-azure-blob) | HTTPS only | Non-Streaming | ✓
| [Azure Data Explorer](destinations-azure-data-explorer) | HTTPS only | Streaming or non-streaming
| [Azure Event Hubs](destinations-azure-event-hubs) | TCP | Streaming
| [Azure Monitor Logs](destinations-azure-monitor-logs) | HTTPS only | Streaming
| [Microsoft Sentinel](destinations-sentinel) | HTTP/S | Streaming
| [ClickHouse](destinations-click-house) | HTTP/S | Streaming | | ✓
| [Confluent Cloud](destinations-confluent) | TCP | Streaming
| [CrowdStrike Falcon LogScale](destinations-humio-hec) | HTTPS only | Streaming
| [CrowdStrike Falcon Next-Gen SIEM](destinations-crowdstrike-next-gen-siem) | HTTPS only | Streaming
| [Datadog](destinations-datadog) | HTTPS only | Streaming
| [Dynatrace HTTP](destinations-dynatrace-http) | HTTP/S | Streaming || ✓
| [Dynatrace OTLP](destinations-dynatrace-otlp) | HTTP/S | Streaming
| [Elasticsearch](destinations-elastic) | HTTP/S | Streaming || ✓
| [Elastic Cloud](destinations-elastic-cloud) | HTTPS only | Streaming || ✓
| [Exabeam](destinations-exabeam) | HTTP/S | Non-Streaming | ✓
| [Filesystem/NFS](destinations-fs) || Non-Streaming | ✓
| [Google SecOps](destinations-google_chronicle) | HTTPS only | Streaming
| [Google Cloud Logging](destinations-google-logging) | HTTPS only | Streaming
| [Google Cloud Pub/Sub](destinations-google_pubsub) | HTTPS only | Streaming
| [Google Cloud Storage](destinations-google-cloud-storage) | HTTPS only | Non-Streaming | ✓
| [Grafana Cloud](destinations-grafana_cloud) | HTTP/S | Streaming
| [Graphite](destinations-graphite) | TCP or UDP | Streaming
| [Honeycomb](destinations-honeycomb) | HTTPS only | Streaming
| [InfluxDB](destinations-influxdb) | HTTP/S | Streaming
| [Kafka](destinations-kafka) | TCP | Streaming
| [Loki](destinations-loki) | HTTP/S | Streaming
| [MinIO](destinations-minio) | HTTP/S | Non-Streaming | ✓
| [NetFlow](destinations-netflow) | UDP | Streaming
| [New Relic Events](destinations-newrelic-events) | HTTPS only | Streaming
| [New Relic Logs & Metrics](destinations-newrelic) | HTTPS only | Streaming
| [OpenTelemetry (OTel)](destinations-otel) | gRPC or HTTP/S | Streaming
| [Prometheus](destinations-prometheus) | HTTP/S | Streaming
| [SentinelOne DataSet](destinations-dataset) | HTTPS only | Streaming
| [ServiceNow Cloud Observability](destinations-servicenow) | gRPC or HTTP/S | Streaming
| [SignalFx](destinations-signalfx) | HTTPS only | Streaming
| [SNMP Trap](destinations-snmp-traps) | UDP | Streaming
| [Splunk HEC](destinations-splunk-hec) | HTTP/S | Streaming || ✓
| [Splunk Load Balanced](destinations-splunk-lb) | TCP | Streaming | ✓
| [Splunk Single Instance](destinations-splunk) | TCP | Streaming
| [StatsD](destinations-statsd) | TCP or UDP | Streaming
| [StatsD Extended](destinations-statsd-extended) | TCP or UDP | Streaming
| [Sumo Logic](destinations-sumo-logic) | HTTP/S | Streaming
| [Syslog](destinations-syslog) | TCP or UDP | Streaming || ✓ (TCP only)
| [TCP JSON](destinations-tcp-json) | TCP | Streaming || ✓
| [Wavefront](destinations-wavefront) | HTTPS only | Streaming
| [Webhook](destinations-webhook) | HTTP/S | Streaming || ✓

> You can adapt the [Amazon S3 Compatible Stores](destinations-s3) Destination
> to send data to downstream services like Databricks and Snowflake,
> for which Cribl Stream currently has no preconfigured Destination.
> For details, please contact Cribl Support.
>
{.box .info}

### Internal Destinations

Internal Destinations are special-purpose Destinations that route data within your Cribl Stream deployment,
or among Workers across [distributed](deploy-distributed) or [hybrid Cribl.Cloud](cloud-enterprise#hybrid) deployments.
The following internal Destinations are available:

- [Default](destinations-default): Specify a default output from among your configured Destinations.
- [Output Router](destinations-output-router): A "meta-Destination." Configure rules that route data to multiple configured Destinations.
- [DevNull](destinations-devnull): Simply drops events. Preconfigured and active when you install Cribl Stream, so it requires no configuration. Useful for testing.
- [Cribl HTTP](destinations-cribl-http): Send data among peer Worker Nodes over HTTP. Streaming and load-balanced.
- [Cribl TCP](destinations-cribl-tcp): Send data among peer Worker Nodes over TCP. Streaming and load-balanced.
- **SpaceOut**: This experimental Destination is undocumented. Be careful!

## Data Delivery to Unreachable Destinations

Cribl Stream attempts to deliver data to all Destinations that are configured to receive it at least once.
When a Destination is unreachable, there are three possible behaviors: 

- **Block** - Cribl Stream will block incoming events. 
- **Drop** - Cribl Stream will drop events addressed to that Destination.
- **Queue** - To prevent data loss, Cribl Stream will write events to a **persistent queue** disk buffer,
  then forward them when a Destination becomes available. (Available on several streaming Destinations.)

For further information about backpressure (a situation when a Destination receives more data than it can send),
see [Destination Backpressure Triggers](destinations-backpressure-triggers).

You can configure your desired behavior through a Destination's **Backpressure Behavior** drop-down.
Where other options are not displayed, Cribl Stream's default behavior is **Block**.
For details about all the above behaviors and options, see [Persistent Queues](persistent-queues).
