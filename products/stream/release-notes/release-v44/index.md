# Cribl Stream 4.4


For our November release, Stream moves forward with a new Azure (ADX) Destination. You'll find enhancements to several other integrations, and a bushel of other new options and fixes. We've also upgraded to Node 18.15/​OpenSSL 3.0, and we've added guardrails to help you control persistent queues' disk footprint.

## New Features 

This release provides the following improvements:

New [Azure Data Explorer](/stream/destinations-azure-data-explorer) (ADX) native Destination enables sending security and observability data directly to ADX. This integration enables sending data to Azure Data Explorer customer tables as well as Cribl's supported native tables. 

System Metrics Source now supports configuring [Process Metrics](/stream/sources-system-metrics#process-metrics) reports, filtered by one or more running processes. 

New `C.Decode.inflate` and `C.Encode.deflate` expressions are available to handle data types like intercepted SAML logs. 

Elasticsearch Destination now supports updating Datasets via the `index` **Write action**. This enables replacing events in legacy indexes, which do not support  Elastic DataStreams' `create` action. 

**Monitoring** > **Logs** page provides a new **Export Logs as JSON** button. 

The Cribl product suite's underlying Node.js version has been upgraded to 18.15.0, including support for OpenSSL 3.0. Cribl now recommends using OpenSSL 3.0, as OpenSSL's most secure and actively supported version. 

## Persistent Queues Configuration Changes

> CRIBL-18456 When you upgrade customer-managed (hybrid and on-prem) Groups to Cribl Stream 4.4 and newer, any undefined PQ **Max queue size** values in Source/Destination configs in versions older than 4.4 will be set to the new 5 GB default value. This is a guardrail to control queues' consumption of host machines' disk space. 
> 
> (Changes here do not affect any existing config with a defined **Max queue size**, and do not affect the nonconfigurable 1 GB **Max queue size** for Cribl-managed Workers in Cribl.Cloud.)
> 
> Adjust this default to match your needs. If some of your integrations' persistent queues had an undefined **Max queue size**, queues larger than 5 GB will trigger PQ fallback behavior until you resize this limit.
> 
> Also in Cribl Stream 4.4 and newer, this field's highest accepted value is defined in the new **Group Settings** > **General Settings** > **Limits** > **Storage** > **Max PQ size per Worker Process** field. The corresponding `limits.yml` [key](limitsyml) is `maxPQSize`. Consult Cribl Support before increasing that limit's 1 TB default.
>
{.box .warning}

## Corrections

This release includes the following fixes:

### Access Control Fixes

CRIBL-20472 Members page no longer displays SSO users as `Unknown Unknown`.

### Sources and Destinations Fixes

CRIBL-19530 File Monitor Source's new **Force text format** [config option](sources-file-monitor#optionalsettings) supports collecting and  displaying binary file contents (such as `/var/log/audit/audit.log`) as text in events.

CRIBL-20861 Corrected File Monitor Source's collection of duplicate events.

CRIBL-20069 S3-related integrations now warn before accepting S3 bucket names containing invalid characters.

CRIBL-20571 Destinations now correctly log, rather than replaying, events whose fields mismatch the Parquet schema.

CRIBL-16217 File-based Destinations no longer throw incrementing errors caused by Parquet events not flushing from staging directories.

CRIBL-14628 Webhook Destination now supports sending data in a wrapper object.

CRIBL-20364 Corrected failures to send nested arrays to Splunk receivers via S2S v4.

### Processing and Functional Fixes

CRIBL-18899 Diag bundles now include git log for past 14 days, unless excluded via new UI toggle or CLI`-g` option.

CRIBL-20845 Inserting new comments in a Route's **Manage as JSON** editor no longer deletes existing comments.

CRIBL-16639 The Stream Projects UI now disallows removal of the `DevNull` Destination. This ensures that at least one Destination will always remain available to terminate data flow.

CRIBL-12118 Functions' key-value tables now support displaying warnings and errors per individual row.

### Persistent Queues Fix

CRIBL-19041 Corrected unintended triggering of persistent queue Notifications.

### Monitoring and UX/UI Fixes

CRIBL-20149 Corrected **Monitoring** > **Flows** page's whimsical `Sankey is a DAG, the original data has cycle!` error. All your data are belong to Flows, again!

CRIBL-19265 Preview/Sample Data pane > **OUT** tab no longer incorrectly renders null values as added/modified fields.

CRIBL-19692 Corrected **Preview Full** option's failure to **Send out** data to Destinations.

CRIBL-17781 **Workers** tab's **Config Version** column now displays 8 characters per Worker, rather than 7.


