# Cribl Stream 4.6


> #### Upgrade Consideration for the Leader Auth Token
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
- Cribl Edge and Worker Nodes on a version older than 4.6.0.
- Non-TLS deployments.
- Local development environments.

All existing scaling configurations will be respected. The existing connection listener will be deprecated in the future.

### Scaling Product-Specific Listeners

You can scale `stream_connections` using the same methods as the original listener:
- Modify `service.yml` and adjust the `procs` field.
- Use the `/system/services-limits` API endpoint.

This improved approach to connection management offers greater granularity and control over your Cribl Stream deployments.

## New Products and Experiences

### Cribl Lake

This release introduces a new product to the Cribl Suite, Cribl Lake! Cribl Lake is a long-term, full-fidelity data storage solution for IT and security data. Cribl Lake is also integrated with Cribl Search so you can search and send data with ease. You can replay data from Cribl Lake using a dedicated Collector Source, and easily send data to Lake via the new Cribl Lake Destination.

### Cribl Copilot 

This release introduces Cribl Copilot, Cribl’s new AI assistant for Cribl.Cloud! Cribl Copilot helps you maximize efficiency without leaving Stream, Edge, Search, or Lake. To access Cribl Copilot, click the teal AI icon at the bottom right of any page. 



Note that the initial version of Cribl Copilot has the following limitations:

