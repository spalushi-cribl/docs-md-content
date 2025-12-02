# Cribl Stream 4.10.0


Cribl Stream 4.10 introduces powerful new features and enhancements to improve performance and overall usability.

## New Features

This release provides the following improvements:

### New Zscaler Cloud NSS Source

The new Zscaler Cloud NSS Source allows you to receive log data from Zscaler's Nanolog Streaming Service (NSS) to optimize data for long-term retention, threat analysis, and SIEM integration.

### NetFlow & IPFIX

The NetFlow Source has been renamed to NetFlow & IPFIX to reflect its expanded capabilities. This update introduces:

- Added support for IPFIX (a.k.a. NetFlow v10), enabling parsing and handling of IPFIX messages.
- IPFIX includes support for template records and options templates.

### Metrics Inspector View in Pipelines

Cribl Stream now includes a metrics-first view in the Data Preview pane for Pipelines and Routes, designed to help users make informed cardinality management decisions when working with metrics-heavy data structures. The first release of this new view provides a high-level summary of metrics events as well as the ability to explore individual metrics to examine dimensions, the number of unique values, and the percentage each metric represents of the total series.

### Source Persistent Queue Notifications

Notifications on Sources can now trigger on a Persistent Queue Usage condition to alert you when disk usage for a queue exceeds a specified threshold. These alerts help you monitor and respond when a persistent queue is running out of storage.

### Personal Identity Verification (PIV) Authentication

Cribl now supports Personal Identity Verification (PIV) authentication using smart cards like the Common Access Card for on-prem deployments.

### Improved Leader High-Availability

This release includes significant performance enhancements for Leader high-availability, including faster failover times, especially for Leaders behind load balancers. It also introduces independent health check endpoints and improved health reporting for standby Leaders.

## Experience Improvements

- The community spoke and we listened: this release introduces a new `decrypt` function in the CLI, allowing you to decrypt configuration secrets such as encryption keys and passwords. This function complements the existing `encrypt` function and enhances troubleshooting, particularly in environments created through automation.
- To reflect the range of scenarios beyond username/password authentication, the login failure message for the UI now reads "Authentication unsuccessful. Please try again."
- You can now enable or disable Cribl Copilot outside the Cribl UI by editing the `ai.yml` file.
- The user interface now detects version mismatches between the Leader Node UI and the backend. When versions differ, a dismissible notification appears with an option to update versions if desired.
- Cribl Stream now displays up to 100 commits in your Git commit history, allowing you to view a larger portion of your commit history.
- Users are now prompted to save changes when clicking outside a modal to prevent accidental loss of edits.
- When inviting Members to your Cribl.Cloud Organization, you can now prefill their information with First and Last name.
- Local Users in on-prem deployments can now change their own password and account information using the **My profile** page.
- In Cribl.Cloud, you can now map a Team to multiple IDP groups to simplify complex group mappings.
- Users can now generate a CPU profile, heap snapshot, or both from the CLI using new `diag` subcommands: `perf`, `cpuprofile`, and `heapsnapshot`.

## Sources and Destinations

- Internal log events are created when event processors (EPs) are blocked, at the `debug` or `silly` logging level. Each event includes the Source (`inputId`) and total blocked EPs (`total`), and specifies the Destinations causing the blockages (`blockingOutputs`).
- We've introduced three new socket management settings to improve resource utilization and connection stability for the following TCP-based Sources: Syslog, Splunk TCP, TCP JSON, TCP, Cribl TCP, and Appscope.
  - **Socket idle timeout**: Closes inactive sockets after a specified duration, preventing resource exhaustion from idle connections.
  - **Socket max lifespan**: Limits the maximum duration a socket can remain open, regardless of activity, further improving resource utilization and mitigating potential issues like TCP pinning.
  - **Forced socket termination timeout**: Provides additional time for client acknowledgment before forcibly closing sockets terminated by idle timeout or exceeding their lifespan, ensuring timely connection cleanup and preventing resource leaks.
- The S3 Collector now supports collecting data from Splunk Dynamic Data Self Storage (DDSS) datasets. This allows you to efficiently replay events directly from DDSS buckets without understanding the DDSS folder structure or journal file format. A new **Partitioning scheme** drop-down in the S3 Collector settings provides the DDSS option.
- The Azure Blob Storage Destination now supports selecting a **Blob access tier**, allowing you to optimize storage costs and performance based on data access patterns.
- On Source and Destination modals' **Charts** tabs, labels now describe the displayed data more clearly. Also, the charts now identify the time range for the data shown (10 minutes).
- The Syslog Source now provides the **Max active connections** setting under **Advanced Settings**. This setting, previously configurable only through JSON modifications, is now accessible through the UI.
- The Amazon S3 Destination now supports **Compression levels**, giving you control over the balance between speed and efficiency. Higher compression levels significantly reduce storage costs and network bandwidth usage, but may slightly increase processing time. Lower levels prioritize faster processing.
- The SignalFx Destination now supports additional realms: `us2`, `eu1`, `eu2`, `au0`, and `jp0`.
- The Datadog Destination no longer sets a default value for **Severity**.
- The Group name and Worker mode are now added to events when the corresponding tags (`cribl_group` and `cribl_mode`) are present in the **Destination Processing Settings** -> **Post Processing System Fields** entries. This enhancement simplifies data filtering and analysis based on Fleet membership and Worker mode in Cribl Stream and Cribl Lake.
- A new **Delete files** setting is now available on the File Monitor Source, which allows you to delete files after they have been processed. This feature is available only for Manual Discovery mode. By enabling this option, you can optimize disk space and simplify log management by automatically removing processed log files.
- The Azure Blob Storage Destination now logs a `Blob upload complete` message upon successful upload.

