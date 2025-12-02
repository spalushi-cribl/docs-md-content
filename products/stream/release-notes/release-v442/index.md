# Cribl Stream 4.4.2


## New Features 

This release provides the following improvements:

Cribl Stream can now send data directly to Elastic Cloud via a new [Elastic Cloud](destinations-elastic-cloud) tile. The new Destination makes it easy to access the search, observability, and security features of Elastic Cloud.

Cribl Stream can now send data to Exabeam security operations platform (SIEM) directly, via a new [Exabeam Destination](destinations-exabeam) tile. While Cribl still supports sending data via a Webhook, the new Exabeam Destination offers superior convenience and performance.

Parquet-enabled Destinations can now [automatically generate Parquet schemas](parquet-schemas#parquet-schemas) based on ingested events, making it far easier to store data in Parquet format when incoming event structure is unknown, among other use cases. Applies to the Filesystem, S3, Google Cloud, Azure Blob, and MinIO Destinations. 

## Corrections

This release includes the following fixes:

### Source Fixes

CRIBL-20444 After upgrade, Workers for a Cribl Stream Amazon Kinesis Data Streams Source now correctly retain state upon init. As a result, the Source correctly resumes reading data after upgrade from the location configured by **Optional Settings** > **Shard iterator start**.

CRIBL-5654 Splunk HEC Sources include redacted auth tokens and a description (if present) in logs.

CRIBL-20502 Splunk HEC Sources now reject empty JSON objects, which previously interfered with Splunk HEC integration and stopped data from reaching the HEC endpoint.

CRIBL-21245 Splunk TCP Sources configured with S2S v4 now honor the `__TZ` field in events received from Windows-based Splunk Universal Forwarders.
 

### Collector Fixes

CRIBL-17566 The S3 Collector now includes a **Disable time filter** option to prevent timestamp conflicts when your Run or Schedule configuration specifies a date range.
  
### Destination Fixes

CRIBL-5654 HTTP Destinations (including HEC) include the number and size of events in error logs.


### Function Fixes

CRIBL-21403 GeoIP Function correctly adds fields to an event.

### Logging Fixes

CRIBL-20253 Reduced excessive logging in Kafka and Splunk TCP Sources and Datadog Destinations.

### Other Functional Fixes

CRIBL-12676 Worker Nodes running in containers now correctly detect number of CPU cores allocated to the container.

CRIBL-18928 `CriblPack` now manages Routes independently so that Routes do not improperly drop events going to Packs.

> The fix for CRIBL-18928 introduced an issue that caused high CPU and memory utilization when using Packs in pre-processing Pipelines, post-processing Pipelines, and QuickConnect. To avoid this issue, Cribl recommends that you upgrade to version 4.4.3 or older. Cribl Stream v.4.4.3 reverts this fix. 
>
{.box .warning}

CRIBL-19794 Cribl Stream now ignores zero-length `.gz` files, which prevent the persistent queue from draining. It also includes the filename when logging PQ errors.

CRIBL-20607 Optimize timing log output to prevent large numbers of logs written in a short time from stopping log output.

CRIBL-21355 Adding wildcards in the **Remove Fields** option no longer causes the [Full Preview](data-preview#simple-versus-full-preview) to drop events in certain circumstances.

CRIBL-21189 The GitOps [read-only](gitops#make-leader-ro) mode no longer prevents organization-level [admins](roles#org) from viewing the [Monitoring](monitoring#monitoring-page) page.

### Diagnostic Fixes

CRIBL-21093 Worker Nodes now create diag bundles correctly when accessed via teleport. 

### Security Fixes

CRIBL-21155 Go version updated to v.1.21.