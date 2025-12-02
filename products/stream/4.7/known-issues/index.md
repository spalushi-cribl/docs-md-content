# Known Issues


This page lists known issues affecting Cribl Stream and/or Cribl Edge.

{{< id `CRIBL-26719` >}}

### 2024-08-06 – v.4.7.3 through Current –  C.Time Functions can crash Worker Nodes with old timestamps [CRIBL-26719]

**Problem**: The `C.Time.s3TimePartition()` and `C.Time.timePartition()` Functions, commonly used for S3 and Cribl Lake Destinations, are susceptible to crashing Worker Processes when encountering events with timestamps prior to January 1st, 1970 (Unix epoch). These events, represented by negative (or `0`) values in the `_time` field, cause Worker Nodes to consume 100% CPU and become unresponsive.

**Workaround**: To prevent Worker Nodes crashes, insert a Pipeline before any Function or Destination using these methods. Configure the Pipeline to modify the `_time` field of affected events to a positive value (`>0`) (e.g., using the `Date.now()` function).

**Fix**: Version TBD.

 {{< id `CRIBL-25524` >}}

### 2024-07-31 – v.4.7.3 through Current –  Chain Functions in Cribl Stream Projects can lead to infinite loops [CRIBL-25524]

**Problem**: Currently, users can create infinite loops within a project by referencing a Pipeline within itself using a Chain Function.

**Workaround**: Until a permanent fix is implemented, users should avoid creating circular dependencies within Pipelines within a single project. Do not reference a Pipeline within itself using a Chain Function.

**Fix**: Version TBD.

{{< id `CRIBL-26485` >}}

### 2024-07-26 – v.4.7.2 through Current – REST Collector state not updated on job cancellation [CRIBL-26485]

**Problem**: Canceling a REST Collector job from the [Job Inspector](monitoring#job-inspector) before it completes prevents state updates for events collected prior to cancellation. As a result, no state is saved for uncompleted tasks within the collection run.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-26478` >}}

### 2024-07-26 – v.4.7.2 through Current – When Source PQ is enabled, disk space fills to near capacity and Cribl Stream issues an error [CRIBL-26478]

**Problem**: With Source-side persistent queues (PQs) enabled, the system is not releasing files correctly, causing the disk space to fill and potential Worker Node failures. In addition, the system frequently logs the following error: `Cannot read properties of undefined (reading 'on')`. 

**Workaround**: You may be able to mitigate this issue by adjusting your Source-side PQ settings. Significantly decreasing the **Max buffer size** and increasing the **Max file size** may resolve the issue.

**Fix**: Version TBD.

 {{< id `CRIBL-26377` >}}

### 2024-07-22 – v.4.7.2 through Current –  `_raw` field expansion is incomplete [CRIBL-26377]

**Problem**: When capturing live data, the `_raw` field unexpectedly expands only to the first level. It should fully expand to reveal its entire nested JSON structure when tapped.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-24336` >}}
{{< id `CRIBL-26359` >}}

### 2024-07-18 – v.4.7.2 through Current – Collection jobs stuck in "pending" status [CRIBL-24336, CRIBL-26359]

**Problem**: Collection jobs can get stuck in **Pending** state when you commit and deploy configuration while the job is running.
This prevents the job from running again, and if multiple jobs freeze, the job queue can fill up.

**Workaround**: Restart the affected Worker and the Leader. To restart the Worker:

1. Log into Cribl Stream as an Admin.
1. Go to the **Workers** page.
1. Select the affected Worker's GUID. You will [teleport](/stream/distributed-guide/#worker-ui-access) to this Worker's UI.
1. On the purple bar, select **Restart Stream**.

To restart the Leader:

1. Get an [API token](/api/#criblcloud) for your Leader node.
1. Use the following curl command to restart the Leader:

   `curl "https://<workspace-org>.cribl.cloud/api/v1/system/settings/restart" -H "Authorization: Bearer $token" -H "Content-Type: application/json" --data-raw "{}"`

   where `workspace-org` comes from your Cribl.Cloud URL,
   and `$token` is the API token.

**Fix**: Version TBD.

{{< id `CRIBL-26311` >}}

### 2024-07-18 – v.4.7.3 – Unable to Load CPU Profiles in Cribl.Cloud [CRIBL-26311]

**Problem**: Cribl.Cloud is currently experiencing an issue where users cannot view or download CPU profiles directly through the UI. CPU profiling is useful in troubleshooting performance issues.

**Workaround**: 
1. **Open DevTools**: While on the Worker Process details page, right-click anywhere and select **Inspect**.
1. **Navigate to the Network Tab**: Locate the **Network** tab within DevTools.
1. **Filter Network Requests**: In the filter bar at the top of the Network tab, type `download`. This will filter the requests to only show those related to CPU profile downloads.
1. **Identify API Call**: Look for the latest network request with a path containing `download` (e.g., `/api/v1/workers/<worker_id>/profile/download`). This is the backend API call that fetches the CPU profile data.
2. **Access Profile Data**: Right-click on the identified API call and select **Open in Sources panel**.
3. **Save Profile**: Copy this data and save it to a file with a `.cpuprofile` extension.

For profile visualization, consider using an open-source tool like [Speedscope](https://www.speedscope.app/) after saving the profile data. A more permanent solution involving UI updates is planned for a future release.

**Fix**: TBD

{{< id `CRIBL-25928` >}}

### 2024-07-01 – v.1.6-4.7.1 – Splunk HEC Destination drops events when `_raw` is an empty string [CRIBL-25928]

**Problem**: When sending events to a Splunk HEC Destination, if the `_raw` field is an empty string, Cribl Stream sends an empty string as `_raw` instead of trying to send the full serialized event. This results in an error from Splunk and the event is dropped.

**Workarounds**: 
- Do not set `_raw` to an empty string in any Pipelines feeding a Splunk HEC Destination.
- If you do not know or cannot change the `_raw` contents prior to the Destination, add an Eval Function to a post-processing Pipeline with the expression: `_raw = _raw?.length ? _raw : undefined;`.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25927` >}}

### 2024-07-01 – v.1.1-4.7.1 – Splunk HEC Destination drops events with null `_time` values [CRIBL-25927]

**Problem**: Events sent to a Splunk HEC Destination with a null `_time` field are dropped by Splunk, halting further processing of the entire payload (potential data loss).

**Workaround**: Assign the `_time` field to the current time in a post-processing Pipeline by adding an Eval Function before the Splunk HEC Destination with the expression: `_time === null ? Date.now() / 1000 : _time`.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL‑25888` >}}

### 2024-06-27 – v.3.3-4.7.1 – Group and Fleet Naming Conflict [CRIBL‑25888]

**Problem**: Groups and Fleets in Cribl Stream can have similar names, potentially causing issues when differentiated only by a hyphen (-) and underscore (_). For example: When configuring a Worker Group (e.g., CRIBL_GOAT) through the user interface (UI), the configuration helper might mistakenly retrieve settings from the similarly named Group or Fleet (CRIBL-GOAT). This crossover can cause significant configuration errors and operational disruptions.

**Workaround**: To avoid this issue, we recommend adopting a naming convention with distinct names for each Group/Fleet.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL‑25763` >}}

### 2024-06-24 – v.4.6.0-4.7.1 – REST Collector job state issues with pagination [CRIBL‑25763]

**Problem**: The REST Collector’s job state is incorrectly updated between tasks when using pagination. This can lead to inconsistent state tracking and potential data gaps or overlaps in the collected data. You may notice that the state from one paginated task affects subsequent tasks within the same job, causing unexpected behavior.

**Workaround**: Do not use pagination in your collection jobs alongside state tracking. Instead, schedule individual collection jobs for each page to ensure state isolation.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25702` >}}

### 2024-06-21 – v.4.7.0-4.7.1 – Unlinked jemalloc library may cause memory spikes [CRIBL-25702]

**Problem**: jemalloc library being unlinked from the Cribl binary can potentially lead to memory spikes,
affecting the stability and performance of the application on large-scale deployments.
This issue only affects on-prem deployments using x84_64 architecture.

**Workaround**: In `/etc/systemd/system/cribl.service`, manually link jemalloc by adding
`Environment=LD_PRELOAD=/opt/cribl/bin/libjemalloc.so`:

```
# existing lines
Environment=CRIBL_SERVICE=1
Environment=CRIBL_SERVICE_NAME=cribl
# link the jemalloc library
Environment=LD_PRELOAD=/opt/cribl/bin/libjemalloc.so
```

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25601` >}}

### 2024-06-17 – v.4.7.1 through Current – Dashboard UI charts missing metrics after application update [CRIBL-25601]

**Problem**: After upgrading the application, dashboard charts may fail to display metrics recorded prior to the upgrade.
This concerns the following charts:

- **Events In and Out**
- **Bytes In and Out**
- **Free Memory**
- **CPU Load Average**

This issue is intermittent and only affects visualization in the dashboard charts.
Underlying data is flowing correctly.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-25572` >}}

### 2024-06-14 – v.4.0.0-4.7.1 – Syslog Sources with octet-count framing malfunction on unframed TCP messages [CRIBL-25572]

**Problem**: Syslog Sources configured with octet-count framing (pre v4.7.0), or with `octetCounting: true` (via **Manage as JSON** since v4.7.0), can malfunction if they receive unframed messages over TCP. This will cause the Source to return many warning messages like `Invalid octet count: undefined. Trying to skip to next frame` and the Worker may run out of memory.

**Workaround**: Do not send non-octet-counted messages to a Syslog Source that is configured for octet-count framing.

**Fix**: In Cribl Stream 4.7.2. (Closed as a duplicate of CRIBL-25408 which is fixed in 4.7.2.)

{{< id `CRIBL-25559` >}} 

### 2024-06-13 – v.4.7.1 – Project Editor unable to edit Pipelines [CRIBL-25559]

**Problem**: A user assigned the `Editor` permission within a Cribl Stream project is unable to edit pipelines in that project. When attempting to edit a **Pipeline**, the user encounters a permissions error.

**Workaround**: None.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25539` >}}

### 2024-06-13 – v.4.6.0-4.7.1 – Collector and Source state lost during Leader failover [CRIBL-25539]

**Problem**: When a Leader Node fails over, the state for REST Collectors, Database Collectors, Wiz Sources, Kinesis Sources, and Windows Event Forwarder (WEF) Sources is not preserved.

**Workaround**: None.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25435` >}}

### 2024-06-07 – v.4.7.0-4.7.1 – Prometheus summary metrics in OTLP Format cause Kafka Destination errors [CRIBL-25435]

**Problem**: Using a Prometheus Source for summary metrics, the `quantile_values` are formatted as an object. However, for OTLP (OpenTelemetry Protocol) serialization, `quantile_values` must be in array format. This discrepancy causes errors when attempting to write summary metrics to Kafka Destinations, resulting in failed metric transmissions.

**Workaround**: None.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25418` >}}

### 2024-06-07 – v.4.7.0-4.7.1 – SQL Server Collector Database Connection fails with Config authentication [CRIBL‑25418]

**Problem**: When upgrading to Cribl Stream 4.7.0, SQL Server Collectors configured to use the `Config` authentication method will fail to load, causing the Database Collector to stop working. The only valid authentication method for SQL Server Collectors will be the `Connection String`. Additionally, the section to manage Database Connections (under Knowledge) will not load existing connections.

**Workaround**: Manually edit each `database-connections.yml` configuration file where SQL Server Database Connections use the `configObj` authentication type. Change the authentication method to `connectionString`.

1. To convert a config object to a connection string, refer to the relevant fields in this table: [SQLConnection.ConnectionString](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring?view=netframework-4.8.1). For each line in the config object, take the key, find the corresponding label in the table, and create a key-value pair in the connection string. For example, if the config object is `{ userid: 'test', server: 'tcp:servername portnumber' }`, the connection string should be: `userID=test;server=tcp:servername portnumber`.

1. Edit the `database-connections.yml` file, change `authType` from `configObject` to `connectionString`, and add the connection string to the file. The config object can remain, but a valid connection string must be added to get the Database Collectors working again in Cribl Stream.

1. To store the connection string encrypted you need to save the configuration in the UI. Make a minor change to the Database Connection, like, add and then remove a character in the description, and then save the configuration. Commit and deploy the changes as necessary.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25411` >}}

### 2024-06-06 – v.4.7.0-4.7.1 – Extra Auth fields are visible when configuring a Splunk Load Balanced Destination [CRIBL-25411]

**Problem**: When configuring a Splunk Load Balanced Destination, you will see an extra **Authentication method** and **Auth token** field in **General Settings** (the fields are visible just above **OPTIONAL SETTINGS** and are outside of the **Authentication tokens** group). These fields are not needed, ignore them.

**Workaround**: Disregard the extra fields and use the fields in the **Authentication tokens** group instead.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-25400` >}}

### 2024-06-06 – v.4.7.0 – Syslog source UDP events incorrectly parsed when not starting with `<` character [CRIBL-25400]

**Problem**: When sending events via UDP to a Syslog Source where the events are not valid syslog and do not start with a `<` character, events may be broken incorrectly, leading to warnings, errors, and incorrectly parsed events.

**Workarounds**:
* Use the **Manage as JSON** option on the Source config and add this line: `inferFraming: false`.
* Use the Raw UDP Source instead for UDP datagrams that are not valid syslog.

**Fix**: In Cribl Stream 4.7.1.

{{< id `CRIBL-25372` >}}

### 2024-06-05 – v.4.7.0-4.7.1 – Some Sources and Destinations don't appear on the Health page for a Fleet [CRIBL-25372]

**Issue**: Some Sources and Destinations will not appear on the **Health** page
for a Fleet. This appears to affect predominantly Windows Sources.

**Workaround**: Navigate to the specific Source or Destination and select
**Chart** to view Fleet health metrics.

**Fix**: In Cribl Edge 4.7.2.

{{< id `CRIBL-25371` >}}

### 2024-06-05 – v.4.7.0 through Current – Upgrading to 4.7.0 with Google Cloud Security Operations Destination (formerly Google Chronicle) [CRIBL-25371]

**Problem**: When upgrading to Cribl Stream 4.7.0, if an existing **Google Chronicle Destination** (now Google Cloud Security Operations) is configured on a particular Worker Group, all Destinations in that Worker Group will fail to load and process data until a commit and deploy is performed on the Worker Group.

If you use the **Google Chronicle Destination**:

**Recommended**: Proceed with the upgrade, but be prepared to immediately perform a commit and deploy on affected Worker Groups after upgrading Worker Nodes and Leaders. Worker Nodes will only resume data processing after a successful commit and deploy.

**Fix**: Version TBD.

{{< id `CRIBL-25339` >}}

### 2024-06-04 – v.4.6.0-4.7.1 – When upgrading from the Leader, some Windows Edge Nodes can fail to upgrade [CRIBL-25339]

**Problem**: When upgrading from the Leader, some Windows Edge Nodes fail to upgrade. This occurs intermittently and is not predictable. This issue may also cause the Windows Edge Node to lose its connection to Leader and stop sending data. 

**Workaround**: If you encounter this issue, you can manually uninstall and reinstall Cribl Edge on any failed Windows Nodes. However, Cribl Edge Nodes on version 4.6.x continue to be compatible with Leader versions between versions 4.6.x and 4.7 so you do not need to upgrade Edge Nodes at this time. Specifically, we recommend that you do not set a `Target Version` on Fleets that contain Windows Edge Nodes until we resolve this issue.  

**Fix**: In Cribl Edge 4.7.2.

{{< id `CRIBL-25241` >}}

### 2024-05-30 – All versions through Current – Stopping a container does not initiate clean shutdown [CRIBL-25241]