## Corrections

This release contains the following bug fixes:

### Security Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-29460</div>| Removed Certificate Authority (CA) cloning to enhance security and prevent the accidental creation of invalid certificates. |
| CRIBL-23773 | This release includes a security enhancement for Leader Nodes. Leader nodes now provide the option to disable UI access on port 4200. This can be configured through the CLI (`./cribl mode-master -f option`) or within **Settings > Global > Distributed Settings > Leader Settings**. |

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-29124</div>| In single-instance modes of Cribl Stream, orphaned scheduled jobs now resume as expected and finish instead of stalling. |
| CRIBL-27546 | Fixed an issue where Sources and Destinations displayed inconsistent time ranges on charts for Worker Groups. The charts now show statistics for the last 10 minutes. |
| CRIBL-29662 | Resolved a Monitoring user interface issue that prevented users from resizing panes on all pages accessed through the Data submenu. |
| CRIBL-28985 | Fixed a user interface issue where recently deleted Worker Groups continued to appear in the Recently Used list. |
| CRIBL-25072 | Fixed an issue that caused some metrics to be dropped, such as a Worker Node’s CPU and memory. |
| CRIBL-26715 | Resolved an issue in the Event Breaker rule editor where logic, including filter conditions, was incorrectly applied in the preview window. |
| CRIBL-28944 | Fixed an issue that prevented failover to a standby Leader with active connections when the primary Leader was terminated. |
| CRIBL-29735 | Resolved an issue where changing an existing Webhook Notification Target to an invalid URL caused continuous error logging and blocked further configuration updates. |
| CRIBL-28333 | Resolved an issue where network restrictions (e.g., proxies/firewalls) could intercept the package download request from the Linux bootstrap script, resulting in an error HTML page instead of the expected TGZ file. The script now validates downloads to ensure only valid Cribl packages are installed. |
| CRIBL-29986 | Fixed an issue where newly created sample files for Packs were not automatically selected after saving. |
| CRIBL-25241 | Kubernetes and Docker containers running Cribl Stream now [shut down gracefully](deploy-docker#shut-down-docker-containers), waiting for the `cribl` processes to all exit before shutting down.|

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-25924</div>| Errors from bad queries in Database Collection jobs are now shown in the **Task Errors** tab, improving visibility and aiding in troubleshooting. Previously, these errors were in the **Logs** tab, making them difficult to find. |
| CRIBL-29275 | The Elasticsearch Destination now logs detailed reasons for event failure errors in `warn` log messages, showing up to five reasons. All reasons are available at the `silly` logging level. |
| CRIBL-28716 | When configuring a REST Collector with pagination for both the Discover and Collect steps, the field used to track the current page for the discover step now correctly handles pagination without interfering with the collect step. Previously, this could cause the collection step to query unexpected pages of data. |
| CRIBL-29527 | The Splunk Load Balanced Destination now preserves the original data type of indexed fields, ensuring that values are not automatically cast to numbers. Previously, if a value could be interpreted as a number, it was cast to a numeric type, removing leading zeros. |
| CRIBL-29398 | The Azure Blob Storage Destination now correctly handles file names and prefix expressions, ensuring that all characters, including `F` and `E`, are preserved in the uploaded object names. |
| CRIBL-28922 | When using AWS KMS with AssumeRole enabled, the Cribl server now starts correctly. Previously, enabling AssumeRole for AWS KMS caused the server to fail. |
| CRIBL-29520 | The Google Cloud Logging and OTLP-based Destinations now correctly handle non-retryable errors in the persistent queue. |
| CRIBL-20622 | HTTP-based and TCP JSON-based Destinations have improved logging when writing failed payloads to disk. |
| CRIBL-28818 | We have removed the **Dynatrace processing type** option from the Dynatrace HTTP Destination. This change aligns with Dynatrace recommendations and does not impact telemetry delivery behavior. |

### Other Functional Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28740</div>|  Improved API response times by limiting the git cache warmup to 15 Worker Groups. This reduces latency for deployments with a large number of Worker Groups. Default Groups will always have a git cache warmup for their group regardless of limits in environment settings. |
