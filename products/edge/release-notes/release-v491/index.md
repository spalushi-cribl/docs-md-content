# Cribl Edge 4.9.1


This maintenance release of Cribl Stream includes bug fixes and experience improvements.

## CentOS 7 Deprecation Notice

We deprecated support for CentOS 7 in the prior 4.9 release. To ensure the
security and stability of your Cribl Edge deployment, we recommend that
you migrate to an operating system that will continue to receive security
updates and patches. Please visit the [CentOS website](https://centos.org/) for
migration information. We will remove full support for CentOS 7 in a future
release.

## Experience Improvements

- Now, when you change the [Edge Nodes Filters](edge-nodes-filter) options, the
  Cribl Edge URL changes correspondingly. This allows you to share the filter with
  other Cribl Edge users by copying and pasting the URL.

- You can now download your monthly Cribl.Cloud invoices. Downloading allows you
  to store invoices or bring them into an external tool for use in budget
  planning, analytics, or record-keeping.

- Cribl.Cloud administrators can now customize the default Git commit message
  for committing and deploying changes. Previously, this customization option
  was available only to on-prem administrators.

- In **List** view, Sources or Destinations now show the number of configured
  Notifications. A badge next to the **Notifications** button indicates how many
  notifications you have configured for each Source or Destination, allowing you
  to quickly confirm Notification setup at a glance.

- We refined the icons for JavaScript, regex, and Grok field inputs to create a
  subtler, less distracting appearance when you're working with specific
  Functions within Pipelines.

- To make navigating through Cribl more efficient, we've added a **Recent Fleets**
  flyout. Select **Products** in the top bar, then hover over **Fleets**
  to view and select items you most recently accessed.

## Sources and Destinations

- The new SNMP Trap Serialize Function serializes compliant events into SNMP
  traps for sending out through SNMP Trap Destinations. It ensures compatibility
  with existing SNMP infrastructure and supports integration with network
  management tools that use SNMP for monitoring and alerting.

- The CrowdStrike FDR, Amazon S3, and Amazon Security Lake Sources now provide
  an **Encoding** setting that lets you select the character encoding to use
  when parsing ingested data.

- The Cribl Internal Source now offers the `CriblLogs` option on Cribl-managed
  ("Cloud") Worker Groups. `CriblLogs` in Cribl.Cloud only includes logs related
  to Sources and Destinations.

- You can now specify custom region endpoints for various Sources and
  Destinations, including services like AWS, Google Cloud, and New Relic. This
  ensures compatibility with newly introduced regions by cloud providers without
  requiring a new release.

- The Azure Event Hubs Source, Confluent Cloud Source, Kafka Source, and Amazon MSK Source have a new
  setting labeled **Maximum number of errors per socket** that defines the
  maximum number of consecutive request errors allowed on a single socket
  connection before it’s discarded and reestablished. This helps to maintain
  stable connections and reduce rebalances in consumer groups.

- The Google Cloud Pub/Sub Destination now supports IAM roles for authentication.

## Corrections

This release contains the following bug fixes:

### Security Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-27356</div>| `aws-sdk` dependencies updated to include the latest security fixes. |
| CRIBL-27353 | `@azure/identity` version updated to include the latest security fixes. |

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-22970</div>|We fixed a memory-leak issue that caused high memory usage when Cribl Edge was deployed on a Windows Node with the File Monitor Source configured on that Node.|
|CRIBL-28774|We updated Cribl Edge installations on Windows to support .NET versions 4.5 or newer.| 
|CRIBL-28475|On the Windows Metrics Source, we increased the default metrics collection timeout to 60 seconds to improve the collection speed of Windows metrics.|
|CRIBL-28170| To assist with troubleshooting out-of-memory (OOM) errors on API child processes, we now include timestamps on messages logged in the `cribl_stderr.log`.|
| CRIBL-28207 | We fixed the error that caused the Cloud persistent queue (PQ) health checker to send false alerts from environments with only one Edge Node.|
| CRIBL-28496 | When you delete a Fleet that has a Mapping Ruleset, the Mapping Ruleset is now deleted as well. This fix prevents orphaning Edge Nodes that match a Mapping Rule for a Fleet that no longer exists. |
| CRIBL-28715 | We fixed a bug that occurred when users switched active Mapping Rulesets in Cribl Edge. After selecting **Activate**, you’re now presented with an “Are you sure?” confirmation. Previously, Cribl Edge treated selecting **No** as **Yes** and updated the active Mapping Ruleset.|
|CRIBL-22861|We fixed an error that prevented the Global Variable `C.logStreamEnv` from evaluating correctly.|

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28044</div>|The Google Security Operations (SecOps) Destination no longer requires you to configure the **Namespace** field.|
|CRIBL-26879 | The Cribl TCP Source now reports event sizes in the same way as the corresponding Destination, ensuring that bytes out and bytes in match across Worker Groups, and reducing confusion about data loss. |
|CRIBL-26864 | Retryable network errors on HTTP-based Destinations are now logged as warnings instead of errors, preventing unnecessary alarms.|
|CRIBL-21669|The Cribl HTTP Source now provides enhanced logging when backpressure occurs, making it easier to identify and troubleshoot data throughput issues.|
|CRIBL-28195| On Windows, the **Partitioning expression** for Amazon S3 Destinations now interprets year/month/date correctly as separators, preventing their inclusion in the file name.|

### Other Functional Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28702</div>| In the **Parser** Function, the **List of Fields** setting is now only visible for structured formats like CSV, CLF, and ELFF. It is not displayed for JSON Object or Key-Value Pair formats. |
