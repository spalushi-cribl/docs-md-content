# Cribl Stream 4.1.2




## Breaking Change

> CRIBL-16729 The `ci` and `co` (duplicate) [internal dimensions](internal-metrics#dimensions) are deprecated as of Cribl Stream 4.1.2, and will be removed in a future version. Update any code that uses these dimensions with their respective replacments, `input` and `output`. (This applies to Cribl Functions and Pipelines, dashboards in downstream services, etc.)
>
{.box .warning}

## Corrections

This release includes the following fixes:

### Startup, Connection, and Version-Control Fixes

CRIBL-17264 With non-default working directories ($CRIBL_HOME other than `/opt/cribl/`), applying new config bundles no longer causes Workers to interrupt data processing.

CRIBL-16304 Corrected Database Connections' failure to authenticate on SQL Server databases using domain credentials.

CRIBL-14177 Corrected removal of remote Git repos.

CRIBL-16438 Commit modal no longer displays an (unusable) **Revert to** button available on latest commit.

### Security and Authorization Fixes

CRIBL-12348 Encryption keys can now be generated using the AES-256‑GCM algorithm. (Update: If you want to use this option, please upgrade to v.4.1.3.) 

CRIBL-14513 Corrected UI's incorrect display of certificates not present in the filesystem.

CRIBL-17053 Corrected permissions error on teleported Workers when navigating to Routes.

CRIBL-15416 Logs now clarify where `bad decrypt` errors originate.

### Sources and Collectors Fixes

CRIBL-16732 Four HTTP-based Sources (Amazon Data Firehose, Elasticsearch API, HTTP/S, and Splunk HEC) now enable you to set a **Socket timeout** threshold. This closes connections when their sockets remain inactive past the threshold interval.

CRIBL-7943 Splunk HEC Source provides [two new fields](sources-splunk-hec#cors-allowed) to enable CORS (cross-origin resource sharing) with Splunk senders.

CRIBL-5397 Splunk TCP Source now logs (at the `info` level) connection metadata from Universal Forwarders and Heavy Forwarders. This metadata includes hostname, GUID, IP address, version, and forwarder type.

SAAS-3981 [Cribl Internal](sources-cribl-internal) Source's CriblMetrics option is now available on Cribl-managed Workers in Cribl.Cloud, as well as customer-managed (on-prem and hybrid) Workers.

CRIBL-16570 Collectors now trigger Notifications as intended.

CRIBL-8230 Script Collector now supports setting environment variables via JavaScript expressions.

CRIBL-14161 Corrected Prometheus Scraper Source's failure to display dynamic targets.

CRIBL-16658 Exec Source now provides an **Environment** option to support GitOps.

### Destinations Fixes

CRIBL-12432 Where Destinations support load balancing, newly created Destinations now enable load balancing by default. This applies to: Splunk HEC, Elasticsearch, Syslog, Cribl HTTP, Cribl TCP, and TCP JSON.

CRIBL-17246 Corrected delayed throttling when specifying Persistent Queues' **Drain rate limit**.

CRIBL-1699 Google Cloud Storage Destination now supports environment variables.

CRIBL-16829 Corrected Projects' failure to configure Default Destination. 

### Other Functional Fixes

CRIBL-17317 Corrected high CPU usage with JSON Array Event Breakers. 

CRIBL-16628 Packs > Routes > Capture modal now provides the intended **Save as Sample File** option.

### Monitoring and Metrics Fixes

CRIBL-16503 Reconciled **Monitoring** > **System** > **Licensing** usage metrics with CriblMetrics Source's data, by compensating for missed heartbeats from Workers.

CRIBL-16902 Reconciled **Monitoring** > **System** > **Licensing** usage metrics with **Monitoring** > **Data** > **Sources** dashboard, by excluding data ingested via Cribl TCP Source.

CRIBL-16298 Reconciled CriblLogs Source's `inBytes` usage metrics with **Monitoring** > **System** > **Licensing** metrics, by preventing double-counting.

CRIBL-16244, CRIBL-16562 Corrected **Monitoring** > **Data** > **Sources** endpoint to correctly retrieve and display Collectors data.

### UX/UI Fixes

CRIBL-17049 Restored **Monitoring** > **CPU load**/​**CPU usage** sparklines' display of specific usage quantities on hover.

CRIBL-16736 Corrected typo in `cribl_metrics_rollup` Pipeline's tooltip.


