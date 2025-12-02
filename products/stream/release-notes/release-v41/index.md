# Cribl Stream 4.1


We don't quite have the answer to life, the universe, and everything yet, but this release **nearly** spells out 42. And we're proud to offer you some useful new capabilities and corrections.

## New Features 

This release provides the following improvements:

### Persistent Queues Predictability

When you configure Persistent Queues on Sources, `Always on` is the [new default mode](persistent-queues#source-pq-busy) for greater reliability. (We list a related [Breaking Change](#breaking) below, but it's an edge case.)

On [Destinations](persistent-queues#dest-4.1), you can now tune how events are sent out when queues drain. Leave the new **Strict ordering** toggle enabled to preserve Cribl Stream's default behavior, prioritizing the earliest events received. Or disable it to prioritize newly received events (with a configurable **Drain rate limit**, in events per second).

### Connections and Load Throttling

Along with the new PQ options, we've provided new controls to prevent connections from overloading Workers. At **Group Settings** > **Worker Processes**, you can now limit how many connections each Worker will accept upon startup or restart. You can also determine how long after startup to impose that limit.

At the same location, you can now set a CPU load threshold above which Workers will refuse new connections. This setting is not time-limited.



### New Sources and Destinations

We've added a new [Raw UDP](sources-udp-raw) Source. We've also added a new [Data Lakes > Amazon S3](destinations-data-lake-s3) Destination to stage data for [Cribl Search](/search/).

### Enhanced Sources, Collectors, and Destinations

The [Splunk HEC](sources-splunk-hec) Source now supports the S2S v4 protocol over HTTP. It also now enables you to specify and validate **Allowed indexes** in the `index` field of HEC events, with wildcard support. 

The Azure Event Hubs [Source](sources-azure-event-hubs) and [Destination](destinations-azure-event-hubs) can now authenticate against Active Directory via OAuth. 

The [Database Collector](collectors-database) now supports PostgreSQL databases. 

The Windows Event Forwarder [(WEF) Source](sources-wef) now supports Kerberos authentication.

The [Open Telemetry](sources-otel) Source now supports receiving events in the [OTLP](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/protocol/otlp.md) protocol, over either of two transports: [HTTP](https://opentelemetry.io/docs/reference/specification/protocol/otlp/#otlphttp) (with Protobuf payloads), or [gRPC](https://opentelemetry.io/docs/reference/specification/protocol/otlp/#otlpgrpc). 

The [Open Telemetry](destinations-otel) Destination now supports sending events to OTLP-compliant targets, using either HTTP (with Protobuf payloads) or gRPC as the transport. Also, this Destination now supports the Deflate compression codec, as well as Gzip. 

The Webhook Destination's new **Batch expression** field now enables sending data in a wrapper object, to interact with APIs that require JSON or other object formats. 

### Cribl.Cloud Shoots for the Moon

Enterprise Cribl.Cloud will now support creating multiple Worker Groups of Cribl-managed (as well as hybrid) Workers. You can manage the infrastructure that Cribl provisions for each Group by estimating (and revising) the Group's anticipated data ingest rate. You can also disable Groups to shut down data flow and costs, while retaining their configuration. [Details here](deploy-distributed#cloud-groups). Early access is by request.

Cribl.Cloud Organizations' Owners and Admins can now now obtain Role-based access tokens directly from the portal.

A Cribl.Cloud Organizations's Owner Role can now be shared and transferred among multiple users on that Org. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios.

### New Security and Access-Management Options

You can now use SAML to integrate third-party identity providers (IDPs) with on-prem Cribl Stream/Edge logins.

In v.4.1 and newer, Stream and Edge now encrypt newly created TLS private-key certs within Cribl config files. (See the related [Breaking Change](#breaking) warning below, to preserve your ability to roll back to earlier versions.)

### Eval Function's Toggles Accelerate Development/Debugging

The Eval Function now enables you to toggle individual **Add Field** expressions on or off. This facilitates iterative development and debugging, while retaining expanded configuration. 

### Lookup Expressions Can Retrieve More Columns

[C.Lookup()](cribl-reference#lookup) internal methods now support returning all columns from lookup files' matched rows.   

### Just in Case Anything Still Goes Wrong

Stream and Edge now support larger diag bundles – up to 1,500 MB. 

## Breaking Changes {#breaking}

> **Leader upgrades to v.4.1.x require Workers to also upgrade to v.4.1.x, for feature compatibility.** If you haven't enabled automatic upgrades of customer-managed (on-prem or hybrid) Workers, be sure to upgrade all Worker Groups before deploying changes from a v.4.1.x Leader. Deploys to Workers on a version older than 4.1 will silently hang.
> 
> **TLS certificate private keys within Cribl configuration files are now encrypted.** Before you upgrade to v.4.1.x and add new private keys, back up your existing unencrypted keys. Otherwise, you will be unable to roll back to earlier versions, because the new keys are not backward-compatible. (CRIBL-13535)
> 
> **Source-side Persistent Queues now default to** `Always on` **mode.** If you have code that automates creating Stream Sources, while assuming the previous default `Smart` mode, you'll need to update your code to preserve the existing behavior. This change does **not** affect existing Sources' configurations. (CRIBL-15565)
>
{.box .warning}

## Corrections

This release includes the following fixes:

### Sources, Collectors, and Destinations Fixes

CRIBL-15082 Amazon S3 Collector no longer browses unintended files.

CRIBL-7083 Improves the scalability of Amazon Kinesis Data Streams Sources.

CRIBL-14244 Splunk Destinations' Persistent Queues now prevent data loss during Worker outages, as intended.

CRIBL-15411 Corrected clearing of Destinations' Persistent Queues on Cribl.Cloud.

CRIBL-15874 Corrected PQ warnings of the form: `found single file with non-zero slice id that's greater than last cursor`.

CRIBL-15484 Corrected SignalFX Destination's recovery from 500 errors.

CRIBL-15852 Corrected Sumo Logic Destination's backpressure Notification and display.

CRIBL-15363] Corrected Cribl TCP Destinations's unnecessary reconnections with load balancing.

### Access and Functional Fixes

CRIBL-14580 LDAP access management now supports RFC 2307–compliant directories.

CRIBL-14423 Corrected `owner_all` Role's spurious `Forbidden` errors on Stream/Edge Home page.

CRIBL-7445 Improved throughput with Chain Function.

CRIBL-15540 `CRIBL_DIST_WORKER_PROXY` environment variable is no longer ignored when using `instance.yml`.

CRIBL-13521 `CRIBL_VOLUME_DIR` environment variable is no longer ignored by `getPidDir`.

### UX/UI Fixes

CRIBL-12655 Splunk Load Balanced Destination's **Status** tab now provides sortable columns.

CRIBL-12023 With Leader high availability, **Monitoring** > **System** display now identifies primary Leader by IP address.

CRIBL-15442 Corrected display of multi-line Filter expressions.