**Problem**: Stopping a container does not issue a graceful shutdown signal SIGTERM to the Cribl process. Containers apply their own timeouts for shutdown, and these override any application-level shutdown timeouts.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-25147` >}}

### 2024-05-29 – v.4.7.0 through Current – Licensing page displays a permission access warning when a Cloud Connection is enabled [CRIBL-25147]

**Problem**: When you have an on-prem [Cloud Connection](cloud-connections#enable-the-on-prem-leader-connection-to-criblcloud) to Cribl.Cloud and navigate
to the Licensing page, a warning displays that the user does not have permission to access the resource.
You won't be able to access the license page with a connected environment (as your Cribl.Cloud account now handles credits for the connected environment) but it is not a permissions issue.

**Workaround**: To view billing information for your connected environments, log into
your Cribl.Cloud account and go to **Organization**, then **Billing & Usage**.

**Fix**: Version TBD.

{{< id `CRIBL-25018` >}}

### 2024-05-17 – v.4.6.1-4.7.1 – REST Collectors fail to capture response headers with pagination [CRIBL-25018]

**Problem**: When you use REST Collectors with pagination enabled and turn on the **Capture response headers** toggle (introduced in the 4.6.1 release and off by default), the response headers are not captured as expected. When you toggle on **Show internal fields** in the result and expand the `__collectible` object, the `resHeaders` field is missing. This issue prevents you from accessing response header information during paginated data collection.

**Workaround**: None.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-24999` >}}

### 2024-05-17 – v.4.6.0-4.6.1 – Unable to upgrade Leader and Workers from the UI when using CDN [CRIBL-24999]

**Problem**: You're unable to upgrade the Leader and Worker Nodes from the UI. The upgrade process fails to check compatibility for the CDN, causing the upgrade to be blocked. The UI may incorrectly indicate that the currently installed version is the latest, even when newer versions are available in the CDN.

**Workaround**: Point the upgrade process to the [download](https://cribl.io/download/) **Path** instead of the CDN.

**Fix**: In Cribl Stream 4.7.

{{< id `CRIBL-24877` >}}

### 2024-05-11 – v.4.6.1 through Current – OTLP Metrics Function does not process metrics unless all metric names are quoted literals [CRIBL-24877]

**Problem**: When the Publish Metrics Function is followed by the OTLP Metrics Function in a Pipeline, the OTLP Metrics Function may fail to produce any results. This failure is possible when the metric name expression for any of the published metrics uses an unquoted string literal and/or evaluates to a numeric value.

**Workaround**: If you must use a metric name expression in Publish Metrics, wrap the results of the expression in single or double quotes to ensure that it returns a string.

Optionally, leave the metric name expression blank and use the Rename Function to rename the metric names before sending them to the OTLP Metrics Function.

**Fix**: Version TBD.

{{< id `CRIBL-24860` >}}

### 2024-05-10 – v.4.5.0-4.6.1 – Incorrect `bucket_counts` format in OTLP Metrics via Kafka Destination [CRIBL-24860]

**Problem**: When sending Prometheus histogram metrics to an OTLP-compatible Destination via Kafka, the `bucket_counts` field is incorrectly formatted as an object instead of an array. This causes errors during Protobuf encoding, specifically the error: `.opentelemetry.proto.metrics.v1.HistogramDataPoint.bucket_counts: array expected`. This results in failed metric transmissions and potential data loss.

**Workaround**: None.

**Fix**: In Cribl Stream 4.7.

{{< id `CRIBL-24687` >}}

### 2024-05-03 – v.4.6.0 – Some licensing limits are being applied incorrectly [CRIBL-24687]

**Problem**: After the Leader restarts, Cribl Stream may not apply your license correctly. Because of this error, you may have insufficient user privileges for certain features and limits. For example, Cribl Stream may not correctly apply certain licensing limits, such as the total data volume allowed. Under certain conditions, this issue can block data ingestion. Note that this issue is timing-dependent so it can affect different areas of the product after a Leader restart.

**Workaround**: Two workarounds are available: 

- Reapply the license in the user interface without restarting the Leader.
- Wait approximately one hour for the license manager to perform a periodic license check. This will reapply the license and may resolve the issue.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-24516` >}}

### 2024-04-29 – v.4.5.1-4.7.1 – Full Preview doesn't work with Packs used as pre-processing Pipeline [CRIBL-24516]

**Problem**: Pipeline's **Full Preview** tab presents incorrect information when using a pre-processing Pipeline coming from a Pack.
This occurs only when previewing from the Leader – **Full Preview** from a Worker Node displays data correctly.

**Workaround**: Teleport to a Worker Node and run **Full Preview** from the Worker Node level.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-24505` >}}

### 2024-04-29 – v.4.5.1-4.6.1 – Throttling causes some Destination senders to reconnect at each DNS resolution interval [CRIBL-24505]

**Problem**: When you’ve configured **Throttling** on a Destination, a design conflict with the DNS resolution process leads to the reconnection of all senders in each DNS resolution cycle (every 10 minutes by default). This could cause the Destination to temporarily block and/or engage PQ, depending on the configuration, until senders reconnect. This affects the Cribl TCP, Syslog, and TCP JSON Destinations when load balancing is enabled.

**Workaround**: Don't use **Throttling** at the Destination.

**Fix**: In Cribl Stream 4.7.

{{< id `CRIBL-24451` >}}

### 2024-04-25 – v.4.6.0 – In environments that use a proxy or a TLS-breaking security appliance, the Worker / Edge Node to Leader communication is disrupted [CRIBL-24451]

**Problem**: In version 4.6.0, we added SNI routing to Cribl Edge and Cribl Stream. This caused a
communication issue between the Leader Node and Worker / Edge Nodes in proxied deployments of Cribl Stream and Edge.
Edge and Worker Nodes reference the Leader Node using the TLS server name, `stream` or `edge`,
which web proxies will block. 

**Workaround**: There are several workaround options:

- Disable SNI routing for firewalls / web proxies. See our instructions here: [Disable SNI Routing](upgrading#sni). 
- Downgrade the Stream Worker / Edge Node to `4.5.1`.
- Bypass Worker / Edge node traffic to the Crib.Cloud Leader or local Leader
  around the proxy or security appliance.
- (Applies to certain security appliances only) Verify whether connections to the
  hostname `stream` or `edge` are being blocked on the security appliance, and add an
  exception or bypass. 
  - Alternatively, modify the `/etc/hosts` file on Worker / Edge
  Nodes (`C:\Windows\system32\drivers\etc\hosts` for Windows Edge Nodes) to override
  the hostname(s) `stream` / `edge` to `127.0.0.1`. This prevents false-positive threat
  evaluations on the security appliance from blocking Worker / Edge to Leader
  traffic, and potentially block inputs/outputs on Worker / Edge Nodes.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-24310` >}}

### 2024-04-18 – v.4.6.0 – Cribl Stream displays a permissions-related error in some areas of the user interface [CRIBL-24310]

**Problem**: This issue is another form of CRIBL-24687, documented above.

**Workaround**: See CRIBL-24687.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-24269` >}}

### 2024-04-17 – v.4.6.0 – Limits settings for Redis are not available in Cribl.Cloud [CRIBL-24269]

**Problem**: The **Limits** [settings](settings-group-fleet#limits) at Worker Group level that customer-managed (on-prem) deployments use are not available in Cribl.Cloud. Because the Redis settings (including **Reuse Redis connections**) are within that **Limits** UI, Worker Nodes in Cribl.Cloud do not have access to those settings.

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-24169` >}}

### 2024-04-13 – v.4.6.0 – UI can omit details when viewing Email Notifications via Manage > Notifications menu [CRIBL-24169]

**Problem**: When an Organization Admin views an Email Notification via the **Manage** > **Notifications** menu, the notification's condition and/or other details may appear empty even though they exist. This also affects Product Admins for customer-managed (on-prem) deployments.

**Workaround**: The Organization or Product Admin should view the notification this way instead: In a Worker Group, open the **Notifications** tab for the Source that was configured to send the notification. The notification and all its details will appear as usual.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-23878` >}}

### 2024-04-02 – v.4.5.1-4.6.0 – Data Lake Amazon S3 Destination partitioning scheme is incorrect  [CRIBL-23878]

**Problem**: When you create a new Data Lake Amazon S3 Destination, its partitioning scheme will be incorrect.

**Workaround**: Deleting the incorrect partitioning scheme will cause the Destination to replace it with the correct one. To do this:

1. Open the Destination's configuration modal and click **Manage as JSON**.
1. Find the row beginning with the key `"partitionExpr"` – its value will be the incorrect partitioning expression `C.Time.strftime(_time ? _time : Date.now()/1000, '%Y/%m/%d')`.
1. Delete that entire row.
1. Click **OK** and save the changes.
1. To verify, reopen the configuration modal and click **Manage as JSON**. Check the `"partitionExpr"` row – its value will now be the correct partitioning expression `C.Time.s3TimePartition(_time ? _time : Date.now()/1000, 'h')`.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-23793` >}}

### 2024-03-28 – v.4.0.0-4.6.0 – Splunk Single Instance and Splunk Load Balanced Destinations sending high-cardinality events over S2S v4 can leak memory [CRIBL-23793]

**Problem**: The memory leak can happen when **Advanced Settings > Max S2S version** is set to `v4` and the combinations of `source`, `sourcetype`, and `host` on events flowing out of the Destination exceed 300 per Worker Process.

**Workaround**: Set **Advanced Settings > Max connections** to a non-zero value. This reduces the number of senders active at any one time, while distributing load more evenly as Cribl Stream rotates randomly through downstream IPs once per load balancing period.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-23657` >}}

### 2024-03-22 – v.4.5.0-4.6.0 – Leader incorrectly indicates version mismatch for Worker Nodes [CRIBL-23657]

**Problem**: After upgrading an instance of Cribl Stream with `CRIBL_VOLUME_DIR` set,
if the Leader has connected Worker Nodes running different versions,
the warning icon is incorrectly located next to Worker Nodes running the same version as the Leader,
instead of those running the Leader's prior version.

**Workaround**: Manually [restart](cli-reference#restart) Cribl Stream after upgrading with `CRIBL_VOLUME_DIR` set.

**Fix**: In Cribl Edge 4.6.1.

{{< id `CRIBL-23627` >}}

### 2024-03-20 – v.4.5.1 – Permissions error appears when you try to edit a Pipeline [CRIBL-23627]

**Problem**: Cribl Stream Project Members with the Editor permission cannot view or edit Pipelines within the Stream Project.

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23496` >}}

### 2024-03-18 – v4.5.1 – An error can occur when you click **Save** to create a new Worker Group or Fleet [CRIBL-23496]

**Problem**: In Edge and Stream, an error can occur when you click **Save** to 
create a new Worker Group or Fleet. This error appears as a warning: 
`The Config Helper service is not available because a configuration file doesn't exist or the settings are invalid. Please fix it and restart Cribl server.`

**Workaround**: Refresh the page.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23495` >}}

### 2024-03-18 – v.4.3.1 through Current – Empty directories that contained Parquet files are not deleted [CRIBL-23495]

**Problem**: When certain Sources and Collectors ingest or collect Parquet files, they create directories for storing those files. After they send and delete the files, they should also delete the directories, but fail to do that, potentially consuming large numbers of inodes on the storage volume. Sources affected: Azure Blob Storage, Amazon S3. Collectors affected: Amazon S3, Filesystem/NFS, Google Cloud Storage.

**Workaround**: Configure a cron job to delete the empty directories. 

**Fix**: Version TBD.

{{< id `CRIBL-23436` >}}

### 2024-03-14 – v4.5.1 – Amazon S3 Source and Destination, Amazon SQS Destination, and CrowdStrike Source crash in FIPS mode [CRIBL-23436]

**Problem**: When Cribl Stream in FIPS mode attempts to read or write files, the Amazon S3 Source and Destination, Amazon SQS Destination, and CrowdStrike Source crash with a `digital envelope routines::unsupported` error. This happens because the underlying Amazon AWS SDK v2 is configured to use the MD5 algorithm, which is not allowed in FIPS mode. Cribl Stream 4.6.0 fixes this problem by configuring the AWS SDK to skip the checksum computation that used MD5. 

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23374` >}}

### 2024-03-14 – v.3.2.0-4.6.0 – Extracted traces from the OpenTelemetry Source not sent to OpenTelemetry Destinations [CRIBL-23394]

**Problem**: When the OpenTelemetry Source is configured to **Extract spans**, the resulting span event cannot be passed to OpenTelemetry Destinations, like Honeycomb.

**Workaround**: Disabling **Extract spans** allows batched spans to be ingested without problems.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-23394` >}}

### 2024-03-13 – v4.4.0-4.5.1 – Diagnostic bundles cannot be created when git is enabled [CRIBL-23374]

**Problem**: Attempting to create a diagnostic bundle fails when git is enabled,
resulting in an error. 

**Workaround**: In the **Create & Export Diag Bundle** window, disable **Include git**.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23338` >}}

### 2024-03-12 – v.4.4.4-4.5.0 – S3 Collector **Path extractors** setting has no effect [CRIBL-23338]

**Problem**: In the S3 Collector, the **Collector Settings** > **Path extractors** setting has no effect because of a defect in the `index.js` file.

**Workaround**: Contact Cribl Support for assistance replacing the `index.js` file with a corrected one. 

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-23312` >}}

### 2024-03-11 – v4.5.0-4.5.1 – Diag doesn't collect logs when the `log` directory is a link [CRIBL-23312]

**Problem**: When the `log` directory is a symbolic link, [diag](diagnosing) doesn't collect log information (without throwing any errors).

**Workaround**: Avoid using symbolic links for the `log` directory, or use a direct mount.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23237` >}}

### 2024-02-29 – v4.5.0-4.5.1 – Users cannot log into unsecured LDAP servers [CRIBL-23237]

**Problem**: 

