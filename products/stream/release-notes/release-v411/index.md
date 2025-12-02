# Cribl Stream 4.1.1


> ##### Known Issue
>
> Due to a serious known issue, deploying config bundles to bootstrapped 4.1.1 customer-managed (on-prem or hybrid) Workers can (under certain conditions) cause them to stop processing data. Before you bootstrap Workers with v.4.1.1, please see [details and workarounds here](known-issues#CRIBL-17264). Cribl is accelerating a fix.
>
{.box .warning}

## New Features 

This release provides the following improvements:

Introducing Stream Projects (Beta). This feature enables Cribl admins to create isolated spaces for teams and users to share and manage their data.


New native MSK (Amazon [Managed Streaming for Kafka](https://aws.amazon.com/msk/)) Source and Destination support accessing Kafka topics via IAM roles.


The Mask Function now supports toggling individual Masking Rules on and off, to enable iterative development and debugging.  It also now supports the SHA512 hash algorithm. 

The Redis Function now supports mutual TLS. 

Destinations' **Post-Processing** > **System fields** can now include `cribl_route`, identifying data's path through a given Route or QuickConnect. (To avoid serializing this field in existing [Serialize Functions](serialize-function), add a `!cribl_route` negation to those Functions' **Fields to serialize**, before any final `*` wildcard.) 

Cribl.Cloud users on all plans can now generate diagnostic bundles and send them to Cribl Support, directly from Stream's **Global Settings**. 

## Corrections

This release includes the following fixes:

### Sources and Collectors Fixes

CRIBL-12776 The REST Collector now enables multiple Collect parameters to share the same key name.

CRIBL-16102 Corrected Azure Event Hubs Source's ingestion of duplicate data.

CRIBL-16405 Corrected loopback through Cribl HTTP Source.

CRIBL-16346 On Kafka Sources and Destinations, the default **Authentication timeout (ms)** setting is now expanded from `1000` to `10000` milliseconds, to prevent SASL handshake timeout errors.

CRIBL-16187 OpenTelemetry Source and Destination can now parse events that contain the "summary" metric type.

### Destinations Fixes

CRIBL-15455 Sending data to an inactive  Destination no longer triggers log errors.

CRIBL-16299 On file-based Destinations, corrected a race condition that stopped Workers' event processing.

CRIBL-14651 HTTP-based Destinations now correctly display unhealthy status for client-side errors.

CRIBL-16494 Corrected Splunk Load Balanced Destination's unintended refreshes of senders' DNS.

CRIBL-16386 On Splunk Destinations sending to Splunk Cloud, corrected `LEB128 value out of range` errors.

CRIBL-7032, CRIBL-16830 On the Amazon S3 and MinIO Destinations, the **Max file open time** and **Max file idle time** can now be set as high as `86400` seconds (1 day). 

CRIBL-16288 The WebHook Destination now logs failed requests' retry status. This facilitates detecting requests that were successfully retransmitted. 

CRIBL-16300 The Elastic Destination now provides an **Omit document _id** option, so as to support Elastic TSDS (time series data stream) indexes.

### Startup and Authentication Fixes

CRIBL-16417 On Cribl.Cloud, corrected `JobStore` errors occurring after restarts and upgrades.

CRIBL-16479 Corrected inode exhaustion errors on config bundles. 

CRIBL-15467 Corrected Workers' failure to update to new config versions.

CRIBL-15516 Workers now reload TLS certificates without a restart after config deployments.

CRIBL-15352 KMS integration with HashiCorp Vault using AWS authentication now supports custom login paths.

### Event Breakers Fixes

CRIBL-15107 Corrected Event Breakers' inaccurate timestamp extraction after a Rule's configured **Future timestamp allowed** time.

CRIBL-15986 JSON Array Event Breakers now obey **Array Field** setting to recognize multiple arrays within an object.

### Packs Fixes

CRIBL-15492 Corrected Pack import errors on Packs exported with data samples.

CRIBL-15971 Previously discovered invalid Packs are now removed from the default package (`package.json`). 

### Diags Fixes

CRIBL-12757 Diagnostic bundles now exclude temporary files that hindered bundle creation.

CRIBL-15736 Corrected incomplete diagnostic bundles generated with Leader Failover enabled.

### Other Functional Fixes

CRIBL-16142 In QuickConnect, clicking a Pipeline's gear ⚙️ button now opens Pipeline Settings as intended.

CRIBL-15365 Changing logging levels via the Leader now correctly modifies logs for the Connections, Lease Renewal, Metrics, and Notifications services. 

### UX/UI Fixes

CRIBL-15717 Encrypted JavaScript fields are now formatted as hidden, with characters replaced by asterisks.

CRIBL-14173 Destination modals' **Test** tab no longer falsely shows Persistent Queue engagement when PQ is configured but not actually engaged. 

CRIBL-15960 Worker Group upgrades now correctly display their progress.



