# Cribl Stream 4.1.3


## New Features 

This release provides the following improvements:

A new native [Amazon Security Lake](destinations-security-lake) Destination is now generally available. 

On Cribl.Cloud Enterprise plans, [multiple Cribl-managed Worker Group](deploy-distributed#cloud-groups)s are now generally available. 

Cribl.Cloud Organizations' **Account** > **Organization** > **Billing** page has new color-coding to break out charges for processing versus infrastructure. You'll also see other UI improvements here to clarify credits usage over time. 

When adding new encryption keys, admins now have a one-time option to view or download the key in cleartext. 

Where Stream Pipelines have removed fields from events, Splunk (S2S v4) receivers no longer show them as undefined. 

Webhook Destination now supports [mutual TLS](destinations-webhook#tls-client). 

Datadog Destination's **Datadog site** field now supports [four additional locations](destinations-datadog#optionalsettings) beyond `US` and `Europe`. 

API process profiling is now available in **Settings** > **Global Settings** > **System** > **Service Processes** > **Processes**, and for Worker processes in **Worker Settings** > **System** > **Worker Processes**.  

## Corrections

This release includes the following fixes:

### Startup, Connection, and Version-Control Fixes

CRIBL-17779, CRIBL-17756 Corrected excessive CPU consumption in large deployments. This could cause the Leader to [become slow or unresponsive](known-issues#CRIBL-17779), which could also delay Collection jobs.

CRIBL-17443 Corrected `Possible EventEmitter memory leak detected` errors, which could overwhelm logs and cause Destinations to fail. We now scale up event listeners to match Sources' maximum connections.

CRIBL-17388 Enabled rolling upgrades by default, ensuring that new Workers start up before retiring Workers shut down.

### Security and Authorization Fixes

CRIBL-18038 Fixed a security vulnerability related to the AES-256-GCM encryption option introduced in v.4.1.2.

CRIBL-16928 Corrected `api/v1/m/default/lib/expression` endpoint's RBAC compliance.

### Sources and Collectors Fixes

CRIBL-16731, CRIBL-17493 Splunk HEC and Splunk TCP Sources now apply consistent event breaking, to prevent dropped index fields.

CRIBL-5397 Splunk TCP Source now relays connection metadata from Universal Forwarders and Heavy Forwarders to logs. This metadata includes `guid`, `hostname`, `capabilities`,`fwdType`, and `version`.

CRIBL-17538 Five HTTP-based Sources now have a configurable **Socket timeout** option: Prometheus Remote Write, Grafana, Loki, Raw HTTP, and Windows Event Forwarder. This enables these Sources to close idle sockets that clients have left open.

CRIBL-17737 Amazon S3, Amazon SQS, and CrowdStrike FDR Sources' **Polling timeout (secs)** default has been increased from 1 second to 10 seconds. This longer interval between retries reduces AWS costs.

CRIBL-16939 Office 365 Message Trace Source now supports OAuth certificates.

CRIBL-14129 Office 365 Message Trace, Prometheus Scraper, and Splunk Search Sources now provide a configurable **Job timeout**. This can be used to prevent metrics requests from hanging when upstream services fail to respond.

CRIBL-17584 Job Inspector's **Copy job permalink** option now copies the correct URL to the clipboard.

### Destinations Fixes

CRIBL-17715 CrowdStrike Falcon LogScale Destination's **LogScale endpoint** field is restored to the UI.

CRIBL-17749 Azure Monitor Logs Destination > **Log type** field's tooltip now clarifies allowed character set.

### UX/UI Fixes

CRIBL-15373 Packs list page now shows minimum Stream version on each Pack's row.


