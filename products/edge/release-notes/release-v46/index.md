# Cribl Edge 4.6


It’s April, so we’re showering you with new products, features, and bug fixes.
This release includes: Support for [Scaling Edge to 50k Nodes](how-to-scale-edge),
adding clarity to the [Global Settings Upgrade UI](upgrading-ui) when
you're upgrading the Leader, support for [Fleet tagging](creating-a-fleet), exposing
Edge usage in both [Cribl.Cloud Billing](cloud-portal#usage) and on-prem [Licensing](/stream/monitoring#licensing).

We're also introducing a new product, [Cribl Lake](/lake/), a new Cribl.Cloud user
interface, and your new favorite in-product assistant, Cribl Copilot!

> #### Upgrade Consideration for Leader Auth Token
>
> If the auth token for your Leader Node contains special characters, you
> must update the token to contain only allowed characters. Starting in version
> 4.5.1, check your auth token for any disallowed characters before you upgrade
> Cribl Edge to 4.5.1 and newer. The auth token can only contain these allowed
> characters:

> - a-z: Lowercase letters from a to z. 
> - A-Z: Uppercase letters from A to Z.
> - 0-9: Digits from 0 to 9. 
> - _: Underscore character. 
> 
> See [How to Secure the Auth Token for the Leader Node](securing-auth-token#changing-the-auth-token-value) for instructions.
{.box .warning}

## Product-Specific Connection Listeners

This update enhances connection management in Cribl Stream by introducing dedicated listener services for Cribl Edge and Worker Nodes.

### New Services

- `edge_connections`: Manages connections from Cribl Edge instances.
- `stream_connections`: Manages connections from Cribl Stream Worker Nodes.

### Existing Connection Listener

The existing connection listener continues to function for backward compatibility. It handles connections for:
- Cribl Edge and Worker Nodes on versions older than 4.6.0.
- Non-TLS deployments.
- Local development environments.

All existing scaling configurations will be respected. The existing connection listener will be deprecated in the future.

This improved approach to connection management offers greater granularity and control over your Cribl Edge deployments.

## New Products and Experiences

### Cribl Lake

This release introduces a new product to the Cribl Suite, [Cribl Lake](/lake/)! Cribl Lake
is a long-term, full-fidelity data storage solution for IT and security data.
Cribl Lake is also integrated with Cribl Search so you can search and send data
with ease!

To send data from Cribl Edge to Cribl Lake, you need to use the Cribl HTTP or
Cribl TCP Destinations as intermediaries. See [Integrating Cribl Lake with Cribl Edge](/lake/integrating-lake-with-edge) for more information.

### Cribl Copilot 

This release introduces Cribl Copilot, Cribl’s new AI assistant for Cribl.Cloud! Cribl Copilot helps you maximize efficiency without leaving Stream, Edge, Search, or Lake. To access Cribl Copilot, click the teal AI icon at the bottom right of any page. 



Note that the initial version of Cribl Copilot has the following limitations:

* To enable Cribl Copilot, a Cribl.Cloud Organization Owner or Admin must provide consent. This enables the assistant for all users in their Organization. Standard users can only access the assistant once their Organization Owner or Admin enables the feature.
* Organization Owners and Admins cannot withdraw consent from within the product. To disable Cribl Copilot, please contact Support. 
* Cribl Copilot leverages only two pieces of data when generating an answer: the documentation available on [docs.cribl.io](https://docs.cribl.io/), and whatever you type into the question box.

### New Cribl.Cloud User Interface with Workspaces

This release lets you opt in to the new Cribl.Cloud portal user
interface, which gives you access to the new Workspaces and Teams features. See
[Opt in to Workspaces](/edge/workspaces/#opt-in-to-workspaces) for more information.

Select **Try the New Cribl.Cloud** from the top bar to visit and examine the new
interface. You can come back to the classic UI at any time by clicking your
**Account** icon and disabling **Set the new experience as my default**.

#### Multi-tenancy Support with Workspaces

With [Workspaces](workspaces), Cribl.Cloud now supports multi-tenancy by
allowing you to create up to five dedicated isolated instances, to strengthen
your security, compliance, and isolation requirements. Each Workspace offers a
dedicated VPC that provides complete separation. You can manage all Workspaces
in an Organization from a single interface, which lets you control user access
to each Workspace separately with Teams.

Workspaces are an Enterprise-only offering.

## New Features 

This release provides the following improvements:

### Edge Leader Support for 50,000 Nodes

A Leader can now manage 50,000 Edge Nodes as you scale your deployments. To
support this effort, we’ve moved some things around in the UI and made some
configuration changes. To help you adjust your Leader settings and successfully
scale Edge, we’ve created a guide: [Scaling Edge Beyond 3k
Nodes](how-to-scale-edge) for more information.

### Improvements to Global Upgrade Settings

While core upgrading functionality remains unchanged, we’ve made significant improvements
to the Upgrade UI (accessed via Global Settings) in both [Edge](upgrading-ui)
and [Stream](/stream/upgrading#upgrade-the-leader-node-via-the-ui). You’ll see
the biggest difference in the UI when you select Path, an upgrade option
available in customer-managed deployments.

![Upgrade Path user interface](stream-upgrade-path-4-6.png)
{border="true"}

Here are the changes you can expect to see, by deployment type:

#### Customer-managed (on-prem)

- We’ve renamed **Enable automatic upgrades** to **Upgrade Workers
  automatically**, which more accurately describes the functionality. 
- When you select **Path**, you can now add paths for multiple versions of Cribl,
  and quickly see which version will be used to upgrade the Leader. 
- When upgrade packages are missing, you can easily copy / paste them into a new
  **Platform-specific package location** field. 
- Easily see which versions of Edge and Stream are in use, and on which
  platform.

#### Cribl.Cloud

- We’ve removed **Enable automatic upgrades** and **Enable Edge Legacy** upgrades.
- Hybrid Stream Worker Groups can be upgraded automatically using the **Upgrade
  Workers automatically** toggle, and will follow the version of the Leader.

### View License Usage by Product

On Cloud deployments of Edge, the [**Usage** tab](cloud-portal#usage) in **Cloud Billing** now also
shows Edge ingest bytes, so you can easily see how much of your license is going
towards Edge, Stream, Lake, and Search.

For on-prem deployments of Stream, the **Monitoring** page now shows daily Edge
utilization alongside Stream utilization. In Stream, navigate to **Monitoring**,
then hover over **System** and select
[**Licensing**](/stream/monitoring#licensing).

### Fleet Tagging

You can now [add tags to Fleets](creating-a-fleet), so you can filter your
Fleets by the keywords you designate. Add tags when you first create a Fleet, or
later when editing a Fleet.

### Reducing Ingestion for Dropped Events / Bytes

Any events dropped in pipelines or sent to DevNull Destination are no longer
counted [against your ingestion quota](licensing#exemptions). You must upgrade
Edge Nodes to version 4.6 in order for Nodes to report dropped events and reduce
ingest.

## Shared New Features

These new features apply to both Edge and Stream:

### Email Notification Enhancements

Cribl.Cloud Enterprise customers now have access to a preconfigured [email notification target](email-notification-targets),
 `system_email`, which makes it easy to send an email notification from Cribl
Stream, Cribl Edge, and Cribl Search.

### Netflow Source
The new [Netflow Source](sources-netflow) supports receiving NetFlow v5 data via
UDP.

### Bootstrap Workers

You have more flexibility when using the [Leader's bootstrap script](managing-edge-nodes#add-node)
to self-provision Edge Nodes. A new optional argument, `cribl_user_group`,
allows you to specify a different user group than the user. Previously, the
script assumed these were the same.

### Config Bundles

Config bundles can now be stored remotely in an AWS S3 bucket, offering a new
option for managing their distribution. See [Managing Edge Nodes](managing-edge-nodes#remote-bundle). 
This offloads the burden of bundle distribution from the Leader, to improve
performance and reduce load.

### IAM Roles

When using Assume Role to access resources in a different region than Cribl
Stream, you can target the AWS Security Token Service endpoint specific to that
region by using the `CRIBL_AWS_STS_REGION` environment variable on your Edge
Node.

### Copying Packs

You can now [copy Packs](packs#copy) directly between Worker Groups or Fleets,
eliminating the need for the export and import process, saving time, and
simplifying Pack management.

### Sources and Destinations

The Webhook Destination has a new [Advanced Format](destinations-webhook#advanced-format)
to enable integrating Cribl Stream with APIs that are not otherwise supported.
CRIBL-23170

Cloned Sources or Destinations are now created with `CLONE` appended to their
original ID. CRIBL-23919

The Amazon S3 Destination has improved (less noisy, more informative) logging
when the target S3 bucket for the Destination does not exist or is
misconfigured. The new [**Staging file limit**](destinations-s3#advanced-settings) 
setting provides control over when the Destination applies backpressure, and this
in turn reduces certain redundant messages. CRIBL-23679

[HTTP-based Destinations](destinations-cribl-http) now perform better when they
encounter unhealthy conditions on downstream receivers. CRIBL-23813

## Corrections

We’ve fixed some issues with Windows Edge Nodes:

- Unattended installs are no longer interrupted by a popup for the nssm service. CRIBL-22021
- Leftover binaries are now cleaned up as expected after an upgrade. CRIBL-22035
- Several issues reported by customers during upgrades installation and repairs
  are now fixed. CRIBL-23179
- The Windows System Metrics Source has been updated to more accurately report
  CPU usage metrics. CRIBL-23494 

## Shared Corrections

- The [Redis Function](redis-function) has a new **Reuse Redis connections** setting
  that enables you to manage how many TCP connections Cribl Stream makes to your
  Redis server. This frees you from worrying about burdening your Redis server
  with too many connections when using multiple Redis Functions (or referencing
  them from other Functions). CRIBL-20337

- The [OpenTelemetry Source](sources-otel) now properly logs conditions that had
  incremented the **Error count** in the Source **Status**, but failed to
  produce log entries. CRIBL-21703  

- The OpenTelemetry (OTel) mTLS implementation has been fixed to correctly
  validate TLS certificates. A new regex field has been added to OpenTelemetry
  Source: **TLS Settings (Server Side) > Common Name** which will be present only
  when:

    - **Protocol** is set to **HTTP** in General Settings
    - **TLS Settings (Server Side)** is enabled
    - **Authenticate client (mutual auth)** is toggled on in **TLS Settings (Server Side)**.

  When a client connects with these settings enabled, either the TLS certificate
  Subject Alternative Name (SAN) or Common Name (CN) must match the regex or the
  connection will fail. CRIBL-23758


