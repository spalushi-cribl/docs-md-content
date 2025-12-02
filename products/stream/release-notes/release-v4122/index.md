# Cribl Stream 4.12.2


Cribl Stream 4.12.2 delivers crucial operational improvements, enhanced stability, and important bug fixes.

## New Features

This release adds the following feature:

### Amazon S3 Source Object Tagging

The [Amazon S3 Source](sources-s3) now supports tagging S3 objects after processing. A new **Tag after processing** toggle lets you configure custom S3 object tags. You can set up static strings or dynamic expressions, enabling flexible tagging for downstream automation. For example, these tags can be used with S3 Lifecycle policies to automatically manage object storage and deletion, helping you optimize costs and data retention.

## Experience Improvements

### Packs

- The [CriblVision for Splunk Pack](https://packs.cribl.io/packs/cc-criblvision-for-splunk) was updated this release.

## Corrections

This release contains the following bug fixes:

### Security Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31567</div>| DOMPurify is now upgraded to version 3.2.6. |
|CRIBL-32166| Users who used a **Read Only** role in Cribl Stream or Cribl Edge to fetch the Leader token will now have to use an **Admin** role for that purpose. <li>If you were using `stream_reader`, you must now use `stream_admin`.</li><li>If you were using `edge_reader`, you must now use `edge_admin`.</li> |

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31772</div>| Fixed an issue that caused an error when adding certificates via **Settings > Global > Distributed Settings > TLS Settings**. |
|CRIBL-32619| The Live Data and Pipeline previews now correctly expand to display arrays and objects, even if the field name contains a dot. |
| CRIBL-32824 | Fixed an issue where generating a diagnostic bundle after teleporting into a Worker Node, the system created and uploaded two files instead of one. |
|CRIBL-32507| Fixed an issue where assigning the Cribl Search Admin role, along with Cribl Organization User, Cribl Stream Editor, and Cribl Edge Editor, caused the **View All Groups** page in Cribl Stream to not load with repeated 403 errors and a "Failed to fetch organization details" message. Users could still access individual Groups, but the centralized view was broken. |
|CRIBL-32621| Fixed an issue in the License Monitoring **Product Utilization** dashboard where the Min, Max, and Avg values were incorrectly calculated using the raw input bytes instead of the actual product utilization. |
|CRIBL-31181| Fixed an issue where, after creating a new Source or Destination, the **Commit** button was not enabled until the page was refreshed or another Source or Destination was added. |
|CRIBL-32973| Fixed an issue where disabling **Strict Ordering** on Destination persistent queues (PQ) with Error or Backpressure mode could cause unexpected data loss during PQ drain, even when queues were not full and backpressure was not expected to drop events. |
|CRIBL-32620| Fixed an issue affecting state tracking of collection jobs on Workers mapped to non-default Worker Groups. Workers no longer require a restart to report job status correctly.|
|AI-1859| Fixed an issue where large event samples in Copilot Editor could exceed API limits and cause errors. The system now truncates and filters tool messages before sending, improving reliability.|
|CRIBL-33001| Fixed an issue where HTTP requests would unexpectedly time out after about five seconds, causing operations like data collection and integrations to fail or abort early. This affected several parts of the product that make network requests. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28723</div>| REST Collector jobs now consistently log collectible event metrics (`collectible.collectedEvents`) for all tasks, including those using cached input. These logs are emitted at the `debug` level to minimize noise. |
|CRIBL-33036| Azure Blob Storage integrations now provide an **Azure Cloud** setting to support cloud services in China. |
|CRIBL-30718| Database Collector query validation has been updated to correctly handle SQL queries containing semicolons. Previously, valid SQL queries were rejected if they included a semicolon. |