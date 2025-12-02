# Cribl Edge 4.13.1


Cribl Edge 4.13.1 delivers crucial operational improvements, enhanced stability, and important bug fixes.

## Important Changes

### Upgrade Advisory for S3 Destinations Targeting Buckets with Object Lock

In this release, we have upgraded our internal AWS SDK for JavaScript from v2 to v3 to address the upcoming [end of support for v2 by AWS](https://aws.amazon.com/blogs/developer/announcing-end-of-support-for-aws-sdk-for-javascript-v2/).

* **If you use the Amazon S3 Destination with S3 Object Lock**:
  DO NOT UPGRADE to version 4.13.1. The Amazon S3 Destination will fail when configured to send data to an S3 bucket with default [Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) enabled. Please wait for the upcoming patch release. See the [Known Issue](https://docs.cribl.io/results/?query=CRIBL-34732&searchOption=known-issues&product=edge) for details.
* **If you DO NOT use the S3 Object Lock feature**:
  You can safely upgrade to this version to ensure your software supply chain contains the fully supported AWS SDK v3.

## Sources and Destinations

* The Confluent Cloud and Kafka Sources and Destinations now support JSON schema types for message encoding and decoding via the Confluent Schema Registry. A new **Schema type** setting allows you to select either `Avro` or `JSON` for event serialization.

* The Grafana and Loki Sources and Destinations now support [structured metadata](https://grafana.com/docs/loki/latest/get-started/labels/structured-metadata/) for logs. Toggle on the new **Enable structured metadata** setting to add string key-value pairs from the internal `__structuredMetadata` field, allowing you to enrich events with high-cardinality context (like trace IDs) without inflating label sets.

* The Loki Destination now supports dynamic HTTP headers. The Destination inspects each event for a `__headers` object and batches events with identical `__headers` values, sending them in a single HTTP request. This allows for per-event custom headers, supporting use cases like multi-tenant routing and dynamic authentication.

## Packs

We added more REST Collector Packs to the Cribl Packs Dispensary for [CrowdStrike](https://packs.cribl.io/packs/cribl-crowdstrike-rest), [Okta](https://packs.cribl.io/packs/cribl-okta-rest), and [Microsoft O365](https://packs.cribl.io/packs/cribl-o365-rest).

## Corrections

This release contains the following bug fixes:

### Cribl Edge Exclusive Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-30293</div>| We fixed an issue where upgrading Cribl Edge on Windows via the MSI installer did not allow disabling TLS. |

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-33815</div>| Fixed an issue where sorting Packs by Display Name in the UI did not work as expected. Packs now sort correctly by Display Name. |
| CRIBL-33362 | Resolved an issue in the Flows chart on the Monitoring page where the pre-processing Pipeline's bytes metric incorrectly mirrored the last active route's value. The pre-processing Pipeline now accurately displays the bytes received directly from the Source. |
| CRIBL-29531 | Fixed an issue where Syslog Sources configured with only TCP ports or UDP (but not both) did not appear in the Flows chart on the Monitoring page. All Syslog Source configurations now correctly display in the graph, providing a complete view of your data flows. |
| CRIBL-30721 | Fixed a UI issue in the Parser Function where the Destination field option was missing for Regex and Grok types. The Destination field is now consistently available for all Parser Function data types. |
| CRIBL-33597 | Fixed the confusing UI output in the Event Breaker Preview. The Event Breaker output preview now shows all events that were broken out by the rule, including events that don't match the filter.  Events that don't match the filter appear with a strike-through them. |
| CRIBL-32737 | Fixed a misleading UI issue in the Event Breaker Rules preview where the timestamp highlight disappeared when using a non-local timezone and the `%H` format. The timestamp now applies correct highlighting, ensuring accurate visual feedback during configuration. |
| CRIBL-31765 | Fixed an issue in our Git commit process to more securely attribute commit messages to the correct user. |
| CRIBL-33777 | Fixed an issue that prevented Packs from importing configurations with load-balanced Sources or Destinations when running in Edge. |
| CRIBL-34479 | Fixed a regression that prevented the HashiCorp Vault KMS (Key Management Service) from functioning correctly. This fix restores it to its expected behavior, allowing you to once again use HashiCorp Vault for key management. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-30022</div>| Fixed an issue where the File Monitor Source would intermittently mix up the source/headers of events from two CSV files created sequentially. |
| CRIBL-32559 | The HTTP Source now completes TLS handshakes when **Show originating IP** is enabled. Previously, this setting would cause the handshake to fail, preventing secure connections. |
| CRIBL-33157 | The Google Security Operations Destination now reliably refreshes its authentication token on all Worker Processes. Previously, some Workers could stop refreshing the token, resulting in expired tokens and loss of data delivery. Affected Workers would repeatedly log “Auth token has expired” without a corresponding “Auth token refreshed” entry. |
| CRIBL-29523 | The `process_resident_memory_bytes` metric for Linux in System Metrics Source now correctly reports the values in bytes instead of pages. This results in larger values, which could affect thresholds, alerts, or visuals in dashboards. |

### Other Functional Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-33595</div> | You can now sort by connection status in Edge Nodes and Workers lists. |
