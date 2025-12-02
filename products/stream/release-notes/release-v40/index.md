# Cribl Stream 4.0


## New Features 

One good thing that's not increasing gas prices this fall is the very thing you've all been waiting for – Cribl Stream 4.0. Here are the cool new things we're offering you at a 0% inflation rate:

### Cribl Search

We proudly announce a whole new product, [Cribl Search](/search/). It offers you federated searching through data at rest, on the edge, or in motion. Cribl Search is available exclusively on [Cribl.Cloud](deploy-cloud), and if you already have a Cribl.Cloud account, you'll see a very tempting Search tile next time you log into your Cloud portal.

### Cloud SSO Authentication

Cribl.Cloud now offers [federated authentication](cloud-sso), via OIDC or SAML, enabling you to manage users and groups in your identity provider of choice.

### Cloud Persistent Queues

With an Enterprise plan, you can now buffer and retain data by enabling [Persistent Queues](persistent-queues) on Cribl-managed, as well as hybrid, Workers. This feature offers simplified configuration – each queue is preconfigured with up to 1 GB of disk space per Cribl-managed Worker Process.



### Database Collector

A new [Database Collector](collectors-database) is available to pull events from MySQL and SQL Server database systems (DBMSs). Combine this structured data with unstructured machine data, to gain new insights in your downstream systems of analysis. You can configure multiple complementary [Database Connections](database-connections) in Stream's Knowledge libraries.

### REST Collector Enhancements

The REST API Collector now enables you to define custom HTTP verbs/methods, and custom/overridden Authorization headers. (CRIBL-9588, CRIBL-12659, CRIBL-9025)



### AppScope Configs Library

You can now define [AppScope Configs](appscope-configs) in the Knowledge libraries, as templates for [filtering](sources-appscope#filter) the data you ingest from AppScope.

### S2S v4 on 4

The Splunk Single Instance and Splunk Load Balanced Destinations now support v4 of the S2S (Splunk to Splunk) binary protocol. (CRIBL-10372)

### Google Chronicle Goes Custom

The Google Chronicle Destination now enables you to define multiple custom Log types, and select any one as the default type. (CRIBL-12311)

### Redis Goes Clusters

The Redis Function now [supports Redis Cluster](redis-function#cluster) instances. (CRIBL-8820)

### Cribl App Goes Sleuth

The Cribl App for Splunk now supports a `criblencrypt` command. (CRIBL-7672)

### Pipeline Profiling

New [Pipeline Profile](data-preview#pipeline-profiling) options enable you to analyze and optimize individual Functions' impacts on data volume, events count, and the parent Pipeline's processing time.

### Miscellaneous Improvements

Idle-session expiration time can now be extended as high as 60 minutes (from the previous 10).

To generate smaller diagnostic bundles when required, the on-prem UI now offers you the option to either enable or disable **Include metrics**.

Many UX/UI tweaks and improvements.

### Consolidated Top Navigation

Here's the one you've been waiting for: Cribl Stream has a whole new look. We've tossed the left nav, and we've consolidated all primary navigation into text links running along the top of your browser. 

No more struggling to remember what each of the left nav's little alien symbols stood for. Actually, no more left nav to hover over, expand, and mask your work. 

We've also harmonized the navigation across distributed versus single-instance deployments. Now, in both deployment modes, the top nav's new **Manage** link is your first step in configuring data I/O and processing, Packs, Knowledge libraries, and Notifications. Also, in both modes, the **Monitoring** link is now consistently on the top nav.

"Some of our options have changed," but the underlying pages are mostly the same, and we hope you'll find them easier to get to.

![Cribl Stream 4.0 navigation](nav-4.0-stream-annotated.fff0313b22.png)
{border="false"}

> The product selector at upper left appears only in distributed deployments, and it displays a **Cribl Search** option only on Cribl.Cloud.
> 
> In distributed deployments, most **Limits** settings – including **Metrics** and **Jobs** – have moved from **Global Settings** to **Group Settings**. (In single-instance deployments, they remain under **Global Settings**.)
> 
> The few remaining alien symbols at the top right are required by the Area 51 technology that Cribl software incorporates.
>
{.box .info}



## Renamed Sources and Integrations

CrowdStrike and Humio integrations are now renamed "CrowdStrike Falcon" in the UI and documentation, because there's a new sheriff in town. Access endpoints are unchanged.
- The CrowdStrike Source is now labeled [CrowdStrike FDR](/stream/sources-crowdstrike).
- The Humio HEC Destination is now labeled [CrowdStrike Falcon LogScale](destinations-humio-hec).

![](artist-formerly-known-as.f0123b9ae1.png)
{scale="7%" border="false"} 

## Cross-Compatibility for This Release

Cribl Stream 4.0, Cribl Edge 4.0, Cribl Search 1.0, and AppScope 1.2 are mutually compatible. If you integrate any of these products with earlier versions of peer products, some or all features will be unavailable.



## Deprecation Notices

> The following resources, previously badged Deprecated, are no longer supported as of this release. Please transition your configurations accordingly:
>
{.box .warning}
- Cribl Stream Source: Please use the [Cribl HTTP](sources-cribl-http) or [Cribl TCP](sources-cribl-tcp) Source instead.
- Cribl Stream Destination: Please use the [Cribl HTTP](destinations-cribl-http) or [Cribl TCP](destinations-cribl-tcp) Destination instead.
- Prometheus Publisher Function: Please use the [Prometheus Destination](destinations-prometheus) instead.
- Reverse DNS Function: Please use the [DNS Lookup Function](dns-lookup-function) instead.

Also as of this release, Cribl has removed the Intercom (support) chat widget from the lower-right corner of all on-prem and Cribl.Cloud deployments.  For other ways to contact support, please see [Get Product Help](/support/support-contact/).

## Corrections

This release includes the following fixes:

- CRIBL-10100 Corrected connection failure between Worker Nodes and the Leader caused by authentication token decryption. 

- CRIBL-11498 In API calls with the method `PATCH`, the `currentPassword` value must now match the current password.

- CRIBL-10280 Non-admin users now have more restrictive permissions, preventing access to certain secrets and passwords.

- CRIBL-11972 Using CRIBL_GIT_* environment variables no longer crashes the Leader on startup.

- CRIBL-12097 Sources no longer reject connections on ACME certifications with no Subject.

- CRIBL-10693 Fixed Cribl.Cloud bug that prevented changing the **Max sample size** in **General Settings** > **Limits**. 

- CRIBL-10909 Upgrading Workers no longer fails when **Validate Client Certs** is enabled. 

- CRIBL-11472 Adding special characters to cloned routes no longer produces an error. 

- CRIBL-9937 All events visible in Cribl Stream logs are now forwarded to external systems.

- CRIBL-12203 Fixed bug causing buffer flush errors and memory leaks with load-balanced Destinations that have persistent queueing enabled. 

- CRIBL-12124 Using **Preview Full** now correctly sends previewed data through the Source and Routes.

- CRIBL-11879 Resolved invalid metrics values.

- CRIBL-12102 AWS STS requests are now sent to a regional STS endpoint for reduced latency.

- CRIBL-11973 Bug fixed causing **Charts** tab for Sources to incorrectly display 0.00 for **Thruput In**. 

- CRIBL-12393 Malformed HEC events no longer cause spikes in CPU usage.

- CRIBL-13187 The Groups page no longer reloads every time a Group is committed or deployed.

- CRIBL-9829 **Next Processing Queue** and **Default _TCP_ROUTING** fields are now flagged, in the UI and docs, as taking effect only within the Cribl App for Splunk.

