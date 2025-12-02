# Destinations


Cribl Stream can send data to various Destinations, including Splunk, Kafka,
Kinesis, InfluxDB, Snowflake, Databricks, TCP JSON, and many others.
Destinations can write data to either IPv4 or IPv6 addresses.

![](sources-and-destinations-1a.da1ff6acc5.png)

## Destinations Summary

Cribl Stream supports the following Destinations. Streaming Destinations accept events in real time. All HTTP-based Destinations are [proxyable](proxy-config).

[Amazon S3 Compatible Stores](destinations-s3)

- HTTP/S
- Non-streaming
- Filesystem-based

> The [Amazon S3 Compatible Stores](destinations-s3) Destination can be adapted to send data to downstream services like Databricks and Snowflake, for which Cribl Stream currently has no preconfigured Destination. For details, please contact Cribl Support.
>
{.box .info}

[Amazon CloudWatch Logs](destinations-cloudwatch-logs)

- HTTP/S
- Streaming 

[Data Lakes > Amazon S3](destinations-data-lake-s3)

- HTTPS only
- Non-streaming
- Filesystem-based
 
[Data Lakes > Amazon Security Lake](destinations-security-lake)

- HTTP/S
- Non-streaming
- Filesystem-based

[Amazon Kinesis Streams](destinations-kinesis-streams)

- HTTP/S
- Streaming

[Amazon MSK](destinations-msk)

- TCP
- Streaming

[Amazon SQS](sources-sqs)

- HTTP/S
- Streaming

[Azure Blob Storage](destinations-azure-blob)

- HTTPS only
- Batching
- Filesystem-based

[Azure Data Explorer](destinations-azure-data-explorer)

- HTTPS only
- Streaming or non-streaming

[Azure Event Hubs](destinations-azure-event-hubs)

- TCP
- Streaming

[Azure Monitor Logs](destinations-azure-monitor-logs)

- HTTPS only
- Streaming

[Azure Sentinel](destinations-sentinel)

- HTTP/S
- Streaming

[Confluent Cloud](destinations-confluent)

- TCP
- Streaming

[CrowdStrike Falcon LogScale](destinations-humio-hec)

- HTTPS only
- Streaming

[Datadog](destinations-datadog)

- HTTPS only
- Streaming

[Elasticsearch](destinations-elastic)

- HTTP/S
- Streaming
- Load-balanced

[Elastic Cloud](destinations-elastic-cloud)

- HTTPS only
- Streaming
- Load-balanced

[Exabeam](destinations-exabeam)

- HTTP/S
- Non-streaming
- Filesystem-based

[Filesystem/NFS](destinations-fs)

- Non-streaming
- Filesystem-based

[Google Chronicle](destinations-google_chronicle)

- HTTPS only
- Streaming

[Google Cloud Logging](destinations-google-logging)

- HTTPS only
- Streaming

[Google Cloud Pub/Sub](destinations-google_pubsub)

- HTTPS only
- Streaming

[Google Cloud Storage](destinations-google-cloud-storage)

- HTTPS only
- Non-streaming
- Filesystem-based

[Grafana Cloud](destinations-grafana_cloud)

- HTTP/S
- Streaming

[Graphite](destinations-graphite)

- TCP or UDP
- Streaming

[Honeycomb](destinations-honeycomb)

- HTTPS only
- Streaming

[InfluxDB](destinations-influxdb)

- HTTP/S
- Streaming

[Kafka](destinations-kafka)

- TCP
- Streaming

[Loki](destinations-loki)

- HTTP/S
- Streaming

[MinIO](destinations-minio)

- HTTP/S
- Non-streaming
- Filesystem-based

[New Relic Events](destinations-newrelic-events)

- HTTPS only
- Streaming

[New Relic Logs & Metrics](destinations-newrelic)

- HTTPS only
- Streaming

[OpenTelemetry (OTel)](destinations-otel)

- gRPC over HTTP/S or TCP
- Streaming

[Prometheus](destinations-prometheus)

- HTTP/S
- Streaming

[SentinelOne DataSet](destinations-dataset)

- HTTPS only
- Streaming

[SignalFx](destinations-signalfx)

- HTTPS only
- Streaming

[SNMP Trap](destinations-snmp-traps)

- UDP
- Streaming

[Splunk HEC](destinations-splunk-hec)

- HTTP/S
- Streaming
- Load-balanced

[Splunk Load Balanced](destinations-splunk-lb)

- TCP
- Streaming
- Load-balanced

[Splunk Single Instance](destinations-splunk)

- TCP
- Streaming

[StatsD](destinations-statsd)

- TCP or UDP
- Streaming

[StatsD Extended](destinations-statsd-extended)

- TCP or UDP
- Streaming

[Sumo Logic](destinations-sumo-logic)

- HTTP/S
- Streaming

[Syslog](destinations-syslog)

- TCP or UDP
- Streaming
- Load-balanced (TCP only)

[TCP JSON](destinations-tcp-json)

- TCP
- Streaming
- Load-balanced

[Wavefront](destinations-wavefront)

- HTTPS only
- Streaming

[Webhook](destinations-webhook)

- HTTP/S
- Streaming

## Internal and Special-Purpose Destinations {#internal} 

