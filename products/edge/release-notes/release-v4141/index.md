# Cribl Edge 4.14.1


Cribl Edge 4.14.1 includes significant performance improvements, new capabilities, and important bug fixes.

## New Features​

This release provides the following improvements:

### New Cribl.Cloud Home Page

We’ve updated the Cribl.Cloud UI, so you can augment your workflows from any given Workspace. From the new landing page, you can:

- Gain a bird’s eye view of the health and performance of your Workspace.
- Get at-a-glance health status for your Worker Groups and Fleets.
- Jump in with quick actions, such as creating a new Source from a Stream Worker Group or Edge Fleet.
- Track changes on usage and performance from volumes stored in Lake or queries processed in Search.

![New Cribl.Cloud Home Page](workspace-release-notes.png)

### SSO Authentication for Multiple Organizations in Cribl.Cloud

Administrators can now manage [SSO authentication for multiple Organizations](https://docs.cribl.io/edge/manage-multiple-orgs) that share the same email domain.

### IAM Admin Permission in Cribl.Cloud

The IAM Admin is a new Organization-level Permission that can manage the Organization's SSO configuration and invite, update, and remove Members without having access to data engineering functions. This reduces the risk of unauthorized data access or modification.

### Google Cloud Chronicle API Destination

The new **Google Cloud Chronicle API** Destination lets you send data to Google Cloud Security Operations using the Chronicle v1alpha `ImportLogs` ingestion method. Use this Destination to add labels to individual events, not only at batch level, and to take advantage of the improved performance and scaling offered by Chronicle API.

## Experience Improvements

- FinOps Center improvements:
  - New credit spend projection charts per-product, based on current credit spend.
  - View your usage per product, and answer questions such as the top 5 Workspaces that are driving the most usage.
  - Product-specific tabs provide more visibility into your consumption and usage, enabling you to view usage by metrics specific to your products (like Worker Groups for Stream and Workspace for Search and Lake).

## Sources and Destinations

- Support for encrypted private keys has been extended to the gRPC protocol. A **Passphrase** setting is now available in the TLS settings for both the Model Driven Telemetry Source and the OpenTelemetry Source when configured for gRPC.

- To improve clarity for Destinations that use Amazon S3, we've added a **Key Prefix** column to their respective configuration tables. You can now see the S3 object prefix being used, simplifying auditing and troubleshooting.

- The Syslog Source won’t display the **Enable TCP load balancing** setting until a **TCP port** is provided.

- The Google Cloud Logging Destination has a new configuration option, **Validate and correct log name**. When enabled, log names are sanitized by replacing any illegal characters with underscores. This prevents ingestion failures and excessive event dropping caused by invalid log names.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-20667 and CRIBL-33599</div> | Fixed a bug that incorrectly displayed and extracted UTC timestamps as one hour behind the correct time in the Data Preview window. |
| CRIBL-35583 | Resolved a display bug in the Manage as JSON tab where line numbers 10,000 and above weren’t displaying correctly. Line numbers now display accurately for large configuration files. |
| CRIBL-35486 | Fixed an issue that incorrectly prevented some Cribl.Cloud customers from editing log levels when the TTL logging feature was disabled. The system now correctly checks the feature flag, allowing users to save log level changes without error. |
| CRIBL-35111 | Fixed an issue where the Outpost table would not remember added or removed columns as well their custom sizing or order. |
| CRIBL-35261 | Connecting through an Outpost now allows enabling or disabling TLS separately for both connections: between an Edge Node and the Outpost, and between the Outpost and the Leader. |
| CRIBL-35624 | You can now configure the number of processes for an Outpost Node by configuring `outpost.procs` in `service.yml`, or via `CRIBL_SERVICE_OUTPOST_PROCS`. |
| CRIBL-32315 | Fixed a bug where enabling **Reuse Redis connections** in Pipelines broke client-side caching for Redis read operations. When connection reuse was enabled with a low connection limit, each Pipeline read bypassed the local cache and issued a GET to Redis. |
| CRIBL-35253 | Resolved an issue that prevented `C.Lookup()` expressions from resolving correctly within Edge Leader Fleet Mappings. The Leader API process can now properly validate and reference lookups from any Fleet directory. |
| CRIBL-34648 | Resolved a rare `uncaughtException` error that could occur in the API process during graceful shutdown. This fix ensures that logging continues without crashing the process, even if the underlying log stream (like stdout/stderr) closes prematurely. |
| CRIBL-33806 | Fixed an issue where using an Aggregation Function in a Pack Pipeline could cause the `cribl_route` field to incorrectly include the names of unrelated Routes and Subscriptions. The `cribl_route` field now accurately reflects only the Routes that processed the event. |
| CRIBL-35132 | Resolved a regression in PathFilter time parsing that broke time formats without spaces between units (such as `%H%M`). |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-35054</div> | Fixed the Datadog Destination when sending in plain/text, where events were incorrectly wrapped in square brackets; events are now output as raw text separated by newlines. |
| CRIBL-29663 | Fixed an issue in the Cribl Internal Metrics Source where blocked Destinations could cause memory problems. Now, if too many metrics build up, extra metrics are dropped and a warning is logged. |
| CRIBL-30490 | The **Validate server certs** setting was not applied to authentication for Azure Data Explorer and Event Hubs Destinations, causing certificate validation to be skipped even when enabled. This led to potential network errors and failed connections. |
| CRIBL-29004 | Resolved an issue where sending data to the Exabeam Destination from an Edge Node on Windows failed with a "file missing, nothing to moveToFinal" error. |