With **Access Management > Authentication > Type** set to `LDAP` and **Secure** toggled to `No`, you should be able to configure and run an unsecured (`ldap://`)  LDAP server. See [LDAP Authentication](authentication#ldap). However, this bug causes the **Secure** toggle to have no effect when set to `No`, meaning that your LDAP server must be configured with secure (`ldaps://`) connections or you will not be able to log in.

**Workaround**: There are two possible workarounds:
1. Run your LDAP server with [secure LDAP authentication](authentication#ldap).
2. If you wish to run your LDAP server **without** secure LDAP authentication, downgrade or upgrade to run a Cribl Stream version **other than** v4.5.0 or v.4.5.1.

**Fix**: In Cribl Stream 4.6.0.

{{< id `CRIBL-23197` >}}

### 2024-02-29 – v4.5.0-4.8.0 – Stream Projects: Post-processing Pipelines defined in Destination do not work [CRIBL-23197]

**Problem**: When using Cribl Stream Projects, a post-processing Pipeline defined only in the Destination,
not in the Project itself, will be ignored.

**Workaround**: Copy the post-processing Pipeline inside the Project:

1. In Cribl Stream's **Processing** > **Pipelines** table, click the Pipeline you're using for post-processing.
1. Select the Pipeline's gear (⚙️) button, and then the "Manage as JSON" button at the top right of the Pipeline settings.
1. Select **Export** and save the Pipeline in JSON format in a convenient location.
1. Go back to the **Project** screen and select **Pipeline** at the top.
1. Select **Add Pipeline**, and then **Import from File**.
1. Choose your exported configuration and confirm.

Post-processing should now function correctly in your Project.

**Fix**: In Cribl Stream 4.8.0.

{{< id `CRIBL-23189` >}}

### 2024-02-28 – All versions through 4.6.0 – Azure Data Explorer (ADX) Destination does not retry HTTP requests that come back with `520` response codes [CRIBL-23189]

**Problem**: Cribl Stream should, but does not, retry HTTP requests that return an `520 Internal server error` response code.

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.1.

### 2024-02-23 – 4.5.0 – Running a reverse proxy with a custom baseUrl breaks the user interface [CRIBL-23089]

**Problem**: If Cribl Stream is configured with a custom baseUrl, Leader 
deployments sitting behind a reverse proxy server experience user interface 
errors. The user interface fails to load data correctly from the backend, URLs 
fail to display the custom baseUrl address in the address bar, and using an 
address with the custom baseURL results in a 404 error.

**Workaround**: None.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-23032` >}}

### 2024-02-22 - v4.4.4-4.6.0 – Windows upgrades of Cribl Edge do not respect original installation directory [CRIBL-23032]

**Problem**: When upgrading a Windows installation of Cribl Edge that was originally
installed in a custom Windows directory ( `E:\` for example), the upgrader moves Cribl
Edge back to the `C:\` directory.

**Workaround**: None.

**Fix** In Cribl Stream 4.6.1.

{{< id `CRIBL-22801` >}}

### 2024-02-13 – v4.5.1 through Current – Edge Leaders running 4.5.1 can't use the Legacy upgrade method to upgrade 4.5.0 Nodes when jobs/tasks are disabled [CRIBL-22801]

**Problem**: Edge Leaders on version 4.5.1 that have Legacy upgrades enabled and
**Enable Jobs/Tasks** toggled to `No` cannot upgrade their 4.5.0 Edge Nodes. The job finishes
with the task timing out, and the Edge Node is not upgraded.

**Workaround**: On the 4.5.1 Leader, enable Legacy upgrades **and** toggle
**Enable Jobs/Tasks** to `Yes`. Commit and deploy as necessary. The Leader will
be able to create a job with the upgrade task and upgrade its Edge Nodes to
4.5.1.

**Fix**: No planned fix for this legacy mode issue. 

{{< id `CRIBL-22822` >}}

### 2024-02-12 – v4.5.0 – The API Reference automatic authorization doesn't work in Cribl.Cloud  [CRIBL-22822]

**Problem**: In Cribl.Cloud, the API Reference (under **Global Settings**) uses an invalid access token. Because of this, you will receive an HTTP 401 error when you attempt to run an API command.    

**Workaround**: To obtain a valid token, do the following: 
1. Open the Developer Tools network tab in your browser and copy the authorization bearer token from one of your requests. Alternatively, you can follow the docs steps [here](https://docs.cribl.io/stream/api-tutorials/#criblcloud-free-tier) to create a Cribl.Cloud API credential and obtain a token. Copy the token.
2. On the API Reference page, click **Authorize**.
3. In the resulting modal, click **Logout**.
4. Enter your copied token in the **Value** field and click **Authorize**.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-22762` >}}

### 2024-02-12 – v4.5.0 – When in the context of a Pack, the Pipeline link in the routing table is broken  [CRIBL-22762]

**Problem**: In the Pack context, under **Routes**, if you click the hyperlinked Pipeline name in the table, you will get an `Item not found` error. Clicking `Try again` from the error page does not resolve the issue. However, the link icon at the end of the Pipeline field in the routing table works as expected and brings you to the intended Pipeline page. 

**Workaround**: Use the link icon at the end of the Pipeline field in the Route to access the Pipeline.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-22731` >}}

### 2024-02-09 – v4.5.0 – OTLP Metrics function does not parse `__criblMetrics` when metric values are strings  [CRIBL-22731]

**Problem**: Documentation for the Publish Metrics Function indicates that it evaluates the **Metric Name Expression** to the **Event Field Name**, but it actually returns a `null`. As a result, the OTLP Metrics function is unable to properly parse `__criblMetrics` when the metrics values are strings.

**Workaround**: When using Publish Metrics and OTLP Metrics together, use the Numerify Function before the OTLP Metrics Function.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-22716` >}}

### 2024-02-09 – v4.5.0 – Fleet-level log searches no longer work  [CRIBL-22716]

**Problem**: The **Edge** > **Manage** > **Logs** UI no longer supports searching logs across entire Fleets. The option will be removed in an upcoming release.

**Workaround**: Search individual nodes or across Fleets in Cribl Search instead.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-22623` >}}

### 2024-02-06 – v4.4.4-4.7.1 – Inactive Destinations in GitOps environments block data flow [CRIBL-22623]

**Problem**: In GitOps environments, inactive Destinations are causing data flow to be blocked because the default backpressure behavior is to block. When a Destination is inactive, it triggers backpressure, preventing data from being processed and sent to other active Destinations.

**Workaround**: Change the backpressure behavior from `Block` to `Drop` or `Buffer` for the affected Destinations.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-22502` >}}

### 2024-01-30 – v4.1.0-4.6.0 – Product Documentation: Reference Architecture image download link is broken [CRIBL-22502]

**Problem**: The editable diagram that appears in the [All-in-One Reference Architecture](https://docs.cribl.io/stream/deploy-reference-all/) section of the documentation is broken for all non-current release versions. Clicking the link for the `SVG` or `draw.io` file download results in a 403 error.     

**Workaround**: Use the links in the most recent version of the documentation.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-22360` >}}

### 2024-01-24 – v4.4.4 through Current – After restarting the File Monitor Source, Event Breakers aren't processing CSV files correctly when the File Monitor Source is configured to collect CSV logs. [CRIBL-22360]

**Problem**: When the File Monitor Source is configured to collect CSV file logs
and use CSV Event Breakers, subsequent event breaking is not working when the
Source resets. The Event Breaker expects the first line of column headers, but
they resume reading from a later point in the file where those headers aren’t
present.  

**Workaround**: Use the default line breaker and a pre-processing Pipeline to drop
lines that start with `#`. Optionally, use an `Eval` function to split `_raw` values
on the column-separator character into separate fields and rename them as needed.

**Fix**: Version TBD.

{{< id `CRIBL-22299` >}}

### 2024-01-19 – v4.4.0-v4.4.4 – Unable to upgrade Leader from 4.3.x to 4.4.x when an external KMS is enabled [CRIBL-22299]

**Problem**: When attempting to upgrade the Leader from 4.3.x to 4.4.x with an external KMS enabled, you will receive several `Secret failed to decrypt` errors and the web UI will fail to load. This is due to an issue with how `client.secret` is handled when an external KMS is enabled.  

**Workaround**: None.

**Fix**: In Cribl Stream 4.5.

{{< id `CRIBL-22292` >}}

### 2024-01-19 – v3.1.0 through Current – Notification targets created in one Cribl product can be deleted from another without warning [CRIBL-22292]

**Problem**: When you create a Notification target, it will be made available to all products in the Cribl suite. When you attempt to delete a target from the same application you created it in (for example, Cribl Stream), the product will properly prevent the deletion when there is an active notification using the target. However, if you switch to a new application (for example, Cribl Search), you will be allowed to delete the target without error or warning. This will leave the active notification in the original product (Stream) with an undefined target. You should not be able to delete the target if it is used by any active notification in any Cribl suite product. 

**Workaround**: None.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-22184` >}}

### 2024-01-16 – v4.4.4 through Current – Already-existing Webhook Destinations show unexpected UI behavior upon upgrade to v4.4.4 [CRIBL-22184]

**Problem**: If you have a Webhook Destination in a Cribl Stream deployment, and then upgrade that deployment to version 4.4.4, the UI will show an error; the new **Load balancing** toggle will be turned on with associated settings displayed; and the UI will not allow you to save changes.

**Workaround**: First, toggle **Load balancing** off. This will return the UI to normal behavior. Then, if you want to use the Destination without load balancing, configure as usual and save. Or, if you prefer, update the **Webhook URLs** to include one or more valid endpoints you want to send to, and save the Destination with **Load balancing** enabled.

**Fix**: Version TBD.

{{< id `CRIBL-21965` >}}

### 2024-01-05 – v4.0.0-v4.4.3 – Windows Event Forwarder Source used up its allowed quota of connections [CRIBL-21965]

**Problem**: When the Windows Event Forwarder (WEF) Source could not process a request coming from a Windows client, it failed to correctly close the socket or send an error response to the client. 
Eventually, maximum allowable active connections could be reached, stopping the flow of data.

**Workaround**: For any affected WEF Source, set **Advanced Settings** > **Max active requests** to `0` to allow unlimited requests. Note that this will eventually cause other problems as the number of open-inactive sockets grows, meaning that the Worker nodes will eventually need to be restarted.

**Fix**: In Cribl Stream 4.4.4.

{{< id `CRIBL-21845` >}}

### 2023-12-21 – v.4.0.0-4.5.1 – Splunk TCP Source logs include an unclear message about an unsupported payload [CRIBL-21845]

**Problem**: Because the Splunk TCP Source does not support ingesting compressed data via the Splunk S2S protocol, it cannot parse such payloads, and a correct error message in this situation would say "Could not parse payload. Turn compression off in upstream sender and try again." Instead, the Source logs the unclear message "Failed to parse S2S payload". 

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-21690` >}}

### 2023-12-13 v.4.0.0–4.4.3 – Sources incorrectly report failure [CRIBL-21690]

**Problem**: When using OpenTelemetry HTTP, the Source always listens on the default address (either `0.0.0.0` or `::`, or both, depending on the network configuration of the host), ignoring the option you configured in **General Settings** > **Address**.

**Workaround**: Unidentified.

**Fix**: In Cribl Stream 4.4.4.

{{< id `CRIBL-21443` >}}

### 2023-11-30 – v4.4.0-4.4.1 – Edge-only Sources are unavailable in standalone Edge instances [CRIBL-21443]

**Problem**: Edge-only Sources are disabled in standalone (not distributed) Edge instances.

**Workaround**: None.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-21335` >}}

### 2023-11-21 – v.4.4.0–4.4.3 – Azure Data Explorer Destination exposes irrelevant PQ settings in Cribl.Cloud [CRIBL-21335]

**Problem**: For Worker Nodes in Cribl.Cloud with Azure Data Explorer (ADX) Destinations: such Destinations' **Persistent Queue Settings** should expose only a **Clear Persistent Queue** button. They are incorrectly also exposing the PQ settings required for on-prem deployments.   

**Workaround**: In Cribl.Cloud ADX Destinations, ignore all Persistent Queue settings except the **Clear Persistent Queue** button.

**Fix**: In Cribl Stream 4.4.4.


{{< id `CRIBL-21289` >}}

### 2023-11-16 – v4.4.0–4.4.3 – "Unsupported system page size" error when upgrading the Leader[CRIBL-21289]

**Problem**: Fedora users can experience an "unsupported system page size" error when upgrading the Leader. This is due to an issue with `jemalloc`. 

**Workaround**: You can manually remove `jemalloc.so`. Docker users can use the following: 

`docker ... -e CRIBL_BEFORE_START_CMD_0="rm /opt/cribl/bin/libjemalloc.so" ...`

**Fix**: In Cribl Stream 4.4.4.


{{< id `CRIBL-21258` >}}
{{< id `CRIBL-21557` >}}

### 2023-11-15 – v4.4.0 through Current – With Leader HA/Failover, creating diag bundles via CLI produces incorrect results [CRIBL-21258, CRIBL-21557]

**Problem**: For deployments configured for [Leader high availability/failover](/stream/deploy-add-second-leader/), creating diagnostic bundles via the UI works normally but creating them via the CLI does not. In Cribl Stream v.4.4.0 and newer, the operation fails with a `not a git repository` error. In v.4.4.0 and older, the diag bundle created contains obsolete data. In both cases the underlying cause is that the process that creates the diag bundle cannot access the `CRIBL_CONF_DIR` environment variable. 

**Workaround**: Use the UI to create the diag bundle. Or, if you use the CLI, preface the `diag` command with the needed environment variable, like this: `CRIBL_CONF_DIR=/<path_to_failover_volume> cribl diag create`. If you have an `instance.yml` config file, you can find the path to the failover volume in the `distributed` [section](/stream/deploy-add-second-leader#yaml).

**Fix**: Error message improvements were made as part of CRIBL-21258. Fix for CRIBL-21557 is TBD.

{{< id `SAAS-4399` >}}

### 2023-11-09 – All Versions through 4.4.4 – High-volume UDP data dropped in Cribl.Cloud [SAAS-4399]

**Problem**: Ingesting high rates of UDP events per second can cause Cribl.Cloud to drop some of the data. This limitation of the UDP protocol affects UDP-supporting Sources: [Raw UDP](sources-raw-udp), [Metrics](sources-metrics), [SNMP Trap](sources-snmp-traps), and [Syslog](sources-syslog) in UDP mode.

**Workaround**: To minimize the risk of data loss, deploy a hybrid Stream Worker Group, with Worker Nodes as close to the UDP senders as possible.

Cribl also recommends tuning the OS UDP buffer size.

**Fix**: In Cribl Stream 4.5.0.

{{< id `CRIBL-21155` >}}

### 2023-11-09 – v.4.4.0–4.4.1 – Go version update required [CRIBL-21155]

**Problem**: The Go version requires an update.

**Workaround**: None.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-21123` >}}

### 2023-11-08 – v.4.4.0–4.4.1 – Azure Data Explorer Destination fails to validate database settings in **Streaming** mode [CRIBL-21123]

**Problem**: When Azure Data Explorer Destination is in **Streaming** mode and **Validate database settings** is turned on, database validation will fail unless **Ingestion service URI** has been set in **Batching** mode first.

**Workaround**: Switch to **Batching** mode, set the **Ingestion service URI** field, and save the Destination. Then switch back to **Streaming** mode and save the Destination again.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-21099` >}}
{{< id `CRIBL-22072` >}}

### 2023-11-07 – v.4.4.0–4.4.3 – Stream Groups' Members indicators always show zero count [CRIBL-21099, CRIBL-22072]

**Problem**: On a Stream Worker Group's **Manage** > **Overview** page, the **Members** counters show `0` in all Roles/​Permissions, even when Members have been added.

**Workaround**: Navigate to **Group Settings** > **Members** for an accurate view of all Members and their access levels.

**Fix**: Fix for CRIBL-22072 in Cribl Stream 4.4.4. (CRIBL-21099 closed as a duplicate.)

{{< id `CRIBL-21093` >}}

### 2023-11-07 – v.4.4.0–4.4.1 – Worker Node generates an incomplete diagnostic bundle when **Include git** is enabled [CRIBL-21093]

**Problem**: When you teleport to a Worker Node and create a diagnostic bundle, if you leave **Include git** toggled on in the **Create & Export Diag Bundle** modal, the bundle that the system creates will be malformed and missing information, and the UI will produce a fatal error notification.

**Workaround**: When you create diagnostic bundles, turn **Include git** off.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-21047` >}}

### 2023-11-06 – v.1.2 through Current – Kafka and Confluent mTLS requirement limits Schema Registry authentication [CRIBL-21047]

**Problem**: The Kafka and Confluent Cloud Source and Destination only support mutual TLS (mTLS) authentication for the Kafka Schema Registry. This prevents you from configuring separate authentication mechanisms for the Schema Registry and Kafka brokers, which is necessary when the Schema Registry and brokers are managed by different departments with distinct authentication requirements.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-21060` >}}

### 2023-11-06 – v.4.4.0–4.4.1 – Azure Data Explorer Destination does not correctly turn off **Validate database settings** [CRIBL-21060]

**Problem**: Once the Azure Data Explorer Destination has been saved with **Validate database settings** on, turning the setting off has no effect, and the Destination performs validation anyway.

**Workaround**: To ensure that **Validate database settings** is turned off, perform either of these two workarounds:

Option 1: Clone the Destination or create a new one, and turn **Validate database settings** off before saving.

Option 2: Turn off **Validate database settings**, then restart the Cribl Stream Worker Node.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-20607` >}}

