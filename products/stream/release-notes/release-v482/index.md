# Cribl Stream 4.8.2


This maintenance release of Cribl Stream includes bug fixes and new features.

## New Features

This release provides the following improvements:

### NetFlow v9 Source

You can now use Cribl to collect, route, and control NetFlow v9 data records 
sent via UDP. 

### NetFlow v5 and v9 Destination

Route unmodified NetFlow v5 and v9 options, template, and data records to a 
downstream NetFlow Collector via the new NetFlow Destination.

### CrowdStrike Falcon Next-Gen SIEM Destination

The new CrowdStrike Falcon Next-Gen SIEM Destination allows you to send data 
directly to CrowdStrike Falcon Next-Gen SIEM for advanced threat detection and 
analytics. Simply provide your SIEM endpoint and authentication token.

## Experience Improvements

- The OTLP Metrics Function now supports period (.) and underscore (_) separated 
  prefixes allowing you to specify more precise attribute matching.

- Destination health indicators are now more accurate. In prior releases, 
  Destination health status colors sometimes displayed incorrect health status 
  indicators. Destination health calculations are now based on consistent time 
  metric queries, improving the accuracy of Destination health status colors.

- Cribl.Cloud's [Billing tab](cloud-portal#billing) 
  now details the infrastructure costs for Cribl-managed Stream Workers, taking 
  into account the Cloud provider and region. Refer to 
  [Cribl Pricing](https://cribl.io/pricing/stream/) for a breakdown of regional 
  pricing.

- Cribl Stream now offers enhanced FIPS mode support. We’ve implemented strict 
  checks to verify that the OpenSSL version is 3.0.2 or higher, and that the FIPS 
  Provider module is 3.0.5 or greater. This uses the most recent cryptographic 
  libraries, which include the newest security patches. The specific versions of 
  OpenSSL and the FIPS Provider module are recorded at application startup, to 
  provide an audit trail for tracking changes and troubleshooting potential 
  issues.

## Sources and Destinations

- You can now dynamically set the **Site name** in the Exabeam Destination using 
  JavaScript expressions. This allows you to reference fields like 
  `cribl_pipeline`, providing greater flexibility and precision in categorizing 
  data as it lands in Exabeam.

- We have expanded the new retry options introduced in 4.8 to additional 
  Sources. These retry options improve connection reliability. The **Retry 
  Connection Timeout** setting automatically tries to reconnect after temporary 
  network disruptions. The **Retry Connection Reset** setting automatically 
  reconnects when a connection is unexpectedly closed. These settings are now 
  available for the Office 365 Message Trace Source, Office 365 Services Source, 
  Wiz Source, and REST Collector.

- The [Splunk HEC Source](sources-splunk-hec) now supports the secrets
  store, so you can securely store and manage authentication tokens.

- When using Azure Blob Source, Collector, or Destination, you can now take 
  advantage of Azure service principal authentication.

- You can now customize the **Authentication URL** and **audience** on the Wiz 
  Source to match your organization's specific needs. This update allows you to 
  integrate Wiz with self-hosted instances or custom identity providers.

- Partition assignment is now randomized for the following Sources: Kafka, 
  Confluent Cloud, Amazon MSK, and Azure Event Hubs. This change randomizes 
  which Worker Processes are selected to consume data during partition 
  assignment, alleviating the problem of uneven load distribution when there are 
  fewer partitions than Worker Processes. Note that random assignment is not 
  the same as balanced assignment, and even distribution of work is not 
  guaranteed.

- We've enhanced the Kafka and Confluent Cloud Sources/Destinations with new 
  configuration options to improve their reliability and resilience when 
  interacting with schema registries. These settings empower administrators to 
  fine-tune connection behavior, request timeouts, and retry attempts, ensuring 
  effective handling of transient errors.

- You can now specify the character encoding to use when parsing ingested data 
  in the Splunk Search Source and the following Collectors: Azure Blob Storage, 
  Filesystem, Google Cloud Storage, Health Check, REST, S3, and Splunk Search. 
  UTF-16LE and Latin-1 are now supported in addition to UTF-8. For new 
  configurations, UTF-8 is the default. To avoid issues parsing multi-byte UTF-8 
  characters, we recommend setting existing configurations with this value.

- To reduce the potential for data loss with scheduled REST Collectors, the 
  **Job timeout** setting now defaults to 60 minutes. This applies only to newly 
  created REST Collector schedules.

- The Google Cloud Pub/Sub Source and Google Cloud Storage Collector Source now
  support IAM roles for authentication.

- The Exabeam Destination now supports the (`me-central2`) Dammam region.

- File system-based Destinations can now handle various error conditions and are 
  more reliable when handling file uploads. With the refined retry logic, failed
  uploads are now more likely to succeed, minimizing data loss and ensuring a 
  smoother data flow experience.

- The system now logs access denied errors when attempting to write to Google 
  Cloud Storage. This will help users troubleshoot permission issues.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-17579</div>| To minimize stale Git locks that block commits, the (user-configurable) **Git timeout** default setting is now increased from 60 seconds to 5 minutes.|

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-26709</div>|The Elasticsearch API Source now ignores empty lines in payloads. This change addresses a problem encountered by users of recent Filebeat versions (8.14.3 and newer), where empty lines between actions in the payload caused `unsupported bulk api action line...` warnings.|
|CRIBL-26782|The Windows Event Forwarder Source no longer produces error messages about `The requested action is not supported by the service`.|
|CRIBL-26333|The UI now correctly displays the actual **Max S2S Version** default, `v3`, instead of `v4` for a Splunk TCP Source or Destination you created before the **Max S2S Version** setting existed. This discrepancy sometimes lead to version mismatch errors when forwarders running Splunk 9.1.4 attempted to send data.|
|CRIBL-24280| The REST Collector incorrectly displayed the **Discovery URL** as the source field in collected events, instead of the actual **Collect URL**.|
|CRIBL-22184|We fixed some issues with the Webhook Destination in a Cribl Edge deployment. If you upgraded the deployment to version 4.4.4, the UI showed an error; the new **Load balancing** toggle was enabled with associated settings displayed; and the UI did not allow you to save changes.|
|CRIBL-26965|The Wiz Source in Cribl Stream did not correctly handle pagination. The default queries for each data type in the Wiz Source incorrectly passed the `endCursor` parameter, resulting in the same page of data being returned repeatedly.|
|CRIBL-26854|Resolved an error related to OTLP traces from the C SDK being rejected when sent to Cribl version 4.7.3 and OTLP v1.3.1. The error occurred while parsing OTLP request payloads, causing trace data to be improperly processed.|
|CRIBL-27081, CRIBL-26785|Improved Windows Event Forwarding log handling by removing some duplicate entries, downgrading some expected messages to info level, and improving message clarity and accuracy.|
|CRIBL-27082|Updated the Windows Event Forwarder bookmark handling to ensure that bookmarks are not updated until all events in the envelope are ingested.|
|CRIBL-27368|The Kafka, Confluent Cloud, Amazon MSK, and Azure Event Hubs Sources now allow heartbeats during `Fetch` requests. This reduces the likelihood of unnecessary rebalances and improves the stability and reliability of consumer groups.|
|CRIBL-24949|The Azure Blob Storage Destination now handles invalid characters in filenames more effectively.|
|CRIBL-25846|The Kafka, Confluent Cloud, Amazon MSK, and Azure Event Hubs Sources had a bug that was causing consumers to fail to self-recover. When impacted by this bug, consumers entered into a loop where they could not get out without restarting the process. Having one or more consumers in a group hitting this state would cause an excess of group rebalances that were triggered periodically. We corrected this to ensure consumers can self-recover from transient errors.|
|CRIBL-22366|The filenames for Exabeam outputs were previously calculated using an incorrect time unit, leading to inconsistent filenames. We corrected this to ensure accurate filenames.|
