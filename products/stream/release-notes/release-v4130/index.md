# Cribl Stream 4.13.0


Cribl Stream 4.13.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes

### Node.js Updated to Version 22

The Node.js version used for Cribl Stream has been updated to 22. This may cause baseline memory usage to increase where there is room to grow, because the new Node.js version is more conservative with memory release when there's no memory pressure to improve general compute performance.

### Action Required: End of Support Notice for AWS SDK v2

AWS will end support for their [AWS SDK for JavaScript v2](https://aws.amazon.com/blogs/developer/announcing-end-of-support-for-aws-sdk-for-javascript-v2/) on September 8, 2025. This SDK is used by Cribl AWS Sources and Destinations. To ensure uninterrupted operation and compatibility, we will update our SDK to v3 in the August 2025 Cribl release and completely remove the v2 SDK in September 2025.

**What you need to do:** Plan to upgrade your Cribl deployment to the latest version by September 2025 to align with this change.

### `PATCH /api/v1/system/limits` Now Requires Full Payload

To prevent accidentally removing critical limits, `PATCH /api/v1/system/limits` now requires a complete request body. Before sending a `PATCH` request, you must first call `GET /api/v1/system/limits` and use the full response to build your `PATCH` payload. Partial updates or missing fields may now result in errors.

## New Features

This release provides the following improvements:

### SentinelOne AI SIEM Destination

We've introduced a new SentinelOne AI SIEM Destination for direct and optimized data ingestion into SentinelOne's cloud-native SIEM. This Destination leverages SentinelOne's HEC-compatible endpoint, providing enhanced flexibility and functionality.

If you currently use the Splunk HEC Destination to send data to SentinelOne's AI SIEM, update your workflows to use this new Destination.

### FinOps Center

This Cribl.Cloud offering builds on our improvements to the Billing & Usage page, including new features such as:

- Custom time filters that let you explore trends on your usage and related Cribl bills over time.
- View credit refund and rollover using the credit ledger.
- More detailed visibility into your usage, such as ingest by environment type – connected, hybrid, or Cloud.

### New Drop Dimensions Function

The new Drop Dimensions Function removes dimensions from outgoing metrics data. It helps solve high cardinality problems by reducing the number of unique dimension combinations in time series data, which improves query performance and reduces downstream storage costs.

### Enhanced Oracle Database Connection Security

We've improved Oracle Database connections with the new Stored Credentials authentication method. You can now securely manage your sensitive login information by referencing separate secrets for your username/password and your connection string. This improves security and compliance by centralizing and protecting your database credentials.

## Experience Improvements

- In Cribl.Cloud, we’ve improved the performance of the Leader Node when running scheduled jobs. Administrators should see reduced CPU and memory use on their Leader Nodes in environments with large numbers of scheduled collection jobs.

- In Cribl.Cloud, to prevent Owner lockout due to SSO configuration issues, if only one User is assigned to an Organization's Owner Role, IDP group updates will not remove them.

- Stream Worker Node visibility capabilities have been improved, with disconnected Nodes being tracked in the UI. You can now determine whether each of your Nodes is currently connected to the Leader, check when it last reported a heartbeat, and filter Nodes on the connection status. You can also configure how long after disconnecting the Nodes are tracked in the UI.

- We’ve introduced several improvements to Copilot Editor, Cribl’s AI-powered assistant for building Pipelines. You can now use Copilot Editor to edit existing Pipelines, not just create new ones. We’ve also enhanced usability and expanded our list of out-of-the-box schema support. Copilot Editor now supports more categories of OCSF schemas, the UDM and ECS schemas, and custom schemas saved as schema knowledge objects.

- When capturing sample data from a specific Route, the filter now includes a condition to reflect only the events that are present at that specific Route and match the filter expression for that Route. For example: `cribl_route.includes("routerName") && packId === 'foo'`. It can also handle Routes that are included in Packs. This enhancement makes the Capture tool more accurate and consistent with how Routes are evaluated in practice, helping you better understand which events are processed by each Route.

- You can now filter the Workers list by Worker Group, connection status, Stream version, host, architecture and IP address.

## Sources and Destinations

- Cribl.Cloud now provides enhanced observability for integrations, with new **Charts** displaying key metrics such as latency (P95/P99), failure rate, and endpoint health directly within the Source and Destination configuration modals. These charts leverage the expanded internal metrics introduced in 4.12, providing actionable insights into performance and reliability.

## Packs

- Sources and Destinations within a Pack can now reference Worker Group TLS certificates by name. When you import the Pack into a new Worker Group, Cribl automatically resolves certificate references by looking for a certificate with the same name in that group’s certificate store, then binding it if found. This enhancement supports greater portability of Packs across environments.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31948</div>| Fixed an issue where the `CRIBL_DIST_MASTER_URL` environment variable was ignored in failover mode. TLS settings defined through environment variables now correctly override the values in `leader.yml`, as intended. |
| CRIBL-27189 | Resolved an issue where cloned Packs used the original name instead of the user-defined Pack ID during import. With this fix, the cloned Pack now correctly retains the user-defined Pack ID, avoiding naming collisions and preserving intended structure during import and export workflows. |
| CRIBL-28482 | Fixed an issue where absolute file paths in `package.json` prevented Packs from working across environments with different `$CRIBL_HOME` locations. Paths are now saved using `$CRIBL_HOME` as a relative reference, improving portability for GitOps workflows and deployments. If your environment is on-prem and you want to update all existing Pack paths, remove the `package.json` file or remove the specific Pack entities from `package.json`, then restart the Leader. |
| CRIBL-26590 | Fixed an issue with Pack dependency paths that could cause Packs to disappear from the child Fleet view. This release replaces parent Fleet paths using a new group: tag in child Fleets, which improves reliability and portability across Fleets. |
| CRIBL-31054 | Fixed an issue where selecting **Clear Persistent Queue** on a Destination could cause all outbound connections to close and not re-establish, resulting in stalled data output. |
| CRIBL-23952 | Fixed an issue where invalid XML input could cause the `C.Text.parsexml()` method to hang or delay Pipeline previews. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-26498</div>| We fixed an issue where the System Metrics Source emitted metrics with null values. |
| CRIBL-33343 | The Cribl HTTP Source generates fewer error and warning logs during high-load or error scenarios. Previously, excessive logging under stress could overwhelm system resources, causing Worker Processes to run out of memory and degrade performance. |
| CRIBL-33318 | The `source` internal field of the Exec Source no longer includes a timestamp. |
| CRIBL-28895 | The `df_metric.usepct` field from Splunk Universal Forwarder (UF) version 8.2.7 was not processed correctly in Cribl Stream versions 4.7.2 and 4.8.2. This could have led to inaccurate monitoring and analysis of system performance. |

### Other Functional Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31733X</div>| The Mask Function now works correctly if the Replace expression field is empty, with the field defaulting to `''`. |
