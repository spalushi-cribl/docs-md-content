# Cribl Edge 4.2


> ##### Breaking Change
>
> A critical regression caused Fleets to become unresponsive, preventing access to Sources and Destinations. This issue was caused by incorrectly encrypted credentials. Users should install (or upgrade to) Cribl Edge v.4.2.1 or later when available. 
>
{.box .warning}

Cribl Edge has been busy leveling up and at 4.2.0 we are flying high as kites. 

## New Features 

This release provides the following improvements:

### Permissions-based Access Control 

Cribl now offers permission-based access controls for enterprise users with distributed deployments. Using [Members and Permissions](members), you can assign access and capabilities to users independently at the Product and Fleet levels.

### File Monitor Enhancements

The [File Monitor](sources-file-monitor) Source now supports monitoring and processing compressed (gzip and zstd), archived, and binary  log files based on lines and records extracted from the content. Binary files are broken into base64-encoded chunks and streamed. 

The File Monitor Source will also no longer exclude `*.gz` files by default. This change will not affect new installations. 

#### Updated Defaults for Manual Discovery Mode 

There has been an update to the defaults on the **Manual** discovery mode in the File Monitor Source (Linux). The **Path** is now set to the `/var/log` directory, with an **Allowlist** of `*/log/*` and `*log`, and a **Max depth** of `4`.

#### One-time File Ingesting

Using the File Explorer on Cribl Edge, you can now ingest file content and send it to Routes or Pipelines for further processing or downstream destinations. This is a useful option for testing or troubleshooting your configurations.

### System State Source is Steady Collecting 

The [System State](sources-system-state) Source improvements include more collectors, such as Listening Ports, Logged-in Users, and Services.

### Enhanced User Experience for Cribl Edge and AppScope integration

Cribl Edge now displays the AppScope connection status and configuration for processes scoped, making it easier to monitor the AppScope Source.

### Prometheus Edge Scraper

Edge is edgier than ever before thanks to the new [Prometheus Edge Scraper](/edge/sources-edge-prometheus). In addition to the functionality already supported in the existing Prometheus Scraper, this Source is designed to work seamlessly in Kubernetes environments; and no longer uses internal jobs framework allowing it to handle large-scale Cribl Edge deployments. This Source now supports disk spooling.

> ##### Deprecation Notice
>
> The following resources, previously badged Deprecated, are no longer supported as of this release. Please transition your configurations accordingly:
> 
> - The deprecated Prometheus Source in Edge has been disabled and no longer appears on the UI. Use [Prometheus Edge Scraper](sources-edge-prometheus) instead. 
>
{.box .warning}


## Corrections

This release includes the following fixes:

CRIBL-18400 The **File Monitor** Source and the **Explore** > **Files** tab UI, the **Manual** mode now honors the **Max depth** setting.

CRIBL-18473 (Linux) System Metrics Source now emits "Per interface" metrics when set to "All".

CRIBL-17685 When Cribl Stream loses its connection to a downstream indexer, diverting events to a different configured indexer, Splunk Load Balanced Destinations will now send these events in correct form. Prior to this fix, Stream sometimes omitted internal fields that Splunk requires (`_linebreaker` and `_done`), resulting in Splunk merging content from multiple events together upon arrival.

CRIBL-18465: When rotating container logs, the Kubernetes Logs Source now handles saved cursor positions for existing containers.