### 2023-10-12 – v.4.3.0–4.4.1 – Rapid logging causes Worker Node memory spikes and prevents logging [CRIBL-20607]

**Problem**: A large number of logs written in a short time causes memory spikes in Worker Nodes and stops log output.

**Workaround**: Unidentified.

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-20504` >}}

### 2023-10-05 – v.4.3.0 – Microsoft Windows Installer (MSI) does not create an Edge desktop shortcut [CRIBL-20504]

**Problem**: When you install Cribl Edge on Windows, the MSI doesn't create the expected
desktop shortcut for Edge, even if you select **Create a shortcut for Cribl Edge on the desktop**.

**Workaround**: None.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-20444` >}}

### 2023-10-03 – v.4.3.0–4.4.1 – Upgrading to v.4.3.0 or later causes Amazon Kinesis Data Streams and WEF Sources to lose state [CRIBL-20444]

**Problem**:

After an upgrade to v.4.3.0, Worker Nodes for a Cribl Stream Amazon Kinesis Data Streams Source or Windows Event Forwarder (WEF) Source can lose state upon init. This can be especially problematic for customers with large streams and high data ingestion.

An Amazon Kinesis Data Streams Source that loses state will fail to read a data stream from the point where it most recently left off. Instead, the Source will start reading from the location configured by **Optional Settings** > **Shard iterator start**.

* If **Shard iterator start** is set to `Earliest Record` (the default), the Source can start too far behind in the stream to ever catch up. 
* If **Shard iterator start** is set to `Latest Record`, the Source can start later than where it left off, causing some records to be skipped.

A Microsoft Windows Event Forwarder (WEF) Source (with bookmarks configured for the subscription) that loses state will fail to request that upstream clients send events starting at a bookmarked location. Instead, Cribl Stream will send no bookmark, prompting upstream clients to send **all** events that otherwise match the subscription. This can cause Cribl Stream to ingest duplicate events from WEF clients.

**Workaround**: If you have not yet upgraded to v.4.3.0 or later, consider delaying the upgrade until this issue is fixed. 

**Fix**: In Cribl Stream 4.4.2.

{{< id `CRIBL-20088` >}}

### 2023-09-15 – v.4.3.0 – Scheduled (cron-based) jobs fail to load for some Sources and Collectors [CRIBL-20088]

**Problem**: Collection tasks for Office 365 Sources, the Splunk Search Source, the Prometheus Scraper Source, and scheduled Collectors fail to load when the Leader restarts, fails over, or upgrades. Collectors running jobs ad hoc are not affected.

**Workaround**: If you're using one of the listed Sources or use scheduled Collector jobs on-prem, do not upgrade to 4.3.0. If you are already on 4.3.0, downgrade to 4.2.2, or wait and upgrade to 4.3.1 when it's available.

You can also restart the config helper for on-prem installations using the command:  
`pkill -f CONFIG_HELPER`



You must run this command each time the Leader fails over, or is restarted (ideally, not too often).

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-20083` >}}

### 2023-09-14 – v.4.3.0 – Chain Function's link breaks when the chained Pipeline/Pack is resaved [CRIBL-20083]

**Problem**: After linking a Chain Function to a Pipeline or Pack, resaving the target Pipeline/Pack triggers `Invalid link` errors.

**Workaround**: 1. [Copy the Pipeline](pipelines#actions) containing the Chain Function to your clipboard, delete the original Pipeline, and then paste the copy to resolve the error. 2. Alternatively, on-prem admins can restart the config helper that manages the Worker Group. (In the UI, navigate to **Settings** > **Global** > **System** > **Services** > **Processes**.) 

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-20038` >}}

### 2023-09-14 – v.4.3.0 – In Cribl.Cloud, Admin Members cannot modify Global Settings [CRIBL-20038]

**Problem**: After upgrading to 4.3.0, Members granted the Admin Permission cannot modify Global Settings.

**Workaround**: Members with the Owner Permission can still modify Global Settings.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-20086` >}}

### 2023-09-14 – v.4.3.0 – Worker Nodes cannot be upgraded in Leader-managed upgrades [CRIBL-20086]

**Problem**: In Stream and Edge, if you upgrade the Leader to 4.3.0, then use **Commit & Deploy** to
push configs to Worker Groups before upgrading Worker Nodes to 4.3.0, you won't be able to
upgrade the Worker Nodes from the Leader thereafter.

> This issue affects only Leader-managed upgrades of Worker Nodes.
>
{.box .warning}

**Workaround**: If a Leader is on 4.3.0, you should upgrade all
Worker Nodes to 4.3.0 before making any config changes or
committing and deploying to the Nodes.

If you have already committed and deployed to Worker Nodes that are on a version
prior to 4.3.0 (and the Leader is on 4.3.0), here are three workaround options:

- Revert and redeploy the last commit, then upgrade the Worker Nodes and deploy the most up-to-date changes, **OR**
- Upgrade the Worker Nodes via command line or script, **OR**
- Downgrade the Leader to 4.2.2 and then upgrade to 4.3.1 when it is available.

If you run into issues, contact support@cribl.io for resolution assistance.

**Fix**: In Cribl Stream 4.3.1. 

{{< id `CRIBL-20067` >}}

### 2023-09-13 – v.4.2.1 – Edge upgrades occasionally fail when Fleets contain a large number of Nodes [CRIBL-20067]

**Problem**: Leader-managed upgrades occasionally fail when a Fleet contains a large number of Nodes.

**Workaround**: Restart the upgrade operation to upgrade additional Nodes. 

**Fix**: Determined to be an environment issue, no fix issued.

{{< id `CRIBL-20040` >}}

### 2023-09-13 – v.4.3.0 – Any Code Function before another Function breaks the Data Preview OUT tab display [CRIBL-20040]

**Problem**: Any Code Function inserted before the end of a Pipeline prevents the Data Preview OUT tab from rendering properly. This occurs even if the Code Function contains only a single comment, or is blank. Adding a Code Function at the end of a Pipeline does not cause an issue. This is a Data Preview UI issue only – if you capture events to a Destination, Cribl Stream processes them as expected.

**Workaround**: None.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-20017` >}}

### 2023-09-12 – v.4.0–4.3.0 – Numeric sub-second values sent over S2S v4 require care to avoid incorrect stringification [CRIBL-20017]

**Problem**: Events received from S2S v4 can have sub-second granularity, meaning that the Splunk TCP Source reads a sub-second value from ingested events and uses it to set a `_subsecond` field added to outgoing events.
If, for example, the Source sets the `_subsecond` field with a stringified numeric value instead of the actual number, problems can occur that affect downstream receivers such as Splunk indexers.

**Workaround**: Either of the following workarounds can be effective:

1. Upgrade to Cribl Stream v.4.3.1 or newer.
2. In a Pipeline, drop or numerify any `_subsecond` field whose value is a string.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-19818` >}}

### 2023-08-31 – v.4.2.2 – When in XML format, the Windows Event Logs Source doesn't accept log names that contain spaces [CRIBL-19818]

**Problem**: When the Windows Event Logs Source is set to XML format, any configured log names that contain spaces (e.g., `Windows Powershell`) cause an error and the log can't be read.

**Workaround**: None.

**Fix**: In Cribl Edge 4.3.

{{< id `CRIBL-19756` >}}

### 2023-08-28 – v.4.2.2–4.3.0 – PQ doesn't drain for the Event Hubs Destination when Acknowledgements is set to All or Leader [CRIBL-19756]

**Problem**: When the `Acknowledgements` setting is set to `All` or `Leader`, Kafka-based Destinations (especially Azure Event Hubs) can fail to drain the persistent queue (PQ) and the logs are filled with `Attempting to send faster than the downstream can receive – consider checking the network connection and broker health` errors.  

**Workaround**: You can set a limit using the `Persistent Queue Drain rate limit (EPS)` setting (for example, set it to `500`) to get the PQ to drain when the datagen is turned off. However, if the rate of data input continues to exceed the rate at which the Destination can send events downstream (which is slowed by the `Acknowledgements` setting), the PQ will continue to build. Alternatively, you can set the `Acknowledgements` setting to `None`, which significantly increases the rate at which data can be sent to Event Hubs.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-19709` >}}

### 2023-08-24 – v.4.2.0–4.2.2 – Deleting a Source via QuickConnect doesn't refresh the view [CRIBL-19709]

**Problem**: When using QuickConnect, if you delete a Source by opening its drawer and clicking `Delete Source`, the Source is deleted. However, it will continue to show on the page until you manually refresh the page.  

**Workaround**: Manually refresh the page after deleting a Source.

**Fix**: In Cribl Stream 4.3.0.

{{< id `CRIBL-19676` >}}

### 2023-08-23 - v.4.0.0–4.4.4 - Packs installed in Fleets are not visible to Subfleets. [CRIBL-19676]

**Problem**: When a Fleet has Subfleets, and you install a Pack in the parent Fleet, none of the Subfleet's Pipeline drop-downs make the Pack available. This is true for Routes, pre-processing Pipelines in Sources, and post-processing Pipelines in Destinations.

**Workaround**: Install the Pack both in the parent Fleet, and in any Subfleets that need to use the Pack.

**Fix**: In Cribl Stream 4.5.

{{< id `CRIBL-19700` >}}

### 2023-08-23 – v.4.0–4.2.x – Inherited Packs in Fleets lose their configured Routes [CRIBL-19700]

**Problem**: Routes in a Pack at the Fleet level revert to default Routes when inherited by a SubFleet.   

**Workaround**: None.

**Fix**: Couldn't reproduce the error.

{{< id `CRIBL-19571` >}}

### 2023-08-16 – v.4.2.2 – When adding or editing a Route, typeahead misbehaves in the **Filter** field [CRIBL-19571]

**Problem**: Entering text in the **Filter** field of a Route is difficult in Cribl Stream 4.2.2 because the typeahead function behaves erratically, sometimes moving the cursor around or inserting text fragments.   

**Workaround**: 1. Assemble the filter expression in a different Cribl expression field, or in any text editor, then copy and paste it here. 2. Alternatively, click the Manage as JSON button at the **Data Routes** table's upper right. Then, in the JSON editor, build the expression as the `"filter":` key's value.

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-19322` >}}

### 2023-08-02 - v.4.2.1 through Current - Healthy Destinations spuriously report `Blocked Status` [CRIBL-19322]

**Problem**: Healthy TCP-based, Kafka-based, Splunk, and Metrics Destinations can spuriously report `Blocked Status` in the **Charts** tab, sometimes also logging the blocked status inaccurately.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-19154` >}}

### 2023-07-26 – v.4.2.0–4.2.1 – File Monitor Source causes memory leaks and Worker Node crashes [CRIBL-19154]

**Problem**: The File Monitor Source, when running in Manual Discovery mode, leaks memory and a file descriptor each time it rediscovers a file that it has already collected. The leaked resources increase with every polling interval, for every rediscovered file, eventually causing Worker Nodes to crash. This does not affect Auto Discovery mode, and does not affect deployments without an active File Monitor Source.

**Workaround**: File Monitor users should bypass (or roll back from) versions 4.2.0–4.2.1.

**Fix**: In Cribl Stream 4.2.2.



{{< id `CRIBL-19044` >}}

### 2023-07-20 – v.4.2.0–4.2.1 – AppScope Source might reference a nonexistent config [CRIBL-19044]

**Problem**: The ‘sample_config’ stock configuration was removed in v.4.2.0. This missing config causes the Appscope Source to disappear from the UI, and changes to other Sources also fail with validation errors.

**Workaround**: For on-prem/customer-managed deployments, clone one of the existing AppScope configurations and rename it to `sample_config`. For Cribl.Cloud customers, there is no workaround yet.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-19107` >}}

### 2023-07-19 – v.4.2.2 – Group Information for Cribl.Cloud Worker Groups not showing on summary page for SSO users [CRIBL-19107]

**Problem**: On Cribl.Cloud Organizations, when you open a Stream Group's **Manage** > **Overview** page, the **Group Information** section is not populated for users imported using SSO from external identity providers. (This section populates correctly for users/Members configured natively within your Cribl Organization.)

**Workaround**: None. 

**Fix**: Couldn't reproduce the error.

{{< id `CRIBL-19015` >}}

### 2023-07-19 – v.4.2.0–4.2.1 – Users with the Admin Permission cannot view Notifications [CRIBL-19015]

**Problem**: Users with the **Admin** Permission receive a `You do not have sufficient permissions to access this resource` message when attempting to view Notifications. Users with this Permission should be able to access Notifications.   

**Workaround**: None. Organization **Owners** can access these notifications.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-19014` >}}

### 2023-07-19 – v.4.2.0–4.2.1 – Users with the Admin Permission cannot bootstrap a Worker due to a missing auth token [CRIBL-19014]

**Problem**: Users with the **Admin** Permission can’t add a Worker using **Add / Update Worker Node**, because the modal doesn’t have the **Leader hostname/IP** or the **Auth token** fields populated. You can enter the hostname/IP into its field, but not the auth token. Users with the **Admin** Permission should see the auth token.

**Workaround**: Organization **Owners** can perform this action and share the auth token.

**Fix**: In Cribl Stream 4.2.2.

{{< id `SAAS-4823` >}}

### 2023-07-19 – v.4.2.0 – Credential encryption errors preventing user access to Cribl Stream Sources and Destinations [SAAS-4823]

**Problem**: A critical regression caused Worker Groups to become unresponsive, preventing access to Sources and Destinations. This issue was caused by incorrectly encrypted credentials.

**Workaround**: Upgrade to Cribl Stream 4.2.1.

**Fix**: In Cribl Stream 4.2.1.

{{< id `CRIBL-18973` >}}

### 2023-07-18 – v.4.2.0 through Current – Migrated Local Users appear in the Members UI with "No Access" [CRIBL-18973]

**Problem**: Local Users are incorrectly shown in the **Settings** > **Members** UI with `No Access`. However, their Roles still function as originally configured, and still display correctly at **Settings** > **Global Settings** > **Access Management** > **Local Users**.

**Workaround**: Rely on the **Local Users** UI. 

**Fix**: Version TBD.

{{< id `CRIBL-18970` >}}
{{< id `CRIBL-20113` >}}

### 2023-07-18 – v.4.2.0–4.4.4 – Product-level Editor Permissions do not allow a commit/deploy action on the Worker Groups/Fleets page [CRIBL-18970, CRIBL-20113]

**Problem**: Users with product-level Edge **Editor** or Stream **Editor** Permissions are unable to commit and deploy changes made to Worker Groups or Fleets from the Group or Fleet page. However, if the user navigates to the Group or Fleet where the change was saved, the **Commit and Deploy** button is available.   

**Workaround**: Commit and deploy from the Worker or Group where you saved the change.  

**Fix**: In Cribl Stream 4.5. CRIBL-18970 is closed and expanded to CRIBL-20113.

{{< id `CRIBL-18969` >}}

### 2023-07-18 – v.4.2.0–4.2.1 – Users with the Edge Editor Permission cannot delete secrets under Fleet Settings [CRIBL-18969]

**Problem**: Users with the Edge **Editor** Permission can create secrets under **Fleet** > **Fleet Settings** > **Secrets** but they are unable to delete the secret once it is created. Edge **Editors** should have this capability. 

