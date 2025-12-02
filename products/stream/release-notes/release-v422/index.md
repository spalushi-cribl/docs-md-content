# Cribl Stream 4.2.2




## Corrections

This release includes the following fixes:

### Access and Authorization Fixes

CRIBL-19074, CRIBL-19132 Organization-level Admins can now execute and view Leader commits, and can modify and view Global Settings.

CRIBL-19014 Organization-level Admins can now obatin auth tokens when bootstrapping Workers.

CRIBL-19078, CRIBL-19015, CRIBL-19062 Product-level Admin and Editor Permissions now include viewing and modifying Notifications and Notification targets, and creating Notifications.

### Sources Fixes

CRIBL-19154 Corrected File Monitor Source's memory leak.

CRIBL-19157 File Monitor Source now includes the `host` property in events as expected.

CRIBL-14488 Corrected Exec Source's failure on large input volumes.

CRIBL-19117 Splunk TCP Source now correctly logs an `Unsupported S2S protocol version detected error` on S2S version mismatch.

CRIBL-16340 Office 365 Sources now display verbose errors that Microsoft returns on bad requests.

CRIBL-18778 The Syslog Source now logs a `stats` message, by default every 60 seconds, that counts UDP events in, buffered, and dropped. The Syslog Source's **Status** tab > **Dropped** column now provides a clearer tooltip, explaining why UDP events were dropped.

### Destinations Fixes

CRIBL-19346 Corrected Azure Event Hubs Destination's excessive logging of `Flush called while flush is already in progress` errors.
	 	
CRIBL-17930 S3 Destination now allows opting out of verifying bucket permissions.

CRIBL-19077 Grafana Cloud Destination can now be configured with a Loki URL, a Prometheus URL, or both.

CRIBL-16944 Kafka-based Destinations no longer block accepting new events while flushing batches to brokers. This corrects these Destinations' failure to correctly engage persistent queueing when a downstream receiver was exerting backpressure.

CRIBL-16452 Enabled Kafka Destination's `debug` and `silly` logging event upon lost connection to brokers.

### Persistent Queue Fixes

CRIBL-18156 Corrected blocked data ingest on Source persistent queue read/write conflicts.

CRIBL-18162 Added stability check on Worker Nodes before recovering persistent queue files.

CRIBL-19385 Corrected PQ-related throughput drop, accompanied by `Failed to init from cursor` errors.

### Other Functional Fixes

CRIBL-19444 Corrected memory leaks when Workers reload after config updates.

CRIBL-18409 DNS Lookup Function can now resolve DNS short names, using three options: `Use /etc/resolv.conf`, `Use search or domain fallback(s)`, and `Fall back to DNS.lookup()`.

### Diagnostics Fixes

CRIBL-18005 Corrected failure to generate Diag bundles on teleported Workers (`Error while sending diag bundle` and `ERR_CONTENT_LENGTH_MISMATCH` errors).

CRIBL-19248 The `diag heapsnapshot` command enables capturing memory snapshots from on-prem deployments. (See [Including Memory Snapshots](diagnosing#memory-snapshots).)

CRIBL-18667 Increased `FileSystemOut` logging to better diagnose hung event processing.

### Monitoring and Metrics Fixes

CRIBL-17983, CRIBL-18837 Corrected **Monitoring** > **Daily License Utilization** dashboard's fidelity and labeling.

CRIBL-17763 Corrected **Monitoring** > **Notifications** page's failure to display Notification events.

CRIBL-18868 Destination Monitoring now shows health status from range's latest, instead of earliest, timestamps.

### Other UX/UI Fixes

CRIBL-16165 Worker Groups with large numbers of Sources now display Sources' status as intended.

CRIBL-15054 Git/Version Control history is now expanded to cover the last 50 commits.

CRIBL-17075 **Manage** > **Workers** page now shows Workers' IP addresses.

CRIBL-19279 Corrected **Upgrade & Share Settings**' visibility on teleported Groups.

CRIBL-17901	Accelerated Routes UI's responsiveness when searching Routes and adjusting filter expressions.


