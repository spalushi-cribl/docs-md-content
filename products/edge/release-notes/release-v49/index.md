# Cribl Edge 4.9


It's spooky season, and Cribl Edge 4.9 is a treat! This October, we're handing
out Cribl Edge improvements like candy. This release introduces:

- A brand new user interface for all.
- The ability the filter Edge Nodes in a form-based filter window and with
  advanced JavaScript expressions.
- Source and Destination health status indication at the Fleet level.
- Preview support for Windows 10 and 11 on laptops and desktops with power
  management enabled, and more.

## CentOS 7 Deprecation

We've deprecated support for CentOS 7 on the Cribl Edge Leader and Edge Nodes.
We will remove full support for CentOS 7 in a future release. To ensure the
security and stability of your Cribl Edge deployment, we recommend that you
migrate to an operating system that will continue to receive security updates
and patches. Please visit the [CentOS website](https://centos.org/) for
migration information.

## New Features
These features are new to Cribl Edge:

### New Navigation Experience

We’ve streamlined your UI navigation experience across all Cribl products! The
UI changes that have been opt-in for Enterprise customers since release 4.5 are
now the default for all users. This preview period allowed us to incorporate
your feedback and optimize the layout. The main change you'll notice is that
we've decluttered the top of your browser, consolidating most navigation in a
collapsible left sidebar. This sidebar is context-sensitive, enabling you to
access product-specific options, as well as Global Settings.

### Improved Edge Node Filtering

When your Cribl Edge deployment has a large number of Nodes, you may want to
filter your Node list down to a manageable subset of Nodes. This can help you
monitor your Edge Nodes and troubleshoot any issues.

This release introduces the **[Filters](edge-nodes-filter)** option, which allows
you to:

- Apply filter criteria to your Node list using the **Filters** field, or by
  toggling on **Advanced** mode to create a custom filter.
- Quickly find Nodes by OS Platform, IP Address, install type, version, and more.
- Copy a link to the filter and share it with other Cribl users in the Workspace.

![New Edge Nodes Filters option](filters-ui.png)
{border="true" scale="75%"}

With Filters, you can find all of the Edge Nodes that aren't upgraded to
the latest version. Or, you may need to update the configuration of Nodes that
are running a certain operating system version, but you don't know which Fleets
they belong to.

### Fleet-level Source and Destination Health Status

To quickly identify any Fleets that are experiencing issues with Sources or
Destinations, you can view health information on a per-Fleet basis for the
Sources and Destinations you have configured.

![Fleet-level Source and Destination health](edge-home-ui.png)
{border="true" scale="75%"}

Cribl Edge surfaces Source and Destination health status for Fleets:

- On the [**Edge Home** page](explore-edge#edge-home), on each Fleet tile.
- On the [**Fleets** page](fleets-access#access-the-fleets-page), in the list of Fleets.
- On each Fleet, [on the Monitor tab](monitoring#fleet-level-source-and-destination-health-status).

### Windows 10 and 11 Laptop and Desktop Support (Preview)

> #### Preview Feature
> This Preview feature is still being developed. While Cribl considers it to be
> generally stable, we do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance,
> and documentation for the feature might be incomplete.
>
> While you should continue to submit feedback and bug reports through normal
> Cribl support channels, support might be limited while the feature remains in
> Preview.
{.box .info}

You can now deploy Cribl Edge onto [Windows 10 and 11 workstations](deploy-windows)
with power management enabled, such as laptops and desktops. This is in
addition to support for Windows 10 and 11 on always-on workstations. As this is
a preview feature, keep the following in mind when deploying:

- Cribl Edge might lose some data sent to TCP Destinations after a workstation
  resumes from a suspended or sleep state. This happens when the data is in
  local network buffers but hasn’t been sent out to the Destination. Instead,
  you can send data to an HTTP Destination.
- You might see partially broken events after a laptop resumes from a suspended
  state.
- HTTP Destinations might contain duplicate events.

### Three New Destinations

We now support Destinations for [ClickHouse](destinations-click-house), [Dynatrace HTTP](destinations-dynatrace-http) (logs only), and
[Dynatrace OpenTelemetry Protocol](destinations-dynatrace-otlp) (OTLP); giving
you more choices for handling your logs, traces, metrics, and more.

### Leader Limits Settings

We’ve introduced new **KV Store** [limit](limitsyml) settings to alleviate high CPU usage from
the API Process on the Leader Node.

## Functional Improvements

We introduced the following functional changes to Cribl Edge:

- We’ve updated the [Windows Add/Upgrade Node](deploy-windows#add-update-windows-node) scripts
  to include parameters to log the installation time for troubleshooting purposes.

- The [Windows Metrics Source](sources-windows-metrics) now uses the native
  Windows API module by default to collect system metrics. This toggle is
  located in Advanced Settings.

- Default listening host for Edge Nodes has been changed from `127.0.0.1` to
  `0.0.0.0`. You can override it with the `$CRIBL_API_HOST` environment variable.
  Now, you don’t have to remember to manually set the API Host field to `0.0.0.0`
  in Kubernetes deployments, since the Cribl container image sets the API Host
  when running with `CRIBL_EDGE=1`.

  - In Cribl.Cloud, you can now create API credentials for each individual
  Workspace. This gives you more precise control over Workspace management and
  lets you control Products programmatically. For each Workspace, you can define
  a Workspace permission and, if relevant, product-level permissions.

- An Admin user now has full permissions for all products, and can execute Leader
  commits and modify **Global Settings**.

### User Interface Improvements

- We’ve added support for vertical scrolling on the Cribl login page to better
  support display-related requirements.

- You can now reuse your preceding commit message by selecting **Use Previous
  Commit Message** in the Git Commit Changes modal.

- The user interface has new icons to better denote fields expecting JavaScript
  or regular expression format.

## Source and Destination Updates

We've made the following Source and Destination updates:

### General Integrations Improvements

- Sources and Destinations now support optional description fields.

- When Cribl Edge encounters an invalid octet count (per RFC 6587) while parsing an
  octet-counted stream of TCP syslog messages, it will now emit the events
  preceding the invalid count and close the socket connection. This reduces CPU
  and memory usage since Cribl Edge is not leaving the socket open.

### Improvements to Sources

- The [Splunk HEC Source](sources-splunk-hec) authentication tokens interface
  now has a more user-friendly display. You can expand or collapse
  authentication token details to view either a more detailed or compact version
  of the token details as needed.

- The [Splunk TCP](sources-splunk) and Splunk HEC (S2S v4) Sources now include an **Extract metrics**
  setting to ensure that Splunk-generated metric events are correctly identified
  and processed as Cribl metrics, maintaining their metric nature when
  forwarded.

### Improvements to Destinations

- Destinations now have the additional persistent queue modes: `Always On` and
  `Backpressure`. These modes allow you to choose how Cribl Edge writes data to disk
  during a backpressure event.

* To provide better error handling and data recovery for Filesystem-based
  Destinations, we've added a new dead-lettering feature. Cribl Edge disables
  this feature by default. Dead-lettering allows you to configure where Cribl
  Edge will store failed files, and how many times it will retry the file
  before considering it "dead." Edge will move failed files to the specified
  location after exceeding the retry limit.

- Filesystem-based Destinations now provide a **Disk space protection** setting,
  allowing you to either block or drop incoming events when the disk space falls
  below the globally defined **Min free disk space** amount.

## Corrections
We made the following fixes in this release:

### Operational Fixes

| ID          | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-27640</div> |Out of memory (OOM) errors on API child processes are logged in the `cribl_stderr.log` to assist with troubleshooting.|

### Source and Destination Fixes

| ID          | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-27624</div>|Because the **System fields** setting is unnecessary for the NetFlow and SNMP Trap Destinations, it has been removed from the **Post-Processing** options for those Destinations.  |
|CRIBL-28080|The Splunk Load Balanced Destination using S2Sv4 encountered issues when processing certain events:<ul><li>When events contained a field with an array value with many items, parsing could fail with the error message `RangeError: offset is out of bounds`.</li><li>When events contained a field with a non-stringified object value greater than 1 KB and **Nested field serialization** was set to JSON, the value could be truncated after stringification. The event would be sent but could miss some stringified object value.</li></ul>|
|CRIBL-25352|The Elasticsearch Destination did not honor retry attempts for failed HTTP requests when the target Elasticsearch Cluster was full and set indices to read-only.|
|CRIBL-27361|The Windows Event Forwarder Source was causing excessive load on the Leader, due to frequent individual RPC calls for state updates.|
|CRIBL-27926|Cribl TCP/HTTP Sources and Destinations did not log any errors if the Leader auth token could not be loaded from disk.|
