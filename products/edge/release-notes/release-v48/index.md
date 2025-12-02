# Cribl Edge 4.8


4.8 is looking great! We've tinkered with Cribl Edge to bring you a whole host
of improvements. This release showcases performance enhancements to Windows
Event Logs Source, improved visibility into Source and Destination health status
at a Node level, a streamlined Global Settings experience in Cribl.Cloud, and so
much more.

## New Features
These features are new to Cribl Edge:

### Improved Source and Destinations Health Status Visibility at a Node Level

To help you monitor and troubleshoot Edge Nodes, you can use view the [health status for Sources and
Destinations](managing-edge-nodes) associated with a specific Node.

![Edge Node View showing the new Source and Destination health columns](edge-node-tab.png)
{scale="75%" border="true"}

To quickly identify Nodes that have issues, you can filter and sort the Source
and Destination columns in the Node list.

![Filter and sort the Sources and Destinations column in the Node list](source-destination-node-filter.png)
{scale="55%" border="true"}
 
In addition, you can investigate the activity of Sources, Destinations,
Pipelines, Routes, and Packs for an individual Node by looking at the [Data
Activity Tab of the Node Information drawer](node-data-activity) – you don't
need to Teleport into the Node.

![Data Activity panel showing Sources, Destinations, Pipelines, Routes, and Packs for a Node](node-info-panel-gif.gif)
{scale="75%" border="true"}

### Enhanced Windows Event Logs Source

We enhanced the [Windows Event Logs Source](sources-windows-event-logs) to
interact directly with the Windows Operating System to collect logs without relying
on Windows Tools like PowerShell or wevtutil. This newly enhanced Source now
supports rendering for XML events.

To enable this performant module, navigate to the Windows Event Log Source
configuration. Then, select **Advanced Settings** and toggle **Use Windows
Tools** off (default). Save the Source.

Here are some of the changes we made to events:

**JSON**

- The Keywords value is now a string instead of the number that didn't work
  correctly.
- Fields that had null values are no longer included.

**XML**

- Rendered event messages contain the `<RenderingInfo>...</RenderingInfo>` tag
  on `_raw` events.

## Experience Improvements

We introduced the following changes to Cribl Edge to improve your experience:

### Synergy Between the Target Version and Upgrade Status Functionality

We’ve updated the icon on the [Target Software Version control](upgrading-ui-status)
and updated the window that will open when you
select it. Now, select one button to change both the target software version for
a Fleet and view the status of upgrades in progress.

![New Target Version button and icon](target-version-button.png)
{scale="75%" border="true"}

### Include Windows MSI Logs in Error Diagnosis

If Windows MSI logs are available, you can choose to include them in the [diag bundle](diagnosing).

### Fleet-level Certificate Management

