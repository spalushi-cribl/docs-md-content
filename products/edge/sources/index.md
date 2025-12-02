# Sources


Each Cribl Edge **Source** is a configuration that enables Edge nodes to collect
or receive observability data – logs, metrics, application data, and so on – in
real time. Edge can receive continuous data input from Splunk, HTTP senders,
Elastic Beats, Prometheus, TCP JSON, and many others. Sources can receive data
from either IPv4 or IPv6 addresses.

Cribl Edge offers a configuration modal for each type of supported Source.
However, you can add multiple instances of each Source type – with each
configured to match the parameters of the corresponding sender. For example, you
can have multiple File Monitors and multiple listeners for Syslog, Splunk,
Elastic Beats, Prometheus, TCP JSON, and many others.

## System and Internal Sources 

Sources that generate data locally at the Edge Node, monitor resources, or move
data among Edge Nodes and/or Stream Workers within your Cribl deployment.

  * [AppScope](sources-appscope)  
  * [Cribl Internal](sources-cribl-internal)
  * [Cribl HTTP](sources-cribl-http)
  * [Cribl TCP](sources-cribl-tcp)
  * [Datagen](sources-datagens)
  * [Exec](sources-exec)
  * [File Monitor](sources-file-monitor)
  * [Journal Files](sources-journal-files)
  * [Kubernetes Events](sources-kubernetes-events)
  * [Kubernetes Logs](sources-kubernetes-logs)
  * [Kubernetes Metrics](sources-kubernetes-metrics)
  * [System Metrics](sources-system-metrics)
  * [System State](sources-system-state)
  * [Prometheus Edge Scraper](sources-edge-prometheus)
  * [Windows Event Logs](sources-windows-event-logs)
  * [Windows Metrics](sources-windows-metrics)

## Push Sources {#push}

Supported data Sources that **send** to Cribl Edge.

  * [Amazon Data Firehose](sources-kinesis-firehose)
  * [Cloudflare](sources-cloudflare-hec) (HTTP/S)
  * [Datadog Agent](sources-datadog-agent) 
  * [Elasticsearch API](sources-elastic) 
  * [Grafana](sources-grafana) 
  * [HTTP/S (Bulk API)](sources-https)
  * [Loki](sources-loki)
  * [Metrics](sources-metrics)
  * [Model Driven Telemetry](sources-model-driven-telemetry)
  * [Netflow & IPFIX](sources-netflow)
  * [Raw HTTP/S](sources-raw-http) 
  * [OpenTelemetry (OTel)](sources-otel)
  * [Prometheus Remote Write](sources-prometheus-remote-write) 
  * [SNMP Trap](sources-snmp-traps)  
  * [Splunk HEC](sources-splunk-hec) 
  * [Splunk TCP](sources-splunk) 
  * [Syslog](sources-syslog)
  * [TCP JSON](sources-tcp-json)
  * [TCP (Raw)](sources-tcp-raw)
  * [UDP (Raw)](sources-raw-udp)
  * [Windows Event Forwarder](sources-wef)
  * [Zscaler Cloud NSS](sources-zscaler-hec)

### Comparison of Generic Push Sources

This table compares generic, protocol-level Push Sources. Refer to this table when you need to ingest raw data or custom formats over basic protocols like HTTP, TCP, or UDP.

| Source | Key Differentiator | Best For... |
| :--- | :--- | :--- |
| **HTTP/S (Bulk API)** | **Protocol-aware:** Understands and responds using specific API semantics, including acknowledgments. | Receiving data from systems that require custom or generic HTTP/S event ingestion. |
| **Raw HTTP/S** | **Protocol-agnostic:** Accepts any HTTP/S payload without validating its structure. | Capturing any arbitrary or custom HTTP/S traffic where only the raw payload matters. |
| **TCP JSON** | **Content-aware:** Specifically designed to parse a stream of distinct JSON objects. | Ingesting a continuous stream of well-formed JSON objects over a reliable TCP connection. |
| **TCP (Raw)** | **Content-agnostic:** Treats the incoming data as an unstructured stream of bytes. | Ingesting binary data, custom application protocols, or any non-JSON data over TCP. |
| **UDP (Raw)** | **Stateless with best-effort delivery:** Offers lightweight, connectionless data ingestion. | High-volume, loss-tolerant data streams like syslog, NetFlow, or SNMP traps. |

## Pull Sources

Cribl Edge does not contain any Sources categorized as **Pull**. The contents of
the **Pull** tab will be blank.

## Configuring and Managing Sources

For each Source *type*, you can create multiple definitions, depending on your requirements.

