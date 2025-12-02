# Sources Overview


Cribl Stream can receive continuous data input from various Sources, including
Splunk, HTTP, Elastic Beats, Kinesis, Kafka, TCP JSON, and many others. Sources
can receive data from either IPv4 or IPv6 addresses.

![Push and Pull Sources](load-balancing-may-2025.png)

## Sources Summary

Cribl Stream supports a variety of HTTP-, TCP-, and UDP-based Sources. All HTTP-based Sources are [proxyable](proxy-config).

### Data Ingestion Overview

A quick conceptual walkthrough of Cribl Stream [data ingestion](https://vimeo.com/469949169) capabilities.

{{< video 469949169 >}}


### Collector Sources

Collectors ingest data **intermittently** in on-demand bursts ("ad hoc collection"), on preset schedules, or by "replaying" data from local or remote stores.

All Collector Sources are proxyable.

> These Sources can ingest data only if a Leader is active:
>
{.box .info}

- [Amazon S3](collectors-s3) (HTTPS only)
- [Azure Blob Storage](collectors-azure-blob) (HTTPS only)
- [Cribl Lake](collectors-cribl-lake)
- [Database](collectors-database)
- [File System/NFS](collectors-filesystem)
- [Google Cloud Storage](collectors-google-cloud-storage) (HTTPS only)
- [Health Check](collectors-health-check) (HTTP/S)
- [REST/API Endpoint](collectors-rest) (HTTP/S)
- [Script](collectors-script) (executes any shell script you provide and returns the output of that script/command)
- [Splunk Search](collectors-splunk-search) (HTTP/S)

For background and instructions on using Collectors, see:

- [Collectors](collectors)
- [Scheduling and Running](collectors-schedule-run)
- [Job Limits](collectors-job-limits)

> Check out the example REST Collector configurations in Cribl's Collector Templates [repository](https://github.com/criblio/collector-templates/tree/main). For many popular Collectors, the repo provides configurations (with companion Event Breakers, and event samples in some cases) that you can import into your Cribl Stream instance, saving the time you'd have spent building them yourself.
{.box .success}

### Push Sources {#push}

Supported data Sources that **send** to Cribl Stream.

> In the absence of an active Leader, these Sources can generally continue data ingestion. However, some Sources may experience functional limitations during prolonged Leader unavailability. See the respective Source documentation for details.
>
{.box .info}

- [Amazon Data Firehose](sources-kinesis-firehose) (HTTPS only)
- [Datadog Agent](sources-datadog-agent) (HTTP/S)
- [Elasticsearch API](sources-elastic) (HTTP/S)
- [Grafana](sources-grafana) (HTTP/S)
- [HTTP/S (Bulk API)](sources-https)
- [Raw HTTP/S](sources-raw-http)
- [Loki](sources-loki) (HTTP/S)
- [Metrics](sources-metrics) (TCP or UDP)
- [Model Driven Telemetry](sources-model-driven-telemetry) (gPRC)
- [NetFlow & IPFIX](sources-netflow) (NetFlow v5, v9, and IPFIX over UDP)
- [OpenTelemetry (OTel)](sources-otel) (gRPC or HTTP/S)
- [Prometheus Remote Write](sources-prometheus-remote-write) (HTTP/S)
- [SNMP Trap](sources-snmp-traps) (UDP)
- [Splunk HEC](sources-splunk-hec) (HTTP/S)
- [Splunk TCP](sources-splunk)
- [Syslog](sources-syslog) (TCP or UDP)
- [TCP JSON](sources-tcp-json)
- [TCP (Raw)](sources-tcp-raw)
- [UDP (Raw)](sources-raw-udp)
- [Windows Event Forwarder](sources-wef) (HTTP/S)
- [Zscaler Cloud NSS](sources-zscaler-hec) (HTTP/S)

Data from these Sources is normally sent to a set of Cribl Stream Workers through a load balancer. Some Sources, such as [Splunk](sources-splunk) forwarders, have native load-balancing capabilities, so you should point these directly at Cribl Stream.

### Pull Sources

Supported Sources that Cribl Stream **fetches** data from.

- [Amazon Kinesis Data Streams](sources-kinesis-streams) (HTTPS only) - [requires active Leader](sources-kinesis-streams#how-the-source-and-kinesis-interact)
- [Amazon SQS](sources-sqs) (HTTPS only)
- [Amazon S3](sources-s3) (HTTPS only)
- [Amazon Security Lake](sources-security-lake) (HTTPS only)
- [Google Cloud Pub/Sub](sources-google_pubsub) (HTTPS only)
- [Azure Event Hubs](sources-azure-event-hubs) (TCP)
- [Azure Blob Storage](sources-azure-blob) (HTTPS only)
- [Confluent Cloud](sources-confluent) (TCP)
- [CrowdStrike FDR](sources-crowdstrike) (HTTPS only)
- [Office 365 Services](sources-office-365-services) (HTTPS only) - requires active Leader for job management
- [Office 365 Activity](sources-office-365-activity) (HTTPS only) - requires active Leader for job management
- [Office 365 Message Trace](sources-office365-msg-trace) (HTTPS only) - requires active Leader for job management
- [Prometheus Scraper](sources-prometheus) (HTTP/S) - requires active Leader for job management
- [Kafka](sources-kafka) (TCP)
- [Amazon MSK](sources-msk) (TCP)
- [Splunk Search](sources-splunk-search) (HTTP/S) - requires active Leader for job management
- [Wiz](sources-wiz) (HTTPS only) - requires active Leader for job management

Pull Sources fetch data from various external systems, with state management varying by Source. Some Sources, like Kafka, manage state independently, while others, such as Office 365 and Kinesis, require an active Leader for job management. This dependency is noted in the Pull Sources list above, indicating which Sources rely on the Leader for state and job management. For details on how each Source interacts with Cribl Stream, refer to the documentation for each Source.

### System Sources

Sources supply information generated by Cribl Stream about itself or move data among Workers within your Cribl Stream deployment.

- [AppScope](sources-appscope) (TCP or Unix socket)
- [Datagen](sources-datagens)
- [Exec](sources-exec) (on-prem only)
- [File Monitor](sources-file-monitor) (on-prem only)
- [Journal files](sources-journal-files) (on-prem only)
- [System Metrics](sources-system-metrics) (on-prem only)
- [System State](sources-system-state) (on-prem only)

### Internal Sources

Similar to System Sources, Internal Sources also supply information generated by Cribl Stream about itself.
However, unlike System Sources, they don't count towards the license usage.

- [Cribl HTTP](sources-cribl-http) (HTTP/S)
- [Cribl TCP](sources-cribl-tcp)
- [Cribl Internal](sources-cribl-internal)

## Configure and Manage Sources

For each Source *type*, you can create multiple definitions, depending on your requirements.

From Stream Home, under **Worker Groups**, select a Worker Group to configure. Next, you have two options:

- To configure via [QuickConnect](quickconnect):
  1. Select **Routing**, then **QuickConnect** (Stream) or **Collect** (Edge).
  1. Select **Add Source** at left, and select the Source you want from the list, choosing either **Add Existing** or **Add New**.
- To configure via [Routing](routes):
  1. Select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge).
  1. Select the Source you want.
  1. Select **Add Source** to open a **New Source** modal.

To edit any Source’s definition in a JSON text editor, select **Manage as JSON** at the bottom of the **New Source** modal, or on the **Configure** tab when editing an existing Source. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing Source configurations as `.json` files.

> When JSON configuration contains sensitive information, it is redacted during export.
>
{.box .info}

## Capture Source Data

To capture data from a single enabled Source, you can bypass the [Data Preview](data-preview) pane, and instead capture directly from the list of Sources. Just select the Source type, and then select the **Live** button beside the Source you want to capture.

> To capture live data, you must have Worker Nodes registered to the Worker Group
for which you're viewing events. You can view registered Worker Nodes from the
**Status** tab in the Source.
>
{.box .info}

![Source > Live button](source-live-button-4101.png)
{border="true"}

You can also start an immediate capture from within an enabled Source's config modal by selecting the **Live Data** tab.

![Source modal > Live Data tab](source-live-data-tab.74538cb58d.png)
{border="true"}

## Monitor Source Status

You can get a quick overview of Source health status by referring to their status icons.

Additionally, each Source's configuration modal offers two tabs for monitoring: [**Status**](#status-tab) and [**Charts**](#charts-tab).

### Source Status Icons

Source status icons are available on the **Data** > **Destinations** page
and for each individual Source in the list for a specific Source type.

The icons have the following meanings:

| Icon | Meaning |
|----- | ------- |
| ![](status-icon-healthy.svg) | Healthy. Operating correctly. |
| ![](status-icon-warning.svg) | Warning. Experiencing issues.</br>The Source is not functioning fully. Specific conditions will depend on the Source type. |
| ![](status-icon-critical.svg)  | Critical. Experiencing critical issues.</br>Drill down to the Source's [Status](#status-tab) tab to find out the details. |
| ![](status-icon-disabled.svg)  | Disabled.</br> The Source is configured, but not enabled. |
| ![](status-icon-configured.svg) | No health metrics available.</br>This may mean that a Source is enabled, but has not been deployed yet. |
| | Inactive. When using [GitOps](gitops), a Source appears `Inactive` if its **Environment** field (configured under **Advanced Settings**) does not match the currently active environment determined by the deployed Git branch. This ensures integrations only activate in their designated environments, preventing unintended data flow or misconfiguration. |

> You can also find status icons on the Cribl Stream front page on Worker Group tiles.
> ![Warning icon](status-icon-warning.svg) or ![Critical icon](status-icon-critical.svg) on any tile
> indicate that one or more Sources or Destinations in this Worker Group are experiencing issues.
> Hover over the icon to see more details.
{.box .success}

### Status Tab

The **Status** tab provides details about the Workers in the group and their status.
An icon shows whether the Worker is operating normally.

You can select each Worker's row to see specific information, for example, to identify issues when the Source displays an error.
The specific set of information provided depends on the Source type.
The data represents only process 0 for each Worker Node.

> The content of the **Status** tab is loaded live when you open it and only displayed when all the data is ready.
> With a lot of busy Workers in a group, or Workers located far from the Leader, there may be a delay before you see any information.
>
> The statistics presented are reset when the Worker restarts.
{.box .info}

### Charts Tab

The **Charts** tab presents a visualization of the most recent 10 minutes of activity on the Source. The following data is available:

- Total events in
- Average throughput (events per second)
- Total bytes in
- Average throughput (bytes per second)

> This data (in contrast with the [status tab](#status-tab)) is read almost instantly
> and does not reset when restarting a Worker.
{.box .info}

## Preconfigured Sources

To accelerate your setup, Cribl Stream ships with several common Sources configured for typical listening ports, but not switched on. Open, clone (if desired), modify, and enable any of these preconfigured Sources to get started quickly.

> On a Cribl.Cloud deployment, **do not delete** any preconfigured Sources.
> If you don't plan to use them, keep them disabled.
{.box .cloud}

  * [Syslog](sources-syslog) – UDP Port 9514, TCP Port 9514, TCP + TLS Port 6514
  * [Splunk TCP](sources-splunk) – Port 9997
  * [Splunk HEC](sources-splunk-hec) – Port 8088
  * [TCP JSON](sources-tcp-json) – Port 10070
  * [TCP](sources-tcp-raw) – Port 10060
  * [HTTP](sources-https) – Port 10080
  * [Elasticsearch API](sources-elastic) – Port 9200
  * [SNMP Trap](sources-snmp-traps) – Port 9162 (preconfigured only on on-prem setups)
  * [OpenTelemetry](sources-otel) - Port 4317 (preconfigured only on Cribl.Cloud)

System and Internal Sources:

  * Cribl Internal > [CriblLogs](sources-cribl-internal#logs) (preconfigured only on on-prem setups)
  * Cribl Internal > [CriblMetrics](sources-cribl-internal#metrics)
  * [Appscope](sources-appscope)
  * [Cribl HTTP](sources-cribl-http) – Port 10200 (preconfigured only on distributed setups)
  * [Cribl TCP](sources-cribl-tcp) – Port 10300 (preconfigured only on distributed setups)
  * [System Metrics](sources-system-metrics) (preconfigured only on on-prem setups)
  * [System State](sources-system-state) (preconfigured only on on-prem setups)
  * [Journal files](sources-journal-files) (preconfigured only on on-prem setups)

> In a preconfigured Source's configuration, never change the **Address** field, even though the UI shows an editable field. If you change these fields' value, the Source will not work as expected.
>
> After you create a Source and deploy the changes, it can take a few minutes for the Source to become available in Cribl.Cloud's load balancer. However, Cribl Stream will open the port, and will be able to receive data, immediately.
>
{.box .warning}

## Single Source Support

The following Source types are limited to preconfigured Sources:

- Cribl Internal
- System Metrics
- System State

### Cribl.Cloud Ports and TLS Configurations {#ports-certs}

Cribl.Cloud provides several data Sources and ports already enabled for you,
plus 11 additional TCP ports (`20000`-`20010`) that you can use to add and configure more Sources.

The Cribl.Cloud portal's **Data Sources** page displays the pre‑enabled Sources, their endpoints, the reserved and available ports, and protocol details. For each existing Source listed here, Cribl recommends using the preconfigured endpoint and port to send data into Cribl Stream.

To access the **Data Sources** page, on the top bar, select **Products**, and then select **Cribl** > **Workspace** > **Data Sources**.

> ##### Cribl HTTP and Cribl TCP Sources/Destinations {{< id `free-ingress` >}}
>
> Use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http), and/or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp), to relay data between Worker Nodes connected to the same Leader. This traffic does not count against your ingestion quota, so this routing prevents double-billing. (For related details, see [Exemptions from License Quotas](licensing#exemptions).)
>
{.box .success}

## Backpressure Behavior and Persistent Queues {#backpressure-behavior}

By default, a Cribl Stream Source will respond to a **backpressure** situation by blocking incoming data. Backpresssure triggers exist when an in-memory buffer is full and/or when downstream Destinations/receivers are unavailable. The Source will refuse to accept new data until it can flush its buffer.

This will propagate block signals back to the sender, if it supports backpressure. Note that UDP senders (including SNMP Traps and some syslog senders) do not provide this support. In this situation, Cribl Stream will simply drop new events until the Source can process them.

#### Persistent Queues

[Push Sources](#push)' config modals provide a **Persistent Queue Settings** option to minimize loss of inbound streaming data. Here, the Source will write data to disk until its in-memory buffer recovers. Then, it will drain the disk-queued data in FIFO (first in, first out) order.

When you enable Source PQ, you can choose between two trigger conditions: **Smart** Mode will engage PQ upon backpressure from Destinations, whereas **Always On** Mode will use PQ as a buffer for **all** events.

For details about the PQ option and these modes, see [Persistent Queues](persistent-queues).

> Persistent queues, when engaged, slow down data throughput somewhat. It is redundant to enable PQ on a Source whose upstream sender is configured to safeguard events in its own disk buffer.
>
{.box .info}

#### Other Backpressure Options

The [S3 Source](sources-s3) provides a configurable **Advanced Settings > Socket timeout** option, to prevent data loss (partial downloading of logs) during backpressure delays.

#### Diagnosing Backpressure Errors

When backpressure affects HTTP Sources ([Splunk HEC](sources-splunk-hec), [HTTP/S](sources-https), [Raw HTTP/S](sources-raw-http), and [Amazon Data Firehose](sources-kinesis-firehose)), Cribl Stream internal logs will show a `503` error code.
