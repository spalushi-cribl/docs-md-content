# Cribl Edge 4.11.1


Cribl Edge 4.11.1 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

### Deprecation Notice

Upcoming deprecation for all Elasticsearch API Sources: in an upcoming release, these Sources will ignore extra HTTP headers that do not start with `X-`, `x-`, or `-es`.

### New Cribl.Cloud Domain Rollback

In our previous release, we announced a plan to begin migrating Cribl.Cloud Organizations to a new unified domain (`https://app.cribl.cloud/`). After evaluating customer feedback and internal considerations, we’ve decided **not to move forward with this domain migration**.

Your Cribl.Cloud environment will continue to operate under the existing URLs you’ve always used — there is no change to login or portal access. We won't adopt the previously communicated domain (`app.cribl.cloud`), and we won't implement redirects.

This rollback requires **no action on your part**. All existing functionality
and URLs remain unchanged.

### Upcoming Changes to Feature Availability on Free Tier

In an upcoming release, certain features, including persistent queueing (PQ), will no longer be available in the free tier of Cribl Edge. These features were temporarily accessible due to an oversight and are intended for Standard and Enterprise license plans only, as outlined on our [Pricing](https://cribl.io/pricing/) page. We hope the temporary access provided value and we encourage you to consider upgrading to maintain access to these advanced capabilities.

## New Features

This release provides the following improvements:

### Cortex XSIAM Destination

Bridge your security alert and audit log data directly into Palo Alto Networks’ Cortex XSIAM platform with the new Cortex XSIAM Destination. This Destination optimizes data delivery through features like automatic format parsing (CEF, LEEF, Syslog, JSON, Raw), efficient batching with configurable flush periods, and payload management, ensuring seamless ingestion into XSIAM.

Cortex XSIAM will support ingestion from this Cribl Destination as the new XSIAM ingestion endpoints are gradually rolled out between April 27th and May 18th.

## Experience Improvements

- As a maintainer of a Pack published in the Cribl Packs Repository, you can now
  transfer the ownership of your Pack to any other user.

- This release increases the RPC timeout limit to help prevent problems with
  slow networks and improve reliability when downloading upgrade packages from
  the CDN (`cdn.cribl.io`).

- The Metrics View in Data Preview now includes a header bar that summarizes key
  details about the sample metrics data, such as total events, creation date,
  and sample duration. Hovering over the duration reveals the sample’s start and
  end times. This enhancement helps you quickly assess the scope of your sample
  metrics data and ensure it is representative.

## Sources and Destinations

- The Windows Event Forwarder Source now includes an advanced setting, **Log CA fingerprint mismatch warning**, to aid in troubleshooting certificate issues. When enabled, it logs a warning if the client certificate authority (CA) fingerprint does not match the expected value, preventing Cribl from receiving events.

## Corrections

This release contains the following bug fixes:

### Cribl Edge Exclusive Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-29168</div>|We fixed an issue where the UI was incorrectly showing the ability to assign Members and Teams to a Subfleet, but selecting the option resulted in a 404 error.|
|CRIBL-31516|We taught the Cribl Edge Windows installer to clean up after itself, so it no longer leaves behind old upgrade files from previous versions of Cribl Edge. You should expect to see only the MSI file for the current version of Cribl Edge. Messy disk space no more!|
|CRIBL-31492|We fixed an issue that caused an error message to appear when you tried to delete a Fleet, even though the Fleet was successfully deleted.|
|CRIBL-31478|We resolved an issue where the Windows Event Logs Source would stop collecting and forwarding events, due to the Source reporting the `newestRecordId` value as less than the `lastRecordId` value.|
|CRIBL-31416|Teleporting into an Edge Node and selecting these cards no longer results in a blank page: <ul><li>On Windows Nodes: the **Running Processes** card.</li><li>On Kubernetes and Linux Nodes: **Running Processes**, **Containers Discovered**, **Files Discovered** cards.</li></ul>|
|CRIBL-31648|We fixed an issue where Cribl Edge Nodes were not reconnecting to the Leader Node.|

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-30551</div> | Previously, when cloning Sources or Destinations, their associated Notifications were incorrectly cloned with the original ID of the Source or Destination, leading to incorrect alerts. With this release, Notifications are no longer cloned. To maintain alerting, recreate any Notifications manually on the cloned Source or Destination. |
| CRIBL-29685 | Resolved an issue where secret updates were not picked up by Fleets after commit and deploy. Previously, when credentials that were only used in inputs or outputs changed, users had to manually restart the Workers for the changes to take effect. With this fix, a commit and deploy now correctly updates inputs and outputs with the new secret values and no restarts are required. |
| CRIBL-31398 | Fixed an issue impacting Data Preview where copying field names using dot notation to the clipboard resulted in extra characters being added to the copied text. Field names now copy and paste as expected. |
| CRIBL-31444 | Resolved an issue where clearing the Pipeline description field did not persist. After saving, the cleared description would reappear when refreshing the UI or viewing the YAML configuration. |
| CRIBL-30858 | Fixed an issue that prevented user-uploaded sample files from appearing in the **Test** tab for Destinations. |
| CRIBL-27706 | Fixed an error in the UI that incorrectly indicated you needed to Deploy immediately after a Commit & Deploy operation. |
| CRIBL-29969 | Fixed an issue when using the Publish Metrics Function with a `Counter` metric type without specifying a **Metric Name Expression**, and attempting to save the Pipeline resulted in an error: `Cannot read properties of null (reading 'startsWith')`. |
| CRIBL-29616 | Resolved an error where the Download button did not appear after creating a diagnostic (Diag) bundle for hybrid Edge Nodes. |
| CRIBL-31233 | Fixed an issue affecting the Aggregate Metrics function configuration generated in Data Preview when building a Pipeline. The generated aggregation expression no longer uses a fallback when getting the metric value. |
| CRIBL-30495 | Resolved an issue where setting the `__id` field in a Pipeline caused Simple Preview to stop tracking changes, making it difficult to preview data. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-31564</div> | Fixed an issue where Splunk Load Balanced Destinations could experience intermittent health issues. A missing compress setting in the Destination configuration caused senders to reconnect when the DNS refresh interval was hit.  |
| CRIBL-30301 | The Amazon S3 Source health status logic has been updated to accurately reflect download failures. Previously, the health icon for an Amazon S3 Source remained green even when all file downloads failed. |
