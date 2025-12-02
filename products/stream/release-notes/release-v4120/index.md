# Cribl Stream 4.12.0


Cribl Stream 4.12.0 delivers key performance enhancements, new features, and critical fixes.

## Important Changes

### Upcoming Changes to Feature Availability on Free Tier

In an upcoming release, certain features, including persistent queueing (PQ), will no longer be available in the free tier of Cribl Stream. These features were temporarily accessible due to an oversight and are intended for Standard and Enterprise license plans only, as outlined on our [Pricing](https://cribl.io/pricing/) page. We hope the temporary access provided value and we encourage you to consider upgrading to maintain access to these advanced capabilities.

## New Features

This release provides the following improvements:

### Disk-Based Lookups

[Lookups](lookups-about) now provide an option to load lookups from disk as an alternative to the current in-memory approach. Where the size of a lookup and/or the number of Worker Processes require significant memory allocations, disk-based lookups optimize memory usage across all Worker Processes. This release also gives you more control over lookup file deployment to Workers, decoupling the deployment of lookups from configuration changes to your Worker Groups.

### Copilot Editor

This release introduces the next generation of Cribl’s Pipeline-building assistance with Copilot, our AI-powered assistant. Instead of manually writing and troubleshooting Regex or JavaScript Expressions, you can now use an interactive plain-language prompting experience to describe what you want to do with your data. Copilot handles the rest. Build sophisticated Pipelines that can filter and route data, change one data schema to another, or transform your data using some of our most commonly-used Functions.

### New Fold Keys Function

The Fold Keys Function transforms flat field names that include a common separator (such as `.` or `/`) into a more logical, nested structure. This Function restructures the field names into grouped, nested objects without changing their values, similar to the `foldkeys` operator in Cribl Search.

### Detailed Billing Data Visibility in Cribl.Cloud

We've made more improvements to your Cribl.Cloud billing and usage portal. Now, you can view all of these statistics in labeled, intuitive tabs:

- Your remaining credits, consumed credits, and average monthly consumption.
- Cumulative consumed credits per month across all products.
- Monthly data usage across all products, including infrastructure.
- Per-product consumption and credit cost in an easy-to-understand table format.

A detailed view lets you drill down a level deeper and see total, monthly, and average consumption and usage for each product. Finally, a separate tab just for invoices provides a one-stop shop to view finalized invoices that you can download and export, as well as draft invoices to see where you currently stand.

### Expanded Metrics and Configurable Levels for Integrations

On Cribl.Cloud, we've introduced configurable [metrics levels](internal-metrics#integration-metrics-levels) for integrations, allowing you to optimize system performance by tailoring metric collection. Administrators and Editors can adjust the granularity of metrics collected for Source and Destinations with the new **Metrics Levels** page in Worker Group settings. There are three available levels, Minimal, Basic, and Detailed, where each progressively increases the depth of collected metrics.

Additionally, we've expanded the [metrics available](internal-metrics#integration) for integrations. You can monitor throughput using rate-based metrics, assess success and failure with error-related metrics, and analyze latency and processing times through duration-based metrics.

### Data Transfer Across Workspaces or Environments

You can use the [Cribl TCP](sources-cribl-tcp) or [Cribl HTTP](sources-cribl-http)
Source and Destination pair to transfer data between
Workers that **do not** share a Leader, and only pay for the ingested data on
the receiving Organization. These restrictions apply:

- The Workspaces must be within the same Organization (Cribl.Cloud).
- The two Leaders must have the exact same license key installed (on-prem deployments).
- The Workers must have a paid Cribl.Cloud plan or active on-prem license.

For more details about this feature, go to [Transfer Data Between Workspaces or Environments](usecase-transfer-data).

>#### Limitations
>- This feature only supports sharing data between Cribl.Cloud Workers **or** on-prem Workers. 
>- Connected Environments/Universal Subscription setups are not supported.
{.box .info}

## Experience Improvements

- Packs now support Sources, Destinations, and Event Breakers, which allows you to create full, end-to-end solutions for data flows in a single Pack. You can then export these self-contained Packs to other Worker Groups/Fleets or environments in your organization to easily develop, test, and deploy configurations using DevOps best practices. You can also view metrics for Sources and Destinations at the Pack level.

- The Import Packs from Git feature just got better. You can now write back to the Git repository that your Pack was imported from, enabling full round-trip Git integration. Collaborate more effectively using branching, version control, and code-promotion workflows. Trigger external CI/CD pipelines with Git runners for streamlined deployments.

- The internal logs file `audit.log` now includes the first line of the Git commit.

- The Helm chart now allows you to configure a Kubernetes Service Account for the Leader deployment. When a Service Account is configured, IAM roles can be directly granted to the pods using that Service Account, simplifying access to cloud resources like AWS KMS without requiring separate credentials.

## Sources and Destinations

- The Cribl HTTP Source introduces a new `encoding` parameter for specifying character encoding. To ensure proper handling of multi-byte UTF-8 characters across chunks, set `"encoding": "utf8"` in the JSON configuration via Manage as JSON.

- The Syslog Source now includes an **Enable enhanced TLS handshake for proxy protocol** toggle to make TLS handshakes compatible with PROXY protocol v1 and v2. When enabled, the Source parses PROXY headers during the TLS handshake, reliably preserving the original source IP and port even for encrypted traffic routed through proxies or load balancers that prepend these headers.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-31666</div> | Fixed an error that prevented the ability to keep job artifacts in the Job Inspector. |
| CRIBL-23089 | Resolved an issue that prevented the global search box from returning results when a deployment is configured to use a baseUrl. |
| CRIBL-30594 | Fixed an error that prevented secrets from resolving correctly in standalone mode. |
| CRIBL-31430 | Fixed a regression that prevented the bootstrap script from working with local file paths. |
| CRIBL-30627 | Resolved an issue where proxy credentials that were stored in environment variables were unintentionally exposed in diagnostic bundles. These credentials are now properly masked. |
| CRIBL-31431 | Fixed an error that prevented the negative operator “`!`” from functioning as expected in wildcard lists within the Eval Function. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-30290 & CRIBL-31248</div> | Elasticsearch API Sources will now ignore the `Content-Type` and `Content-Disposition` headers when specified as **Extra HTTP Headers**. |
