# Cribl Edge 4.7


Cribl Edge brings the heat this summer with some sizzling updates. This season, you can handle even bigger deployments with support for a whopping [250K Nodes](how-to-scale-edge). Plus, Cribl Edge now runs on Windows 10 and 11 (64-bit only), giving you more flexibility. On top of that, the update brings improvements in licensing management, on-premise access controls, and enhanced support for Google SecOps and OpenTelemetry, making your data management this summer a breeze.

## New Features 

### Edge Leader Now Supports Up to 250K Edge Nodes

A Leader can now manage significantly larger deployments, scaling up to 100k in `Basic` metrics and 250k in `Minimal`. To accommodate this increase, the following UI and configuration changes have been implemented:

- **Map View** : The "Chart" and "During" fields have been removed to optimize performance when managing large deployments.
- **List View**: The health and status of all connected Sources and Destinations for each Node are now readily available in the List View.
- **Automated Connection Process Management**: Manual configuration of connection processes is no longer required. Simply enter the total number of Edge Nodes in the **Edge Nodes Count** setting within Global Settings. Edge will automatically deploy the necessary connection processes to support your infrastructure.
- **Enhanced Status Filtering**: The **Status** tab for Sources and Destinations now allows filtering Edge Nodes by their health status. By filtering based on health status, you can prioritize investigating nodes that are malfunctioning or experiencing errors.

For a detailed explanation of these changes and best practices for scaling beyond 10,000 Nodes, refer to the updated guide: [Scaling Edge Beyond 10k Nodes](how-to-scale-edge)

### Expanded Windows Support

You can now run Edge Nodes on Windows 10 and 11 (64-bit only), with limitations. To ensure continuous operation, power management features like sleep or hibernate must be disabled, and a reliable, always on network connection is required.


### Performance Improvements 

We've achieved a reduction in memory footprint by streamlining the Cribl Edge binary. This translates to improved performance, especially for deployments with limited resources.

## Shared New Features

These new features apply to both Cribl Edge and Cribl Stream:

### Universal Subscription
Do you have on-prem deployments of Cribl Edge and Cribl Stream, but you’re tired of managing multiple licenses and tracking spend across environments? If you’re excited about the simplicity of Cribl.Cloud credits but you’re not ready to fully convert from on-prem deployments, try [Connected Environments](cloud-connections) in our new Universal Subscription feature.

Connecting an on-prem Leader to Cribl.Cloud allows you to:
- Move your Cribl Edge and Cribl Stream from peak-ingest to true-consumption based usage.
- Monitor credit consumption for all of your on-prem Cribl Edge and Cribl Stream environments from one Cribl.Cloud account.
- Run multiple environments under one central billing model - no need for new licenses or purchases.

### New Global Navigation 

Building on the last release, in which we introduced the new Cribl.Cloud experience with [Workspaces](workspaces), we’ve added a global navigation system. Now when you opt in to the new Cribl.Cloud user interface, you'll get a streamlined global navigation experience. When you open a Cribl product in a Workspace, a product navigation pane on the left helps you move around within that product. Click **Products** to switch to a different product or go back to Cribl.Cloud home. 


### AuthZ Teams for Customer-Managed deployments 
Teams are now available in on-prem, customer-managed installations. You can easily assign Organization- and product-level Permissions to multiple Members by inviting them to a Team. Teams and Permissions will be replacing Local Users and Roles, so we encourage users to use Teams going forward.

### Expanded Support for Google Security Operations (SecOps) {#google}

