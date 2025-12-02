# Cribl Stream 4.5


## New Features

### Observability

Cribl Stream 4.5 introduces new capabilities for working with streaming telemetry data and Model Driven Telemetry (MDT):

* Via the new Model Driven Telemetry Source, Cribl Stream can now receive network device metrics and events directly from MDT, encoded as Protocol Buffers (protobufs), from devices that support dial-out MDT via gRPC. This increases observability while eliminating the need to pull data via cumbersome legacy SNMP tools. MDT’s messaging granularity is much finer than SNMP’s. Its push-based model reduces load on network devices, and its extensive ecosystem of data modules enables devices to collect many more kinds of data than they could with SNMP tools.

* The new OTLP Metrics Function can transform dimensional metrics to the OpenTelemetry Protocol (OTLP) metric data model, making them compatible with any OTLP-compliant downstream receiver.

* Cribl Stream's Kafka-based Destinations (the Kafka, Confluent Cloud, and Amazon MSK Destinations) can now send trace, metric, and log data encoded as OpenTelemetry Protocol (OTLP) using protobufs to downstream Kafka brokers.

### Notification Enhancements

* The new **Email notification** target type is a highly flexible tool for sending notifications via email, including alerting when operational issues arise, and scheduling search notifications to alert on specific conditions within data.

* An improved Notifications UI offers new capabilities includes cloning Notification targets, editing Notification configuration in a JSON editor, and limiting Notifications to the onset and resolution of triggering conditions.

### Randomly Generated Auth Tokens in New Installs {#random-auth-token}

* In new customer-managed Cribl Stream deployments, the Leader now uses a randomly generated auth token during installation. This improves the installation experience for Cribl administrators, who now no longer need to manually generate the Leader's auth token when enabling distributed mode. This does not affect existing installations; nor is it relevant for Cribl.Cloud, where auth tokens are already randomly generated.

### FIPS Mode (GA)

* FIPS (Federal Information Processing Standards) mode for Customer Managed deployments of Cribl Stream has now moved from beta to GA status. When running in FIPS mode, Cribl Stream will use only FIPS compliant cryptography algorithms, and will require FIPS compliant passwords for user login. New features include a CLI command for generating the NodeJS configuration file used to start Cribl Stream in FIPS mode; and, FIPS mode support for both systemd and non-systemd environments. See the [FIPS Mode](fips-mode) topic for details.

### More Features

* In **Retry Settings**, you can now configure HTTP-based Destinations and Notifications targets to honor `Retry-After` headers, improving reliability by better respecting the rate limits of potentially overwhelmed downstream systems. You can also configure retry behavior for when HTTP requests fail or time out, controlling retry timing (using an exponential backoff algorithm) for individual HTTP response codes. This can make delivery more reliable by adapting to the characteristics of different downstream systems. **Retry Settings** are now available for:

    * All HTTP Destinations
    * Azure Data Explorer Destination
    * Google Chronicle Destination
    * Elasticsearch Destination
    * Notifications targets that use HTTP (PagerDuty, Slack, and Webhook)

* The Database Collector now supports state tracking for all job types (ad hoc, full, and scheduled). Maintaining state between collection jobs avoids collecting the same events twice. **Manage State** operations now include view, refresh, modify, and delete, giving you more powerful ways to test and debug Database Collectors.  

* The Exabeam Destination now sends more metadata (specifically, the Exabeam Site ID) to Exabeam. This helps the Exabeam SIEM better detect the regional origin of incoming data.

* The Amazon Kinesis Data Streams Destination has new options for sending data the way different downstream receivers expect it: as an alternative to batching multiple events into a single NDJSON record (available previously), the Destination can now send a single event per record. It's now also possible to send events uncompressed.

* The Graphite Destination can now handle Graphite metrics that use `CR-LF`.
  
* Splunk HEC Source now allows you to disable individual auth tokens via a toggle in the UI.
  
* Cribl.Cloud can now receive data from syslog senders that are hard-coded to use port `514`.

* Cribl.Cloud Organizations can now be created in the AWS Europe (London) Region.

## Corrections

This release includes the following fixes:

### Sources

* The SNMP Source now handles SNMP traps containing protocol data units (PDUs) whose length is greater than 127, improving interpretation of those PDUs and avoiding dropping affected traps. CRIBL-22111

* The AppScope package is no longer included by default in a deployment. Instead, it is downloaded on the first usage of the AppScope Source. CRIBL-21554

### Collectors

* The REST Collector now handles:
   
    * URL parameters defined in the **Collect URL** field when using the Response Body Attribute pagination method, fixing a bug where these parameters were only included for the first page of data. CRIBL-19750

    * Discovery results that contain an HTTP `charset` parameter, avoiding the need to log `Failed to parse discover result, unsupported content type` errors. CRIBL-22521
  
### Destinations

* Filesystem-based Destinations running on Windows now write file paths with forward slashes, fixing a bug that caused certain partition expressions to produce paths containing backslashes. CRIBL-20601

### Functions 

* Using `C.Lookup` in a Code Function no longer breaks logging in the Worker process. CRIBL-22043

* The Masking Function now shows an appropriately truncated UI for users with limited permissions. CRIBL-21658

### Security Fixes

* As noted [above](#random-auth-token), new installs now default to using randomly generated auth tokens for the Leader. See [Securing Auth Token](securing-auth-token/). CRIBL-12596

### Other Functional Fixes 

* When restarted or upgraded, Cribl Stream deployments that use an external key management service (KMS) such as HashiCorp Vault or AWS KMS, can now enable the KMS to retrieve encryption keys for secrets. CRIBL-22299

* Managed Stream Worker Nodes have an improved upgrade process that gracefully prevents Leader Nodes from trying to upgrade Worker Nodes running in containers (such as containerd, Docker, or Kubernetes). Worker Nodes running in containers must be upgraded using an approach that is appropriate for the type of container (for example, you can use a Helm chart to upgrade Worker Nodes running in Kubernetes), without involving the Leader. CRIBL-18999

* The Routes UI is more responsive when searching a large number of Routes. CRIBL-20988

### End of Support for CentOS 6

* Cribl Stream version 4.5 **does not support** CentOS 6, which reached End-of-Life (EOL) status in 2020.

* If you are currently running Cribl Stream on CentOS 6, Cribl strongly recommends that you upgrade your operating system to CentOS 7 or later before updating to Cribl Stream version 4.5.
