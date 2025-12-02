# Cribl Edge 4.12.0


Cribl Edge 4.12.0 includes significant performance improvements, new capabilities, and important bug fixes.

## Important Changes



### Upcoming Changes to Feature Availability on Free Tier

In an upcoming release, certain features, including persistent queueing (PQ),
will no longer be available in the free tier of Cribl Edge. These features were
temporarily accessible due to an oversight and are intended for Standard and
Enterprise license plans only, as outlined on our
[Pricing](https://cribl.io/pricing/) page. We hope the temporary access provided
value and we encourage you to consider upgrading to maintain access to these
advanced capabilities.

## New Features

This release provides the following improvements:

### Custom Environment Variables in Cribl Edge Installer for Windows

The latest Cribl Edge installer for Windows now allows you to set custom
environment variables for the Edge service during installation.
This enhancement provides greater flexibility in tailoring your Edge service
configuration to your specific deployment needs. You can set these variables
using either the command-line interface or the interactive wizard.

### Federal Information Processing Standards (FIPS) Mode Support

This release of Cribl Edge introduces support for
Federal Information Processing Standards (FIPS) mode on both Linux and Windows,
allowing organizations with strict security requirements to confidently deploy
and manage their Edge instances.

### Toggle to Disable Listening on Port 9420

When Cribl Edge is in Distributed mode, you can now use the **Listen on port** toggle
in Fleet Settings to disable listening on port `9420`. This port is used by the
Cribl Edge UI, and can sometimes cause local vulnerability scanners to flag it
as untrustworthy.

### File Monitor Parses UTF-8 Encoded File Data

The File Monitor Source can now read and return event data for UTF-8 encoded files.

> Cribl Edge would previously identify log files with non-ascii UTF-8 as binary
> and stream it with a base64 encoding (assuming you do not have force text enabled).
> Now, Cribl Edge identifies those types of files as text and streams them with
> UTF-8 encoding.
{.box .info}

### Copilot Editor

This release introduces the next generation of Cribl Pipeline-building
assistance with Copilot, our AI-powered assistant. Instead of manually writing
and troubleshooting Regex or JavaScript Expressions, you can now use an
interactive plain-language prompting experience to describe what you want to do
with your data. Copilot handles the rest. Build sophisticated Pipelines that can
filter and route data, change one data schema to another, or transform your data
using some of our most commonly-used Functions.

### New Fold Keys Function

The Fold Keys Function transforms flat field names that include a common
separator (such as `.` or `/`) into a more logical, nested structure. This
Function restructures the field names into grouped, nested objects without
changing their values, similar to the `foldkeys` operator in Cribl Search.

### Detailed Billing Data Visibility in Cribl.Cloud

We've made more improvements to your Cribl.Cloud billing and usage portal. Now,
you can view all of these statistics in labeled, intuitive tabs:

- Your remaining credits, consumed credits, and average monthly consumption.
- Cumulative consumed credits per month across all products.
- Monthly data usage across all products, including infrastructure.
- Per-product consumption and credit cost in an easy-to-understand table format.

A detailed view lets you drill down a level deeper and see total, monthly, and
average consumption and usage for each product. Finally, a separate tab just for
invoices provides a one-stop shop to view finalized invoices that you can
download and export, as well as draft invoices to see where you currently stand.

### Data Transfer Across Workspaces or Environments

You can use the [Cribl TCP](sources-cribl-tcp) or [Cribl HTTP](sources-cribl-http)
Source and Destination pair to transfer data between Workers that **do not**
share a Leader, and only pay for the ingested data on the receiving Organization.
These restrictions apply:

- The Workspaces must be within the same Organization (Cribl.Cloud).
- The two Leaders must have the exact same license key installed (on-prem deployments).
- The Workers must have a paid Cribl.Cloud plan or active on-prem license.

For more details about this feature, go to [Transfer Data Between Workspaces or Environments](usecase-transfer-data).

>#### Limitations
>- This feature only supports sharing data between Cribl.Cloud Workers **or** on-prem Workers.
>- Connected Environments/Universal Subscription setups are not supported.
{.box .info}

## Experience Improvements

- The System State Source can now obtain event data from Windows machines via
  the native WMI. This change improves event collection performance.

- Packs now support Sources, Destinations, and Event Breakers, which allows you
  to create full, end-to-end solutions for data flows in a single Pack. You can
  then export these self-contained Packs to other Fleets or
  environments in your Organization to easily develop, test, and deploy
  configurations using DevOps best practices. You can also view metrics for
  Sources and Destinations at the Pack level.

- The Import Packs from Git feature just got better. You can now write back to
  the Git repository that your Pack was imported from, enabling full round-trip
  Git integration. Collaborate more effectively using branching, version
  control, and code-promotion workflows. Trigger external CI/CD pipelines with
  Git runners for streamlined deployments.

- The internal logs file `audit.log` now includes the first line of the Git
  commit.

- The Helm chart now allows you to configure a Kubernetes Service Account for the Leader deployment. When a Service Account is configured, IAM roles can be directly granted to the pods using that Service Account, simplifying access to cloud resources like AWS KMS without requiring separate credentials.

## Sources and Destinations

- The Cribl HTTP Source introduces a new `encoding` parameter for specifying character encoding. To ensure proper handling of multi-byte UTF-8 characters across chunks, set `"encoding": "utf8"` in the JSON configuration via Manage as JSON.

- The Syslog Source now includes an **Enable enhanced TLS handshake for proxy protocol** toggle to make TLS handshakes compatible with PROXY protocol v1 and v2. When enabled, the Source parses PROXY headers during the TLS handshake, reliably preserving the original source IP and port even for encrypted traffic routed through proxies or load balancers that prepend these headers.

## Corrections
This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31438</div>|Accessing **Health > Data > Sources** when teleported into an Edge Node no longer shows an incomplete Source list.|
| CRIBL-32285 | We improved an unclear error message that occurred when Cribl Edge installations failed due to a custom action running unsuccessfully.|
| CRIBL-23089 | Resolved an issue that prevented the global search box from returning results when a deployment is configured to use a baseUrl. |
| CRIBL-30594 | Fixed an error that prevented secrets from resolving correctly in standalone mode. |
| CRIBL-31430 | Fixed a regression that prevented the bootstrap script from working with local file paths. |
| CRIBL-30627 | Resolved an issue where proxy credentials that were stored in environment variables were unintentionally exposed in diagnostic bundles. These credentials are now properly masked. |
| CRIBL-31431 | Fixed an error that prevented the negative operator “`!`” from functioning as expected in wildcard lists within the Eval Function. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
| <div style="width: 100px">CRIBL-30290 & CRIBL-31248</div> | Elasticsearch API Sources will now ignore the `Content-Type` and `Content-Disposition` headers when specified as **Extra HTTP Headers**. |