# Cribl Edge 4.14.0


Cribl Edge 4.14.0 includes significant performance improvements, new capabilities, and important bug fixes.

## New Features​

This release provides the following improvements:

### Cribl Outpost (Preview)

Cribl Outpost (a Preview feature) is a new solution for managing deployments in restricted environments with multiple data centers or complicated networking setups. Instead of opening firewall rules or configuring third-party proxies, you can now establish Cribl Outposts to handle the control plane traffic between multiple Edge Nodes and the Cribl.Cloud or on-prem Leader.

### Cribl Edge on macOS (Preview)

Cribl Edge is now available on macOS. You can now install and run Cribl Edge on supported macOS environments, monitoring and collecting log files.

### Cribl Terraform Provider (Preview)

The [Cribl Terraform provider](https://registry.terraform.io/providers/criblio/criblio/latest/docs) is now available in Preview from the Terraform Registry. Use the criblio provider to manage Cribl as part of your infrastructure-as-code workflows and make large deployments repeatable and reliable.

### New Cribl.Cloud Regions

We are excited to now support creating Cribl.Cloud Organizations in Switzerland and Singapore AWS Regions (`eu-central-2`, `ap-southeast-1`).

## Experience Improvements

- We’ve increased the granularity of the Route audit logs to provide a more precise audit trail. Instead of logging a generic update event with an ambiguous `id` default, we now log an event for every updated, deleted, and reordered Route.

- You can now improve the efficiency of Fleet reconnections to the Leader after restarts. Set the **Minimum reconnect interval**, **Maximum reconnect interval**, and **Random reconnect interval** settings per Fleet to stagger reconnections so as not to overwhelm the Leader.

- The `C.Mask` expression now supports hashing with the SHA3-256 and SHA3-512 algorithms. This expands the available cryptographic methods for data masking.

- We changed the log level for HTTP 4xx errors from `error` to `warn` to better distinguish between user errors and application errors. This adjustment helps reduce log noise and ensures that error logs are reserved for critical system failures, while still providing a clear warning for client-side issues.

- We added the ability to drop dimensions directly from the Data Preview pane in the UI. This enhancement allows you to easily remove specific metric dimensions that contribute to high cardinality through the UI.

- We added a new environment variable, `CRIBL_GIT_REMOTE_URL`, that enables you to set a Git remote URL from the UI without removing the existing local repository and restarting the Leader.

- Cribl is enabling the use of AWS SDK v3 for self-managed customers and hybrid Workers after additional testing.

-  We have improved the reliability and performance of Cribl-to-Cribl connections over HTTP/TCP, especially under high-load or failure conditions. To prevent your systems from being overwhelmed during a failure, we've added a small, randomized delay to retries. This ensures that not all of your Nodes try to reconnect at the exact same moment, which keeps the system running smoothly and avoids overwhelming destination servers. We've also enabled intelligent retry behavior by default for `CriblHTTPOut` Destinations, helping your Edge Nodes automatically manage their traffic to avoid overloading your Stream Worker Nodes and providing better backpressure protection.

- We enhanced the JSON Array Event Breaker to give you more control over your data. You can now retain and copy top-level fields from the original parent JSON object to each of the new, extracted events. This is particularly useful for logs from services like AWS CloudTrail, EKS CloudWatch, or Google Cloud Audit Logs, where a single log entry contains an array of event records.

- We improved the observability of Sources, Destinations, and persistent queues (PQs) to help you more accurately monitor the health of Worker Processes and manage backpressure. Now when you view any configured integration, you can select the **Status** tab to assess the health status of PQs. This view aggregates data across all Worker Processes to show how many Worker Processes are in a healthy or unhealthy state, making it easier to pinpoint issues.

## Sources and Destinations

- The default value for **Read mode** in the Windows Event Logs Source is now **From last entry** instead of **Entire Log**. This will prevent mass ingestion of data when deploying Edge Nodes with large historical logs.

- We've expanded I/O Observability monitoring to additional integrations, bringing our existing health and performance metrics—request failures, latency, and endpoint health—to new Sources including Azure EventHub, Elastic, and Azure Blob, as well as Destinations including CrowdStrike NextGen SIEM, Azure Sentinel, Webhook, and Exabeam. These metrics are aggregated every minute. You can access these metrics through the Internal Cribl Source and view performance charts directly on each Source and Destination's configuration page for greater visibility into your data pipeline integrations.

- The CrowdStrike Falcon Next-Gen SIEM Destination now defaults the **Request Format** from Raw to JSON when you are configuring a new Destination. This change simplifies the setup process and aligns with the most common configuration.

- In Cribl.Cloud deployments, the default parser for Grafana and Prometheus Remote Write Sources has been updated to [version 3.5.3](https://grafana.com/docs/loki/latest/release-notes/v3-5/#353-2025-07-23), which supports structured metadata across all existing and new configurations.

- The Google Cloud Pub/Sub Destination now includes the **Flush period (sec)** setting for specifying the maximum time to allow between requests.

- Support for sending entity data to the Google Security Operations (SecOps) Destination is now available. Entities like `user` and `asset` are needed for enriching logs and providing additional context. This feature addresses a major roadblock for customers needing to send entity data directly to their SIEM.

- Native support for OAuth authentication is now available for the Kafka and Confluent Cloud Source and Destination. This feature allows administrators to securely connect to Kafka systems that use OAuth for client authentication, providing a modern way to ingest and send data.

- You can now include System State, System Metrics, Windows Metrics, and Windows Event Logs Sources in Packs. As a result, System State, System Metrics, and Windows Metrics Sources now allow configuring more than one instance of Source per Fleet.

- Source and Destination statuses when Minimal metrics are collected have been improved for clarity.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-34266</div>CRIBL-31316</div> | Empty states for Edge Nodes in Event Breaker Ruleset's Import Edge Data modals now consistently display the number of Edge Nodes and allow adding new Nodes. |
| CRIBL-34345 | Resolved an issue where editing default Knowledge Objects in a Subfleet prevented the Config Bundler from creating bundles. The Bundler now correctly handles edits to default objects like Schemas and Grok files, which allows Subfleets to be managed as expected. |
| CRIBL-34266 | Resolved an issue where the Subscriptions monitoring page was unavailable when GitOps was enabled in read-only mode. |
| CRIBL-34966 | Fixed an issue where, when Worker persistence was enabled, restarting connection listeners would fail with `database is locked` or `No route for servername: undefined` error messages. |
| CRIBL-35024 | Resolved an issue where the **Group** field for a new Edge Node defaulted to the wrong Fleet. The field now correctly displays the selected Fleet, which ensures new Nodes are added as expected. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-34996</div> | Fixed a validation issue with the Microsoft Sentinel Destination that prevented the use of new Data Collection Endpoint URLs. The validation now supports Microsoft's new ingestion URL format, which includes `.logs.z1.ingest.monitor.azure.com`. |
| CRIBL-33920 | An outdated time zone library was updated to correct an issue with America/Monterrey, which was incorrectly observing Daylight Saving Time (DST). The updated library now correctly shows this time zone as UTC-6 year-round, aligning with changes made in 2022. |
| CRIBL-34820 | We fixed a bug in the Kafka Source's JSON Schema Registry that caused errors when receiving data. Previously, the system assumed that all schemas were JSON objects and would fail to process messages with a scalar or primitive schema. The fix ensures that the system now correctly handles both object and primitive schemas, preventing these errors. |
| CRIBL-34136 | We fixed an issue in Cribl-to-Cribl connections over HTTP/TCP where a warning message, "Failed to refresh license information for auth token generation," was incorrectly being logged. This warning was a result of an unsupported data transfer mode and did not impact data flow or performance. The fix removes the erroneous warning to prevent confusion.
| CRIBL-31944 | We've improved the Exabeam Destination to resolve "Access denied" errors that could occur after a connection timeout during a file upload. If an upload fails, the Destination will now automatically retry by regenerating the file and re-uploading it to the external object store, ensuring data is not lost. |
| CRIBL-35262 | Fixed an issue where the **Enable load balancing** toggle was missing from Kubernetes Logs Source settings. |
| CRIBL-34744 | Fixed an issue where the **File Monitor** would not delete files, with **Delete files** enabled, when **Idle time** was set to a high number. |


### Other Functional Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-32284</div> | You can now filter Stream Workers and Edge Nodes by partial product version (for example, 4. or 4.13), instead of the full version. |
| CRIBL-35169 |  We have introduced internal performance optimizations for node persistence. |
