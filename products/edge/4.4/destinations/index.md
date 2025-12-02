# Destinations


Cribl Edge can send data to various Destinations, including Cribl HTTP, Cribl TCP, Kafka, Kinesis, InfluxDB, Grafana Cloud, TCP JSON, Splunk, and others. 
Destinations can write data to either IPv4 or IPv6 addresses.

![Destinations in the Edge ecosystem](edge-sources-destinations-1.1e390290b2.jpg)

## Streaming Destinations

Destinations that accept events in real time are referred to as streaming Destinations:

  * [Amazon CloudWatch Logs](destinations-cloudwatch-logs)
  * [Amazon Kinesis Streams](destinations-kinesis-streams) 
  * [Amazon MSK](destinations-msk) 
  * [Amazon SQS](sources-sqs) 
  * [Azure Data Explorer](destinations-azure-data-explorer) when in streaming mode
  * [Azure Event Hubs](destinations-azure-event-hubs) 
  * [Azure Monitor Logs](destinations-azure-monitor-logs) 
  * [Azure Sentinel](destinations-sentinel)
  * [Confluent Cloud](destinations-confluent)
  * [Cribl HTTP](destinations-cribl-http) 
  * [Cribl TCP](destinations-cribl-tcp)  
  * [CrowdStrike Falcon LogScale](destinations-humio-hec)
  * [Datadog](destinations-datadog)
  * [Elasticsearch](destinations-elastic) 
  * [Elastic Cloud](destinations-elastic-cloud)
  * [Google Chronicle](destinations-google_chronicle) 
  * [Google Cloud Logging](destinations-google-logging) 
  * [Google Cloud Pub/Sub](destinations-google_pubsub)
  * [Grafana Cloud](destinations-grafana_cloud) 
  * [Graphite](destinations-graphite) 
  * [Honeycomb](destinations-honeycomb) 
  * [InfluxDB](destinations-influxdb) 
  * [Kafka](destinations-kafka) 
  * [Loki](destinations-loki) 
  * [New Relic Events](destinations-newrelic-events) 
  * [New Relic Logs & Metrics](destinations-newrelic)
  * [OpenTelemetry (OTel)](destinations-otel)
  * [Prometheus](destinations-prometheus) 
  * [SentinelOne DataSet](destinations-dataset)
  * [SignalFx](destinations-signalfx)  
  * [SNMP Trap](destinations-snmp-traps) 
  * [Splunk HEC](destinations-splunk-hec) 
  * [Splunk Load Balanced](destinations-splunk-lb)
  * [Splunk Single Instance](destinations-splunk)
  * [StatsD](destinations-statsd) 
  * [StatsD Extended](destinations-statsd-extended) 
  * [Sumo Logic](destinations-sumo-logic)
  * [Syslog](destinations-syslog) 
  * [TCP JSON](destinations-tcp-json)
  * [Wavefront](destinations-wavefront) 
  * [Webhook](destinations-webhook)

## Non-Streaming Destinations

Destinations that accept events in groups or batches are referred to as non-streaming Destinations:

  * [Amazon S3 Compatible Stores](destinations-s3) 
  * [Data Lakes > Amazon Security Lake](destinations-security-lake)
  * [Azure Blob Storage](destinations-azure-blob) 
  * [Azure Data Explorer](destinations-azure-data-explorer) when in batching mode
  * [Exabeam](destinations-exabeam) 
  * [Filesystem/NFS](destinations-fs) 
  * [Google Cloud Storage](destinations-google-cloud-storage) 
  * [MinIO](destinations-minio) 

> The [Amazon S3 Compatible Stores](destinations-s3) Destination can be adapted to send data to downstream services like Databricks and Snowflake, for which Cribl Edge currently has no preconfigured Destination. For details, please contact Cribl Support.
>
{.box .info}

## Internal and Special-Purpose Destinations {#internal} 

