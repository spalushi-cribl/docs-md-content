# Cribl Edge 4.8.2


This maintenance release of Cribl Edge includes bug fixes and new features.

## New Features

This release provides the following new features:

### NetFlow v9 Source

You can now use Cribl to collect, route, and control [NetFlow v9](sources-netflow) data records
sent via UDP.

### NetFlow v5 and v9 Destination

Route unmodified NetFlow v5 and v9 options, template, and data records to a
downstream NetFlow Collector via the new [NetFlow Destination](destinations-netflow).

### CrowdStrike Falcon Next-Gen SIEM Destination

The new [CrowdStrike Falcon Next-Gen SIEM Destination](destinations-crowdstrike-next-gen-siem)
allows you to send data directly to CrowdStrike Falcon Next-Gen SIEM for
advanced threat detection and analytics. Simply provide your SIEM endpoint and
authentication token.

## Experience Improvements

- We added a new property, `__winEvent`, to events collected by the [Windows
  Event Logs Source](sources-windows-event-logs). This property contains additional data from logs (`UserData`,
  `DebugData`, `BinaryEventData`, and `ProcessingErrorData`) if available. When
  you're sending events to the [Cribl TCP](destinations-cribl-tcp#advanced-settings) or [Cribl HTTP](destinations-cribl-http#advanced-settings) Destinations, Cribl Edge
  excludes the `__winEvent` property by default. You can remove the property from
  **Excluded fields** on these Destinations to send it along.

- In the [Windows Event Logs Source](sources-windows-event-logs#advanced-settings), 
  the **Polling interval** and **Batch size properties** are now located in the
  **Advanced Settings** tab (these settings are visible and applicable only when you
  toggle **Use Windows Tools** on.)

- The [OTLP Metrics Function](otlp-metrics-function) now supports period (`.`) and underscore (`_`)
  separated prefixes allowing you to specify more precise attribute matching.

- [Destination health indicators](monitoring#source-and-destination-health-status)
  are now based on consistent time metric queries and provide more accurate
  Destination health status colors. In prior releases, Destination health status
  colors sometimes displayed incorrect health status indicators.

## Sources and Destinations

- You can now dynamically set the Site name in the [Exabeam Destination](destinations-exabeam) using
  JavaScript expressions. This allows you to reference fields like
  `cribl_pipeline`, providing greater flexibility and precision in categorizing
  data as it lands in Exabeam.

- The [Splunk HEC Source](sources-splunk-hec) now supports the secrets
  store, so you can securely store and manage authentication tokens.

- When using the [Azure Blob Source](sources-azure-blob) or [Destination](destinations-azure-blob),
  you can now take advantage of Azure service principal authentication.

- We've enhanced the Kafka and Confluent Cloud Sources/Destinations with new
  configuration options to improve their reliability and resilience when
  interacting with schema registries. These settings empower administrators to
  fine-tune connection behavior, request timeouts, and retry attempts, ensuring
  effective handling of transient errors.

- We now support IAM roles for authentication for the Google Cloud Pub/Sub
  Source and the Google Cloud Storage Collector Source.

- The [Exabeam Destination](destinations-exabeam) now supports the (`me-central2`) Dammam region.

- File system-based Destinations can now handle various error conditions and are
  more reliable when handling file uploads. With the refined retry logic, failed
  uploads are now more likely to succeed, minimizing data loss and ensuring a
  smoother data flow experience.

- The system now logs access denied errors when attempting to write to Google
  Cloud Storage. This will help users troubleshoot permission issues.


## Corrections

This release contains the following bug fixes:

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-26944</div>|Windows Event Logs Source incoming events without a valid provider did not include the `_raw` property. This happened in the Windows Event Logs Source when **Render event message strings** was enabled.|
|CRIBL-26466|Teleporting into a Windows Edge Node and then attempting to browse a file path containing `%40` or `%4V` resulted in a `URI malformed` error.|
|CRIBL-26972|Some users weren't able to add a file to the File Monitor Source using the **Explore > Files > Monitor this file** path or using the **Actions** column, **Monitor** option.|
|CRIBL-17579|To minimize stale Git locks that block commits, we increased the **Git timeout** default from 60 seconds to 5 minutes. You can configure this timeout.|
|CRIBL-26709|The Elasticsearch API Source now ignores empty lines in payloads. This change addresses a problem encountered by users of recent Filebeat versions (8.14.3 and newer), where empty lines between actions in the payload caused `unsupported bulk api action line… warnings.`|
|CRIBL-26782|The Windows Event Forwarder Source no longer produces error messages about `The requested action is not supported by the service`.|
|CRIBL-26333|The UI now correctly displays the actual **Max S2S Version** default, `v3`, instead of `v4` for a Splunk TCP Source or Destination you created before the **Max S2S Version** setting existed. This discrepancy sometimes lead to version mismatch errors when forwarders running Splunk 9.1.4 attempted to send data.|
|CRIBL-22184|We fixed some issues with the Webhook Destination in a Cribl Edge deployment. If you upgraded the deployment to version 4.4.4, the UI showed an error; the new **Load balancing** toggle was enabled with associated settings displayed; and the UI did not allow you to save changes.|
|CRIBL-26854|Resolved an error related to OTLP traces from the C SDK being rejected when sent to Cribl version 4.7.3 and OTLP v1.3.1. The error occurred while parsing OTLP request payloads, causing trace data to be improperly processed.|
|CRIBL-27081, CRIBL-26785|Improved Windows Event Forwarding log handling by removing some duplicate entries, downgrading some expected messages to info level, and improving message clarity and accuracy.|
|CRIBL-27082|Updated the Windows Event Forwarder bookmark handling to ensure that bookmarks are not updated until all events in the envelope are ingested.|
|CRIBL-27368|The Kafka, Confluent Cloud, Amazon MSK, and Azure Event Hubs Sources now allow heartbeats during Fetch requests. This reduces the likelihood of unnecessary rebalances and improves the stability and reliability of consumer groups.|
|CRIBL-24949|The Azure Blob Storage Destination now handles invalid characters in filenames more effectively.|
|CRIBL-22366|The filenames for Exabeam outputs were previously calculated using an incorrect time unit, leading to inconsistent filenames. We've corrected this to ensure accurate filenames.|