**Workaround**: Assign the Edge **Admin** Permission to users that need to be able to delete secrets.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18963` >}}

### 2023-07-17 – v.4.2.0–4.2.1 – Edge Node Settings not visible when teleporting [CRIBL-18963]

**Problem**: When you teleport into an Edge Node, you can't view **Node Settings** in the UI.     

**Workaround**: None.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18951` >}}

### 2023-07-17 – v.4.2.0–4.2.1 – Users with the Edge Editor Permission cannot delete Sources, Destinations, or Pipelines [CRIBL-18951]

**Problem**: If you assign a user the Edge **Editor** permission at the product-level, the user will not be able to delete Sources, Destinations, or Pipelines. Edge **Editors** should have this capability.         

**Workaround**: Assign the Edge **Admin** Permission to users that need to be able to delete these items. 

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18950` >}}

### 2023-07-17 – v.4.2.0–4.2.1 – Change from Edge User to Edge Editor can fail to take effect [CRIBL-18950]

**Problem**: If you assign the **Editor** Permission to an Edge **User** at the Fleet-level and then later assign the Edge **Editor** Permission to the same User at the product-level (Edge **User** now becomes an Edge **Editor**), the **Editor** Permission at the product-level can fail to take effect. The user will have **Editor** access to the one Fleet at the Fleet-level but they will still show as an Edge User and will not have **Editor** Permissions at the product-level.        

**Workaround**: None.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18948` >}}

### 2023-07-17 – v.4.2.0–4.2.1 – Product logins for users with product-level Read Only and Editor Permissions can result in misleading log entries [CRIBL-18948]

**Problem**: When users that are assigned product-level **Read Only** or **Editor** Permissions log in to a product, the browser incorrectly makes requests for resources that users at this level are not allowed to access. While these requests are correctly denied and there is no risk of compromising security, Cribl admins may see misleading log entries indicating that these users tried to access something they are not allowed to access.     

**Workaround**: None. 

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18925` >}}

### 2023-07-16 – v.4.2.0–4.4.3 – After upgrading, Monitoring dashboards are not visible for users with `GroupRead` and `GroupCollect` Policies [CRIBL-18925]

**Problem**: Users that were assigned `GroupRead` and `GroupCollect` Policies in Cribl Stream 4.1.x will no longer be able to access the **Monitoring** tab and dashboard after upgrading to Cribl Stream 4.2.    

**Workaround**: For `GroupRead`, migrate the user to **Read Only** Permission at the Group-level in Members and Permissions. Assign the Member as Stream **User** then set their Permissions for the group as **Read Only**. For `GroupCollect`, there is no specific workaround but users can be assigned the `GroupEdit` Policy if they need to view Monitoring dashboards. 

**Fix**: In Cribl Stream 4.4.4.

{{< id `CRIBL-18923` >}}

### 2023-07-15 – v.4.2.0–4.2.1 – Edge users with the Fleet-level Editor Permissions cannot use manual file discovery [CRIBL-18923]

**Problem**: When using manual file discovery in Cribl Edge, users that have been assigned the **Editor** Permission at the Group/Fleet-level will not see any results. 

**Workaround**: You can assign the **Admin** Permission for users that require the manual file discovery feature.  

**Fix**: In Cribl Stream 4.2.2.



{{< id `CRIBL-18848` >}}

### 2023-07-13 – v.4.2.0–4.2.1 – Stream Users are able to access the Monitoring page [CRIBL-18848]

**Problem**: Stream **Users** with no Group-level Permissions are able to access the **Monitoring** page. The **Monitoring** menu item is hidden, but the page can be access manually using a URL.

**Workaround**: None. 

**Fix**: Not observed as of Cribl Stream 4.2.2.

{{< id `CRIBL-18844` >}}

### 2023-07-13 – All Versions through Current – When configuring an S3 Collector, JavaScript expressions break the "Auto-populate from" option for the Path field [CRIBL-18844]

**Problem**: When automatically populating an S3 Collector from an S3 Destination, the Collector **Path** field won’t resolve JavaScript expressions, even if the expression was valid on the Destination that the field was populated from. The S3 Collector path is treated like a literal string and the software fails to warn you that the path is invalid.

**Workaround**: None. 

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-18818` >}}

### 2023-07-12 – All Versions through Current – Recent Actions endpoint returns results for certain unauthorized users [CRIBL-18818]

**Problem**: The Recent Actions endpoint `GET /api/v1/ui/recentActions` returns more recent actions than intended for non-admin users. 

**Workaround**: None.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18784` >}}

### 2023-07-12 – v.4.2.0–4.2.1 – No Access to Subfleets for the Edge User Permission [CRIBL-18784]

**Problem**: Regardless of what Permissions are granted at the Fleet-level, Edge members assigned to the **User** Permission will not have access to Subfleets.

**Workaround**: At the (Edge) product level, you must grant your members **Read Only**, **Editor**, or **Admin** Permissions so that they can access Subfleets. 

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18668` >}}

### 2023-07-07 – v.4.1.3-4.2.1 – When configuring an HTTP Source, enabling Source PQ changes the inputId in events [CRIBL-18668]

**Problem**: When Source-side Persistent Queueing is enabled on an HTTP Source, the trailing colon is dropped from `__inputId`. This will break Routes and filters that are based on the Input ID if it is copied from the configuration page. Live captures will also stop working until you modify them.

**Workaround**: None. 

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-18639` >}}

### 2023-07-06 – v.4.2.0–4.2.1 – Default Pack is not visible to Project Editor when a Project is shared with them [CRIBL-18639]

**Problem**: When a Cribl admin sets up a Project, a default Pack is included. If the Admin then shares that Project with a Project editor, the editor will not see the default Pack. 

**Workaround**: In the Project view, the Project editor can add any Pack that has been made available by the admin, including the default Pack. 

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-18556` >}}

### 2023-07-03 – v.4.2.0–4.3.0 – Users with Group-level Read Only Permissions can interact with logging level menus  [CRIBL-18556]

**Problem**: Users with the **Read Only** Permission at the Group-level can access the **Add Channel** and **Delete** buttons under **Group** > **Manage** > **Group Settings** > **Logging** > **Levels**. However, they are unable to save changes. When performing these actions, **Read Only** users will see a **Forbidden** message displayed in the UI and no changes will be saved. The setting should not be accessible to users with this Permission.

**Workaround**: None.  

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-18473` >}}

### 2023-06-29 – All Versions through v.4.1.3 – (Linux) System Metrics Source does not emit "Per interface" metrics when set to "All" [CRIBL-18473]

**Problem**: In the (Linux) **System Metrics** Source > **Host Metrics** tab, selecting **All** does not emit **Per Interface** metrics.

**Workaround**: In the **Host Metrics** tab, select **Custom** > **Network** > **Custom** then toggle **Per interface metrics** to `Yes`. 

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-18447` >}}

### 2023-06-28 – v.4.2.0–4.2.1 – Project editor can't delete Pipelines via Options menu [CRIBL-18447]

**Problem**: In a Project's **Pipelines** modal, when a Project editor opens a Pipeline's ••• (Options) menu, the **Delete** option is unavailable. Cribl Stream displays a spurious error about insufficient permissions. 

**Workaround**: Use the list's check boxes to select Pipelines, then click **Delete Selected Pipelines**. 

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-18400` >}}

### 2023-06-26 – All Versions through v.4.1.3 – High CPU usage in File Monitor Source's Manual mode [CRIBL-18400]

**Problem**: In the **File Monitor** Source and the **Explore** > **Files** tab UI, the **Manual** mode does not honor the **Max depth** setting. The API process consumes high CPU resources because the discovery logic (used in the **File Monitor** Source and **Files** tab) recurses in the directory tree.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-18374` >}}

### 2023-06-26 – All Versions through Current – Fleet Upgrade Errors [CRIBL-18374]

**Problem**: A Fleet's upgrade from a Leader can result in errors when the Edge Nodes use different `host`/`port`/`tls` settings than Worker Nodes.

**Workaround**: The Edge Nodes and Workers must connect to the leader using the `host`/`port`/`tls` connection details.  If this is not possible, upgrade Edge Nodes separately using the [boostrap script](/edge/managing-edge-nodes#bootstrap).

**Fix**: Version TBD.

{{< id `CRIBL-18281` >}}

### 2023-06-21 – v.3.5.2 through Current – Datadog Agent Source fails to receive metrics [CRIBL-18281]

**Problem**: When a Datadog Agent using Datadog API v2 sends metrics, Datadog Agent Source fails to receive them, because the Source uses Datadog API v1. Datadog Agent logs will show `API Key invalid, dropping transaction` errors. Debug logging will likely produce `Dropping request because of unallowed path message` errors with `statusCode: 403`, which Datadog Agent interprets as "invalid API key."

**Workaround**: In the `datadog.yaml` file, ensure that `use_v2_api.series` is set to `false`. Cribl Stream's Datadog Agent Source will then receive metrics normally, since both the Source and the Datadog Agent will be using Datadog API v1, which Datadog still supports. 

**Fix**: Version TBD.

{{< id `CRIBL-18285` >}}

### 2023-06-21 – v.3.5.2–4.1.3 – Using the Rename Function gives unexpected results due to skipped internal fields [CRIBL-18285]

**Problem**: When internal fields are present in the `Rename fields` list, the Rename Function may incorrectly assign field keys or values.

**Workaround**: This function is not intended to operate on internal fields. Avoid this operation.   

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-18218` >}}

### 2023-06-20  – All versions through 4.2.2 – HTTP Destinations sometimes send oversized payloads [CRIBL-18218]

**Problem**: HTTP-based Destinations do not strictly enforce the **Max Body Size** limit in all cases. This can cause payloads to be sent that exceed the maximum size some downstream receivers can accept.

**Workaround**: Experiment with lower **Max Body Size** values until downstream receivers are reliably accepting all events.

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-18184` >}}

### 2023-06-15 – v.4.1.3 – Amazon CloudWatch Destinations log error while flushing events older than 24 hours [CRIBL-18184]

**Problem**: Amazon CloudWatch Destination doesn't filter batches of events to ensure all events in the batch are within 24 hours of each other as required by the API, resulting in the batch being rejected by Cloudwatch.

**Workaround**: Add a Post-Processing Pipeline with a Drop Function using a Filter Expression of `_time<(Date.now() / 1000) - 86400`.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-18164` >}}

### 2023-06-14 – v.4.1.2–4.1.3 – The Office 365 Activity Source misses events [CRIBL-18164]

**Problem**: The Office 365 Activity Source misses events due to the current collection system. For services such as Exchange, SharePoint, OneDrive, and Teams, Microsoft indicates that audit record availability is typically 60 to 90 minutes after an event occurs. Our current collector methodology does not account for this availability window. Instead, the Source completes runs as scheduled and collects only events it finds during the configured time range, which can lead to missed events.

**Workaround**: None.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-18038` >}}

### 2023-06-08 – v.4.1.2 – AES-256-GCM security vulnerability [CRIBL-18038]

**Problem**: The AES-256-GCM encryption option introduced in v.4.1.2 included a security vulnerability. 

**Workaround**: Do not use this option in v.4.1.2.

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17939` >}}

### 2023-06-01 – v.4.0.0–4.1.3 – GroupEdit is not able to commit changes [CRIBL-17939]

**Problem**: Users having the `GroupEdit` policy are able to make changes (e.g. create new Sources) but are unable to **Commit** their changes. These users should be able to **Commit**, but not **Deploy**, changes. 

**Workaround**: Apply the `GroupFull` policy for users that need to be able to **Commit** changes. These users will also have the ability to deploy a Worker Group or Fleet.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-17907` >}}

### 2023-06-01 – v.4.1.0–4.1.2 – Kubernetes Logs Event Breaker Incorrectly Breaks Events [CRIBL-17907]

**Problem**: The **Kubernetes Logs** Event Breaker incorrectly breaks events with out-of-order timestamps. 

**Workaround**: In the Event Breaker, move the logic that adjusts `_raw` into a **Pre-Processing** Pipeline.

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17779` >}}

### 2023-05-27 – v.4.1.1–4.1.2 – Perf bug: RPC consuming excessive CPU compiling expressions [CRIBL-17779]

**Problem**: In 4.1.1, to improve product security, we changed how we manage our RPC traffic from Leaders and Workers. This change can have a negative impact in larger environments with many Worker Processes. Following a 4.1.1 or 4.1.2 upgrade, the Leader can become non-responsive to UI requests due to a steady 100% CPU utilization. When this happens, you cannot manage or monitor Cribl Stream environments.

**Workaround**: Roll back to a previous stable version, such as 4.1.0.

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17792` >}}

### 2023-05-26 v.4.1.2–4.1.3 – Worker Nodes don't update from Cribl Stream Leader [CRIBL-17792]

**Problem**: In Cribl Stream 4.1.2–4.1.3, if you define the `baseURL` key on a Leader, Worker Nodes don't get the latest config version from the Leader. The log may also contain `checksum mismatch` warnings.

**Workaround**: Clear **URL base path** in **Settings** > **General Settings** > **Advanced** or specify an empty `baseURL` key  in `cribl.yml`. If you need to define a base URL, do not upgrade to the affected versions.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-17715` >}}

### 2023-05-19 – v.4.1.2 – CrowdStrike Destination's "LogScale endpoint" field hidden from UI [CRIBL-17715]

**Problem**: The CrowdStrike Falcon LogScale Destination's **LogScale endpoint** field is hidden from the 4.1.2 configuration modal, but can be restored.

**Workaround**: Follow these steps, in sequence, for each LogScale Destination.  
1. Populate the LogScale config modal, and save once.  
2. Reopen the modal, then select **Manage as JSON**.  
3. Change the `"loadbalanced":` key's value from `true` to `false`.  
4. Copy this default nested element:
`"url": "https://cloud.us.humio.com/api/v1/ingest/hec",`  
5. Paste it above the whole `"urls"` element (typically at line 21).  
6. Click **OK** to restore the visual UI, with the **LogScale endpoint** field now visible.  
7. Enter your actual endpoint URL, and save the config.

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17632` >}}

### 2023-05-16 – v.4.1.1–4.1.2 – Expanding Status of Output Router Destination triggers error [CRIBL-17632]

**Problem**: When you attempt to expand a node on the **Status** tab for an Output Router Destination, the page crashes and the error `Cannot convert undefined or null to object` appears. 

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17614` >}}

### 2023-05-12 – v.4.0–4.1.3 – Windows Event Forwarder Source causes duplicate events when experiencing backpressure [CRIBL-17614]

**Problem**: In certain cases where the Windows Event Forwarder Source experiences backpressure from a downstream Pipeline or Destination, it will return an incorrect error message to the client that sent the events. This causes that client to re-send the same events and results in duplicate events ingested in Stream. 

**Workaround**: In cases where the cause of the backpressure can be resolved, this issue is mitigated. However if the backpressure cannot be completely removed, there is no additional workaround.

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-17602` >}}

### 2023-05-12 – v.4.0–4.1.2 – The Kubernetes Logs Source drops events [CRIBL-17602]

**Problem**: Events from a running container will stall when the underlying container runtime rotates the log file. Any further rotation of log files will result in data loss. 

