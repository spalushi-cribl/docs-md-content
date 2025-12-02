# Cribl Stream 4.14.0


Cribl Stream 4.14.0 includes significant performance improvements, new capabilities, and important bug fixes.

## New Features​

This release provides the following improvements:

### Sensitive Data Scanning with Cribl Guard

With Cribl Guard enabled on your Destinations, Cribl Stream scans incoming data in real time to identify sensitive data in your events, then applies corrective measures to that data before it leaves Cribl and reaches your downstream Destinations.

Cribl Guard can automatically protect your Destinations with a simple workflow, or you can customize Cribl Guard using the Cribl Guard Function in your Pipelines.

### New Cribl.Cloud Regions

We are excited to now support Cribl-managed Worker Groups in Switzerland and Singapore for both AWS (`eu-central-2`, `ap-southeast-1`) and Azure (Switzerland North, Southeast Asia).

You can also create Cribl.Cloud Organizations in Switzerland and Singapore AWS regions.

### Cribl Terraform Provider (Preview)

The [Cribl Terraform provider](https://registry.terraform.io/providers/criblio/criblio/latest/docs) is now available in Preview from the Terraform Registry. Use the criblio provider to manage Cribl as part of your infrastructure-as-code workflows and make large deployments repeatable and reliable.

## Experience Improvements

- We’ve increased the granularity of the Route audit logs to provide a more precise audit trail. Instead of logging a generic update event with an ambiguous `id` default, we now log an event for every updated, deleted, and reordered Route.

- You can now improve the efficiency of Worker Group reconnections to the Leader after restarts. Set the **Minimum reconnect interval**, **Maximum reconnect interval**, and **Random reconnect interval** settings per Worker Group to stagger reconnections so as not to overwhelm the Leader.

- The `C.Mask` expression now supports hashing with the SHA3-256 and SHA3-512 algorithms. This expands the available cryptographic methods for data masking.

- We changed the log level for HTTP 4xx errors from `error` to `warn` to better distinguish between user errors and application errors. This adjustment helps reduce log noise and ensures that error logs are reserved for critical system failures, while still providing a clear warning for client-side issues.

- We added the ability to drop dimensions directly from the Data Preview pane in the UI. This enhancement allows you to easily remove specific metric dimensions that contribute to high cardinality through the UI.

- We added a new environment variable, `CRIBL_GIT_REMOTE_URL`, that enables you to set a Git remote URL from the UI without removing the existing local repository and restarting the Leader.

- For on-prem Cribl Stream users, we've enabled the new API job process that
  provides you with better Leader performance.

- Cribl is enabling the use of AWS SDK v3 for self-managed customers and hybrid Workers after additional testing.

- We have improved the reliability and performance of Cribl-to-Cribl connections over HTTP/TCP, especially under high-load or failure conditions. To prevent your systems from being overwhelmed during a failure, we've added a small, randomized delay to retries. This ensures that not all of your Nodes try to reconnect at the exact same moment, which keeps the system running smoothly and avoids overwhelming destination servers. We've also enabled intelligent retry behavior by default for `CriblHTTPOut` Destinations, helping your Edge Nodes automatically manage their traffic to avoid overloading your Stream Worker Nodes and providing better backpressure protection.

- We enhanced the JSON Array Event Breaker to give you more control over your data. You can now retain and copy top-level fields from the original parent JSON object to each of the new, extracted events. This is particularly useful for logs from services like AWS CloudTrail, EKS CloudWatch, or Google Cloud Audit Logs, where a single log entry contains an array of event records.

- We improved the observability of Sources, Destinations, and persistent queues (PQs) to help you more accurately monitor the health of Worker Processes and manage backpressure. Now when you view any configured integration, you can select the **Status** tab to assess the health status of PQs. This view aggregates data across all Worker Processes to show how many Worker Processes are in a healthy or unhealthy state, making it easier to pinpoint issues.

- Copilot Editor now supports the OCSF 1.5.0 and OCSF 1.6.0 data schemas. You can now convert data from these schemas without additional support.

## Sources and Destinations

- A new **Wiz Webhook Source** is now available in Cribl Stream. This HTTP-based Source makes it easy to collect and control all your Wiz Defend alert data. You'll find it in the new **Wiz folder** in the Source catalog. To make sure you get all the raw data you need, this Source defaults to the "Cribl - Do Not Break" Event Breaker Ruleset. This gives you more control and flexibility over how you manage your security data.

- We've expanded I/O Observability monitoring to additional integrations, bringing our existing health and performance metrics—request failures, latency, and endpoint health—to new Sources including Azure EventHub, Elastic, and Azure Blob, as well as Destinations including CrowdStrike NextGen SIEM, Azure Sentinel, Webhook, and Exabeam. These metrics are aggregated every minute. You can access these metrics through the Internal Cribl Source and view performance charts directly on each Source and Destination's configuration page for greater visibility into your data pipeline integrations.

- The CrowdStrike Falcon Next-Gen SIEM Destination now defaults the **Request Format** from Raw to JSON when you are configuring a new Destination. This change simplifies the setup process and aligns with the most common configuration.

- In Cribl.Cloud deployments, the default parser for Grafana and Prometheus Remote Write Sources has been updated to [version 3.5.3](https://grafana.com/docs/loki/latest/release-notes/v3-5/#353-2025-07-23), which supports structured metadata across all existing and new configurations.

- The Google Cloud Pub/Sub Destination now includes the **Flush period (sec)** setting for specifying the maximum time to allow between requests.

- Support for sending entity data to the Google Security Operations (SecOps) Destination is now available. Entities like `user` and `asset` are needed for enriching logs and providing additional context. This feature addresses a major roadblock for customers needing to send entity data directly to their SIEM.

- The REST Collector can now handle Newline Delimited JSON (NDJSON) and plaintext during discovery. A new, optional strict discovery parsing mode lets you explicitly set the response format for parsing, providing a more direct and configurable way to handle data from a wider variety of APIs.

- Native support for OAuth authentication is now available for the Kafka and Confluent Cloud Source and Destination. This feature allows administrators to securely connect to Kafka systems that use OAuth for client authentication, providing a modern way to ingest and send data.

- The Splunk Search Collector now provides a **Preserve escaped characters** toggle to preserve backslash-escaped characters in **Search** queries.

- The Amazon S3 and Security Lake Sources now offer an **Include notification metadata** option to enrich events with the original SQS notification payload. This embeds the payload in a `__sqsMetadata` field, giving you access to `awsRegion` and `eventTime` for precise latency measurement and region-based routing.

## Packs

- You can now build Packs that include Collectors, including REST Collectors and S3 Collectors. This allows you to track and share configurations for a greater variety of integrations across your Worker Groups or environments.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-34266</div> | Resolved an issue where the Subscriptions monitoring page was unavailable when GitOps was enabled in read-only mode. |
| CRIBL-34966 | Fixed an issue where, when Worker persistence was enabled, restarting connection listeners would fail with `database is locked` or `No route for servername: undefined` error messages. |
| CRIBL-35024 | Resolved an issue where the **Group** field for a new Worker Node defaulted to the wrong Worker Group. The field now correctly displays the selected Worker Group, which ensures new Nodes are added as expected. |
| AI-2157 | We increased the character limit for custom schemas in Copilot Editor. Now you can upload custom schemas up to 100,000 characters with less risk of experiencing an upload error. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-34996</div> | Fixed a validation issue with the Microsoft Sentinel Destination that prevented the use of new Data Collection Endpoint URLs. The validation now supports Microsoft's new ingestion URL format, which includes `.logs.z1.ingest.monitor.azure.com`. |
| CRIBL-33920 | An outdated time zone library was updated to correct an issue with America/Monterrey, which was incorrectly observing Daylight Saving Time (DST). The updated library now correctly shows this time zone as UTC-6 year-round, aligning with changes made in 2022. |
| CRIBL-34820 | We fixed a bug in the Kafka Source's JSON Schema Registry that caused errors when receiving data. Previously, the system assumed that all schemas were JSON objects and would fail to process messages with a scalar or primitive schema. The fix ensures that the system now correctly handles both object and primitive schemas, preventing these errors. |
| CRIBL-34136 | We fixed an issue in Cribl-to-Cribl connections over HTTP/TCP where a warning message, "Failed to refresh license information for auth token generation," was incorrectly being logged. This warning was a result of an unsupported data transfer mode and did not impact data flow or performance. The fix removes the erroneous warning to prevent confusion.
| CRIBL-31944 | We've improved the Exabeam Destination to resolve "Access denied" errors that could occur after a connection timeout during a file upload. If an upload fails, the Destination will now automatically retry by regenerating the file and re-uploading it to the external object store, ensuring data is not lost. |
| CRIBL-23495 | We've fixed an issue where temporary Parquet directories were not being removed after a Collector job completed, which could cause storage volume issues. This fix impacts the S3, Google Cloud Storage, and Azure Blob Collectors. Individual Collector tasks will now handle the cleanup of their own staging directories, ensuring they are properly removed after a collection run in both preview and full-run modes. |
| CRIBL-31779 | We've resolved an issue in the REST Collector where it would fail to parse a Discover response that was a single integer with a value of 0. The Collector will now correctly accept and process a text/plain response with a value of 0 when configured for strict discovery parsing with plaintext support. This fix eliminates the "Failed to parse discover result" error for this specific scenario. |
| CRIBL-32506 | The NetFlow v9 Source has been re-architected to address a critical scaling bottleneck that was causing high load and crashes on the Leader. The new design improves Worker-side caching, allowing for resilient and performant data processing at scale by reducing the load on the Leader and preventing data flow interruptions. |
| CRIBL-34744 | Fixed an issue where the **File Monitor** would not delete files, with **Delete files** enabled, when **Idle time** was set to a high number. |


### Other Functional Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-32284</div> | You can now filter Stream Workers and Edge Nodes by partial product version (for example, 4. or 4.13), instead of the full version. |
| CRIBL-35169 |  We have introduced internal performance optimizations for node persistence. |