* To enable Cribl Copilot, a Cribl.Cloud Organization Owner or Admin must provide consent. This enables the assistant for all users in their Organization. Standard users can only access the assistant once their Organization Owner or Admin enables the feature.
* Organization Owners and Admins cannot withdraw consent from within the product. To disable Cribl Copilot, please contact Support. 
* Cribl Copilot leverages only two pieces of data when generating an answer: the documentation available on [docs.cribl.io](https://docs.cribl.io/), and whatever you type into the question box.

### New Cribl.Cloud User Interface with Workspaces

This release lets you opt in to the new Cribl.Cloud portal user interface, which gives you access to the new Workspaces and Teams features. 

Select **Try the New Cribl.Cloud** on the top bar to visit and examine the new interface. You can come back to the classic UI at any time by clicking your Account icon and disabling **Set the new experience as my default**.

With Workspaces, Cribl.Cloud now supports multi-tenancy by allowing you to create up to 5 dedicated isolated instances, to strengthen your security, compliance, and isolation requirements. Each Workspace offers a dedicated VPC that provides complete separation. You can manage all Workspaces in an Organization from a single interface, which lets you control user access to each Workspace separately with Teams.

Workspaces are an Enterprise-only offering.

## New Features

This release provides the following improvements:

### Email Notification Enhancements
Cribl.Cloud Enterprise customers now have access to the `system_email` preconfigured email notification target, which makes it easy to send an email notification from Cribl Stream, Cribl Edge, and Cribl Search.

### NetFlow Source

Customers can now use Cribl to collect, route, and control their IP traffic and network monitoring data via Netflow V5 (UDP). 

### TCP Load Balancing for Syslog Source

When Cribl Stream deployments in Distributed mode ingest high volumes of syslog data over TCP, you can enable TCP load balancing on the Syslog Source to load balance traffic across all Worker Processes. This can alleviate or prevent TCP pinning.

### REST Collector State Tracking

There are new options for viewing and managing where REST Collectors have left off after a collection job is interrupted. You can use these to satisfy the requirement, “Give me everything new since my last collection.” This also provides added resiliency against job interruption, meaning that there are virtually no gaps or drops in data when collecting with the REST Collector.

### S3 Source Checkpointing

In certain Worker Process interrupt or restart scenarios, the S3 Source can resume processing a file at its checkpoint – that is, where the Source left off. This includes network connectivity issues, Worker restarts and S3 sourcing errors.

### Bootstrap Workers

Worker self-provisioning via the Leader's bootstrap script gains increased flexibility with a new optional argument, `cribl_user_group`. This allows you to specify a user group that's different than the one associated with the user. Previously, the script assumed these were the same.

### Worker Group Tagging

You can now [add tags to Worker Groups](settings-group-fleet#worker-group-configuration), so you can filter
Worker Groups by the keywords you designate. Add tags when you first create a Group, or
later when editing it.

### Config Bundles

Config bundles can now be stored remotely in an AWS S3 bucket, offering a new option for managing their distribution. This offloads the burden of bundle distribution from the Leader, to improve performance and reduce load.

### Copying Packs

You can now copy Packs directly between Worker Groups or Fleets, eliminating the need for the export and import process, saving time, and simplifying Pack management.

### Documentation Enhancements

The documentation for all Notifications and Notification targets has been refactored into smaller, more focused topics.

Topics that mentioned Azure Active Directory (Azure AD) have been updated to reflect Microsoft's rebranding Azure AD as [Microsoft Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id).

### Sources and Destinations

The Webhook Destination has a new **Advanced** format that enables integration with APIs that Cribl Stream does not otherwise support. 

The Kafka Source now enhances record header handling. Incoming record headers are automatically converted from raw bytes to strings during data ingestion. This simplifies working with headers in Pipelines by eliminating the need to parse byte objects. Additionally, this change ensures accurate transmission of headers when routing data to Kafka Destinations, maintaining data fidelity throughout your Stream processing workflows.

When using Assume Role to access resources in a different region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) endpoint specific to that region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node.

Cloning a Source or Destination now creates an ID with the original ID and `-CLONE` appended.

The Amazon S3 Destination has improved (less noisy, more informative) logging when the S3 bucket that the Destination is trying to send files to does not exist or is misconfigured. The new **Staging file limit** setting provides control over when the Destination applies backpressure and this in turn reduces certain redundant messages.

The Google Pub/Sub Source has a new **Number of concurrent streams** setting that provides control over how many streams to pull messages from at one time, and thus the number of messages this Source pulls from the topic (if available).

HTTP-based Destinations now perform better when they encounter unhealthy conditions on downstream receivers.

The Filesystem Destination has a new **Header line** setting, enabling you to specify a line of text for the Destination to write at the beginning of each output file, followed by a newline character. This can be useful for adding a header row to CSV files, for example.

### REST Collector Improvements

The REST Collector has received several improvements. You can now:

- Track state by time or another arbitrary value over consecutive Collector runs. 
- Include an `authToken` variable in the Collect and Discover steps to dynamically access an auth token.
- Authenticate using Google service account credentials, scopes, and impersonation (with or without a text secret).

### Worker Groups

Customers can now scale Redis over multiple Pipelines by taking advantage of 
the new Worker Group setting **Reuse Redis connections**. When using multiple Redis Functions (or referencing them from other Functions), use this control to manage how many TCP connections Cribl Stream makes to your Redis server. The result is improved performance, better ability to scale, and freedom from worry about burdening your Redis server with too many connections.

### Improvements to Global Upgrade Settings

We’ve made significant improvements to the Upgrade UI (accessed via Global Settings) in both Edge and Stream. You’ll see the biggest difference in the UI when you select **Path**, an upgrade option available in customer-managed deployments.

![Upgrade Path user interface](stream-upgrade-path-4-6.png)
{border="true"}

Here are the changes you can expect to see, by deployment type:

#### Customer-managed (On-Prem)
We’ve removed **Enable automatic upgrades** in favor of **Upgrade Workers automatically**, which more accurately describes the functionality. 

When you select **Path**, you can now add paths for multiple versions of Cribl, and quickly see which version will be used to upgrade the Leader.

When upgrade packages are missing, you can easily copy / paste them into a new **Path** field.

You can now more easily see which versions of Edge and Stream are in use, and on which platform. 

#### Cribl.Cloud

We’ve removed **Enable automatic upgrades** and **Enable Edge Legacy** upgrades.

customer-managed (hybrid) Stream Worker Groups can be upgraded automatically using the **Upgrade Workers automatically** toggle, and will follow the version of the Leader.

## Corrections

### Source and Destination Fixes

The OpenTelemetry Source now properly logs conditions that produced no log entries even though they incremented the Source's **Status** > **Error** count. CRIBL-21703

### Security Fixes

The NodeJS version that runs in Cribl software has been updated to v.18.20.2 to include the latest patches detailed in [this security release](https://nodejs.org/en/blog/release/v18.20.2). CRIBL-23893 

The OpenTelemetry (OTel) mTLS implementation has been fixed to correctly validate TLS certificates. A new regex field has been added to OpenTelemetry Source: **TLS Settings (Server Side)** > **Common Name** which will be present only when:

- **General Settings** > **Protocol** is set to `HTTP`.
- **TLS Settings (Server Side)** > **Enabled** is toggled on.
- **TLS Settings (Server Side)** > **Authenticate client (mutual auth)** is toggled on.

When a client connects with these settings enabled, either the TLS certificate Subject Alternative Name (SAN) or Common Name (CN) must match the regex or the connection will fail. CRIBL-23758 

### Other Functional Fixes

The diagnostic bundle now supports symbolically linked log directories. CRIBL-23312 

Cribl Stream now correctly uses an `ldap://` URL for authentication when, in **Access Management** settings, **Type** is set to `LDAP` and **Secure** is toggled off. CRIBL-23237

Project editors are now able (as expected) to view and edit Stream Project Pipelines. CRIBL-23627

Diagnostic bundle creation will no longer fail, provided the system where Cribl is running has Git version 1.8.3.1 or newer installed. CRIBL-23374

The upgrade indicator dot in Cribl Stream Licensing (**Monitoring** > **System** > **Licensing**) is now displaying correctly. Previously, the dot may not have appeared after a successful upgrade. CRIBL-20604

Collection tasks will no longer be given to newly added Workers until the Workers have been fully configured. This is especially relevant when autoscaling Worker Groups. CRIBL-17336
