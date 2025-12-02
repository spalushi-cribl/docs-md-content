# Cribl Stream 4.11.0


Cribl Stream 4.11.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

### Deprecation Notice

Upcoming deprecation for all Elasticsearch API Sources: in an upcoming release, these Sources will ignore extra HTTP headers that do not start with `X-`, `x-`, or `-es`.

### New Cribl.Cloud Domain

With this release, Cribl.Cloud Organizations will start moving under a new, unified domain. You will be able to log in to Cribl.Cloud through <https://app.cribl.cloud/>, and you'll find your portal under `https://app.cribl.cloud/organizations/<organizationId>`. The changes will be rolled out gradually, and the old domain will still be functional, redirecting to the new one as it is introduced.

The changes do not affect the communication between Stream Workers or Edge Nodes and the Leader Node.
You do not need to take any action to ensure the communication keeps working.

### NodeJS Upgrade to Version 20

Cribl has updated NodeJS to version 20. Node.js 18 will reach its end-of-life (EOL) on April 30, 2025, meaning it will no longer receive security patches or updates. By upgrading to Node.js 20, we are ensuring stronger security, better performance, and long-term support for Cribl’s products.

### Cribl Container Image Upgrade

We're excited to announce that the Cribl Docker distribution has been updated to use Ubuntu 24.04 as its underlying operating system. This upgrade brings significant improvements in security, performance, and compatibility, ensuring a more robust and future-proof platform for your data routing and processing needs.

Alternatively, Cribl is also published on [Wolfi Linux](https://hub.docker.com/r/cribl/cribl/tags?name=wolfi) for enhanced security requirements.

## New Features

This release provides the following improvements:

### Metrics Pipeline Builder

The new Metrics Pipeline Builder simplifies the configuration of Functions for processing metrics. It automates most of the setup process for dropping or aggregating metrics directly from the Data Preview in your Pipeline. Simply select the metrics you want to aggregate, and the interface will automatically add Functions to your Pipeline with an initial configuration based on your selection. This improved workflow makes it easier to aggregate and drop any metrics contributing to high cardinality.

### New Aggregate Metrics Function

We added a new Aggregate Metrics Function designed specifically for processing metrics. The Aggregate Metrics Function performs common metrics aggregations (such as count, sum, average) to help you pre-aggregate metrics data. This helps optimize your metrics before sending them to downstream storage or analysis tools, which improves performance and reduces costs. It supports dimensional metrics formats, including StatsD, Prometheus, and OTLP Metrics.

### Improved Encryption Key ID Generation

We've implemented a new key ID generation algorithm to provide globally unique encryption key IDs across all Worker Groups.

### Suggested Git Commits

In the Git Changes modal, [Cribl Copilot](/copilot/) (where enabled) can now automatically suggest commit messages by analyzing changes against the target branch. You can enable or disable this feature at **Settings > Global > Git Settings > Copilot**.

## Experience Improvements

- In Cribl.Cloud, you'll see a newly updated Plan and Invoices page. This update provides
  a clearer picture of your invoice, and provides a monthly breakdown of your
  credits consumed by product and infrastructure. We've updated the [Understand Your Cribl.Cloud Bill and Plan](cloud-billing) docs to match.
- The **Reload period** setting for Lookup Functions has been promoted above Advanced Settings for easier access. Enable this setting in single-instance deployments to eliminate the need for application restarts when lookup files are updated.
- Encryption keys in Distributed deployments now have the associated Worker Group for reference.
- In Kubernetes deployments, Cribl Stream now automatically sets the `CRIBL_K8S_CPU_LIMIT` environment variable to the `resources.limits.cpu` setting in the pod. Cribl Stream then uses the value as a CPU limit to determine how many Worker Processes to start. This prevents performance problems like resource contention and pod instability from exceeding CPU limits. Additionally, the default resource limits for Cribl Stream have been increased from 2CP/4GB to 6CPU/8GB.
- You can now create a Worker Group directly from the **Products** sidebar by hovering over Worker Groups in the sidebar and selecting the **+** button.
- We enhanced the diagnostic bundle service to improve our customer support experience. When you send a Leader diagnostic to Cribl, you can now optionally include data about the "top talkers" in your deployment: the top five highest-volume Sources, Destinations, Pipelines, Routes, and Packs. This information provides our support team with a better understanding of your data flows so that they can resolve incidents faster.

## Sources and Destinations

- The Amazon Kinesis Data Streams Destination now uses the ListShards API (1,000 calls/sec per stream) for stream validation during initialization. This provides significantly higher rate limits than the DescribeStream API (10 calls/sec per account), which remains available. This change minimizes throttling during initialization, preventing Worker Process failures and ensuring successful data flow.
- The Google Cloud Logging Destination now supports custom **Indexed Fields**. You can use JavaScript expressions to populate these fields, enriching your LogEntries for enhanced analysis and correlation within Chronicle.
- The ClickHouse Destination now allows troubleshooting schema mismatches using the new **Log last schema mismatch** advanced setting.
- The Windows Event Forwarder Source now includes a `Max requests per socket` setting to enhance performance. By limiting the number of HTTP requests per socket connection, it distributes the workload and prevents resource exhaustion.
- You can now specify an optional endpoint suffix to connect to Azure Blob Storage in different regions, such as Azure China. The default remains `core.windows.net`.
- Splunk TCP Source, Splunk Load Balanced Destination, and Splunk Single Instance Destination now offer granular compression control for v4 S2S, allowing for optimized data transfer.
- The Datadog Destination now includes a **Batch by tags** advanced setting, allowing you to control how log events are grouped into batches. By default, log events are batched by API key and Datadog tags, ensuring logical grouping. However, high cardinality in Datadog tags can cause excessive batches, leading to performance issues such as `408` errors and resource exhaustion. Disabling this setting batches log events solely by API key, reducing concurrent requests and improving performance.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28464</div>| The Members and Teams page is now available in single-instance deployments with a Free license. We also fixed an issue for all deployments that temporarily prevented user account creation. |
| CRIBL-30277 | Resolved an issue with the FSSignalEmitter signal emitter that occurred because it was pointed at an incorrect location. It now correctly targets the local instance state directories on local disks. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-15059</div>| The Amazon Kinesis Data Streams Source now requires a stable Leader Node. Leader outages will prevent new shard readers from starting, delaying changes like increasing shard counts. Existing streams will continue to process data. |
