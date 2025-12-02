# Cribl Stream 4.0.1


## New Features 

This release provides the following improvements:

CRIBL-11139 New native Destination enables writing log data to Google Cloud Logging.

CRIBL-10546 Splunk Search Collector and Source now support authentication via Bearer tokens. 

CRIBL-10218 Sumo Logic Destination's new **Data format** drop-down provides the option to output data in `raw` format. 

CRIBL-12199 Kafka, Confluent Cloud, and Azure Event Hubs Sources and Destinations now enable you to specify a number of Kafka retries to avoid losing data.

CRIBL-12784 Azure Monitor Logs Destination now enables you to specify the DNS name of the Log API endpoint that sends log data to a Log Analytics workspace.

## Corrections 

This release includes the following fixes:

### Security and Startup Fixes

CRIBL-13145 When accessing Cribl through a proxy, `manifest.json` now loads properly.

CRIBL-13554 Corrected spurious `Secret decrypt failed with error` log messages.

### Sources, Collectors, and Destinations Fixes

CRIBL-14093 Corrected Amazon S3 Source's stopped data input.

CRIBL-12837 Splunk HEC Source no longer discards multiple-metrics data.

CRIBL-13492 REST API Collector now correctly supports dual pagination parameters .

CRIBL-12198 REST API Collector's full runs now honor zero-based index.

CRIBL-13708 Google Cloud Storage Collector no longer throws invalid header check errors when a file has a `.gz` extension despite not being compressed.

CRIBL-13328 Updating the Office 365 Message Trace Source's **Authentication method** now correctly adjusts the **Resource** field's value.

CRIBL-12372 Amazon S3 Source, Collector, and Destination now validate the **Endpoint** field for proper formatting to avoid downstream errors.

CRIBL-9542 Google Cloud Storage Destination now closes files as intended.

CRIBL-13182 Splunk Load Balanced Destination's indexer discovery now supports targets with IPv6 addresses. 

CRIBL-13551 Splunk Load Balanced Destination's new configurable **Endpoint health fluctuation time allowance** option prevents premature `Destination unhealthy` backpressure and blockage.

### Functions Fixes

CRIBL-9539 Corrected Lookup Functions' intermittent failures within some Pipelines.

CRIBL-13019 Corrected Grok Function's memory leak.  

CRIBL-13860 Corrected Chain Function's creation's of extra Pipelines and Functions during initialization.

### Functional Fixes

CRIBL-12359 Corrected unintended triggering of Leader failover in high-throughput environments.

CRIBL-13868 Leader's remote access to Workers' UI now includes their Distributed Settings and Diagnostics Settings, as intended.

CRIBL-13583 Diag creation now proceeds despite renamed/missing files, and excludes temporary files. 

### UX/UI Fixes

CRIBL-12836 Preview Simple now correctly displays internal fields that have a `false` value.

CRIBL-13814 Routes display no longer breaks on changes to the Leader's base URL.

CRIBL-13207 Routes and Pipelines now support multi-line filtering of columns and input.

CRIBL-13681 Corrected spurious `Forbidden` error banners displayed to non-admin users. 

CRIBL-12201 Corrected embedded Packs Dispensary drawer's search speed and responsiveness. 