These special-purpose Destinations route data within your Cribl Edge deployment, or among Workers across [distributed](/stream/deploy-distributed) or [hybrid Cloud](/stream/deploy-cloud#hybrid) deployments:

  * [Default](destinations-default): Specify a default output from among your configured Destinations.
  * [Output Router](destinations-output-router): A "meta-Destination." Configure rules that route data to multiple configured Destinations.
  * [DevNull](destinations-devnull): Simply drops events. Preconfigured and active when you install Cribl Edge, so it requires no configuration. Useful for testing.
  * [Cribl HTTP](destinations-cribl-http): Send data among peer Edge Nodes over HTTP.
  * [Cribl TCP](destinations-cribl-tcp): Send data among peer Edge Nodes over TCP. 
  * [Cribl Stream (Deprecated)](destinations-logstream): Use either Cribl HTTP or Cribl TCP instead.
  * **SpaceOut**: This experimental Destination is undocumented. Be careful!

### How Does Non-Streaming Delivery Work

Cribl Edge uses a staging directory in the local filesystem to format and write outputted events before sending them to configured Destinations. After a set of conditions is met – typically file size and number of files, further details [below](#conditions) – data is compressed and then moved to the final Destination. 

An inventory of open, or in-progress, files is kept in the staging directory's root, to avoid having to walk that directory at startup. This can get expensive if staging is also the final directory. At startup, Cribl Edge will check for any leftover files in progress from prior sessions, and will ensure that they're moved to their final Destination. The process of moving to the final Destination is delayed after startup (default delay: 30 seconds). Processing of these files is paced at one file per service period (which defaults to 1 second).

### Batching Conditions {#conditions}

Several **conditions** govern when files are closed and rolled out:

1. File reaches its configured maximum size.

2. File reaches its configured maximum open time.

3. File reaches its configured maximum idle time.

If a new file needs to be open, Cribl Edge will enforce the maximum number of open files, by closing files in the order in which they were opened. 

## Data Delivery and Persistent Queues {#data-delivery}

Cribl Stream attempts to deliver data to all Destinations on an at-least-once basis. When a Destination is unreachable, there are three possible behaviors: 

* **Block** - Cribl Stream will block incoming events. 
* **Drop** - Cribl Stream will drop events addressed to that Destination.
* **Queue** - To prevent data loss, Cribl Stream will write events to a **Persistent Queue** disk buffer, then forward them when a Destination becomes available. (Available on several streaming Destinations.)

For further information about backpressure, see [Destination Backpressure Triggers](destinations-backpressure-triggers).

You can configure your desired behavior through a Destination's **Backpressure Behavior** drop-down. Where other options are not displayed, Cribl Stream's default behavior is **Block**. For details about all the above behaviors and options, see [Persistent Queues](persistent-queues).

## Configuring Destinations

For each Destination **type**, you can create multiple definitions, depending on your requirements. 

To configure Destinations, from the top nav of a distributed deployment, first click **Manage**, then select a **Fleet** to configure and choose one of the options below. For a single-instance deployment proceed to the options below: 
- To access the graphical [QuickConnect](quickconnect) UI, click **Collect**. Next, select the desired type, and then click either **Add New** or (if displayed) **Select Existing**. 
- To access the [Routing](routes) UI, click **More** > **Destinations**. On the resulting **Manage Destinations** page's tiles, select the desired type, then click **New Destination**. 

## Capturing Outgoing Data

To capture data from a single enabled Destination, you can bypass the [Preview](data-preview) pane, and instead capture directly from a **Manage Destinations** page. Just click the **Live** button beside the Destination you want to capture.

![Destination > Live button](destination-live-button.eef2f9db2e.png)
{border="true"}

You can also start an immediate capture from within an enabled Destination's config modal, by clicking the modal's **Live Data** tab. 

![Destination modal > Live Data tab](destination-live-data-tab.b3ad974b73.png)
{border="true"}

