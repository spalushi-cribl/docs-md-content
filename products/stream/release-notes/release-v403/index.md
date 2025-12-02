# Cribl Stream 4.0.3


## New Features 

This release provides the following improvements:

CRIBL-14397 Webhook Destination now enables users to add dynamic headers.

CRIBL-13680 HTTP Sources now have a configurable **Keep alive timeout** parameter.

CRIBL-10145 OTEL Source now supports gzip compression on inbound data.

CRIBL-5924 Splunk TCP Source's new **Use Universal Forwarder time zone** option enables Event Breakers to derive events' time zone from UF-provided metadata.

CRIBL-14049 Windows Event Forwarder Source's XML queries now support `<Suppress>` elements.

CRIBL-14431 REST/API Collector pagination now supports custom last-page attributes.

## Corrections 

This release includes the following fixes:

### Security and Administration Fixes

CRIBL-14180 Restored `owner_all` Role's ability to commit and deploy.

CRIBL-13910 Corrected `Unknown config version` errors when deploying changes.

CRIBL-12792 Corrected `getAccessToken` function's unhandled exception.

CRIBL-12814 When **Resiliency:** `Failover` mode is enabled, UI **Distributed Settings** are now locked in both the primary and secondary Leader. Admins can still update distributed settings by [modifying config files](/stream/deploy-add-second-leader#yaml). This is an intermediate fix for `Token: command not found` errors that are triggered by token synchronization failure between the two Leaders.

CRIBL-14456 Restored ability to create new Mapping Rules. 

CRIBL-14307 Restored ability to set hybrid Workers' Worker Process count.

CRIBL-14222 Restored ability to filter Groups display.

### Sources, Collectors, and Destinations Fixes

CRIBL-14449 Corrected Syslog Source's parsing of timestamps when years are included.

CRIBL-12561 Corrected Syslog Source's display of Persistent Queue statistics.

CRIBL-14067 Crowdstrike Source's **Visibility timeout** now defaults to 6 hours and can be set as high as 12 hours.

CRIBL-14168 REST/API Collector's pagination **Response attribute** now returns correct value.

CRIBL-13492 Corrected REST/API Collector's endless loop with dual pagination parameters.

CRIBL-12840 Corrected Elasticsearch Destination's in-product backpressure.

CRIBL-14453 Google Cloud Chronicle Destination now correctly saves changes in distributed deployments with custom log types.

CRIBL-14041 QuickConnect-configured Sources no longer show "Enabled" status while disconnected.

CRIBL-12106 Splunk HEC Destination no longer emits `_CRIBL_QUEUE` and `_CRIBL_TCP_ROUTING` fields – these are now emitted only by the Cribl App for Splunk.

### Functions and Event Breakers Fixes

CRIBL-13216 Corrected Chain Function's timeout when forwarding to a Pack.

CRIBL-14165 Event Breaker Rulesets now support configuring evaluation of a minimum `_raw` length.

### Functional Fixes

CRIBL-8741 Worker metrics no longer steal CPU capacity and slow down overall display.

CRIBL-9089 Cribl.Cloud-managed Workers no longer report doubled events and bytes volumes in telemetry.

### UX/UI Fixes

CRIBL-14386 Corrected truncation of Pipeline groups' displayed names.

CRIBL-10314 Corrected `pqTotalBytes` aggregation.

CRIBL-13862 On single instances, corrected `/api/v1/master/groups` 404 response.