**Workaround**: Restart the Kubernetes Logs Source to reconnect.

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-17317` >}}

### 2023-05-05 – v.4.1.1 – S3 ingestion slows after upgrade [CRIBL-17317]

**Problem**: After upgrading to Cribl Stream 4.1.1, ingesting using any `JSON` Array Event Breaker (such as AWS CloudTrail) will slow or stop due to high CPU usage.

**Workaround**: Roll back to a previous stable version, such as 4.1.0.

**Fix**: In Cribl Stream 4.1.2.

{{< id `CRIBL-17264` >}}

### 2023-04-27 – v.4.1.1 – Worker Nodes integrated with `systemd/initd` can fail upon reconfig  [CRIBL-17264]

**Problem**: Where 4.1.1 on-prem or hybrid Worker Nodes (on Linux) are integrated with `systemd/initd`, deploying new config bundles to those Worker Nodes can stop the Worker Nodes from processing data. The root cause is an incorrect backup step, which (depending on permissions) can also consume large amounts of disk space on their hosts. 

**Precondition:** Cribl-managed Cloud instances are unaffected. This problem has been observed only if at least one of the following directories is populated, or exists with restricted permissions:

```shell
/data/
/default/cribl/
/default/edge/
/local/cribl/
/local/edge/
/default/<any-installed-Pack-name>/
/local/<any-installed-Pack-name>/
```

**Workaround**: Skip v.4.1.1 or roll back to your last stable version. To run v.4.1.1, this workaround is available on systemd's `Service` section: 1. In the script `/etc/systemd/system/cribl.service`, add the line: `WorkingDirectory=/opt/cribl`, (This is the default path – specify your own path equivalent to `$CRIBL_HOME`). 2. Then reload the systemd daemon: `systemctl daemon-reload`. 3. Then restart the Cribl Stream instance using `systemctl restart <service name>`.

**Fix**: In Cribl Stream 4.1.2.

{{< id `CRIBL-17047` >}}

### 2023-04-19 v.4.4.0–4.5.0 – Special characters in Source or Destination auth tokens cause authentication failure [CRIBL-17047]

**Problem**: When you include special characters in a Source or Destination auth token, those characters may cause an authentication failure. 

**Workaround**: Use only alphanumeric characters or the underscore (`_`) in a Source or Destination auth token. Do not use any of the following characters: `<`, `>`, `"`, `` ` ``,  `\r`, `\n`, `\t`, `{`, `}`, `|`, `\`, `^`, or `'`. Alternatively, upgrade to Cribl Stream 4.5.1.

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-16944` >}}

### 2023-04-17 – All Versions through v4.2.2 – Kafka-based Destinations fail to report backpressure properly [CRIBL-16944]

**Problem**: With a downstream receiver exerting backpressure, Kafka-based Destinations do not report backpressure promptly, even though they effectively exert backpressure. As a result, upstream data might stall for several minutes before appropriate errors surface in the Destination's health status, and PQ (if configured) will not engage promptly.

**Workaround**: Decreasing the value for the `Request timeout (ms)` setting within the `Advanced Settings` section of the destination configuration might lead to the Destination reporting backpressure more promptly.

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-16929` >}}

### 2023-04-14 – v.2.2–4.1.x – Requests from Office 365 Message Trace Source failed intermittently [CRIBL-16929]

**Problem**: Microsoft has fixed this Office 365 Message Trace API problem with [this patch](https://portal.office.com/adminportal/home#/servicehealth/:/alerts/EX544330) (Microsoft login required). Before this patch, requests failed intermittently, often with `Non-whitespace` errors.

**Fix**: Fixed by Microsoft (not a Cribl problem).

{{< id `CRIBL-16926` >}}

### 2023-04-13 – v.4.1.1 – Monitoring incorrectly shows metrics for disconnected Subscriptions [CRIBL-16926]

**Problem**: Selecting **Monitoring** > **Data** > **Subscriptions** will falsely show statistics even for Subscriptions that are not connected to a Destination, and therefore have no data flow.

**Workaround**: Ignore these Monitoring statistics.

**Fix**: In Cribl Stream 4.1.2.

{{< id `CRIBL-16549` >}}

### 2023-04-03 – v.4.1.0 – Windows Event Logs Source should have a configurable Max Event Size [CRIBL-16549]

**Problem**: Events aren't broken properly when collecting logs from the PowerShell event log. The Source contains a Max Event Limit of `51200`, which is a hard-coded breaker config. This limit should be configurable. 

**Fix**: In Cribl Stream 4.1.1.

{{< id `SAAS-3681` >}}

### 2023-03-31 – v.4.1.0-4.1.1 – Cribl.Cloud API Credential's Roles aren't honored on tokens [SAAS‑3681]

**Problem**: API Bearer tokens obtained [through the Cribl.Cloud portal](api-tutorials#criblcloud-free-tier) are all granted the `Admin` Role, regardless of the scope specified on the parent Credential.

**Workaround**: Use the [pre-4.0 workaround](/stream/4.0/api-tutorials#tokens) to obtain Bearer tokens from the in-app API Reference.

**Fix**: In Cribl Stream 4.1.2.

{{< id `CRIBL-16494` >}}

### 2023-03-30 v.4.1.0 – Splunk Load Balanced Destination incorrectly re‑creates all connections during DNS resolution [CRIBL-16494]

**Problem**: When **Minimize in-flight data loss** is enabled, the Splunk Load Balanced Destination re-creates all outbound connections during DNS resolution. This will cause the Destination to report a blocked status until the outbound connections refresh. If persistent queues (PQ) are enabled, PQ will engage while the Destination is blocked.

**Workaround**: Select the Destination's **Manage as JSON** option, to add the key-value pair: `"maxFailedHealthChecks": 1`.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-16432` >}}

### 2023-03-28 – v.4.1.0 – Add Kubernetes and Docker modals in the UI aren't checking the `CRIBL_BOOTSTRAP_HOST` environment variable  [CRIBL-16432]

**Problem**: The Add Windows and Linux modals check for a bootstrap hostname to see if a user has configured a host override with the `CRIBL_BOOTSTRAP_HOST` environment variable. The Add Kubernetes and Docker modals aren't checking the environment variable as expected.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-16339` >}}

### 2023-03-23 – v.4.1.0 – Leader's config deployments to pre-4.1 Workers silently fail [CRIBL-16339]

**Problem**: A 4.1.x Leader requires Workers to also be upgraded to – v.4.1.x. If you attempt to deploy configs to Workers running earlier versions, the intended upgrade prompt might not appear. In this case, the deploy will silently hang.

**Workaround**: If you haven't enabled automatic upgrades of on-prem or hybrid Workers, upgrade all Worker Groups to 4.1.x before you deploy to them.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-16304` >}}

### 2023-03-22 – v.4.1.0-4.1.1 – Cribl Stream Database Collector cannot connect with SQL Server using AD authentication [CRIBL-16304]

**Problem**: On the indicated versions, the Database Collector cannot connect with SQL Server using Active Directory (AD) authentication. (Only local auth works.) 

**Workaround**: Reverting to Cribl Stream v.4.0.4 restores the Database Collector's ability to connect using AD authentication.

**Fix**: In Cribl Stream 4.1.2.

{{< id `CRIBL-16284` >}}

### 2023-03-21 – v.4.1.0-4.3.1 – Unable to bind `CRIBL_DIST_LEADER_URL` to an IPv6 address [CRIBL-16284]

**Problem**: Due to an issue with the `CRIBL_DIST_LEADER_URL` environment variable, Cribl will not bind to a specified IPv6 address. 

**Workaround**: Manually update the settings in the two relevant configuration files, [`instance.yml`](/stream/instanceyml/) and [`cribl.yml`](/stream/criblyml/), for RPC and UI communication respectively.

For the UI, manually define the host setting in the `cribl.yml` file:

```yaml
api:
  host: "::"
```

To configure the instance as a Leader node and listen on all IPv6 and IPv4 addresses, manually configure the `instance.yml` file:

```yaml
distributed:
  mode: master
  master:
    host: "::"
    port: 4200
    tls:
      disabled: true
    authToken: criblmaster
```

**Fix**: In Cribl Stream 4.4.

{{< id `CRIBL-16194` >}}

### 2023-03-20 – v.3.5.0–4.1.0 – System Activity drawer on Windows Edge Nodes displays no data [CRIBL-16194]

**Problem**: When you navigate to a Fleet's **List View** and click a row containing a Windows Edge Node, the System Activity drawer displays no data. 

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-16203` >}}

### 2023-03-20 – v.4.1.0-4.2.1 – If a deployment fails, Workers will not automatically revert to the previous version [CRIBL-16203]

**Problem**: When a deployment fails, Workers cannot revert `default/cribl` to a previous version because that directory is no longer backed up. The Worker will enter a broken state if you provide an invalid deploy bundle, because it cannot revert to the last valid state.

**Workaround**: To resolve any broken Workers, you must deploy a valid configuration. The Worker will resume working once it picks up the new configuration.

**Fix**: In Cribl Stream 4.3.1. Closed as a duplicate of CRIBL-15467 which was fixed in 4.1.1.

{{< id `CRIBL-16142` >}}

### 2023-03-15 – v.4.1.0 – QuickConnect Pipeline Settings button fails [CRIBL-16142]

**Problem**: When adding a new Pipeline using QuickConnect, clicking the gear (⚙️) button fails to open Pipeline Settings.

**Workaround**: After saving the Pipeline in QuickConnect, use **Manage** > **Processing** > **Pipelines** to select your new Pipeline. Click the gear button here to access Pipeline Settings as expected.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-16102` >}}

### 2023-03-14 v.4.0.0–4.1.0 – Duplicate data with Event Hubs [CRIBL-16102]

**Problem**: When **Minimize Duplicates** is enabled, the Azure Event Hubs Source assigns partitions to multiple Worker Nodes in the same Worker Group, which generates duplicate data. 

**Workaround**: Toggle **Minimize Duplicates** to `No` in **Azure** > **Event Hubs** > **Advanced Settings**.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-15696` >}}

### 2023-03-01 – v.4.0.4 – Kafka-based Sources are omitting the `_time` field [CRIBL-15696]

**Problem**: Kafka-based Sources (Kafka, Azure Event Hubs, Confluent Cloud) are not sending the `_time` field.

**Workaround**: If Cribl Stream is not retrieving the `_time` field, then in the affected Sources' config modals, use the **Processing Settings** > **Fields** tab to re-create `_time`. This new `_time` field will add the current (ingestion) time instead of the message time.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-15538` >}}

### 2023-02-24 – v.4.1.0-4.2.1 – The Chain Function has a 10% impact on performance [CRIBL-15538]

**Problem**: Using the Chain Function to chain data processing from one Pipeline or Pack to another degrades performance by about 10%, compared to running the original Pipeline or Pack directly.  

**Fix**: In Cribl Stream 4.2.2.

{{< id `CRIBL-15363` >}}

### 2023-02-23 – v.4.0.x – DNS Resolution reconnects Cribl TCP connections every time [CRIBL-15363]

**Problem**: All Cribl TCP connections will reconnect every time DNS Resolution occurs, even when reconnection is not necessary.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-15059` >}}

### 2023-02-03 – v.4.0.0 through Current – Amazon Kinesis Data Streams Source resumes reading at earliest event when Leader Node is down [CRIBL-15059]

**Problem**: After the Worker on which it is running restarts, the Kinesis Streams Source should resume reading a shard from the point where it left off. That point is a sequence number obtained from the Leader Node – but if the Leader Node is down, sequence numbers (state) will not be available. In this situation, the Worker should wait for the Leader Node to come back up, obtain the state, and then resume reading from the correct point in the shard. 

However, due to this issue, the Worker does not wait for the Leader to come back up if the Leader stays down for over one minute. Instead, the Worker starts where **Optional Settings > Shard iterator start** says it should. 
* If **Shard iterator start** is set to its default of `Earliest record`, the Worker will resume reading from the beginning of the shard, potentially collecting events it already collected earlier, producing duplicate events.
* If **Shard iterator start** is set to `Latest record`, the Worker will resume reading from the end of the shard, potentially missing events if the last event it collected before the restart is not the last event in the shard.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-15502` >}}

### 2023-02-23 – v.4.0.4–4.1.2 – Enable Automatic Upgrades deletes remote repo's Git Settings [CRIBL-15502]

**Problem**: Enabling the Leader's **Upgrade** >  **Enable Automatic Upgrade** setting deletes the corresponding remote repo's stored **Git Settings**. 

**Workaround**: Reconfigure your repo at **Git Settings** > **Remote**. 

**Fix**: In Cribl Stream 4.1.3.

{{< id `CRIBL-15455` >}}

### 2023-02-22 – v.4.0.4–4.1.0 – Sending data to an inactive Kafka, Confluent Cloud, or Azure Event Hubs Destination triggers an endless series of log errors [CRIBL-15455]

**Problem**: If you accidentally make a Kafka, Confluent Cloud, or Azure Event Hubs Destination inactive by putting a mismatched value in the **Advanced Settings** > **Environment** setting, Cribl Stream will send an endless series of errors to the log file.

**Workaround**: Correct the **Environment** value or leave it empty.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-15365` >}}

### 2023-02-20 – v.4.0.x–4.1.0 – Logging-level changes do not take effect for services [CRIBL-15365]

**Problem**: Changing logging levels in the Leader's Settings has no effect on the logs for the Connections, Lease Renewal, Metrics, or Notifications services.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-15367` >}}

### 2023-02-20 – v.4.0.3–4.1.0 – On-prem Worker Nodes automatically upgrade, ignoring Leader settings [CRIBL-15367]

**Problem**: Upgrading a Leader Node causes its on-prem Worker Nodes to automatically upgrade, even when the Leader's **Enable automatic upgrades** option is set to `No` (the default). This problem does not affect Cribl-managed Cribl.Cloud Worker Nodes.

**Workaround**: 1. Toggle **Enable automatic upgrades** to `Yes` and save the configuration; then slide it back to `No` and save again. Or: 2. Edit `local/cribl/cribl.yml` to explicitly set `upgradeSettings.disableAutomaticUpgrade` to `true`. 

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-15246` >}}

### 2023-02-14 – v.4.0.0–4.0.4 – Pipeline is deselected in QuickConnect after modification [CRIBL-15246]

**Problem**: If you add a Pipeline in the QuickConnect UI, then modify it in the **Edit Pipeline** modal, the Pipeline is deselected in the **Add Pipeline to Connection** modal. If you then close the **Add Pipeline to Connection** modal, the Pipeline will disappear from the Route.

**Workaround**: Select the modified Pipeline in the **Add Pipeline to Connection** modal before closing it.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-15239` >}}

### 2023-02-14 – v.3.5.0 through Current – Cannot clear Messages drawer while in GitOps Push mode [CRIBL-15239]

**Problem**: When in GitOps `Push` mode, you cannot clear messages from the **Messages** drawer. Cribl Stream returns a `Forbidden` error.

**Fix**: Version TBD.

{{< id `CRIBL-15107` >}}

### 2023-02-06 – v.4.0.x–4.1.0 – Event Breaker timestamp extraction fails after configured time [CRIBL-15107]

**Problem**: Event Breakers' timestamp extraction stops working after the Rule's configured **Future timestamp allowed** value (if set). All subsequent events received get the current time as their timestamp.

**Workaround**: Set a sufficiently large value for **Future timestamp allowed** – e.g., one year from the date you re/configure the rule. Alternately, restore normal timestamp extraction by restarting Worker Processes.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-15540` >}}

### 2023-02-05 – v.3.0.3–4.0.4 – `CRIBL_DIST_WORKER_PROXY` env var is ignored when Leader Node's Master URL is set via `instance.yml` [CRIBL-15540]

**Problem**: Setting the Cribl Stream Leader's Master URL via `instance.yml` causes Worker Nodes to ignore the `CRIBL_DIST_WORKER_PROXY` env var. Instead of trying to connect to the proxy configured in `CRIBL_DIST_WORKER_PROXY`, Worker Nodes will try to connect to other entities (e.g., Leader Nodes) directly, producing unexpected results.

**Workaround**: Set the Cribl Stream Leader's Master URL via the `CRIBL_DIST_LEADER_URL` env var, rather than via `instance.yml`.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-15081` >}}

