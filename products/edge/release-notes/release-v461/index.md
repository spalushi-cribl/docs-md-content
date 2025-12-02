# Cribl Edge 4.6.1


This maintenance release of Cribl Edge includes bug fixes and new features.

## Shared New Features

These new features apply to both Cribl Edge and Cribl Stream:

### Sources and Destinations

Cribl Edge now defaults to Splunk-To-Splunk (S2S) Protocol Version 4. This
change ensures compatibility with your Splunk environment (version >9.1) and
aligns with Splunk's recommendation to use S2S v4 (they have deprecated v3).

[HTTP-based Sources](sources-cribl-http) now provide the option to enable a dedicated health check
endpoint, `http(s)://<host>:<port>/cribl_health`. This allows your Application
Load Balancers (ALBs) to directly monitor the health of individual Sources.

Batched [HTTP Destinations](destinations-cribl-http) have a new **Buffer memory limit (KB)** 
setting that allows you to manage the amount of memory used to temporarily store
outgoing batches before sending them. This is particularly useful for data with
high cardinality (many unique batches). Increasing the limit can allow batches
to grow larger before they're flushed, potentially improving efficiency.

The [Splunk HEC Source](sources-splunk-hec) now offers the optional setting
**Emit per-token request metrics**. This allows you to emit token-level metrics
– that were previously internal and logged in `metrics.log` – and route them to
Destinations, enabling improved visibility into individual token performance for
troubleshooting and monitoring HEC integrations.

The [Google Chronicle Destination](destinations-google_chronicle) now supports
data ingestion from Google Chronicle instances in the Canada and Tokyo regions.
(CRIBL-24797)

AWS S3 Sources and Destinations support many new [AWS Regions](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html#Concepts.RegionsAndAvailabilityZones.Regions):

- Asia Pacific (Hyderabad)
- Asia Pacific (Jakarta)
- Asia Pacific (Melbourne)
- Asia Pacific (Osaka)
- Canada (Central)
- Europe (Spain)
- Europe (Zurich)
- Israel (Tel Aviv)
- Middle East (UAE)

### Functions

The [OTLP Metrics Function](otlp-metrics-function) now automatically parses Resource Attributes from
Prometheus metrics, adhering to 
[OpenTelemetry specifications](https://opentelemetry.io/docs/specs/otel/compatibility/prometheus_and_openmetrics/#resource-attributes-1).
Automatic Resource Attribute parsing is also supported for a customizable list 
of top-level attribute prefixes similar to the [resource processor](https://opentelemetry.io/docs/collector/transforming-telemetry/#enriching-telemetry-with-resource-attributes).
CRIBL-22753

### Deprecation Notice

**Planned End of Support for Amazon S3 Signature Version 2 (SigV2)**

As part of our normal upgrade process and to adhere to Amazon Web Services (AWS)
recommended practices, Cribl will remove support for Amazon S3 Signature Version
2 (SigV2 for short) from our products in a future release.

**Customers using SigV2 should change to a different authentication method as soon as is practical.** 
AWS [deprecated SigV2 in 2019](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingAWSSDK.html#UsingAWSSDK-sig2-deprecation) 
and removed it from their new products in 2020.

## Corrections

In Cribl.Cloud, we fixed an issue where Edge Nodes fail silently when they can’t
connect to the CDN to send telemetry data. CRIBL-23370

When you upgrade a Windows installation of Cribl Edge that was originally
installed in a custom Windows directory ( `E:\` for example), the upgrader now
keeps Cribl Edge in the custom directory. Previously, the installer moved Cribl
Edge back to the `C:\` directory.
CRIBL-23032

In the **Global Settings**, **Group Upgrade** menu, **Latest CDN Version** is
now correctly hidden when you set Package Source to Path. CRIBL-24606

## Shared Corrections

The [Azure Data Explorer (ADX) Destination](destinations-azure-data-explorer)
does not automatically retry requests that fail with a `520 ServiceError`
response code as it should with all response codes over 500. CRIBL-23189

The [SignalFX Destination](destinations-signalfx) is sending fields whose names
begin with underscores, which it should not do. This causes errors when the
downstream SignalFX receiver tries to ingest the data. CRIBL-22404

When you configure the [OpenTelemetry Source](sources-otel) to **Extract spans**, 
the resulting span event cannot be passed to OpenTelemetry Destinations, like
Honeycomb. CRIBL-23394

We identified a memory leak affecting [Splunk Single Instance](destinations-splunk) 
and [Splunk Load Balanced](destinations-splunk-lb) Destinations under these conditions:

- **Max S2S version** is set to `v4`.
- There are more than 300 different combinations of `source`, `sourcetype`, and
  `host` on events flowing out of the Destination per Worker Process.
  CRIBL-23793

#### Improvements in S2S Protocol Support

Previously, when using the [Splunk Load Balanced Destination](destinations-splunk-lb) 
with S2S v4, events containing positive integer fields accidentally represented
as strings (like, "123" instead of 123) weren't converted to numbers before
sending to Splunk. This caused silent field drops in Splunk, as it expects
positive integers as numeric values. This fix ensures automatic conversion of
such string-formatted positive integers to numbers for proper handling by
Splunk. CRIBL-24240

Sources and Destinations that use Splunk S2S get `Failed to parse S2S payload`
errors with `Unexpected value type: 11` messages because values stored as
doubles are being scaled incorrectly. CRIBL-23663

Using S2S v4, the `__TZ` value remains the same even when timestamps change
between standard and daylight saving time. CRIBL-23564

The S2S v4 reader didn’t process some of the flushing signals emitted by Splunk
Universal Forwarders. Doing so caused Cribl Edge to break events incorrectly.
CRIBL-24466

### Packs Fixes

The [Packs](packs) page displays a `lacking sufficient permissions` message for
about 40 minutes after installation. CRIBL-24310

On a Leader Node, the API documentation does not show all the endpoints that
exist within a given Pack. CRIBL-18890

### Other Functional Fixes​

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

See [How to Secure the Auth Token for the Leader Node](securing-auth-token) for 
information on updating your auth token. CRIBL-23852

In proxied deployments of Cribl Stream and Cribl Edge, Nodes are blocked from
communicating with the Leader Node if there is a web proxy in between.
CRIBL-24451
