# Cribl Stream 4.3.1


## New Features 

This release provides the following improvements:

The Azure Blob Storage Collector and Google Cloud Storage Collector can now ingest and replay data in Parquet format. (This matches the S3 and Filesystem Collector' previously announced Parquet support.) 

Amazon SNS and Slack are now available as [Notification targets](notifications-targets). 

The SNMP Source now supports SNMPv3 for user authentication and data privacy. 

The SNMP Trap Source now supports multiple users via SNMPv3, allowing you to receive traps from multiple devices configured with different users. 

## Corrections 

This release includes the following fixes:

### Administration and Stability Fixes

CRIBL-20038 Corrected Organization- and Product-level Admins' access to Global Settings, including Upgrade and Diagnostics.

CRIBL-18102 Corrected Leader Nodes' unresponsiveness when high availability is enabled but the Leader cannot write to an NFS mount.

CRIBL-18573 The Cribl Stream system upgrade prompt now correctly shows the current Leader version.

### Sources and Collectors Fixes

CRIBL-20088 After upgrade to v.4.3.1, Office 365 Sources and scheduled REST Collector jobs will resume working correctly without further configuration changes.

CRIBL-20151 Splunk TCP Source now correctly negotiates S2S v4 payloads from Splunk forwarders.

CRIBL-20017 Splunk TCP Source now correctly handles sub-second values in incoming events.

### Destinations Fixes

CRIBL-20183 Splunk Load Balanced Destination no longer triggers `LEB128` errors when sending to Splunk v9.1.0.2 Indexers over S2S v4.

CRIBL-19912 Corrected data capture on Projects' Destinations with arbitrary filter expressions.

CRIBL-20366 Kafka-based Destinations now log `Response without match` messages only when you set the log level to `silly`. Before this fix, `Response without match` was a warning (not a message), and could flood the logs when **General Settings** > **Acknowledgement**s was set to `None`.

### Persistent Queue Fixes

CRIBL-16289 Azure Event Hubs Destination now correctly writes to persistent queues, instead of blocking or dropping data.

CRIBL-19756 Azure Event Hubs Destination now correctly drains persistent queues when Acks are enabled.

CRIBL-19954 Splunk TCP Source's PQ now drains correctly, preventing backpressure.

### Chain Function Fixes

CRIBL-20087 Chain Function no longer drops data when Workers are running Cribl Stream versions older than 4.3.0.

CRIBL-20083 Chain Function configs can now be resaved after changes to the chained Pack or Pipeline.

### Other Functional Fixes

CRIBL-20380 Corrected excessive `_raw stats` logging of metric series.

CRIBL-18301 Uploading a `.pem` certificate file to `$CRIBL_HOME` no longer generates a spurious error message claiming a missing certificate file.

CRIBL-19643, CRIBL-20304 Parquet throughput no longer misrepresents omitted optional fields as null.

CRIBL-20398 Database connection requests to SQL Server now have a maximum configurable timeout of 10 minutes, extended from the previous 1 minute.

CRIBL-19919 If advised by Cribl, you can now free up CPU resources by reducing the **Max number of metrics** series processed. This limit is configurable on the Leader and individual Groups, via the UI or `limits.yml` (see `maxMetrics` [here](limitsyml)). Cribl generally recommends the default limit of 1 million metric series.

### Monitoring and Metrics Fixes

CRIBL-19510 The `GET /system/metrics` API endpoint now accepts string as well as number values for its `earliest` and `latest` parameters. 

### UI Fixes

CRIBL-19852, CRIBL-19430 UI's Monitoring tab now remains visible when GitOps `Push` workflow is enabled.

CRIBL-20369 Corrected **Global Settings** > **Custom Banner** access with `User` Permissions.

CRIBL-19994 **Global Settings** > **Custom Banner** offers new background color options.

CRIBL-19855 SSO identity providers are now displayed with correct capitalization.

### Documentation Fixes

We've stream-lined the Stream [docs](/stream/about)' left sidebar to collect related topics, and to generally make things easier to find. **Monitoring**, **QuickConnect**, and **Projects** are now promoted to the top level. Individual Sources' and Destinations' reference pages are unchanged, but a new [Using Integrations](/stream/load-balancing) section gathers multi-integration topics and use-case scenarios. Security use cases are now consolidated in a **Securing** > [Usage Examples](/stream/usecase-encrypting-data) subsection. Finally, a retitled [Better Practices](/stream/tips) section has a narrower focus on applying Cribl Functions to particular purposes. We think Aristotle would approve, and we hope you'll find this simplification useful, too.