### 2023-02-03 – v.4.0.0–4.0.4 – Metrics blocklist can’t be changed on a global level [CRIBL-15081]

**Problem**: The Metrics blocklist cannot be modified from **Settings** > **Global Settings** > **General Settings** > **Limits**.

**Workaround**: Edit the `limits.yml` file's `metricsFieldsBlacklist` element. Add the event fields for which you want to disable metrics collection; remove any event fields for which you want to restore metrics collection.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-14914` >}}

### 2023-01-26 – v.3.5.0–4.0.3 – Preview Full > Send Out does not capture events [CRIBL-14914]

**Problem**: Selecting a Sample Data file's **Preview Full** > **Send out** option captures no events. This bug has been observed when capturing on Destinations.

**Fix**: In Cribl Stream 4.0.4.

{{< id `CRIBL-14869` >}} 
 
### 2023-01-25 – v.3.5.x–4.3.1 – Cascading problems when Windows Event Logs Source collects from `ForwardedEvents` channel [CRIBL-14869]
 
**Problem**: Using the Windows Event Logs Source to collect events from a Windows `ForwardedEvents` channel can cause misattribution of logs to the local machine that hosts Cribl Edge, and undercollection of local logs from other logging channels (such as Security, System, or Application).
 
**Workaround**: Exclude the `ForwardedLogs` channel from your **Event Logs** selection.
 
**Fix**: In Cribl Stream 4.4.

{{< id `CRIBL-14681` >}}

### 2023-01-17 – v.4.0.x–4.0.4 – GitOps Push/read-only confirmation banner has dead link [CRIBL-14681]

**Problem**: After you enable GitOps `Push` mode, the resulting red confirmation banner contains a dead link labeled **GitOps Workflow**.

**Workaround**: To switch off `Push` mode, navigate to **Settings** > **Global** > **Git Settings**, and then set **GitOps  workflow** back to `None`.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-14627` >}}

### 2023-01-12 – v.3.5.x through Current – Recently added Worker Nodes fail to appear on the Monitoring page [CRIBL-14627]

**Problem**: When Worker Nodes are added, there may be a delay before the Worker count is updated on the Monitoring page.

**Workaround**: Refresh your web browser.

**Fix**: Version TBD.

{{< id `CRIBL-14609` >}}

### 2023-01-11 – Multiple versions through 4.1.x – Kafka-based Sources' rebalancing is logged with exaggerated severity [CRIBL-14609]

**Problem**: The Kafka, Azure Event Hubs, and Confluent Cloud Sources log `REBALANCE_IN_PROGRESS` events at the `error` level, even though only **frequent** rebalancing indicates a system-level or processing issue.

**Workaround**: Treat infrequent rebalancing events as `warn`.

**Fix**: Depends on a change to the [underlying kafkajs library](https://github.com/tulios/kafkajs/issues/1468).

{{< id `CRIBL-12814` >}}
{{< id `CRIBL-14556` >}}
 
### 2023-01-10 – v.3.5.x through Current – Configuration updates not shared between primary and standby Leaders [CRIBL-12814, CRIBL-14556]
 
**Problem**: With high availability/failover enabled, updates to the active Leader's configuration are not automatically synced to the standby Leader. Workers might not be able to connect to the standby Leader when it takes over.
 
**Workaround**: Use the filesystem to explicitly sync the updated `instance.yml` file across the two Leaders' hosts. In v.4.0.3 through v.4.6.1, as an intermediate fix, enabling HA will prevent further UI-based changes to **Distributed Settings**. This will enforce config changes via edits to portable `instance.yml` files. 
 
**Fix**: In Cribl Stream 4.7 (CRIBL-14556) for automatic synchronization between Leaders' hosts.

{{< id `CRIBL-14453` >}}

### 2023-01-02 – v.4.0.0–4.0.2 – Google Cloud Chronicle Destination drops changes in distributed deployments with custom log types [CRIBL-14453]

**Problem**: A new Google Cloud Chronicle Destination with a custom log type might fail to save changes. This issue affects only distributed deployments.

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-14299` >}} 

### 2022-12-15 – v.3.5.4–4.0.3 – Changes to a Lookup table on the Leader don't always propagate to Worker Nodes [CRIBL-14299] 

**Problem**: Modifying a Lookup table on the Leader doesn't always propagate the changes to Worker Nodes, even after clicking **Commit** and **Deploy**.

**Workaround**: Manually restart the Worker Nodes to refresh Lookup tables. 

**Fix**: In Cribl Stream 4.0.4.

{{< id `CRIBL-14239` >}} 

### 2022-12-13 – v.4.0.0 through Current – Default commit message missing for non-admin users [CRIBL-14239] 

**Problem**: For users who have [Roles](roles#default-roles) as high as `owner_all`, but not `admin`, the Commit modal fails to display any **Default commit message** saved in **Git Settings**.

**Workaround**: Enter (or paste) a message per commit.

**Fix**: Version TBD.

{{< id `CRIBL-14180` >}}

### 2022-12-09 – v.4.0.1–4.0.2 – Users assigned the `owner_all` role cannot perform commits [CRIBL-14180]

**Problem**: When a user with the `owner_all` role tries to perform a commit, the commit fails with the UI displaying a `Forbidden` modal.

**Workaround**: Modify the `GroupFull` policy, as follows:

1. As a user with the `owner_all` role, try a `POST /version/commit` API call with a request body of `{ "message": "test: hello" }`. The commit should fail, and the request payload should be `{ message: "test: hello" }`.

1. If `$CRIBL_HOME/local/cribl/policies.yml` does not exist, copy `$CRIBL_HOME/default/cribl/policies.yml` to `$CRIBL_HOME/local/cribl/policies.yml`.

1. If your OS is Linux, run the command `chmod 0744 policies.yml`.

1. In `$CRIBL_HOME/local/cribl/policies.yml`, edit the `GroupFull` policy to match the following:

    ```yaml
    GroupFull:
      args:
        - groupName
      template:
        - PATCH /master/groups/${groupName}/deploy
        - GroupEdit ${groupName}
        - POST /version/commit
        - GET /version
        - GET /version/*
    ``` 

1. Now retry the `POST /version/commit` API call, again with a request body of `{ "message": "test: hello" }`. The commit should succeed, and the request payload should be `{ message: "test: hello", "group": "default", "effective": true }`.

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-14169` >}}

### 2022-12-08 – v.4.0.1–4.0.2 – Landing page not displaying system metrics from Edge Nodes when teleporting from the Leader [CRIBL-14169]

**Problem**: When you teleport from a Leader running v.4.0.1 or 4.0.2 to an Edge Node that’s running v.4.0.0 or earlier, the system metrics for the landing page will not populate. This also affects the system metrics shown in the Node drawer for the honeycomb and Node List View pages.

**Workaround**: Upgrade Edge Nodes to v.4.0.2 or higher.

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-14160` >}}

### 2022-12-07 – v.4.0.1 – Google Cloud Pub/Sub Source and Destination can break on field validation [CRIBL-14160]

**Problem**: The Google Cloud Pub/Sub Source and Destination strictly validated entries in the **Topic ID** and **Subscription ID** fields. If you entered a full path (rather than the expected ID substring) in these fields, this strict validation broke the integration and broke the UI.

**Workaround**: Trim **Topic ID** and **Subscription ID** field values to just the ID.

**Fix**: Validation is rolled back in Cribl Stream 4.0.2.

{{< id `CRIBL-14067` >}}

### 2022-12-01 – v.4.0.2 – CrowdStrike FDR Source needs 6-hour visibility timeout [CRIBL-14067]

**Problem**: With its default **Visibility timeout seconds** setting of `600` (10 minutes), the CrowdStrike FDR Source can accumulate large backlogs when pulling data from Crowdstrike buckets.

**Workaround**: Set the **Visibility timeout seconds** to `21600` (6 hours), as CrowdStrike recommends. Leave both **Max messages** and **Num receivers** at their default `1` settings.

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-14093` >}}

### 2022-12-01 – v.4.0.0 – Amazon S3 Source stops receiving data after upgrade [CRIBL-14093]

**Problem**: Upgrading to Cribl Stream 4.0.0 can cause the Amazon S3 Source to stop receiving data. This is due to a race condition between (premature) SQS messaging versus the Source's initialization. 

**Workaround**: Skip v.4.0.0 (or temporarily roll back to v.3.5.4).

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-14041` >}} 

### 2022-11-30 – v.4.0.2 – QuickConnect-configured Sources' misleading "Enabled" status while disconnected [CRIBL-14041] 

**Problem**: As you configure a new Source from the QuickConnect UI, the Source's **Enabled** slider will initially switch to `Yes`. This is misleading, because the Source is not yet connected to a Destination, so no data can flow.

**Workaround**: Save the Source in its config drawer. This resets the **Enabled** slider to its accurate `No` status, until you connect the Source to a Destination.

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-13910` >}} 

### 2022-11-28 v.4.0–4.0.2 – “Unknown config version” error can appear when deploying changes [CRIBL-13910] 

**Problem**: Attempting to commit and deploy a change can trigger errors of the form `Unknown config version: "[hash]"`.

**Workaround**: On the Leader's host, upgrade the `git` client to v.1.9.1 or later. 

**Fix**: In Cribl Stream 4.0.3.

{{< id `CRIBL-13999` >}} 

### 2022-11-28 – v.4.1.0 through Current – Diag bundles might fail to download from teleported Worker Nodes [CRIBL-13999] 

**Problem**: When you're remotely accessing a Worker Node's UI, diag bundles might fail to download from **Global Settings** > **Diagnostics**. 

**Workaround**: Refresh the page and **Export** the bundle again. Alternatively, log directly into the Worker Node's UI before creating the diag. 

**Fix**: Version TBD.

{{< id `CRIBL-13863` >}}
{{< id `CRIBL-15507` >}}

### 2022-11-23 – All versions through 4.1.0 – Worker Nodes show incorrect configurations after upgrade or commit/deploy from Leader [CRIBL-13863, CRIBL-15507]

**Problem**: When you commit and deploy changes from the Leader, Worker Nodes sometimes fail to automatically restart with the correct configuration changes. 

**Workaround**: Manually restart the Worker Nodes.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-13868` >}}

### 2022-11-23 – v.4.0.0 – Teleported Worker Nodes don't display Distributed Settings or Diagnostics Settings [CRIBL-13868]

**Problem**: When displayed via remote access from the Leader ("teleporting"), a Worker's/Edge Node's **Settings** UI omits the **Distributed Settings** and **Diagnostics** left-nav links. This blocks remote access to features like [configuring TLS communications](securing-communications) between Worker Node and Leader.

**Workaround**: Access the Worker's UI directly on its host's port 9000 (or other configured port). As a precondition, you might need to undo any [Disable UI Access](securing-and-monitoring#ui-access) setting on the parent Worker Group.

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13860` >}}

### 2022-11-22 – v.3.2.0–4.0.0 – Chain Function creates extra Pipelines and Functions during initialization [CRIBL-13860]

**Problem**: Under certain circumstances, when initializing, the Chain Function created multiple unneeded Pipelines and Functions.

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13826` >}}

### 2022-11-21 – v.4.0.0 – Metrics service unavailable [CRIBL-13826]

**Problem**: The [Monitoring](/stream/monitoring) and [Fleet](/edge/managing-edge-nodes) pages don't load, due to the [systemd-tmpfiles](https://man.archlinux.org/man/systemd-tmpfiles-clean.service.8) service cleaning some of the Cribl socket files from the `/tmp/` directory.

**Workaround**: Stop the host system from cleaning the socket files from the `/tmp/cribl-*` directory. For example, on an Amazon EC2 instance, add a new `tmp.conf` file to `/etc/tmp.conf` with the line `X /tmp/cribl-*`. Then restart the tmpfiles cleanup service with: `systemctl restart systemd-tmpfiles-clean.service`. Finally, restart the Cribl server.

**Fix**: This issue no longer applies to Cribl.Cloud as of 4.0.1. On-prem customers should use the workaround, which will be added to the deployment documentation as a standard practice. 

{{< id `CRIBL-13811` >}}

### 2022-11-18 – v.4.0.0–4.0.3 – License usage visible only for current day [CRIBL-13811]

**Problem**: In the listed versions, selecting **Monitoring** > **System** > **License** displays usage only for the current day.

**Fix**: In Cribl Stream v.4.0.4.

{{< id `CRIBL-13761` >}}

### 2022-11-16 – v.3.4.0–4.0.4 – Source PQ in Smart mode can trigger excessive memory usage and OOM failures [CRIBL-13761]

**Problem**: Enabling Source-side Persistent Queues in [Smart mode](persistent-queues#source-triggers) can trigger excessive memory usage, leading to out-of-memory failures and Worker Node instability.

**Workaround**: If you encounter OOM failures (most likely with high throughput), set PQ to `Always On` mode instead.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-13681` >}}

### 2022-11-14 – version 4.0.0 – `Forbidden` error banners mistakenly displayed to non-admin users [CRIBL-13681]

**Problem**: Non-admin users (with the `reader_all` Role) are mistakenly shown a continuous string of `Forbidden` error banners. 

**Workaround**: Available upon request (documented in the ticket), but cumbersome. Cribl recommends upgrading instead.

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13900` >}} 

### 2022-11-13 – v.4.0.0–4.0.2 – Edge/Kubernetes Logs Source repeats logs due to incorrect timestamp [CRIBL-13900] 

**Problem**: The Kubernetes Logs Source is collecting redundant information for containers that don't emit their own timestamps. This Source assumes "current time" when the timestamps are missing, causing it to incorrectly stream older logs during restarts.

**Fix**: In Cribl Edge 4.0.3.

{{< id `CRIBL-13595` >}}

### 2022-11-08 – v.4.0.0 – Cribl Edge honeycomb display doesn't render null values [CRIBL-13595]

**Problem**: When you view Edge Nodes in **Map View**, a honeycomb displays values for each of the metrics you select in the **Measure** drop-down. When a value is null for a particular the Edge Node, the honeycomb should display `null`; instead, the value incorrectly renders as `ul i`.

**Fix**: In Cribl Stream 4.0.1.

{{< id `DOC-100` >}}

### 2022-11-08 – v.4.0.0 – Broken in-app doc links [DOC‑100]

**Problem**: The following Sources have broken in-app help links: [Kubernetes Logs](/edge/sources-kubernetes-logs), [Kubernetes Metrics](/edge/sources-kubernetes-metrics), [Windows Event Logs](/edge/sources-windows-event-logs), and [Windows Metrics](/edge/sources-windows-metrics).

**Workaround**: Use the above links to access these Sources' [online documentation](https://docs.cribl.io/edge/).

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13554` >}} 

### 2022-11-07 – v.4.0.0 – Spurious “Secret decrypt failed with error" log messages appear after upgrade [CRIBL-13554] 

**Problem**: After upgrading to Cribl Stream 4.0.0, multiple `Secret decrypt failed with error` messages might appear in logs. These are spurious, and you can disregard them.

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13566` >}} 

### 2022-11-07 – v.4.0.0 – Upgrade status shown on Group Upgrade page doesn't match Global Settings [CRIBL-13566] 

**Problem**: The **Enable automatic upgrades** status shown on the **Settings > Group Upgrade** page doesn’t match the slider selection on the **Settings > Global Settings > Upgrade** tab. 

