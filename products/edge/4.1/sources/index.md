# Sources


Each Cribl Edge **Source** is a configuration that enables Edge nodes to collect or receive observability data – logs, metrics, application data, etc. – in real time. Edge can receive continuous data input from Splunk, HTTP senders, Elastic Beats, Prometheus, TCP JSON, and many others.

Edge's UI offers a configuration modal for each type of supported Source. However, you can add multiple instances of each Source type – with each configured to match the parameters of the corresponding sender. E.g., you can have multiple File Monitors and multiple listeners for Syslog, Splunk, Elastic Beats, Prometheus, TCP JSON, and many others.

![Sources in the Edge ecosystem](edge-load-balancing.80cf115555.jpg)
{border="true"}

## System and Internal Sources 

Sources that generate data locally at the Edge Node; or monitor resources; or move data among Edge Nodes and/or Stream Workers within your Cribl deployment.

  * [AppScope](sources-appscope)  
  * [Cribl Internal](sources-cribl-internal)
  * [Cribl HTTP](sources-cribl-http)
  * [Cribl TCP](sources-cribl-tcp)
  * [Datagen](sources-datagens)
  * [Exec](sources-exec)
  * [File Monitor](sources-file-monitor)
  * [Kubernetes Logs](sources-kubernetes-logs)
  * [Kubernetes Metrics](sources-kubernetes-metrics)
  * [System Metrics](sources-system-metrics)
  * [System State](sources-system-state)
  * [Windows Metrics](sources-windows-metrics)

## PUSH Sources {#push}

Supported data Sources that **send** to Cribl Edge.

> In the absence of an active Leader, these Sources can generally continue data ingestion. However, some Sources may experience functional limitations during prolonged Leader unavailability. See the respective Source documentation for details.
>
{.box .info}

  * [Amazon Kinesis Firehose](sources-kinesis-firehose)
  * [Datadog Agent](sources-datadog-agent) 
  * [Elasticsearch API](sources-elastic) 
  * [Grafana](sources-grafana) 
  * [HTTP/S (Bulk API)](sources-https)
  * [Loki](sources-loki)
  * [Metrics](sources-metrics) 
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

## PULL Sources

Supported data Sources that Cribl Edge **fetches** data from.

> These Sources can ingest data only if a Leader is active:
>
{.box .info}

  * [Prometheus Edge Scraper](sources-edge-prometheus)
  * [Prometheus Scraper](/stream/sources-prometheus) (Deprecated)
  * [Windows Event Logs](sources-windows-event-logs)

## Configuring and Managing Sources

For each Source *type*, you can create multiple definitions, depending on your requirements.

To configure Sources, from the top nav, click **Manage**, then select a **Fleet** to configure. Then, you have two options:

- To access the graphical [QuickConnect](quickconnect) UI, click **Collect**. Next, click either **Add New** or (if displayed) Select **Existing**. 

- To access the [Routing](routes) UI, click **More** > **Sources**. On the resulting **Data Sources** page's tiles or left menu, select the desired type, then click **Add New**. 

## Capturing Source Data

To capture data from a single enabled Source, you can bypass the [Preview](data-preview) pane, and instead capture directly from a **Manage Sources** page. Just click the **Live** button beside the Source you want to capture.

![Source > Live button](source-live-button.015e5c4de3.png)
{border="true"}

You can also start an immediate capture from within an enabled Source's configuration modal, by clicking the modal's **Live Data** tab. 

![Source modal > Live Data tab](source-live-data-tab.f9459f810d.png)
{border="true"}

## Preconfigured Sources

To accelerate your setup, Cribl Edge ships with several common Sources configured but not switched on. Open, clone (if desired), modify, and enable any of these preconfigured Sources to get started quickly:

  * **System Metrics** – Basic Level
  * **File Monitor** > **in_file_auto** – Auto Discovery Mode
  * **File Monitor** > **in_file_varlog** – Manual Discovery Mode
  * **AppScope** > **in_appscope** – Unix Domain Socket listener
  * **Cribl Internal** > **CriblLogs** – Internal
  * **Cribl Internal** > **CriblMetrics** – Internal 

> [Cribl University](https://cribl.io/university/) offers a course titled [Collecting Data in Edge](https://university.cribl.io/#/online-courses/b2dd1ea9-b354-4c86-b464-41e18fdd8fe6) that provides an illustrated overview. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Cribl Edge courses](https://university.cribl.io/#/catalog/f397da65-f17d-4c43-b147-5fff8b8c5876).
>
{.box .info}