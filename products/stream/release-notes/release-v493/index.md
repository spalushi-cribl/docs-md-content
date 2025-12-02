# Cribl Stream 4.9.3


This maintenance release of Cribl Stream includes bug fixes and experience improvements.

## CentOS 7 Deprecation Notice

We deprecated support for CentOS 7 in the prior 4.9 release. To ensure the
security and stability of your Cribl Stream deployment, we recommend that
you migrate to an operating system that will continue to receive security
updates and patches. Please visit the [CentOS website](https://centos.org/) for
migration information. We will remove full support for CentOS 7 in a future
release.

## Experience Improvements

- We made several user interface improvements in Cribl Stream/Edge, including enhancing menu visibility on the Pipelines, Packs, and Routes pages. Additionally, we refined the text for managing Notifications to make confirmation dialogs clearer and more user-friendly. 
  
- For Cribl-managed Cribl.Cloud Groups, we’ve removed the Environment setting for Sources and Destinations to streamline configuration. This setting was unnecessary for Cloud deployments, so we eliminated it to reduce confusion and save initial configuration time.   

- To assist with segmentation-fault troubleshooting, we now include timestamps in the `stderr process ID` files logged in the `cribl_stderr_<pid>.log`.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-29035</div>| Resolved an error that caused the Git commit list to incorrectly display multiple commits when users made only a single change.  |
| CRIBL-25232| Fixed a JavaScript handling error affecting Pipeline Functions on virtual machines. Previously, certain arguments incorrectly defaulted to a predefined value instead of resolving to the passed parameter. We resolved this issue, and Pipeline functions now correctly use the provided arguments. |
| CRIBL-29027| Cribl Stream/Edge no longer throws a `Cannot read properties of undefined` error when users name a Pipeline with the same name as a default object property, such as `toString` or `valueOf`.|

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-28844</div>|The **Max age duration** setting in the File Monitor Source has been renamed to **Age duration limit** for improved clarity.|
|CRIBL-27863 | Scheduling a Collector job with a relative time range no longer provides the **Range timezone** option since relative times are based on the current time epoch. |
|CRIBL-28850| The **Bytes Out** metric for `JSON object` data types was not accurately reported in the Google Cloud Logging Destination. This issue led to inaccurate monitoring and analysis of data flow.  |
|CRIBL-28502| The SNMP Trap Serialize Function and SNMP Trap Source no longer provide DES as a **Privacy protocol** option, because Cribl Edge/Stream no longer support this protocol on updated Node.js and OpenSSL versions. |