**Workaround**: View the **Enable automatic upgrades** slider on the **Settings > Global Settings > Upgrade** tab to verify the current upgrade status. 

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13530` >}} 

### 2022-11-04 – v.3.5.1–4.0.4 – Avg Thruput (Bytes Per Second) not displayed for internal logs or metrics [CRIBL-13530] 

**Problem**: No **Avg Thruput (Bytes Per Second)** data is displayed on Cribl Internal logs and metrics Sources' **Charts** tab, nor for these Sources on their attached Routes. This is by design, because Cribl Stream does not perform BPS calculations for internal logs or metrics. But the visual disparity can be confusing. 

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-13505` >}}

### 2022-11-03 – v.4.0.0 – Sorting by column on Manage > Worker Node page results in blank list [CRIBL-13505]

**Problem**: When you click a column header on the **Manage** > **Worker Node** page, the list goes blank and rows are not sorted. 

**Workaround**: Click the heading a couple of times to refresh the list.  

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-13404` >}}

### 2022-11-01 – 4.0.0 through Current – Kubernetes Logs Source doesn't collect logs from exited or short-lived containers [CRIBL-13404]

**Problem**: The Kubernetes Logs Source collects logs only from running
containers, which can lead to missed logs from short-lived containers or those
that exit before they start running.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-13414` >}} 

### 2022-11-01 – All versions through Current – Source PQ in Smart mode could cause premature backpressure [CRIBL-13414]

**Problem**: Source-side persistent queuing enabled in `Smart` mode might trigger backpressure before the configured maximum queue size is reached. For TCP Sources, this will send backpressure back to the sender. For Sources using the UDP protocol, it will cause events to be dropped.

**Fix**: None, works as designed.

{{< id `CRIBL-13400` >}}

### 2022-11-01 – v.4.0.0 through Current – Default AppScope Config file can't be restored when in use [CRIBL-13400]

**Problem**: When the default AppScope Config file is assigned to an [AppScope Source filter](sources-appscope#filter), you can't restore it to its default settings. You'll see an error message that the AppScope Config file is currently in use. 

**Workaround**: On the AppScope Source's **AppScope Filter Settings** tab, delete the default AppScope Config from the **Allowlist**. Then, edit the AppScope Config Knowledge object to restore its default settings. You can now add this AppScope Config, with its restored settings, back to the AppScope Source's Allowlist. 

**Fix**: Will not fix.



{{< id `CRIBL-13125` >}} 

### 2022–10-20 – v.4.0.0 through Current – Pipelines with Clone and other Functions can show inconsistent total processing times [CRIBL-13125] 

**Problem**: For Pipelines containing both a Clone Function and an async Functions (like Redis), the Pipeline Diagnostics modal's **Pipeline Profile** > **Process Time** graph will sum up the duration of all Functions' processing times. This sum can exceed the Pipeline's total duration displayed in the **Summary**. 

**Fix**: Version TBD.

{{< id `CRIBL-13501` >}}

### 2022-11-03 – v.4.0.0 – Git settings don't immediately populate Commit modals [CRIBL-13501]

**Problem**: After you save Git settings (such as a default commit message), the new settings do not immediately appear in Commit/Git Changes modals.

**Workaround**: A hard or soft refresh will close the modal, but when you reopen it, it will reflect your new settings. 

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-12868` >}}

### 2022-10-11 – All versions through 4.7.1 – GitOps Push mode doesn't support ad hoc Collection jobs [CRIBL-12868]

**Problem**: If you've enabled the [GitOps](/stream/gitops) Push workflow, you will be unable to run ad hoc [Collection jobs](/stream/collectors-schedule-run).

**Workaround**: There are two options. 1. Temporarily disable the Push workflow in your environment. 2. Create a scheduled Collection job (with a relaxed cron schedule) on your dev branch, and push it to production through your Push workflow.

**Fix**: In Cribl Stream 4.7.2.

{{< id `CRIBL-12737` >}}

### 2022-10-03 – All versions through 4.0.4 – New Help drawers open under pinned Help drawers [CRIBL-12737] 

**Problem**: If you’ve pinned a Help drawer open, it's possible to open additional Help drawers behind the pinned drawer. 

**Workaround**: Close the pinned drawer to see the newly opened drawer.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-17661` >}}

### 2022-09-27 – All versions through 4.1.3 – Azure Event Hubs Destination with PQ drops events when inbound data is interrupted [CRIBL-17661, replaces CRIBL-12649]

**Problem**: When Azure Event Hubs (and other Kafka-based Destinations) have persistent queues (PQs) configured, an interruption of inbound data flow (e.g., due to network issues) can cause the Destination to start dropping events.

**Workaround**: On the Source whose events are being interrupted, configure an [Always On](persistent-queues-configuring#source) persistent queue. This buffering will cause the Destination to drop fewer events. Note that persistent queuing will not engage until Cribl Stream has exhausted all attempts to send the data to the Kafka receiver. 

**Fix**: In Cribl Stream 4.2.

{{< id `CRIBL-12602` >}} 

### 2022–09-26 – All versions through 4.0.0 – Manage Groups/Fleets pages count Packs in Pipeline totals [CRIBL-12602] 

**Problem**: On the **Manage Groups** and **Manage Fleets** pages, the **Pipelines** column overstates the actual number of Pipelines, because it includes Packs in each Worker Group's total count. 

**Workaround**: For each Worker Group, click the link in its **Pipelines** column to see the Worker Group's actual list of Pipelines. 

**Fix**: In Cribl Stream 4.0.1. 

{{< id `CRIBL-12465` >}}

### 2022-09-19 – v.4.0.0 – Subfleet search doesn't return parent Fleets [CRIBL-12465]​ 

**Problem**: On the **Manage Fleets** page's **Fleets** tab, searching for a Subfleet name returns only the Subfleet, without information about its parent Fleets.

**Fix**: In Cribl Edge 4.0.1.

{{< id `CRIBL-11983` >}}

### 2022-08-20 – v.3.2.0–4.0.3 – Deleting or modifying default Mapping Rule reclassifies Cribl-managed Cribl.Cloud Workers as hybrid Workers [CRIBL-11983]

**Problem**: Cribl.Cloud's UI currently allows deleting or modifying the `default` Mapping Rule. By doing so, an admin can inadvertently reclassify Cribl-managed Cribl.Cloud Workers as hybrid Workers. These might not be supported by your Cribl.Cloud plan.

**Workaround**: If you have an Enterprise plan, create a hybrid Worker Group to manage the resulting hybrid Workers.

**Fix**:  In Cribl Stream 4.0.4.

{{< id `CRIBL-12066` >}}

### 2022-08-25 – Versions 3.4.1 through 4.4.4 – Splunk Load Balanced Destination degradation with acks enabled [CRIBL-12066]

**Problem**: On a Splunk Load Balanced Destination, enabling the **Minimize in-flight data loss** (acknowledgments) field can cause high CPU drain and backpressure. 

**Workaround**: On the Splunk LB Destination's **Advanced Settings** tab, [disable](destinations-splunk-lb#ack) **Minimize in-flight data loss**. 

**Fix**: In Cribl Stream 4.5.

{{< id `CRIBL-11922` >}}

### 2022-08-16 – All versions through current – Splunk HEC shows higher outbound data volume than other Splunk Destinations [CRIBL-11922]

**Problem**: Events sent to the Splunk HEC Destination will show higher outbound data volume than the same events sent to the Splunk Single Instance or Splunk Load Balanced Destinations, which use the S2S binary protocol.

**Fix**: Version TBD.

{{< id `CRIBL-11767` >}}

### 2022-08-08 – v.3.x through Current – `cidr-matcher` does not support IPv6 addresses [CRIBL-11767]

**Problem**: The version of `cidr-matcher` supported by Cribl only supports IPv4 CIDR matching.

**Fix**: Won't fix.

{{< id `CRIBL-11467` >}} 

### 2022-07-20 – v3.5.0–4.0.4 – Cribl Edge/Windows: Upgrading ignores existing mode (etc.) settings [CRIBL-11467]

**Problem**: When rerunning Windows installers on a system that hosts a previous Cribl version as a managed Edge Node, the installer does not read the distributed mode or other settings. The new version might install as a Leader instance.

**Workaround**: Run the new version's installer using the [same mode and other options](/edge/deploy-windows) you used when installing the preceding version. 

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-10965` >}}

### 2022-06-30 – All versions through Current – Persistent queue with compression requires oversize max queue limit [CRIBL-10965]

**Problem**: If you configure [persistent queueing](persistent-queues-configuring) to both enable **Compression** and set a **Max queue size** limit, set that limit optimistically and monitor the results. Due to an error in computing the compression factor, Cribl Stream will not fully use **Max queue size** values below or equal to the volume's available disk space. This can lead to a mostly empty disk, lost data (either blocked or dropped), and/or excessive backpressure.

**Workaround**: With **Compression** enabled, set the **Max queue size** to a value **higher** than the volume's total available physical disk space (disregarding compression). Alternatively, leave **Max queue size** empty, to impose no limit. Monitor overall throughput and the queue size (if PQ engages), to verify that Cribl Stream is maximizing the queue.

**Fix**: Version TBD.

{{< id `CRIBL-10870` >}} 

### 2022-06-28 – v.3.5.x through Current – Cribl Edge/Windows Limitation: Cannot upgrade Edge Nodes from the Leader's UI [CRIBL-10870]

**Problem**: Cribl Edge/Windows does not support upgrading Edge Nodes via the Leader's UI. 

**Workaround**: Rerun the [Windows installer](/edge/deploy-windows) on each Edge Node, specifying the same installation options/parameters you used when installing the preceding version.

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-10402` >}}

### 2022-06-09 – v.4.0.x–4.1.0 – Edge Settings mistakenly display Process Count controls [CRIBL-10402]

**Problem**: Edge Fleets' **Fleet Settings** > **Worker Processes** page incorrectly includes editable **Process Count** and **Minimum Process Count** controls. Each Edge Node is locked to `1` Worker Process, so changes to these fields' values will have no effect.

**Workaround**: Ignore these two fields.

**Fix**: In Cribl Stream 4.1.1.

{{< id `SAAS-1141` >}}

### 2022-04-21 – All versions – Cribl.Cloud login page distorted on iPad [SAAS‑1141]

**Problem**: On certain iPads, we've seen the Cribl.Cloud login page's left text column repeated twice more across the display. These unintended overlaps prevent you from selecting (or tabbing to) the **Log in with Google** button.

**Workaround**: If you encounter this, the only current workarounds are to either use Google SSO on a desktop browser, or else use a different login method.

**Fix**: No planned fix.

{{< id `CRIBL-9539` >}}

### 2022-04-18 – v.3.3.1–4.0.0 – Lookup Functions intermittently fail [CRIBL-9539]

**Problem**: Lookup Functions within some Pipelines were skipped up to ~20% of the time. Restarting Cribl Stream resolves this temporarily, but the failure eventually resurfaces as the new session proceeds. 

**Workaround**: Where a Lookup Function fails, substitute an Eval Function, building a ternary JS expression around a [`C.Lookup`](cribl-reference#lookup) method.

**Fix**: In Cribl Stream 4.0.1.

### 2022-03-08 – v.3.3.1 through Current – GitOps + License expiration = Catch-22 [CRIBL-8600]

**Problem**: If your Enterprise license expires while you have enabled the [GitOps Push workflow](/stream/gitops#ui-options), you will encounter the following block: Cribl Stream is in read-only mode, triggering a `Forbidden` error when you try to update your license key. But you also cannot reset the workflow from `Push` to `None`, because the expired license disables GitOps features.

**Workaround**: Contact Cribl Support for help updating your license.

**Fix**: Version TBD.

{{< id `CRIBL-8210` >}}

### 2022-02-10 – All versions, 3.2.1+ – Syslog Source's previewed data doesn't proceed through Routes [CRIBL-8210]

**Problem**: The Syslog Source's **General Settings** > **Input ID** field's default filter expression is `__inputId.startsWith('syslog:in_syslog:')`. The same filter appears in the **Preview Full** tab's **Entry Point** drop-down, except without the final colon. This means that data sent using **Preview Full**'s version of the filter will not go through Routes that use the **Input ID** version.

**Workaround**: When sending data from **Preview Full** to a Route, if both use the `syslog:in_syslog` filter, edit the Route's filter to remove the final colon. This will make the filter identical in both places, routing data correctly.

{{< id `CRIBL-7998` >}}

### 2022-02-02 – v.3.0.0 through Current – Upgrading a Pack overwrites Lookup and sample-data customizations [CRIBL-7998]

**Problem**: Upgrading a [Pack](packs) overwrites any customizations you've made to the Pack's original/default Lookup tables and sample-data files.  

**Workaround**: Duplicate the Pack's default Lookup tables and sample-data files, and customize your duplicate copies. Upgrading the Pack will not overwrite your local copies.

**Fix**: Version TBD.

{{< id `CRIBL-7445` >}}

### 2021-12-21 – v.3.2.2-4.0.x – Chain Function degrades CPU load and throughput [CRIBL-7445]

**Problem**: Chaining Pipelines via the Chain Function can increase CPU load, and can signficantly slow down data throughput.

**Workaround**: Consolidate all Functions – per processing scenario – into a single Pipeline.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-7364` >}}

{{< id `CRIBL-7113` >}}

### 2021-11-24 – v.3.2.0 through Current – With processing Pipelines in QuickConnect, Preview Full doesn't show Functions' results [CRIBL-7113]

**Problem**: If a processing Pipeline has been inserted between a QuickConnect Source and Destination, selecting the **Pipelines** page, and then selecting **Preview Full** with a data sample, doesn't show the Pipeline's effects on the **OUT** tab. This affects only Pipelines inserted in a QuickConnect connection line. (Attaching a pre-processing Pipeline to a QuickConnect Source doesn't inhibit Preview.)

**Workarounds**: 1. Remove the Pipeline from QuickConnect, and re-create the same Source ‑> Pipeline ‑> Destination architecture under **Routing** > **Data Routes**. 2. Rely on **Preview Simple**.

**Fix**: Version TBD.

{{< id `CRIBL-7053` >}}

### 2021-11-17 – v.2.1.0 through Current – Re-enabling a Function group mistakenly re-enables all its Functions [CRIBL-7053]

**Problem**: When you change a Function group from disabled to enabled, all of its Functions are enabled, regardless of their individual enabled/disabled states when the group was disabled.

**Workaround**: Avoid disabling and re-enabling Functions as a group (e.g., for testing or stepwise debugging purposes).

**Fix**: Version TBD.

{{< id `CRIBL-5611` >}}

### 2021-07-06 – All versions through Current – Duplicate Workers/​Worker GUIDs [CRIBL-5611]

**Problem**: Multiple Workers have identical GUIDs. This creates problems in [Monitoring](/stream/monitoring), [upgrading](upgrading) and versioning, etc., because all Workers show up as one.

**Cause**: This is caused by configuring one Worker and then copying its `cribl/` directory to other Workers, to quickly bootstrap a deployment.

**Workaround**: Don't do this! Instead, use the [Bootstrap Workers from Leader](/stream/deploy-workers) endpoint.

**Fix**: Version TBD.

### 2021-05-17 – v.3.0.0 through Current – Can't Enable KMS on Worker Group after installing license

**Problem**: Enabling HashiCorp Vault or AWS KMS on a Worker Group, after installing a LogStream license on the same Group, fails with a spurious `External KMS is prohibited by the current license` error message.

**Workaround**: On the Leader, navigate to **Settings** > **Worker Processes**. Restart the affected Worker Group's `CONFIG_HELPER` process. Then return to that Worker Group's **Security** > **KMS** Settings, re-enter the same KMS configuration, and save.

**Fix**: Version TBD.

### 2020-02-22 – v.2.1.x – Deleting resources in `default/`

**Problem**: In a Distributed deployment, deleting resources in `default/` causes them to reappear on restart. 

**Workaround/Fix**: In progress. 
