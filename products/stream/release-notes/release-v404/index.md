# Cribl Stream 4.0.4


Encountering a 404 page would be disappointing, but we hope you'll enjoy our 4.0.4 release.

## New Features 

This release provides the following improvements:

CRIBL-5463 Webhook Destination now supports OAuth 2.0 authentication.

SAAS-2621 Cribl.Cloud Organizations now provide static ingress IP addresses, against which you can configure stable firewall rules.

CRIBL-14342 **New Group** creation modal provides a new **Description** field. Also, **Groups** list page now consolidates several columns into a new expandable **Details** column.

## Breaking Change

> CRIBL-14447 Packs developed for Cribl Stream v.4.0.x and above do not work in v.3.5.x. Cribl recommends that you upgrade to v.4.0.x to continue using these Packs. If you cannot upgrade, please consult Cribl Community Slack's `#packs` channel for other options.
>
{.box .warning}

## Corrections

This release includes the following fixes:

### Sources and Destinations Fixes

CRIBL-14301 CRIBL-14154 CRIBL-14579 CRIBL-14742 Corrected unintended rebalancing conflicts among Kafka, Azure Event Hubs, and Confluent Cloud Sources. Different Kafka topics within a namespace can now use the same **Group ID**.

CRIBL-15014 Syslog Source over TCP, with **Enable proxy protocol** toggled on, now properly closes connections.

CRIBL-14274 CRIBL-7017 Amazon Kinesis Data Streams Source provides new `Records limit per call` and `Total records limit` fields, which you can tune to accelerate data collection. This Source now also distributes shards to all Worker Processes evenly.

CRIBL-12461 Corrected S3 Source's memory leak.

CRIBL-13931 Google Chronicle Destination now sends `customer_id` and `namespace` fields in the payload.

CRIBL-14662 On Splunk Destinations writing out S2S v4, corrected the Publish Metrics Function's duplicated dimensions with **Output multiple metrics** enabled.

CRIBL-14797, CRIBL-14293 On Splunk Load Balanced Destination, fixed unintended block status and spurious "sending is blocked" messages when sending to healthy Splunk indexers.

CRIBL-14807 Elasticsearch Destination now places fewer connections in `TIME_WAIT` state, due to less-aggressive retries upon version-check errors from Elasticsearch receivers.

CRIBL-15288 Webhook and other HTTP-based Destinations now provide configurable Keep-alive behavior, enabling you to maintain only active connections and close others.

### Functional Fixes

CRIBL-14950 **Knowledge** > **Lookups** now correctly formats uploaded CSV files.

CRIBL-14299 Lookup Function in Distributed mode now automatically reloads modified Lookup tables.

CRIBL-14914 **Preview Full** > **Send out** option now functions correctly in Distributed mode.

SEARCH-1737 Corrected errors when reading Parquet files with large metadata.

CRIBL-14203 Corrected `Possible EventEmitter memory leak detected` error.

CRIBL-14785 Updates with **Rolling update** enabled now update Workers sequentially.

CRIBL-11198, CRIBL-14588 You can now delete selected logging-level channels.

### UX/UI Fixes

CRIBL-13811 Restored **Monitoring** > **System** > **Licensing** display options up to 365 days back.

CRIBL-14246 Syslog Sources now show correct counts for Events and Bytes in the Reports/Top Talkers UI.

CRIBL-14932 Cribl TCP Destination's status page now displays Hostname.

CRIBL-14987, CRIBL-12377 TCP-based Sources' `connection rejected` message is now logged at the `ERROR` (instead of `INFO`) level.

CRIBL-14258 In Leader's teleported view of individual Workers, the **Restart** button is moved to a clearer location.

CRIBL-14440 A sample’s **Created** date no longer disappears after you modify a sample Event.




