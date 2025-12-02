# Cribl Stream 4.6.1


## New Features

This release provides the following improvements:

### AWS Regions Support

You can now create Cribl.Cloud Organizations in the AWS Canada Central, Australia, and Ohio Regions.

### Sources and Destinations

Cribl Stream now has a [Wiz Source](sources-wiz) for collecting data from the [Wiz](https://www.wiz.io/) cloud security platform. You can retrieve data from the Audit Logs, Configuration Findings, Issues, and Vulnerabilities APIs.

Cribl Stream now defaults to Splunk-To-Splunk (S2S) Protocol Version 4. This change ensures compatibility with your Splunk environment (version 9.1 and newer) and aligns with Splunk's recommendation to use S2S v4, since v3 has been deprecated.

HTTP-based Sources now provide the option to enable a dedicated health check endpoint, `http(s)://<host>:<port>/cribl_health`. This allows your Application Load Balancers (ALBs) to directly monitor the health of individual Sources.

Batched HTTP Destinations have a new **Buffer memory limit (KB)** setting that allows you to manage the amount of memory used to temporarily store outgoing batches before sending them. This is particularly useful for data with high cardinality (many unique batches). Increasing the limit can allow batches to grow larger before being flushed, potentially improving efficiency.

The [REST Collector](collectors-rest) now provides an optional **Capture response headers** setting. Enable this to include the headers from the API response of each collect call within the `__collectible` object added to your collected events. These headers are stored under the key `resHeaders`. This allows you to analyze response details for troubleshooting or gaining deeper insights into API interactions.

The [Splunk HEC Source](sources-splunk-hec) now offers the optional setting **Emit per-token request metrics**. This allows you to emit token-level metrics – that were previously internal and logged in `metrics.log` – and route them to Destinations, enabling improved visibility into individual token performance for troubleshooting and monitoring HEC integrations.

The [Google Chronicle Destination](destinations-google_chronicle) now supports data ingestion from Google Chronicle instances in the Canada and Tokyo regions.

Entries in the `cribl_stderr.log` file now have prepended UTC timestamps, to help with troubleshooting out-of-memory and other errors.

AWS S3 Sources and Destinations support many new [AWS Regions](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Regions):

* Asia Pacific (Hyderabad)
* Asia Pacific (Jakarta)
* Asia Pacific (Melbourne)
* Asia Pacific (Osaka)
* Canada (Central)
* Europe (Spain)
* Europe (Zurich)
* Israel (Tel Aviv)
* Middle East (UAE)

### Functions

The OTLP Metrics Function now automatically parses Resource Attributes from Prometheus metrics, adhering to [OpenTelemetry specifications](https://opentelemetry.io/docs/specs/otel/compatibility/prometheus_and_openmetrics/#resource-attributes-1). Automatic Resource Attribute parsing is also supported for a customizable list of top-level attribute prefixes similar to the [resource processor](https://opentelemetry.io/docs/collector/transforming-telemetry/#enriching-telemetry-with-resource-attributes).

### Deprecation Notice

**Planned End of Support for Amazon S3 Signature Version 2 (SigV2)**

As part of our normal upgrade process and to adhere to AWS recommended practices, Cribl will remove support for Amazon S3 Signature Version 2 (SigV2 for short) from our products in a future release. **Customers using SigV2 should change to a different authentication method as soon as is practical.** SigV2 was [deprecated](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingAWSSDK.html#UsingAWSSDK-sig2-deprecation) by AWS [in 2019](https://aws.amazon.com/blogs/aws/amazon-s3-update-sigv2-deprecation-period-extended-modified/) and removed from new products in 2020.

## Corrections

This release includes the following fixes:

### Source and Destination Fixes

The state tracking settings are incorrectly displayed for [REST](collectors-rest) and [Database](collectors-database) Collectors in the scheduling options when in Discovery mode. CRIBL-23915

The [Prometheus Scraper Source](sources-prometheus) is unable to run the `mkdir` command in its staging directory, causing jobs to fail with `failed to run job` errors. CRIBL-23261

The [Azure Data Explorer (ADX) Destination](destinations-azure-data-explorer) does not automatically retry requests that fail with a `520 ServiceError` response code as it should with all response codes over `500`. CRIBL-23189

The [SignalFX Destination](destinations-signalfx) is sending fields whose names begin with underscores, which it should not do. This causes errors when the downstream SignalFX receiver tries to ingest the data. CRIBL-22404

When the [OpenTelemetry Source](sources-otel) is configured to **Extract spans**, the resulting span event cannot be passed to OpenTelemetry Destinations, like Honeycomb. CRIBL-23394

We identified a memory leak affecting [Splunk Single Instance](destinations-splunk) and [Splunk Load Balanced](destinations-splunk-lb) Destinations under these conditions:
* **Max S2S version** is set to `v4`.
* There are more than 300 different combinations of `source`, `sourcetype`, and `host` on events flowing out of the Destination per Worker Process. CRIBL-23793

#### Improvements in S2S Protocol Support

Previously, when using the [Splunk Load Balanced Destination](destinations-splunk-lb) with S2S v4, events containing positive integer fields accidentally represented as strings (like, "123" instead of 123) weren't converted to numbers before sending to Splunk. This caused silent field drops in Splunk, as it expects positive integers as numeric values. This fix ensures automatic conversion of such string-formatted positive integers to numbers for proper handling by Splunk. CRIBL-24240

Sources and Destinations that use Splunk S2S get `Failed to parse S2S payload` errors with `Unexpected value type: 11` messages because values stored as doubles are being scaled incorrectly. CRIBL-23663

The S2S v4 reader didn’t process some of the flushing signals emitted by Splunk Universal Forwarders. Doing so caused Cribl Stream to break events incorrectly. CRIBL-24466

Using S2S v4, the `__TZ` value remains the same even when timestamps change between standard and daylight saving time. CRIBL-23564

### Packs Fixes

The Packs page displays a `lacking sufficient permissions` message for about 40 minutes after installation. CRIBL-24310

On a Leader Node, the API documentation does not show all the endpoints that exist within a given Pack. CRIBL-18890

### Other Functional Fixes

In the auth token for your Cribl Edge Leader, these special characters are not
allowed:

```
< > " { } | \ ^ ' ` \ 
```
- Whitespace is also not allowed.
- If your auth token contains these special characters, you must update it to
  remove them before upgrading to version 4.6.1.
- Additionally, we’ve added support for these special characters: `-` and `!`. If
  your Leader auth token already contains these characters, it will work as
  expected after you upgrade to 4.6.1.

      See [How to Secure the Auth Token for the Leader Node](securing-auth-token) for information on updating your auth token. CRIBL-23852

Deployments with an Enterprise license experience the same restrictions they would if using the free license: ingestion limits, and errors (including `403 Forbidden`) when trying to use Enterprise-only features such as Subscriptions, Projects, and Packs. CRIBL-24687

In proxied deployments of Cribl Stream and Edge, Nodes are blocked from communicating with the Leader Node if there is a web proxy in between. CRIBL-24451

Users who do not have the Admin role should be able to view the Licensing page, but cannot. CRIBL-24337

The **Redis Cache** setting fails to display properly in Cribl.Cloud. This setting is located at **Manage > `<worker-group>` > System > General Settings > Limits > Redis > Cache**. CRIBL-24269

In Cribl.Cloud, when you have multiple tabs open, leaving one tab inactive long enough to exceed **Global Settings > General Settings > Advanced > Session idle time limit** will cause Cribl.Cloud to log you out, closing all tabs. CRIBL-23887

When the Leader Node is down, Admin users do not have access to their policies, preventing Admin users from logging into Workers. CRIBL-20733

In the **Import Edge Data** window, Nodes are limited to 100 results. CRIBL-23192

On the **Test** tab for a Destination, Worker Nodes in the **Select worker** dropdown are limited to 100 results. CRIBL-23213