Cribl v.4.7 helps get your data into Google SecOps (formerly known as [Google Chronicle](https://www.googlecloudcommunity.com/gc/SIEM-Forum/Chronicle-Security-Operations-has-been-rebranded-to-Google/m-p/744939)) with more choice and control.

You can now choose how to send your data to the Google Security Operations (SecOps) SIEM.
SecOps can receive data using either of two methods, and Cribl supports both: 

- You can send data already structured to satisfy the Unified Data Model (UDM) schema (new in Cribl v.4.7). This approach, where you use Cribl Pipelines and Functions to transform data into UDM, offers you maximum control over the structure of the data you send to SecOps.

- Or, you can send data unstructured, where SecOps parsers transform data into UDM (previously supported).

The Destination also now supports both versions of the API that SecOps uses: [Google Security Operations API](https://cloud.google.com/chronicle/docs/reference/ingestion-api) version 2 (new in Cribl v.4.7) and version 1 (previously supported).

###  More Complete OpenTelemetry Support

This release introduces more complete support for OpenTelemetry (OTel) in three main areas: OTLP Logs, batching for Logs and Metrics, and protocol specification versions.

#### Log Events 

The OpenTelemetry (OTel) Source and Destination can now receive and send OTLP Logs via HTTP and gRPC.

#### Batching Log and Metrics Events

- The new OTLP Logs Function supports batching of OTLP Log events based on Resource Attributes written to the event when it is generated.
- The OTLP Metrics Function can now reassemble extracted Metric events into batches and assemble dimensional metrics converted to OTLP Metrics format into batches too. For Metrics, batching means grouping events that arrive within a specified time window and that share the kind of metadata that OTLP calls Resource Attributes. In a batch, the resource attributes need only be written once (as opposed to once per data point), greatly reducing the amount of data sent over the wire.  

#### Latest OpenTelemetry Protocol Support

- The Cribl Stream OpenTelemetry (OTel) Source and Destination now support the latest version of the OpenTelemetry Protocol and OTLP Protobuf definitions, version 1.3.1. This is an opt-in experience, but is required when using OTLP Logs. (The previous version, 0.10.0, is still supported.) 

## Experience Improvements 

- If any Source or Destinations in a Worker Group become unhealthy, a red indicator (along with a count of number of unhealthy Sources/Destinations) appears on the Worker Group Overview page. 
- Worker Shutdown Telemetry provides visibility into buffered events via the new `shutdown.lost_events` internal metric. 
- Workers now attempt to download upgrade packages directly from Cribl’s CDN first. If the CDN is unavailable, they’ll fall back to retrieving the package from the Leader as usual. 
- Cribl Copilot can now help you use natural language to create Pipelines in Cribl Stream and KQL in Cribl Search. 
- In **Routing** > **Data Routes**, when **Bytes In** or **Bytes Out** shows a value of `0` because the nature of the event prevents bytes from being calculated, a tooltip now explains that this is the case. 


### Persistent Queue Improvements

Source PQ is now more robust to certain edge cases where data could be left on disk after Worker shutdown. 


### Packs Improvements

- When you export a Pack in `merge` mode, Cribl now deletes all encrypted fields (including secrets) from the Pack's Pipelines. If you import the Pack again, you must re-enter any secrets that were originally in the Pack. This improvement does not apply to the `merge safe` and `default only` export modes, which are scheduled for deprecation.

- This fixes a problem where just installing the Redis Pack, even without using it, caused its Redis Functions to attempt to contact Redis servers. 

### Encrypt Secrets Using the Command Line

You now have the option to [create and encrypt sensitive values](securing-and-monitoring#encrypt-secrets-using-the-command-line) using the same mechanism Cribl Edge employs internally for all sensitive fields. This ensures consistency and leverages the built-in encryption capabilities within Cribl.

### Cribl.yml Setting for Disabling SNI Routing

We're introducing a new setting in cribl.yml to help you manage Server Name Indication (SNI) routing, which can be helpful if you're encountering difficulties with proxies or firewalls.

If you have a proxy or firewall deployed between your Cribl Edge Edge Nodes/Workers and the Leader, you might be concerned about SNI usage introduced in Cribl v.4.6. This new setting allows you to disable SNI routing for specific Groups or Fleets.

To disable SNI routing for a Group or Fleet: Navigate to **Group Settings** > **General Settings** > **SNI** > **Disable SNI Routing** option. Commit and Deploy and then proceed with the upgrade. You must enable this setting for every Group/Fleet that is behind a web proxy or firewall.

In the previous release, a workaround existed for disabling SNI routing by modifying `instance.yml` on each Worker/Edge Node. This new setting in `cribl.yml` supersedes that approach and is the recommended method for disabling SNI routing.


### Sources 

The Syslog Source now automatically infers the framing type (transparent vs. octet-count) for incoming Syslog messages.


### Destinations

The Google Security Operations (SecOps) Destination offers new features in Cribl 4.7. See Expanded Support for Google Security Operations (SecOps) [above](#google). 

The Splunk Load Balancer Destination now supports multiple authentication tokens for indexer discovery in high-availability environments. This ensures uninterrupted data flow by allowing Cribl to cycle through tokens if the primary Splunk Cluster Manager connection fails.

### Deprecation Notice

**Trim Timestamp Function is Deprecated**

The Trim Timestamp Function is deprecated as of Cribl v.4.7, and will be removed in the next major version. If you are using this Function, you can instead use the Eval Function to achieve the same result.

## Corrections

### Shared Corrections

| ID                       | Description                   |
| -----------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------|
|  CRIBL-24505    | Throttling caused some Destination senders to reconnect at each DNS resolution interval. The problem occurred because a design conflict with the DNS resolution process led to the reconnection of all senders in each DNS resolution cycle (every 10 minutes by default). This could cause the Destination to temporarily block and/or engage Persistent Queues. |
| CRIBL-24860    | The Prometheus to OTLP Metrics conversion was failing due to an incorrect data type for the `bucket_counts` attribute. The problem occurred because the system provided a scalar instead of an array to the Protobuf encoder.|
| CRIBL-25004    | The Azure Sentinel Destination was incorrectly handling invalid URLs. This resulted in unhandled errors because it failed to validate URL formats, leading to rejections for invalid URLs.   |
| CRIBL-24871    | The Exabeam Destination was failing to send data because of a path construction error. This occurred when the Destination, which does not use a partition expression, received events from a Kafka-based Source with a numeric `__partition` field.   |
| CRIBL-23891   | Multiple `unsupported op-code` error messages were observed when using the Splunk TCP Source. This issue caused communication problems between Splunk Universal Forwarders and the Source, leading to data flow stalls.  |
|  CRIBL-25058  | The Windows Event Forwarding Source no longer displays the confusing "Cannot read properties of undefined" error. Instead, you'll see a clear message: "Client certificate missing Common Name (CN)." This fix addresses cases where incoming requests lack a client certificate with the required Common Name.    |
|  CRIBL-25089   | The Windows Event Forwarding Source required client certificates to have a Common Name (CN) explicitly and did not handle Subject Alternative Names (SAN). This caused errors when the incoming request did not have a client certificate with a CN attached.   |


## Function Fixes

| ID                       | Description                   |
| -----------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------|
| CRIBL-24508  |Changes to Redis limits settings in Distributed mode were not being reloaded. This affected both cache limits and connection limits when using the Redis Function, causing the new settings to not take effect without a manual restart.   |
|  CRIBL-23995 | The Event Breaker Function within a Pipeline failed when using Splunk TCP S2S version v4. The problem was due to the `__channel` attribute being set to undefined, causing the JSON Array event breaker to throw an exception and discard events.   |

## Security Fixes

| ID                       | Description                   |
| -----------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------|
| CRIBL-21784   | You can now configure the Splunk Load Balancer Destination, and/or Splunk Authentication, to validate certificates when discovering Splunk indexers by enabling the new **General Settings** > **Validate cluster manager certificates** control. Although this control is off by default, Cribl strongly recommends that you enable it, except when you must allow untrusted (for example, self-signed) certificates.|
| CRIBL-23513    |The Nodemailer dependency (third-party package) has been updated to `v6.9.12` to include the latest security updates.  |
| CRIBL-24643    | Upon startup, Cribl now automatically encrypts plaintext secrets found in `secrets.yml`. |
| CRIBL-24867    |Cribl Stream now requires Role-Based Access Control (RBAC) to be enabled for deployments in FIPS mode.|
| CRIBL-24907   | You can now use the `bundler` object within a GitOps workflow to help manage environment-specific secrets for applications managed by group configurations. For details, see [Worker Deployment: Handling Customer-Defined `.gitignore` Exclusions](version-control#bundler).|



## UI Fixes

| ID                        | Description                                                |
| ------------------------- | ---------------------------------------------------------- |
| CRIBL-24999 CRIBL-25020   | We've fixed the issue with Leader and Edge Nodes upgrades in v.4.6. You can now upgrade them in the UI as expected. |