You can now configure and manage certificates at the Fleet level. 
[Subfleets automatically inherit these certificates](fleets#saving-and-managing-certificates)
and propagate them to Edge Nodes, simplifying deployment. View and manage
certificates in Fleet Settings.

### Improved Global Settings 

Now when you opt in to the new [Cribl.Cloud](workspaces) user interface, you’ll
get an improved Global Settings experience, in addition to the streamlined
global navigation experience and access to Workspaces, delivered in previous
releases.

### New Scale Reads Redis Function
The Redis function has a new setting, [**Scale Reads**](redis-function#cluster),
that allows you to specify which nodes in a Redis cluster should handle read
commands. You can choose to direct reads to primary nodes, replica nodes, or
both. This setting helps balance load and optimize read performance while
providing flexibility between strong consistency and improved scalability.

### Storage Card Display
To improve disk usage accuracy, the [Storage card](monitoring) now displays
total storage rounded to two decimal places on the Landing and Monitoring pages.
The result is a more precise view of your disk space, which allows you to plan
your capacity and use it more effectively.

### Run Cribl Image as Non-Root in Docker

To run the Cribl image as a non-root user, use Docker's `--user` option or the
Docker Compose `user` setting. For correct operation, set the `CRIBL_VOLUME_DIR`
environment variable to a writable location. Using a persistent volume mounted
to `CRIBL_VOLUME_DIR` is highly recommended for performance and data integrity.

### Adjust the Data Routes Table Width

In the Data Routes table, you can now adjust the column width by dragging and
dropping column dividers.

### Assign a Mapping ID to Teams

You can now assign a **Mapping ID** to Teams in Cribl.Cloud, allowing you to map
IDP groups to Teams.

## Sources and Destinations
We made improvements to these Sources and Destinations:

### ServiceNow Cloud Observability Destination

✨ **New** ✨  
We’ve added the [ServiceNow Cloud Observability Destination](destinations-servicenow). This allows you to
seamlessly route logs, metrics, and traces to the ServiceNow Cloud Observability
platform using the OpenTelemetry Protocol (OTLP). You can configure the
Destination with either HTTP or gRPC protocols, and securely manage your access
tokens using Cribl's text secret storage.

### Azure Sentinel Destination is Now Microsoft Sentinel

To align with Microsoft, we've updated the name of the Azure Sentinel
Destination to Microsoft Sentinel. We’ve also added a new setting, **Scope**,
that allows you to configure the integration across various Azure environments,
including Azure Public Cloud, Azure US Government Cloud, and Microsoft Azure
operated by 21Vianet.

### HTTP Raw and Bulk API: Enhanced Auth Token Metadata Handling

We’ve introduced enhanced metadata handling for **Auth tokens** in both HTTP Raw
and HTTP (Bulk API) Sources. This provides greater flexibility and control over
event processing, enabling you to add context to events based on the token used,
and enhancing security by avoiding direct use of authorization values in Routes
and Pipelines.

### More Supported Metric Types for OTel

The OpenTelemetry (OTel) Source and Destination now support OTLP Metrics sum,
summary, and histogram metric types. This ensures that all relevant metrics are
accurately represented and transmitted, including support within the OTLP
Metrics Function.

### Default Certificate Verification for HTTPS

We've strengthened security by enabling default certificate verification for
HTTPS connections. Several newly created Sources and Destinations now
automatically reject expired, self-signed, or mismatched certificates. This
change aligns with industry best practices for TLS verification and provides a
more secure and reliable data processing experience.

### Exclusive Support for Signature Version 4 Authentication

The Amazon CloudWatch Logs Destination now exclusively supports Signature
Version 4 authentication.

## Corrections
We made the following fixes in this release:

### Security Fixes

| ID          | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-25761</div> | @azure/msal-node has been updated to version 2.9.2 to include the latest security updates.|
| CRIBL-25764 | mssql2 has been updated to version 10.0.3 to include the latest security updates.|
|CRIBL-25758  |@google-cloud/pubsub has been updated to version 4.5.0 to include the latest security updates.|
| CRIBL-25756 | @grpc/grpc-js has been updated to version 1.9.15 to include the latest security updates.|

### Source and Destination Fixes

| ID          | Description|
| ------------| -----------|
|<div style="width: 100px">CRIBL-26107</div>| Splunk TCP Sources previously defaulted to the older S2S protocol version 3. This behavior affected new instances and Worker Groups/Fleets. Newly created Splunk TCP Sources within existing Worker Groups/Fleets, however, correctly defaulted to version 4. This issue has been resolved, and all newly created Splunk TCP Sources now default to the correct S2S protocol version 4. |
| CRIBL-25842 | Splunk Destinations experienced issues when sending data via Splunk-to-Splunk (S2S) protocol to Splunk. The connection was terminated before any data was sent, only when acknowledgments (ACKs) were enabled. |
| CRIBL-25972 | The SNMP Trap Source allowed MD5 authentication in FIPS mode. As MD5 doesn't meet FIPS security requirements, it caused data flow problems and error logs. | 
| CRIBL-26377 | The `_raw` field now expands fully to reveal its entire nested JSON structure when tapped, as expected. |

### Other Functional Fixes

| ID          | Description|
| ------------| -----------|
|CRIBL-25523|Kubernetes logs no longer show unnecessary repeated log entries for errors and warnings.|
|<div style="width: 100px">CRIBL-26719</div>| The C.Time.s3TimePartition() and C.Time.timePartition() Functions no longer cause Worker Processes to crash when encountering events with timestamps prior to the Unix epoch. |
|CRIBL-26478|The system no longer fails to properly release files from disk when you enable Source-side PQ.|