To configure Sources, from the top nav, select **Manage**, then select a **Fleet** to configure. Then, you have two options:

- To access the graphical [QuickConnect](quickconnect) UI, select **Collect**. Next, select either **Add New** or (if displayed), select **Existing**. 

- To access the [Routing](routes) UI, select **More** > **Sources**. On the resulting **Data Sources** page's tiles or left menu, select the desired type, then select **Add New**. 

## Capturing Source Data

To capture data from a single enabled Source, you can bypass the [Preview](data-preview) pane, and instead capture directly from a **Manage Sources** page. Select the **Live** button beside the Source you want to capture.

> In order to capture live data, you must have Edge Nodes registered to the
Fleet for which you're viewing events. You can view registered Edge Nodes
from the **Status** tab in the Source.
>
{.box .info}

![Source > Live button](source-live-button-4101.png)
{border="true"}

You can also start an immediate capture from within an enabled Source's configuration modal, by selecting the **Live Data** tab. 

![Source modal > Live Data tab](source-live-data-tab.74538cb58d.png)
{border="true"}

## Monitoring Source Status

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
| | Inactive. When using [GitOps](stream/gitops), a Source appears `Inactive` if its **Environment** field (configured under **Advanced Settings**) does not match the currently active environment determined by the deployed Git branch. This ensures integrations only activate in their designated environments, preventing unintended data flow or misconfiguration. |

### Status Tab

The **Status** tab provides details about the Edge Nodes in the Fleet and their status.
An icon next to each Edge Node uses color to clearly signal its health:

- ![Green checkmark icon](status-icon-healthy.svg): All systems go! Your Edge Node is operating normally.
- ![Yellow warning sign icon](status-icon-warning.svg): Attention needed. There's a potential issue with this Edge Node.
- ![Red exclamation mark icon](status-icon-critical.svg): Stop! This Edge Node has encountered a critical error.

The way you view Edge Node statuses depends on the size of your Fleet:

- **Fewer than 1000 Edge Nodes**: All Edge Nodes are conveniently displayed on the Status tab, along with their statuses. Use the **Status** checkboxes at the top to filter the list by health (healthy, warning, error).                           
  
  Select any Edge Node row to view specific details to help diagnose issues. The specific set of information provided depends on the Source type. Keep in mind that this data only reflects process `0` for each Edge Node.

- **More than 1000 Edge Nodes**: With a larger Fleet, you can select a specific Edge Node from the drop-down menu (showing up to 100 Nodes). You can search by `hostname` or `GUID` to find a specific Node. You can also use the **Status** checkboxes to filter which Edge Nodes appear in the drop-down list.


> The content of the **Status** tab is loaded live when you open it and only displayed when all the data is ready.
> With a lot of busy Edge Nodes in a group, or nodes located far from the Leader, there may be a delay before you see any information.
> 
> The statistics presented are reset when the Edge Node restarts.
{.box .info}

### Charts Tab

The **Charts** tab presents a visualization of the most recent 10 minutes of activity on the Source. The following data is available:

- Total events in
- Average throughput (events per second)
- Total bytes in
- Average throughput (bytes per second)

> This data (in contrast with the [status tab](#status-tab)) is read almost instantly
> and does not reset when restarting an Edge Node.
{.box .info}

## Preconfigured Sources

To accelerate your setup, Cribl Edge ships with several common Sources configured but not switched on. Open, clone (if desired), modify, and enable any of these preconfigured Sources to get started quickly.

> On a Cribl.Cloud deployment, **do not delete** any preconfigured Sources.
> If you don't plan to use them, keep them disabled.
{.box .cloud}

  * **System Metrics** – Basic Level
  * **System State** > **in_system_state**
  * **File Monitor** > **in_file_auto** – Auto Discovery Mode
  * **File Monitor** > **in_file_varlog** – Manual Discovery Mode
  * **AppScope** > **in_appscope** – Unix Domain Socket listener
  * **Kubernetes Events** > **in_kube_events**
  * **Kubernetes Logs** > **in_kube_logs**
  * **Kubernetes Metrics** > **in_kube_metrics**
  * **Cribl Internal** > **CriblLogs** – Internal
  * **Cribl Internal** > **CriblMetrics** – Internal 
  * **Windows Metrics** > **in_windows_metrics**
  * **Windows Event Logs** > **in_win_event_logs**
  * **Journal Files** > **in_journal_local**

## Single Source Support

The following Source types are limited to preconfigured Sources:

- Cribl Internal
- Kubernetes Events
- Kubernetes Logs
- Kubernetes Metrics
