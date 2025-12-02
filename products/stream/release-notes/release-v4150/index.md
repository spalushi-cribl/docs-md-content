# Cribl Stream 4.15.0


Cribl Stream 4.15.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

### Deprecation Notice

The [Azure Monitor Log Destination](destinations-azure-monitor-logs) is now deprecated. The replacement for routing data to Microsoft’s Log Analytics platform is the [Microsoft Sentinel Destination](destinations-sentinel). This change aligns with Microsoft’s recommendation to use the [Azure Monitor Logs Ingestion API](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview), which the Microsoft Sentinel Destination uses.

Microsoft is retiring the legacy HTTP Data Collector API on September 14, 2026. For details, see the Microsoft deprecation notice: [Send log data to Azure Monitor by using the HTTP Data Collector API (deprecated)](https://learn.microsoft.com/en-us/previous-versions/azure/azure-monitor/logs/data-collector-api?tabs=powershell).

## New Features​

This release provides the following improvements:

### Fabric Real-Time Intelligence Destination

A new [**Fabric Real-Time Intelligence Destination**](destinations-fabric-real-time-intelligence) is now available in Cribl Stream. This Destination allows you to send telemetry and observability data from Cribl Stream to Microsoft Fabric Eventstreams via the new Kakfa-based Cribl Data source in the Microsoft Fabric Portal. Plus, there are no load balancing requirements—Fabric handles load balancing when it receives the data. You’ll find the Destination in the new **Fabric** folder in the Destinations catalog.

### Databricks Destination

The new **Databricks Destination** sends data to Databricks Unity Catalog volumes. It lets you configure where your data lands and ensures reliable, secure delivery by following Databricks API best practices. This Destination is designed for teams that need governed, auditable data delivery into Databricks for analytics and compliance workflows.

### Cloudflare Source and Cloudflare R2 Destination

The new **Cloudflare Source** receives Cloudflare Logpush data via HTTP Event Collector (HEC) endpoints. Logpush quickly delivers logs to Cribl in batches, providing information almost in real-time. The new **Cloudflare R2 Destination** sends data to Cloudflare R2, an S3-compatible object storage service.

### Temporary Log Level Settings

Use a configurable TTL (time-to-live) setting to temporarily change log levels for up to 24 hours and debug without overwhelming your logs or incurring unnecessary storage costs. The log level automatically reverts to the permanent setting after the TTL setting expires.

### Cribl Outpost in Stream

We have extended Cribl Outpost (a preview feature) to route control plane communication not only from Edge Nodes, but also from Stream Workers.

## Experience Improvements

- In Cribl.Cloud Workspaces, we removed the crusty **Trust** submenu and moved its one lonely item (Trust ARN) to **Access > Stream Worker Group Details**.
- The **C.Decode** and **C.Encode** Cribl Expressions now support MIME RFC 2047 encoding, which improves the ability to parse email subject lines.
- You can now use Connected Environments to send data from an on-prem environment to a Cribl.Cloud environment. For example, if you have an on-prem deployment of Cribl and are moving forward with a Cloud-only solution, you can use connected environments to streamline the process of onboarding your data.

## Packs

The Hello Packs sample Pack is no longer included in Cribl Stream starting with this release. Existing deployments will retain the Hello Packs content.

## Sources and Destinations

- The Confluent Cloud and Kafka Sources and Destinations now automatically detect and apply the correct schema (Avro or JSON) for message encoding and decoding via the Confluent Schema Registry. Schema type selection in the UI is no longer available.
- The Splunk HEC Destination now supports **TLS**. You can specify CA and client certificates, private keys, and related settings to enable secure, authenticated connections to Splunk HEC endpoints.
- In Cribl.Cloud, the Amazon S3, Azure Blob Storage, Google Cloud Storage, and Minio Destinations now close and upload leftover files in parallel during Worker or Edge Node shutdown, improving shutdown performance and reliability.
- ZSTD compression is now supported for Kafka, Confluent Cloud, and Amazon MSK Sources and Destinations. Kafka Sources automatically detect and decompress messages in ZSTD formats. Kafka Destinations have the ZSTD compression option.
- We've added new `iometrics` and dimensions, including consumer lag data for Kafka-based Sources, status codes and retryable/non-retryable counts for failed requests for HTTP Destinations, and reason for rejected connections for TCP Sources.
- We’ve expanded integration `iometrics` metrics to the Kafka and Zscaler Cloud NSS Sources, Elasticsearch, Elastic Cloud, Dynatrace HTTP, and Cribl TCP Destinations. These metrics include request failure counts, latency percentiles (p95, p99), and endpoint health percentages (reported only for load-balanced Destinations), aggregated every minute. Metrics can be accessed via the Cribl Internal Source and are also available in each Worker Process’s `metrics.log` file, which can be viewed from the **Monitoring** > **Logs** page. Performance charts are displayed on each Source and Destination configuration page for improved monitoring and troubleshooting of data pipelines.
- The Azure Data Explorer Destination now supports the Azure 21Vianet region by updating the **Cluster base URI** validation to allow endpoints ending with `.kusto.chinacloudapi.cn`.
- The Syslog Destination (UDP only) now features **Enable Source IP Spoofing**, a setting that preserves the event's original Source IP (from `__srcIpPort`) instead of the Worker IP. This is ideal for systems that rely on the Source IP for identification. This option is limited to on-prem/hybrid groups. It requires manually installing the privileged udp-senderhelper binary, which must be configured with the `CAP_NET_RAW` capability on all relevant Worker Nodes.
- The Filesystem/NFS, Amazon S3, Azure Blob Storage, Google Cloud Storage, and Minio Destinations now provide a **Force close on shutdown** setting that closes and uploads all staged files immediately during Node shutdown, bypassing idle time, file age, and size thresholds to reduce data loss risk in dynamic environments.
- The Google Cloud Chronicle API Destination now supports event-level RBAC labeling. You can Enable RBAC to designate a custom label for role-based access control and filtering, allowing granular multi-tenant data management directly within Chronicle.
- In Cribl.Cloud, the Amazon S3, Amazon Security Lake, Azure Blob Storage, Azure Data Explorer, Exabeam, Filesystem/NFS, Google Cloud Storage, and MinIO Destinations now provide a **Directory batch size** setting to control how many directories are processed per batch during empty staging directory cleanup.
- The File Monitor Source lets you configure the minimum age for the ingested file in the **Minimum age duration** setting. Use this setting if you need to ensure files have finished writing before starting to ingest them, for example if you are synchronizing them with a tool such as rsync. The previous **Age duration limit** setting is now called **Maximum age duration**.
- The File Monitor Source now collects from the end of files by default. This will prevent mass ingestion of data when deploying Edge Nodes with large historical logs.
- The File Monitor Source now uses `Manual` as the default discovery mode to allow for more customizable discovery from the get-go.
- The File Monitor Source offers a new **Apply filename allowlist internal to archive files** toggle that lets you apply the allowlist to files inside archive files.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-34952, CRIBL-35075</div> | Resolved a race condition that could cause persistent queues (PQ) to unexpectedly re-initialize. This fix stabilizes the PQ processes under high load or backpressure, eliminating the source of `uncaughtException` errors. |
| PLAT-8108 | We fixed an issue where jobs would appear to be completed, but they would timeout waiting for one final task. |
| PLAT-847 | Cribl.Cloud users can’t provision new SSO users the first time they log in to their tenant URL. |
| CRIBL-36328 | Resolved an issue where the Import from Git function failed in environments requiring a proxy. Pack operations now correctly honor the `HTTP_PROXY` environment variables for cloning and importing Packs. |
| CRIBL-36034 | Fixed an issue that caused jobs to get stuck in a **Running** status after a Worker failed to signal task completion. Cribl now attempts multiple retries to signal task completion, ensuring jobs gracefully end and do not block subsequent jobs. |
| CRIBL-36459 | Resolved an issue where the Leader Node occasionally failed to send configuration updates to all Workers in a Worker Group. This occurred because the Leader incorrectly assumed certain Workers were still undergoing a configuration update. All Workers now receive configuration bundles as expected. |
| CRIBL-35920 | Resolved a small Source persistent queue stability issue for PQs that were set to Smart Mode. The issue occasionally caused non-deterministic data loss during backpressure scenarios in some rare cases. The system now safely resumes collecting data after backpressure events are resolved, ensuring data integrity and preventing data loss. |
| CRIBL-36394 | Fixed an underlying issue that caused `CriblMasterNode is undefined` errors when selecting a Worker Group while viewing Destination logs. The issue that caused the UI to display these errors incorrectly is now resolved. |
| CRIBL-36294 | Fixed an issue where managing Subscriptions using the **Manage as JSON** editor failed. This fix restores the ability to correctly create, update, and delete Subscriptions using the editor. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-36319</div> | Worker initialization no longer hangs if a Redis connection is misconfigured and Redis Functions have client-side caching enabled. Cribl Stream now handles failed Redis connections gracefully during startup, allowing Workers to initialize and data flow to continue even if Redis is unavailable. |
| CRIBL-31800 | The REST Collector response body attribute pagination now correctly supports gzip-compressed HTTP responses. |
| CRIBL-34406 | The REST Collector now ensures that state updates are fully committed before a job ends, preventing race conditions where the next job could start with stale state. |
