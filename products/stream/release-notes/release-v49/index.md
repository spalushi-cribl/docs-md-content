# Cribl Stream 4.9


Double, double, toil and trouble; Cribl Stream 4.9 is ready to rumble! This October, we're conjuring up some powerful new features and improvements.

## CentOS 7 Deprecation

We've deprecated support for CentOS 7 on the Cribl Stream Leader and Worker Nodes.
We will remove full support for CentOS 7 in a future release. To ensure the
security and stability of your Cribl Stream deployment, we recommend that you
migrate to an operating system that will continue to receive security updates
and patches. Please visit the [CentOS website](https://centos.org/) for
migration information.

## New Features

This release provides the following improvements:

### New Destinations

We now support Destinations for [ClickHouse](destinations-click-house), [Dynatrace HTTP](destinations-dynatrace-http) (logs only), and [Dynatrace OpenTelemetry Protocol](destinations-dynatrace-otlp) (OTLP); giving you more choices for handling your logs, traces, metrics, and more.

### New Source

With the new [Amazon Security Lake Source](sources-security-lake), you can transform, trim, or shape incoming data from Amazon Security Lake and then send it to a downstream Destination, such as your preferred security analytics tool. This Source supports receiving Amazon Simple Queue Service (SQS) data from Amazon Security Lake event subscriptions.

### Worker Groups Across AWS Regions

In Cribl.Cloud, you can now create Cribl-managed Worker Groups in different AWS regions. You are no longer limited to the region where the Organization Leader resides.

### New Navigation Experience

We've streamlined your UI navigation experience across all Cribl products! The UI changes that have been opt-in for Enterprise customers since release 4.5 are now the default for all users. This preview period allowed us to incorporate your feedback and optimize the layout. The main change you'll notice is that we've decluttered the top of your browser, consolidating most navigation in a collapsible left sidebar. This sidebar is context-sensitive, enabling you to access product-specific options, as well as Global Settings.

### New Leader Limits Settings

We've introduced new **KV Store** [limit](limitsyml) settings to alleviate high CPU usage from the API process on the Leader Node.

## Functional Improvements

We introduced the following functional changes to Cribl Stream:

### Cribl.Cloud

* In Cribl.Cloud, you can now create [API credentials](/api/#criblcloud) for each individual Workspace. This gives you more precise control over Workspace management and lets you control Products programmatically. For each Workspace, you can define a Workspace permission and, if relevant, product-level permissions.

* An Admin user now has full permissions for all products, and can execute Leader commits and modify **Global Settings**.

* Persistent queue (PQ) support is now available for Cribl managed Cloud Worker Groups hosted on Azure, which helps prevent data loss when a receiver is unreachable.

* You can now provision Cribl-managed Stream Worker Groups in any region under a single workspace. This is available for both AWS and Azure, providing the ability to locate Worker Groups within the region where your data is being processed and routed.

### User Experience Updates

* We've added support for vertical scrolling on the Cribl login page to better support display-related requirements.

* You can now reuse your preceding commit message by selecting **Use Previous Commit Message** in the Git Commit Changes modal.

* The user interface has new icons to better denote fields expecting JavaScript or regular expression format.

### Sources and Destinations

#### General Integrations Improvements

* Sources and Destinations now support optional description fields.

* When Cribl Stream encounters an invalid octet count (per RFC 6587) while parsing an
  octet-counted stream of TCP syslog messages, it will now emit the events
  preceding the invalid count and close the socket connection. This reduces CPU
  and memory usage since Cribl Stream is not leaving the socket open.

* You can now use the Cribl command-line interface to disable the Script Collector in your entire Cribl Stream deployment. Running this command removes the Script Collector from all current Worker Nodes and applies this configuration to any new Worker Nodes you create.

#### Improvements to Sources and Collectors

* The Splunk HEC Source authentication tokens interface now has a more user-friendly display. You can expand or collapse authentication token details to view either a more detailed or compact version of the token details as needed.

* We've updated the REST Collector for a more intuitive and streamlined configuration experience.

* The REST Collector now supports pagination when configured for HTTP Discover. The pagination options are identical to the Collect step.

* The Amazon S3 Source can now receive Amazon Simple Queue Service (SQS) data from Amazon Security Lake event subscriptions in addition to S3 SQS event notifications.

* The Splunk TCP and Splunk HEC (S2S v4) Sources now include an **Extract metrics** setting to ensure that Splunk-generated metric events are correctly identified and processed as Cribl metrics, maintaining their metric nature when forwarded.

#### Improvements to Destinations

* Destinations now have the additional persistent queue modes `Always On` and `Backpressure`. These modes allow you to choose how Cribl Stream writes data to disk during a backpressure event.

* To provide better error handling and data recovery for Filesystem-based Destinations, we've added a new dead-lettering feature. Cribl Stream disables this feature by default. Dead-lettering allows you to configure where Cribl Stream will store failed files, and how many times it will retry the file before considering it "dead". Stream will move failed files to the specified location after exceeding the retry limit.

* Filesystem-based Destinations now provide a **Disk space protection** setting allowing you to either block or drop incoming events when the disk space falls below the globally defined **Min free disk space** amount.

## Corrections

We made the following fixes in this release:

### Operational Fixes

| ID | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-27640</div> | Out of memory (OOM) errors on API child processes are logged in the `cribl_stderr.log` to assist with troubleshooting. |

### Source and Destination Fixes

| ID | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-27624</div>| Because the **System fields** setting is unnecessary for the NetFlow and SNMP Trap Destinations, it has been removed from the **Post-Processing** options for those Destinations. |
| CRIBL-19123 | To prevent potential issues from unnecessary buffer series, Cribl Stream now hides the **Max Buffer Size** setting when you enable Persistent Queue (PQ) for these Sources: Metrics, Raw UDP, SNMP Trap, and Syslog. |
| CRIBL-28080 | The Splunk Load Balanced Destination using S2Sv4 encountered issues when processing certain events: <ul><li>When events contained a field with an array value with many items, parsing could fail with the error message `RangeError: offset is out of bounds`.</li><li>When events contained a field with a non-stringified object value greater than 1 KB and **Nested field serialization** was set to JSON, the value could be truncated after stringification. The event would be sent but could miss some stringified object value. </li></ul> |
| CRIBL-27682 | HMAC authentication could fail when an API requires empty strings in the signature string builders. Cribl currently filters out any `falsy` values, including empty strings, which can prevent the correct generation of the HMAC value and lead to authentication failures. |
| CRIBL-25826 | REST Collector jobs with state tracking enabled were generating misleading warning logs about state evaluation expressions even when no events were processed. |
| CRIBL-25352 | The Elasticsearch Destination did not honor retry attempts for failed HTTP requests when the target Elasticsearch Cluster was full and set indices to read-only. |
| CRIBL-27361 | The Windows Event Forwarder Source was causing excessive load on the Leader due to frequent individual RPC calls for state updates. |
| CRIBL-27926 | Cribl TCP/HTTP Sources and Destinations did not log any errors if the Leader auth token could not be loaded from disk. |
| CRIBL-25252 | Unable to collect GCC High Activity or Service messages from Azure, receiving a 401 authorization failure against graph.microsoft.us despite having the correct permissions. |
| CRIBL-25194 | The S3 Source encountered errors when attempting to download compressed `.gz` files from S3, specifically receiving an `Incorrect Header Check` error. This issue arose because the files contained trailing garbage or non-compressed data appended. |
| CRIBL-26817 | Cribl Stream no longer experiences errors displaying multi-byte UTF-8 characters from Collectors such as AWS S3. |