These special-purpose Destinations route data within your Cribl Stream deployment, or among Workers across [distributed](deploy-distributed) or [hybrid Cribl.Cloud](cloud-enterprise#hybrid) deployments:

  * [Default](destinations-default): Specify a default output from among your configured Destinations.
  * [Output Router](destinations-output-router): A "meta-Destination." Configure rules that route data to multiple configured Destinations.
  * [DevNull](destinations-devnull): Simply drops events. Preconfigured and active when you install Cribl Stream, so it requires no configuration. Useful for testing.
  * [Cribl HTTP](destinations-cribl-http): Send data among peer Worker Nodes over HTTP. Streaming and load-balanced.
  * [Cribl TCP](destinations-cribl-tcp): Send data among peer Worker Nodes over TCP. Streaming and load-balanced.
  * [Cribl Stream (Deprecated)](destinations-logstream): Use either Cribl HTTP or Cribl TCP instead.
  * **SpaceOut**: This experimental Destination is undocumented. Be careful!

### How Does Non-Streaming Delivery Work

Cribl Stream uses a staging directory in the local filesystem to format and write outputted events before sending them to configured Destinations. After a set of conditions is met – typically file size and number of files, further details [below](#conditions) – data is compressed and then moved to the final Destination. 

An inventory of open, or in-progress, files is kept in the staging directory's root, to avoid having to walk that directory at startup. This can get expensive if staging is also the final directory. At startup, Cribl Stream will check for any leftover files in progress from prior sessions, and will ensure that they're moved to their final Destination. The process of moving to the final Destination is delayed after startup (default delay: 30 seconds). Processing of these files is paced at one file per service period (which defaults to 1 second).

> In Cribl.Cloud, using a staging directory is only available on [hybrid](cloud-enterprise#hybrid), customer-managed Workers.
{.box .cloud}

### Batching Conditions {#conditions}

Several **conditions** govern when files are closed and rolled out:

1. File reaches its configured maximum size.

2. File reaches its configured maximum open time.

3. File reaches its configured maximum idle time.

If a new file needs to be open, Cribl Stream will enforce the maximum number of open files, by closing files in the order in which they were opened. 

## Data Delivery and Persistent Queues {#data-delivery} 

Cribl Stream attempts to deliver data to all Destinations on an at-least-once basis. When a Destination is unreachable, there are three possible behaviors: 

* **Block** - Cribl Stream will block incoming events. 
* **Drop** - Cribl Stream will drop events addressed to that Destination.
* **Queue** - To prevent data loss, Cribl Stream will write events to a **Persistent Queue** disk buffer, then forward them when a Destination becomes available. (Available on several streaming Destinations.)

For further information about backpressure, see [Destination Backpressure Triggers](destinations-backpressure-triggers).

You can configure your desired behavior through a Destination's **Backpressure Behavior** drop-down. Where other options are not displayed, Cribl Stream's default behavior is **Block**. For details about all the above behaviors and options, see [Persistent Queues](persistent-queues).

## Configuring Destinations

For each Destination **type**, you can create multiple definitions, depending on your requirements. 

To configure Destinations, in single-instance deployments, select **Manage**, then proceed to the options below. In distributed deployments, first click **Manage**, then select a **Worker Group** to configure and choose one of the options below. 

- To access the graphical [QuickConnect](quickconnect) UI, click **Destinations**. Next, select the desired type, and then click either **Add New** or (if displayed) Select **Existing**. 
- To access the [Routing](routes) UI, click **Data** > **Destinations**. On the resulting **Manage Destinations** page's tiles, select the desired type, then click 
**Add New**. 

To edit any Destination’s definition in a JSON text editor, click **Manage as JSON** at the bottom of the **New Destination** modal, or on the **Configure** tab when editing an existing Destination. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing Destination configurations as `.json` files.

> When JSON configuration contains sensitive information, it is redacted during export.
> 
{.box .info}

## Capturing Outgoing Data

To capture data from a single enabled Destination, you can bypass the [Preview](data-preview) pane, and instead capture directly from a **Manage Destinations** page. Just click the **Live** button beside the Destination you want to capture.

![Destination > Live button](destination-live-button.eef2f9db2e.png)
{border="true"}

You can also start an immediate capture from within an enabled Destination's config modal, by clicking the modal's **Live Data** tab. 

![Destination modal > Live Data tab](destination-live-data-tab.b3ad974b73.png)
{border="true"}

## Monitoring Destination Status

Each Destinations's configuration modal offers two tabs for monitoring: **Status** and **Charts**.

### Status Tab

The **Status** tab provides details about the Workers in the group and their status.
An icon shows whether the Worker is operating normally.

You can click each Worker's row to see specific information, for example, to identify issues when the Destination displays an error.
The specific set of information provided depends on the Destination type.
The data represents only process 0 for each Worker Node.

> The content of the **Status** tab is loaded live when you open it and only displayed when all the data is ready.
> With a lot of busy Workers in a group, or Workers located far from the Leader, there may be a delay before you see any information.
> 
> The statistics presented are reset when the Worker restarts.
{.box .info}

### Charts Tab

The **Charts** tab presents a visualization of the recent activity on the Destination.
The following data is available:

- Events in
- Thruput in (events per second)
- Bytes in
- Thruput in (bytes per second)
- Blocked status

> This data (in contrast with the [status tab](#status-tab)) is read almost instantly
> and does not reset when restarting a Worker.
{.box .info}
