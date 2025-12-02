# Known Issues


This page lists known issues affecting Cribl Stream and/or Cribl Edge.

{{< id `CRIBL-24860` >}}

### 2024-05-21 – v.4.5 – Incorrect `bucket_counts` format in OTLP Metrics via Kafka Destination [CRIBL-24860]

**Problem**: When sending Prometheus histogram metrics to an OTLP-compatible Destination via Kafka, the `bucket_counts` field is incorrectly formatted as an object instead of an array. This causes errors during Protobuf encoding, specifically the error: `.opentelemetry.proto.metrics.v1.HistogramDataPoint.bucket_counts: array expected`. This results in failed metric transmissions and potential data loss.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-25018` >}}

### 2024-05-20 – v.4.6.1 – REST Collectors fail to capture response headers with pagination [CRIBL-25018]

**Problem**: When you use REST Collectors with pagination enabled and turn on the **Capture response headers** toggle (introduced in the 4.6.1 release and off by default), the response headers are not captured as expected. When you toggle on **Show internal fields** in the result and expand the `__collectible` object, the `resHeaders` field is missing. This issue prevents you from accessing response header information during paginated data collection.

**Workaround**: None.

**Fix**: Version TBD.

{{< id `CRIBL-24505` >}}

### 2024-05-15 – v.4.5.1 – Throttling causes some Destination senders to reconnect at each DNS resolution interval [CRIBL-24505]

**Problem**: When you’ve configured **Throttling** on a Destination, a design conflict with the DNS resolution process leads to the reconnection of all senders in each DNS resolution cycle (every 10 minutes by default). This could cause the Destination to temporarily block and/or engage PQ, depending on the configuration, until senders reconnect. This affects the Cribl TCP, Syslog, and TCP JSON Destinations when load balancing is enabled.

**Workaround**: Don't use **Throttling** at the Destination.

**Fix**: Version TBD.

{{< id `CRIBL-24877` >}}

### 2024-05-11 – v.4.6.1 – OTLP Metrics Function does not process metrics unless all metric names are quoted literals [CRIBL-24877]

**Problem**: When the Publish Metrics Function is followed by the OTLP Metrics Function in a Pipeline, the OTLP Metrics Function may fail to produce any results. This failure is possible when the metric name expression for any of the published metrics uses an unquoted string literal and/or evaluates to a numeric value.

**Workaround**: If you must use a metric name expression in Publish Metrics, wrap the results of the expression in single or double quotes to ensure that it returns a string.

Optionally, leave the metric name expression blank and use the Rename Function to rename the metric names before sending them to the OTLP Metrics Function.

**Fix**: Version TBD.

{{< id `CRIBL-24687` >}}

### 2024-05-03 – v.4.6.0 – Some licensing limits are being applied incorrectly [CRIBL-24687]

**Problem**: After the Leader restarts, Cribl Stream may not apply your license correctly. Because of this error, you may have insufficient user privileges for certain features and limits. For example, Cribl Stream may not correctly apply certain licensing limits, such as the total data volume allowed. Under certain conditions, this issue can block data ingestion. Note that this issue is timing-dependent so it can affect different areas of the product after a Leader restart.

**Workaround**: Two workarounds are available: 

- Reapply the license in the user interface without restarting the Leader.
- Wait approximately one hour for the license manager to perform a periodic license check. This will reapply the license and may resolve the issue.

**Fix**: In Cribl Stream 4.6.1.

{{< id `CRIBL-24451` >}}

### 2024-04-25 – v.4.6.0 – In environments that use a proxy or a TLS-breaking security appliance, the Worker / Edge Node to Leader communication is disrupted [CRIBL-24451]

**Problem**: In proxied deployments of Cribl Stream and Edge, Worker Nodes /
Edge Nodes reference the Leader Node using the TLS server name, `stream` or `edge`,
which web proxies block. 

**Workaround**: There are several workaround options:

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

{{< id `CRIBL-23436` >}}

### 2024-03-14 – v4.5.1 – Amazon S3 Source and Destination, Amazon SQS Destination, and CrowdStrike Source crash in FIPS mode [CRIBL-23436]

**Problem**: When Cribl Stream in FIPS mode attempts to read or write files, the Amazon S3 Source and Destination, Amazon SQS Destination, and CrowdStrike Source crash with a `digital envelope routines::unsupported` error. This happens because the underlying Amazon AWS SDK v2 is configured to use the MD5 algorithm, which is not allowed in FIPS mode. Cribl Stream 4.6.0 fixes this problem by configuring the AWS SDK to skip the checksum computation that used MD5. 

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23374` >}}

### 2024-03-14 – v.3.2.0-v.4.6.0 – Extracted traces from the OpenTelemetry Source not sent to OpenTelemetry Destinations [CRIBL-23394]

**Problem**: When the OpenTelemetry Source is configured to **Extract spans**, the resulting span event cannot be passed to OpenTelemetry Destinations, like Honeycomb.

**Workaround**: Disabling **Extract spans** allows batched spans to be ingested without problems.

**Fix**: In Cribl Stream v.4.6.1.

{{< id `CRIBL-23394` >}}

### 2024-03-13 – v4.4.0-v.4.5.1 – Diagnostic bundles cannot be created when git is enabled [CRIBL-23374]

**Problem**: Attempting to create a diagnostic bundle fails when git is enabled,
resulting in an error. 

**Workaround**: In the **Create & Export Diag Bundle** window, disable **Include git**.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-23338` >}}

### 2024-03-12 v.4.4.4-v.4.5.0 – S3 Collector **Path extractors** setting has no effect [CRIBL-23338]

**Problem**: In the S3 Collector, the **Collector Settings** > **Path extractors** setting has no effect because of a defect in the `index.js` file.

**Workaround**: Contact Cribl Support for assistance replacing the `index.js` file with a corrected one. 

**Fix**: In Cribl Stream 4.5.1.

{{< id `CRIBL-23237` >}}

### 2024-02-29 – v4.5.0-v.4.5.1 – Users cannot log into unsecured LDAP servers [CRIBL-23237]

**Problem**: 

With **Access Management > Authentication > Type** set to `LDAP` and **Secure** toggled to `No`, you should be able to configure and run an unsecured (`ldap://`)  LDAP server. See [LDAP Authentication](authentication#ldap). However, this bug causes the **Secure** toggle to have no effect when set to `No`, meaning that your LDAP server must be configured with secure (`ldaps://`) connections or you will not be able to log in.

**Workaround**: There are two possible workarounds:
1. Run your LDAP server with [secure LDAP authentication](authentication#ldap).
2. If you wish to run your LDAP server **without** secure LDAP authentication, downgrade or upgrade to run a Cribl Stream version **other than** v4.5.0 or v.4.5.1.

**Fix**: In Cribl Stream 4.6.0.

{{< id `CRIBL-23189` >}}

### 2024-02-28 – All versions though 4.6.0 – Azure Data Explorer (ADX) Destination does not retry HTTP requests that come back with `520` response codes [CRIBL-23189]

**Problem**: Cribl Stream should, but does not, retry HTTP requests that return an `520 Internal server error` response code.

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.1.

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

### 2023-12-21 – v.4.0.0-v4.5.1 – Splunk TCP Source logs include an unclear message about an unsupported payload [CRIBL-21845]

**Problem**: Because the Splunk TCP Source does not support ingesting compressed data via the Splunk S2S protocol, it cannot parse such payloads, and a correct error message in this situation would say "Could not parse payload. Turn compression off in upstream sender and try again." Instead, the Source logs the unclear message "Failed to parse S2S payload". 

**Workaround**: None.

**Fix**: In Cribl Stream 4.6.

{{< id `CRIBL-21690` >}}

### 2023-12-13 v.4.0.0–4.4.3 – Sources incorrectly report failure [CRIBL-21690]

**Problem**: When using OpenTelemetry HTTP, the Source always listens on the default address (either `0.0.0.0` or `::`, or both, depending on the network configuration of the host), ignoring the option you configured in **General Settings** > **Address**.

**Workaround**: Unidentified.

**Fix**: In Cribl Stream 4.4.4.

{{< id `CRIBL-21443` >}}

### 2023-11-30 – v4.4.0-v.4.4.1 – Edge-only Sources are unavailable in standalone Edge instances [CRIBL-21443]

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

### 2023-11-09 – All Versions through Current – High-volume UDP data dropped in Cribl.Cloud [SAAS-4399]

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

### 2023-10-03 – v.4.3.0–4.4.1 – Upgrading to v.4.3.0 or later causes Amazon Kinesis Streams and WEF Sources to lose state [CRIBL-20444]

**Problem**:

After an upgrade to v.4.3.0, Worker Nodes for a Cribl Stream Amazon Kinesis Streams Source or Windows Event Forwarder (WEF) Source can lose state upon init. This can be especially problematic for customers with large streams and high data ingestion.

An Amazon Kinesis Streams Source that loses state will fail to read a data stream from the point where it most recently left off. Instead, the Source will start reading from the location configured by **Optional Settings** > **Shard iterator start**.

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

### 2023-09-13 – v.4.2.1 through Current – Edge upgrades occasionally fail when Fleets contain a large number of Nodes [CRIBL-20067]

**Problem**: Leader-managed upgrades occasionally fail when a Fleet contains a large number of Nodes.

**Workaround**: Restart the upgrade operation to upgrade additional Nodes. 

**Fix**: Version TBD.

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

### 2023-08-28 – v.4.2.2 through 4.3.0 – PQ doesn't drain for the Event Hubs Destination when Acknowledgements is set to All or Leader [CRIBL-19756]

**Problem**: When the `Acknowledgements` setting is set to `All` or `Leader`, Kafka-based Destinations (especially Azure Event Hubs) can fail to drain the persistent queue (PQ) and the logs are filled with `Attempting to send faster than the downstream can receive – consider checking the network connection and broker health` errors.  

**Workaround**: You can set a limit using the `Persistent Queue Drain rate limit (EPS)` setting (for example, set it to `500`) to get the PQ to drain when the datagen is turned off. However, if the rate of data input continues to exceed the rate at which the Destination can send events downstream (which is slowed by the `Acknowledgements` setting), the PQ will continue to build. Alternatively, you can set the `Acknowledgements` setting to `None`, which significantly increases the rate at which data can be sent to Event Hubs.

**Fix**: In Cribl Stream 4.3.1.

{{< id `CRIBL-19709` >}}

### 2023-08-24 – v.4.2.0 through 4.2.2 – Deleting a Source via QuickConnect doesn't refresh the view [CRIBL-19709]

**Problem**: When using QuickConnect, if you delete a Source by opening its drawer and clicking `Delete Source`, the Source is deleted. However, it will continue to show on the page until you manually refresh the page.  

**Workaround**: Manually refresh the page after deleting a Source.

**Fix**: In Cribl Stream 4.3.0.

{{< id `CRIBL-19676` >}}

### 2023-08-23 - v.4.0.0 through 4.4.4 - Packs installed in Fleets are not visible to Subfleets. [CRIBL-19676]

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

### 2023-02-03 – v.4.0.0 through Current – Amazon Kinesis Streams Source resumes reading at earliest event when Leader Node is down [CRIBL-15059]

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
 
**Workaround**: Use the filesystem to explicitly sync the updated `instance.yml` file across the two Leaders' hosts.
 
**Fix**: In – v.4.0.3, as an intermediate fix, enabling HA will prevent further UI-based changes to **Distributed Settings**. This will enforce config changes via edits to portable `instance.yml` files. Version TBD for automatic synchronization between Leaders' hosts.

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

### 2022-11-16 – versions 3.4.0–4.0.4 – Source PQ in Smart mode can trigger excessive memory usage and OOM failures [CRIBL-13761]

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

{{< id `CRIBL-13414` >}} 

### 2022-11-01 – All versions through Current – Source PQ in Smart mode could cause premature backpressure [CRIBL-13414]

**Problem**: Source-side persistent queuing enabled in `Smart` mode might trigger backpressure before the configured maximum queue size is reached. For TCP Sources, this will send backpressure back to the sender. For Sources using the UDP protocol, it will cause events to be dropped.

**Fix**: None, works as designed.

{{< id `CRIBL-13400` >}}

### 2022-11-01 – v.4.0.0 through Current – Default AppScope Config file can't be restored when in use [CRIBL-13400]

**Problem**: When the default AppScope Config file is assigned to an [AppScope Source filter](sources-appscope#filter), you can't restore it to its default settings. You'll see an error message that the AppScope Config file is currently in use. 

**Workaround**: On the AppScope Source's **AppScope Filter Settings** tab, delete the default AppScope Config from the **Allowlist**. Then, edit the AppScope Config Knowledge object to restore its default settings. You can now add this AppScope Config, with its restored settings, back to the AppScope Source's Allowlist. 

**Fix**: Version TBD.



{{< id `CRIBL-13125` >}} 

### 2022–10-20 – v.4.0.0 through Current – Pipelines with Clone and other Functions can show inconsistent total processing times [CRIBL-13125] 

**Problem**: For Pipelines containing both a Clone Function and an async Functions (like Redis), the Pipeline Diagnostics modal's **Pipeline Profile** > **Process Time** graph will sum up the duration of all Functions' processing times. This sum can exceed the Pipeline's total duration displayed in the **Summary**. 

**Fix**: Version TBD.

{{< id `CRIBL-13501` >}}

### 2022-11-03 – v.4.0.0 – Git settings don't immediately populate Commit modals [CRIBL-13501]

**Problem**: After you save Git settings (such as a default commit message), the new settings do not immediately appear in Commit/Git Changes modals.

**Workaround**: A hard or soft refresh will close the modal, but when you reopen it, it will reflect your new settings. 

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-12914` >}}

### 2022-10-12 – All versions through 3.5.4 – Event preview can hide some fields [CRIBL-12914]

**Problem**: Preview panes can hide some fields at the bottom of each event.

**Workaround**: Resize your browser window to give the preview pane more depth.

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-12868` >}}

### 2022-10-11 – All versions through Current – GitOps Push mode doesn't support ad hoc Collection jobs [CRIBL-12868]

**Problem**: If you've enabled the [GitOps](/stream/gitops) Push workflow, you will be unable to run ad hoc [Collection jobs](/stream/collectors-schedule-run).

**Workaround**: There are two options. 1. Temporarily disable the Push workflow in your environment. 2. Create a scheduled Collection job (with a relaxed cron schedule) on your dev branch, and push it to production through your Push workflow.

**Fix**: Version TBD.

{{< id `CRIBL-12762` >}}

### 2022-10-04 – All versions through 3.5.4 – File Monitor Source fails with high-volume logs and file rotation [CRIBL-12762]

**Problem**: The File Monitor Source might stop reading logs from an open file, if the log rotation service renames that file.

**Workaround**: Restart the File Monitor Source.

**Fix**: In Cribl Stream 4.0.

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

{{< id `CRIBL-12203` >}}

### 2022-09-06 – v.3.3.0–3.5.4 – Load-balanced Destinations with PQ enabled: Buffer flush errors and memory leaks [CRIBL-12203]​ 

**Problem**: Destination logs might show errors of the form: `Attempted to flush previously flushed buffer token`. No data has been lost, but this will lead to a memory leak until the Worker Node is restarted. This is most likely to occur when there are more available downstream hosts than a **Max connections** limit configured on the Destination.

**Workaround**: To prevent the bug from occurring, set **Max connections** to the default `0` (enabling unlimited connections). This will ensure that all discovered hosts enter the connection pool. If you encounter the bug, restarting the affected Worker Nodes (or letting an OOM the killer do so) will clear the memory leak.

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-12097` >}}

### 2022-08-29 – All versions through 3.5.4 – Sources reject connections on ACME certs with no Subject [CRIBL-12097]

**Problem**: With multiple Sources, if you configure TLS mutual authentication using an ACME certificate with an empty `Subject` field, Cribl Stream will reject connections – even though RFC 6125 allows for `Subject` to be empty. This affects: Splunk Sources, Cribl internal Sources, Syslog, TCP, TCP JSON, Metrics, HTTP/S, Amazon Firehose, Elasticsearch API, DataDog, Grafana, Loki, Prometheus, and Windows Event Forwarder.

**Workaround**: Enter any value in the certificate's `CN` field. Any entry will cause CCribl Stream to match on the certificate's SAN (`subjectAltName`) extension.

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-11983` >}}

### 2022-08-20 – v.3.2.0–4.0.3 – Deleting or modifying default Mapping Rule reclassifies Cribl-managed Cribl.Cloud Workers as hybrid Workers [CRIBL-11983]

**Problem**: Cribl.Cloud's UI currently allows deleting or modifying the `default` Mapping Rule. By doing so, an admin can inadvertently reclassify Cribl-managed Cribl.Cloud Workers as hybrid Workers. These might not be supported by your Cribl.Cloud plan.

**Workaround**: If you have an Enterprise plan, create a hybrid Worker Group to manage the resulting hybrid Workers.

**Fix**:  In Cribl Stream 4.0.4.

{{< id `CRIBL-11972` >}}

### 2022-08-19 – v.3.5.1–3.5.4 – Using GitOps environment variables crashes the Leader [CRIBL-11972]

**Problem**: Where deployments rely on `CRIBL_GIT_*` [environment variables](environment-variables#bootstrap-variables), the Leader might crash on startup.  

**Workaround** Restart the Leader, disabling the environment variables if necessary. 

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-12066` >}}

### 2022-08-25 – Versions 3.4.1 through 4.4.4 – Splunk Load Balanced Destination degradation with acks enabled [CRIBL-12066]

**Problem**: On a Splunk Load Balanced Destination, enabling the **Minimize in-flight data loss** (acknowledgments) field can cause high CPU drain and backpressure. 

**Workaround**: On the Splunk LB Destination's **Advanced Settings** tab, [disable](destinations-splunk-lb#ack) **Minimize in-flight data loss**. 

**Fix**: In Cribl Stream 4.5.

{{< id `CRIBL-11922` >}}

### 2022-08-16 – All versions through 4.3.0 – Splunk HEC shows higher outbound data volume than other Splunk Destinations [CRIBL-11922]

**Problem**: Events sent to the Splunk HEC Destination will show higher outbound data volume than the same events sent to the Splunk Single Instance or Splunk Load Balanced Destinations, which use the S2S binary protocol.

**Fix**: Version TBD.

{{< id `CRIBL-11925` >}}

### 2022-08-16 – v.3.1.2–3.5.2 – Auto timestamping randomly begins using current time [CRIBL-11925]

**Problem**: After a random interval, the auto timestamping feature might insert the current time into the `_time` field instead of using the timestamp from the event.

**Workarounds**: Restart the Stream instances to temporarily remedy the problem, or enable an Event Breaker rule's Manual Timestamp Format option.

**Fix**: In Cribl Stream 3.5.3.

{{< id `CRIBL-11848` >}}

### 2022-08-10 – v.3.5.1-3.5.2 – Changing Notification target type crashes New Target modal [CRIBL-11848]

**Problem**: Changing the **Target Type** while configuring a new Notification target crashes the **New Target** modal. 

**Workaround**: Set the **Target type** once only while the modal is open.

**Fix**: In Cribl Stream 3.5.3.

{{< id `CRIBL-11767` >}}

### 2022-08-08 – v.3.x through Current – `cidr-matcher` does not support IPv6 addresses [CRIBL-11767]

**Problem**: The version of `cidr-matcher` supported by Cribl only supports IPv4 CIDR matching.

**Fix**: Won't fix.

{{< id `SAAS-1905` >}}

### 2022-08-03 – v.3.5.0–3.5.1 – Cribl HTTP and Cribl TCP Sources are not enabled on Cribl.Cloud [SAAS‑1905]

**Problem**: Cribl.Cloud's  ⚙️ **Network Settings** >: **Data Sources** tab lists **Ingest Address** ports for the new Cribl HTTP and Cribl TCP Sources. However, these ports are not yet enabled.

**Workaround**: If you need these ports enabled before the next maintenance release, contact Cribl Support or your Solutions Engineer.

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-11652` >}}

### 2022-08-02 – v.3.3.0–3.5.1 – Managed Edge Nodes mistakenly display "Unregistered" button [CRIBL-11652]

**Problem**: Managed Edge instances erroneously display an **Unregistered** button. Managed Edge Nodes cannot be registered, because they pull their registration/licensing information from the Leader. So this button should not appear.

**Workaround**: Ignore the scary button. Your managed Edge Node has inherited its Leader's registration.

**Fix**: In Cribl Stream 3.5.2.

{{< id `SAAS-1899` >}}

### 2022-08-02 – v.3.x through 3.5.1 – Cribl.Cloud displays unsupported Sharing settings [SAAS‑1899]

**Problem**: On Cribl.Cloud, Cribl Stream's **General Settings** > **Upgrade & Share Settings** display a **Sharing and live help** toggle, even though you cannot turn off telemetry on Cribl- or customer-managed (hybrid) Cribl.Cloud Workers.

**Workaround**: Ignore this toggle. The UI controls enable you to slide the toggle to `No` and save the result, but this will have no effect.

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-11482` >}}

### 2022-07-21 – v.3.5.0–3.5.2 – Job Inspector shows higher Bytes In than Monitoring dashboards [CRIBL-11482]

**Problem**: The Job Inspector sometimes displays a higher byte count than other Monitoring dashboards display for the same collection job. The Job Inspector overstates the throughput because it disregards any Event Breakers and Filter expressions applied between initial collection and Pipelines.

**Workaround**: Rely on the Monitoring metrics to judge accurate data flow through Pipelines to downstream services.

**Fix**: In Cribl Stream 3.5.3.

{{< id `CRIBL-11478` >}}

### 2022-07-21 – v.3.4.0–3.5.1 – Data loss with Source-side persistent queueing enabled [CRIBL-11478]

**Problem**: With Source-side PQ enabled, events got stuck and did not process through Pipelines. The root cause was a Rename Function that used wildcards to rename required internal fields (specifically, `__pqSliceId` and `__offset`).

**Workaround**: Where Rename Functions potentially rename internal fields (especially via **Rename expressions** that include wildcards), either disable PQ on Sources that feed the parent Pipeline, or test and narrow the expression to affect only the desired fields.

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-11481` >}} 

### 2022-07-21 – v3.5.1-3.5.4 – Fake Pack appears after upgrading to v.3.5.1. [CRIBL-11481]

**Problem**: After an upgrade to v.3.5.1., a new Pack appears on the **Manage Packs** page. This Pack has no **Display Name** and no contents. 

**Workaround**: Delete the ghost Pack. 

**Fix**: Not observed as of Cribl Stream 4.0.

{{< id `CRIBL-11474` >}} 

### 2022-07-20 – v3.5.0–3.5.1 – Manage Worker Nodes page fails to lazy-load Workers when scrolled [CRIBL-11474]

**Problem**: Scrolling the **Manage Worker Nodes** page fails to load more than 50 Worker Nodes. 

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-11467` >}} 

### 2022-07-20 – v3.5.0–4.0.4 – Cribl Edge/Windows: Upgrading ignores existing mode (etc.) settings [CRIBL-11467]

**Problem**: When rerunning Windows installers on a system that hosts a previous Cribl version as a managed Edge Node, the installer does not read the distributed mode or other settings. The new version might install as a Leader instance.

**Workaround**: Run the new version's installer using the [same mode and other options](/edge/deploy-windows) you used when installing the preceding version. 

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-11439` >}} 

### 2022-07-19 – v.3.5.0–3.5.1 – Cribl Edge/Windows: Syslog Source disallows UDP binding [CRIBL-11439]

**Problem**: On Cribl Edge/Windows, attempting to bind the Syslog Source to a UDP port might trigger an error of the form: `bind EADDRINUSE 0.0.0.0:<port number>`. The reason is that Edge currently does not fully support the `socket.bind` on Windows. 

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-11183` >}} 

### 2022-07-13 – v.3.5.0–3.5.1 – Cribl Edge: Fleet > List View tab doesn't correctly filter Edge Nodes [CRIBL-11183]

**Problem**: A Fleet Home page's **List View** tab doesn't correctly filter Edge Nodes. This tab displays all the Edge Nodes across all Fleets, instead of only those assigned to the Fleet.

**Workaround**: The **Map View** page is filtered correctly.

**Fix**: In Cribl Edge 3.5.2.

{{< id `CRIBL-8968` >}}
{{< id `CRIBL-10984` >}}

### 2022-07-04 – All versions through v.3.5.3 – No extended characters in Publish Metrics Function > Event field name field [CRIBL-8968, CRIBL-10984]

**Problem**: A Publish Metrics Function's **Event field name** values should contain only letters, numbers, underscores (`_`), and `.` characters (to separate names in nested structures). Using other characters will cause the parent Pipeline to stop sending events. Cribl Stream 3.5.0 and above check for valid characters when you save the Function's configuration, but in prior versions, the invalid config will save without an error message – the failure will happen at runtime, with errors echoed only in the logs.

**Workaround**: Ensure that **Event field name** values contain no extended characters.

**Fix**: In Cribl Edge 3.5.4.

{{< id `CRIBL-10981` >}}

### 2022-07-01 – All versions through 3.5.0 – S3-based Sources' unread Region triggers auth errors [CRIBL-10981]

**Problem**: On Amazon S3–based Sources, if you concatenate the AWS Region into the **Queue** field's URL or ARN, the SQS client is correctly configured to that region, but the S3 client is not. You will see authentication errors of the form: `The AWS Access Key ID you provided does not exist in our records.`

**Workaround**: Explicitly set the **Region** drop-down, even if the URL/ARN already contains the same Region. 

**Fix**: In Cribl Stream 3.5.1.

{{< id `CRIBL-10965` >}}

### 2022-06-30 – All versions through Current – Persistent queue with compression requires oversize max queue limit [CRIBL-10965]

**Problem**: If you configure [persistent queueing](persistent-queues-configuring) to both enable **Compression** and set a **Max queue size** limit, set that limit optimistically and monitor the results. Due to an error in computing the compression factor, Cribl Stream will not fully use **Max queue size** values below or equal to the volume's available disk space. This can lead to a mostly empty disk, lost data (either blocked or dropped), and/or excessive backpressure.

**Workaround**: With **Compression** enabled, set the **Max queue size** to a value **higher** than the volume's total available physical disk space (disregarding compression). Alternatively, leave **Max queue size** empty, to impose no limit. Monitor overall throughput and the queue size (if PQ engages), to verify that Cribl Stream is maximizing the queue.

**Fix**: Version TBD.

{{< id `CRIBL-10907` >}}
{{< id `CRIBL-11457` >}}

### 2022-06-29 – v.3.4.2–3.5.1 – Can't install a new Cribl Stream instance as a named user [CRIBL-10907, CRIBL-11457]

**Problem**: Bootstrapping a new Leader or Worker with a command of this form fails, with no systemd service created: `cribl boot‑start enable ‑u <username>`

The symptom is an error of the form: `error: found user=0 as owner for path=/opt/cribl/local, expected uid=1001.` This does not affect upgrades to existing instances.

**Workaround**: Issue the `[sudo] chown -R <username>: /opt/cribl` command a second time. Then reattempt the `cribl boot-start` command.

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-10870` >}} 

### 2022-06-28 – v.3.5.x through Current – Cribl Edge/Windows Limitation: Cannot upgrade Edge Nodes from the Leader's UI [CRIBL-10870]

**Problem**: Cribl Edge/Windows does not support upgrading Edge Nodes via the Leader's UI. 

**Workaround**: Rerun the [Windows installer](/edge/deploy-windows) on each Edge Node, specifying the same installation options/parameters you used when installing the preceding version.

**Fix**: In Cribl Stream 4.3.

{{< id `CRIBL-10806` >}}

### 2022-06-27 – v.3.3.0–3.5.1 – Edge vs. Stream conflict in `/tmp` directory [CRIBL-10806]

**Problem**: If you install Cribl Edge and Cribl Stream on the same machine or VM – e.g., running Edge as `root` and Stream as a `cribl` user – both services attempt to use the `tmp/cribl_temp` subdirectory. Starting Stream after Edge will throw an error of the form: `EPERM: operation not permitted, rmdir '/tmp/cribl_temp'`. Starting Edge after Stream will change the folder's ownership, blocking subsequent restarts of Stream.

**Workaround**: For either Cribl Edge or Cribl Stream, set the `CRIBL_TMP_DIR` variable to a separate base subdirectory like `/tmp/stream/` or `/tmp/edge/` before starting the app.

**Fix**: Cribl Stream 3.5.2 and later set separate `tmp/` subdirectories by default.

{{< id `CRIBL-10768` >}}
{{< id `CRIBL-11127` >}}

### 2022-06-24 – v.3.5.0 – Leader takes excessive time to report healthy status [CRIBL-10768, CRIBL-11127]

**Problem**: Upgrading from v.3.4.x to v.3.5.0 tripled the latency before some Leaders reported healthy status. The presumed cause was that load time increased due to an accumulation of persistent metrics, which progressively increased with more Worker Nodes and more uptime. 

**Workaround**: If upgrading is not possible or insufficient, move the existing metrics subdirectory away from `$CRIBL_HOME/state/metrics/`. Cribl Stream will re-create that as an empty subdirectory, and begin accruing fresh metrics there.

**Fix**: In Cribl Stream 3.5.1.

{{< id `CRIBL-10665` >}}

### 2022-06-23 – v.3.5.0 – Logs not viewable at Worker Group level [CRIBL-10665]

**Problem**: If you select **Monitoring** > **Logs**, and then select a Worker Group from the drop-down at upper left, you might see no logs at the Group level. Instead, you will see an error banner of the form:  
`ENOENT: no such file or directory, scandir '/opt/cribl_data/failover/log/group/<group‑name>'`

**Workaround**: From the drop-down at upper left, select individual Worker Nodes to view their logs.

**Fix**: In Cribl Stream 3.5.1.

{{< id `CRIBL-10402` >}}

### 2022-06-09 – v.4.0.x–4.1.0 – Edge Settings mistakenly display Process Count controls [CRIBL-10402]

**Problem**: Edge Fleets' **Fleet Settings** > **Worker Processes** page incorrectly includes editable **Process Count** and **Minimum Process Count** controls. Each Edge Node is locked to `1` Worker Process, so changes to these fields' values will have no effect.

**Workaround**: Ignore these two fields.

**Fix**: In Cribl Stream 4.1.1.

{{< id `CRIBL-10250` >}}

### 2022-06-01 – v.3.4.2-3.5.0 – Bootstrapping fails to create SystemD service file, or starts wrong service [CRIBL-10250]

**Problem**: [Bootstrapping](/stream/deploy-workers) a 3.4.2 Worker fails. The terminal displays no `Enabled Cribl to be managed by ...` confirmation message.

**Workaround**: Explicitly run the `boot‑start` [CLI command](cli-reference#boot-start).

**Fix**: In Cribl Stream 3.5.1.

{{< id `CRIBL-10228` >}}
{{< id `CRIBL-10229` >}}

### 2022-05-31 – v.3.4.2 – Cloning/loading Groups page breaks with `Config Helper service is not available...` error [CRIBL-10228, CRIBL-10229]

**Problem**: After cloning a Group containing at least one Pack, the UI hangs with a spinning pinwheel and an error banner reading `Config Helper service is not available...`. The root cause is that cloning the Group failed to copy over the Pack, leaving the new Group with broken references to it. 

**Workaround**: Use the filesystem to manually copy missing Packs from the original Group to the cloned Group. If these Packs' contents are not needed in the new Group, it's sufficient to just create the parallel subdirectory with: `mkdir $CRIBL_HOME/groups/$destGroup/default/$packName`.

**Fix**: In Cribl Stream 3.5.

{{< id `CRIBL-10092` >}}

### 2022-05-19 – v.3.3.0–3.5.1 – System Metrics Source skips filesystem events on LVM volumes [CRIBL-10092]

**Problem**: When monitoring LVM logical volumes, the System Metrics Source omits `node_filesystem_*` events.

**Fix**: In Cribl Stream 3.5.2.

{{< id `CRIBL-9859` >}}

### 2022-05-06 – v.3.5.0-3.5.4 – Running ~1,000 Edge Nodes crashes Leader [CRIBL-9859]

**Problem**: Attempting to bring up approximately 1,000 Edge Nodes (even in small batches) can crash the Leader.

**Workaround**: To support up to 1,000 Nodes, apply this Node.js setting:  
`export NODE_OPTIONS=--max-old-space-size=8192`

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-9708` >}}

### 2022-04-28 – v.3.4.0–3.4.1 – REST Collector Misinterprets `.` character in Response Attributes [CRIBL-9708]

**Problem**: Where a [REST Collector](/stream/collectors-rest)'s Response Attributes contain a `.` character, it misinterprets this as an attribute nested within an object, not a literal character. 

**Workaround**: If possible, use other Response Attributes to fetch the next page. Otherwise, skip this version, or downgrade to a known well-behaved version.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9698` >}}

### 2022-04-27 – v.3.4.1 – Git Changes drop-down returns `Invalid Date` [CRIBL-9698]

**Problem**: After opening the left nav's **Changes** fly-out, the **Version** drop-down can display `Invalid Date` instead of commits' dates.

**Workaround**: Upgrade your installed `git` client to v.2.1.1 or newer; or skip this Cribl Stream version.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9684` >}}

### 2022-04-26 – v.3.4.1 – HTTP 500 error when git is absent [CRIBL-9684]

**Problem**: A `500–Internal Server Error` error banner can indicate that `git` is not installed on Cribl Stream's host.

**Workaround**: [Install](/stream/version-control#git-installation) `git` on the Leader or single instance. On affected Worker Nodes, install `git`, followed by a `git init` and an empty push to a new `master` branch. A simpler workaround for Worker Nodes is to just enable remote access ([Stream](/stream/deploy-distributed#worker-access), [Edge](/edge/setting-up-leader-and-edge-nodes#edge-access)) from the Leader.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9633` >}}

### 2022-04-22 – v.3.3.0-3.4.1 – Datadog Agent API key ignored when forwarding metrics to Datadog [CRIBL-9633]

**Problem**: On a Datadog Destination, when **Allow API key from events** is set to `Yes`, the API key from a Datadog Agent Source is not correctly emitted with metric events sent to Datadog. Instead, Cribl Stream simply sends the static **API key** configured in the Destination. (This affects metrics events only; other data types correctly forward the event's API key.)

**Workaround**: Set **Allow API key from events**. to `No`. Configure the desired API key in the Destination's static **API key** field. (If you need multiple API keys, can create multiple Destinations, and route events to each based on the `__agent_api_key` field value. This value contains the Agent API key per event.)

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9614` >}}

### 2022-04-21 – v.3.4.1 – Failed restart, `Uncaught (in promise)` error, after changing Authentication settings [CRIBL-9614]

**Problem**: After changing Authentication settings, or configuring an external auth provider, Cribl Stream might fail to restart. Indications are errors of the form: `Uncaught (in promise) TypeError: Cannot read properties of undefined (reading 'workerRemoteAccess')`, or: `Uncaught (in promise) TypeError: l.api is undefined`.

**Workaround**: Directly edit the `cribl.yml` [file's](criblyml) `auth:` section.

**Fix**: In Cribl Stream 3.4.2.

{{< id `SAAS-1141` >}}

### 2022-04-21 – All versions – Cribl.Cloud login page distorted on iPad [SAAS‑1141]

**Problem**: On certain iPads, we've seen the Cribl.Cloud login page's left text column repeated twice more across the display. These unintended overlaps prevent you from selecting (or tabbing to) the **Log in with Google** button.

**Workaround**: If you encounter this, the only current workarounds are to either use Google SSO on a desktop browser, or else use a different login method.

**Fix**: No planned fix.

{{< id `CRIBL-9568` >}}

### 2022-04-19 – All versions through Current – Upgrade Version List is Limited to Latest Version [CRIBL-9568]

**Problem**: The **Choose Version** drop-down list displays the latest version only. This creates an issue when you want to incrementally upgrade between prior versions.  

**Fix**: Version TBD.

{{< id `CRIBL-9565` >}}

### 2022-04-19 – v.3.3.0–3.4.2 – Spurious buffer token flush error in TCP-based Destinations with PQ enabled [CRIBL-9565]

**Problem**: This error message can mistakenly appear in logs: `Attempted to flush previously flushed buffer token`. You can ignore it on TCP-based, load-balanced Destinations (Splunk Load Balanced, TCP JSON, Syslog/TCP, and Cribl Stream) with persistent queueing enabled.

**Fix**: In Cribl Stream 3.5.

{{< id `CRIBL-9539` >}}

### 2022-04-18 – v.3.3.1–4.0.0 – Lookup Functions intermittently fail [CRIBL-9539]

**Problem**: Lookup Functions within some Pipelines were skipped up to ~20% of the time. Restarting Cribl Stream resolves this temporarily, but the failure eventually resurfaces as the new session proceeds. 

**Workaround**: Where a Lookup Function fails, substitute an Eval Function, building a ternary JS expression around a [`C.Lookup`](cribl-reference#lookup) method.

**Fix**: In Cribl Stream 4.0.1.

{{< id `CRIBL-9530` >}}

### 2022-04-15 – All versions through 3.4.1 – Git permission errors [CRIBL-9530]

**Problem**: Multiple Linux distro's have backported a permissions-restriction patch from `git-2.35.1` to earlier versions, notably including Ubuntu 20.04 LTS. If you see errors of the form `fatal: unsafe repository ('/opt/cribl' is owned by someone else)` on the command line or in the Cribl Stream UI, this indicates an ownership mismatch (current user versus file base) on the directory corresponding to `$CRIBL_HOME` or `$CRIBL_VOLUME_DIR`. 

**Workaround**: Use `chown` to recursively set permissions on all files in `/opt/cribl/` (or in or `$CRIBL_VOLUME_DIR`'s target directory) to match the user running Cribl Stream.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9524` >}}

### 2022-04-15 – v.3.4.0 – Workers report incomplete metrics [CRIBL-9524]

**Problem**: After upgrading to v.3.4.0, Worker Nodes failed to report all metrics. Missing metrics were logged with a warning of the form: `failed to report metrics`, and with a reason of the form: `Cannot read property 'size' of undefined`.

**Fix**: In Cribl Stream 3.4.1.

{{< id `CRIBL-9510` >}}

### 2022-04-14 – v.3.4.0-3.4.1 – Monitoring visualizations lack color indicators [CRIBL-9510]

**Problem**: When you hover over the Monitoring page's graphs, the expected red/yellow/green status indicators are missing.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9235` >}}

### 2022-04-07 – v.3.4.0–3.4.1 – Chain Function referencing a Pack triggers errors on Pack's Functions [CRIBL-9235]

**Problem**: After you define a Chain Function that references a Pack, Functions on Pipelines within that Pack will throw errors of the form: `Failed to load. Function <Function‑name> is missing. Please fix, disable, or remove the Function.` You might also find that you cannot add more Functions within the Pack. (However, data might continue to flow, despite these errors.)

**Workaround**: Remove the Chain Function, or its Pack reference. Refactor your flow logic to avoid this combination.

**Fix**: In Cribl Stream 3.4.2.

{{< id `CRIBL-9175` >}}

### 2022-04-05 – v.3.4.0 through Current – Upgrade to 3.4.0 disables Worker/Node UI access [CRIBL-9175]

**Problem**: Upon upgrading to v.3.4.0, even if the (prior, global) Worker/Node **UI access** option was set to `Yes`, the new per–Worker Group toggles are all set to `No` by default.

**Workaround**: On the **Manage Worker Groups** page, re-enable the **UI access** toggles as desired. If these toggles are not displayed (with a Free license), edit `groups.yml` to add the key-value pair `workerRemoteAccess: true` within each desired Worker Group.

**Fix**: Version TBD.

{{< id `CRIBL-9019` >}}

### 2022-03-29 – v.3.4.0 – `this.dLogger.isSilly is not a function` errors [CRIBL-9019]

**Problem**: Errors of this form have been observed with multiple triggers: 1. Disabling a customized Source or Destination (it remains enabled). 2. Importing and deleting a Pack. 3. Changing a channel's logging level.

**Workaround**: Delete and re-create affected Sources and Destinations. (No workaround has been identified for the Pack or logging errors.)

**Fix**: In 3.4.1.

{{< id `CRIBL-8938` >}}

### 2022-03-23 – v.3.4.0 – Monitoring page error in distributed environments with many Worker Nodes [CRIBL-8938]

**Problem**: The Monitoring page displays an error where distributed environments have more than 30 Worker Nodes. 

**Fix**: In 3.4.1.

{{< id `CRIBL-8932` >}}

### 2022-03-23 – Collect parameters values not evaluated as expressions [CRIBL-8932]

**Problem**: In a [REST Collector](/stream/collectors-rest)'s **Collect parameters** table, entering a parameter name like "earliest" as plain text will cause it to be interpreted as a literal string. This will not be evaluated as the `earliest` time-range parameter.

**Workaround**: Backtick the parameter name, e.g.: `` `earliest` ``. 

**Fix**: Tooltip clarified in Cribl Stream 4.0.

{{< id `CRIBL-8918` >}}

### 2022-03-23 – Upgrading v.3.3.1 Leader to v.3.4.0 blocks upgrade to Workers [CRIBL-8918]

**Problem**: When you explicitly upgrade a Leader Node to 3.4.0, you can't push upgrades to Workers.  

**Workaround**: Automatic upgrades work as expected. At **Settings** > **Global Settings** > **System > Upgrade**, set **Disable Automatic upgrades** to `No`. 

**Fix**: In 3.4.1.

{{< id `CRIBL-8807` >}}

### 2022-03-17 – All versions through 3.4.0 – Failed systemd restarts due to erroneous `ExecStopPost` command [CRIBL-8807]

**Problem**: On systemd, the Cribl service can stop with an unsuccessful return code. This was caused by a malformed `ExecStopPost` command in Cribl's provided `cribl.service` file.

**Workaround**: In `cribl.service`, delete the whole `ExecStopPost=...` line, and change the `Restart` parameter's value from `on-failure` to `always`. (The first deletion necessitates the second change.)

**Fix**: Versions 3.4.1 and later install a `cribl.service` file incorporating both the above changes. **If you are upgrading from v.3.4.0 or earlier, and you notice dead Workers, check and update your existing  `cribl.service` configuration to match the changes above.**

{{< id `CRIBL-8800` >}}

### 2022-03-17 – v.3.4.0 – Source PQ re-engages while draining, causing blockage/backpressure [CRIBL-8800]

**Problem**: A Source with Persistent Queueing enabled can mistakenly re-engage queueing while still draining its queue – leading to blocked throughput and backpressure. This occurs after a Destination goes offline and back online, once or more, leaving the Source in an undetermined queue state.

**Workaround**: Wait 60 seconds for another queue drain event, to see if PQ behavior goes back to normal. If the problem persists, restart Cribl Stream.

**Fix**: In Cribl Stream 3.4.1.

{{< id `CRIBL-8799` >}}

### 2022-03-17 – v.3.4.0 – Monitoring pages break without git [CRIBL-8799]

**Problem**: Unless git is installed locally, two Monitoring pages fail to display, with errors of the form `Versioning not supported`. The pages are **Monitoring** > **Overview** and **Monitoring** > **System** > **Licensing**. To resolve the errors, git must be installed even on single-instance deployments.

**Workaround**: [Install git](/stream/version-control#git-installation).

**Fix**: In 3.4.1.

{{< id `CRIBL-8784` >}}

### 2022-03-16 – v.3.2.2-3.4.0 – Upgrading from earlier versions degrades performance [CRIBL-8784]

**Problem**: After upgrading to any of the indicated versions, customers with active Splunk Destinations noticed that CPU load increased, triggering backpressure more readily.

**Workaround**: Skip these versions, or downgrade to a known well-behaved version.

**Fix**: In 3.4.1.

{{< id `CRIBL-8707` >}}
{{< id `CRIBL-8945` >}}

### 2022-03-11 – v.3.4.0 – In Free versions, Worker/Node UI access can't be accessed via UI [CRIBL-8707, CRIBL-8945]

**Problem**: In v.3.4, Cribl moved the Worker/Node **UI access** toggle from global ⚙️ **Settings** > **System** > **Distributed Settings** to per–Worker Group toggles on the **Manage Worker Groups** page. But with a Free license, these controls aren't displayed in the UI at all.

**Workaround**: Directly edit `groups.yml` to add the key-value pair `workerRemoteAccess: true` within each desired Worker Group.

**Fix**: In 3.4.1.

{{< id `CRIBL-8600` >}}

### 2022-03-08 – v.3.3.1 through Current – GitOps + License expiration = Catch-22 [CRIBL-8600]

**Problem**: If your Enterprise license expires while you have enabled the [GitOps Push workflow](/stream/gitops#ui-options), you will encounter the following block: Cribl Stream is in read-only mode, triggering a `Forbidden` error when you try to update your license key. But you also cannot reset the workflow from `Push` to `None`, because the expired license disables GitOps features.

**Workaround**: Contact Cribl Support for help updating your license.

**Fix**: Version TBD.

{{< id `CRIBL-8572` >}}

### 2022-03-07 – v.3.2.x-3.4.x – Undercounted `total.in_bytes` dimension metrics [CRIBL-8572]

**Problem**: The `cribl.logstream.total.in_bytes` dimension sent to Destinations can show a lower data volume (against license quota) than `cribl.logstream.index.in_bytes`. This latter `index.in_bytes` dimension displays the correct license usage, and matches what's reported on the **Monitoring** dashboard. 

**Workaround**: Within the `cribl_metrics_rollup` Pipeline that ships with Cribl Stream, disable the Rollup Metrics Function.

**Fix**: Couldn't reproduce the error.



{{< id `CRIBL-8515` >}}

### 2022-03-03 – v.3.3.x – Elasticsearch API Source (distributed mode) stops receiving data after upgrade to 3.3.x [CRIBL-8515]

**Problem**: After upgrading a distributed deployment to version 3.3.x, the ElasticSearch API Source might stop receiving data. This occurs when your existing Source config's **Authentication type** was set to `Auth Token`. The root cause is a 3.3.x schema change that unset this auth type to `None`.  

**Workaround**: In the Elasticsearch API Source configuration, reset the **Authentication type** to `Auth Token`. Then commit and deploy the restored config.

**Fix**: In Cribl Stream 3.4.

{{< id `CRIBL-8451` >}}

### 2022-02-24 – v.3.3.x – After upgrade to 3.3.0, ElasticSearch Destination can't write to indexes whose name includes `-` [CRIBL-8451]

**Problem**: After upgrade to version 3.3.x, LogStream cannot send data to Elasticsearch indexes whose name includes a hyphen (`‑`).

**Workaround**: In the Elasticsearch Destinations's **Index or Data Stream** field, surround these index names in "quotes" or backticks. 

**Fix**: LogStream 3.3.1 added format validation and error-checking.

{{< id `CRIBL-8361` >}}

### 2022-02-18 – v.3.3.x – Worker/Leader communications blocked by untrusted TLS certificate after upgrade to 3.3.0 [CRIBL-8361]

**Problem**: With TLS enabled on Worker/Leader communications, after an upgrade from LogStream 3.2.x to v.3.3.x, Workers might fail to communicate with the Leader due to an untrusted SSL certificate on the Leader. Logs will show `connection error`s of the form: `"unable to verify the first certificate"` or `self signed certificate in certificate chain`.

**Workaround**: On each affected Worker: In the `instance.yml` file's' `distributed:` `master:` `tls:` section, set: `"rejectUnauthorized: false"`. Then restart Cribl Stream.

**Fix**: In 3.4.1.

{{< id `CRIBL-8354` >}}

### 2022-02-17 – v.3.3.0 – Do not configure InfluxDB Destination on LogStream 3.3.0 [CRIBL-8354]

**Problem**: Configuring InfluxDB Destinations can fail on LogStream 3.3.0. This problem has been observed only on distributed deployments, and data does flow to InfluxDB. But the InfluxDB config will not appear in LogStream's UI, and the broken config will block editing of other Destinations.

**Workaround**: If you need to send data to InfluxDB, skip v.3.3.0, and wait for the next release. If you encounter the broken UI, edit `outputs.yml` to remove any `influxdb_output` sections, and then restart LogStream. This will unblock UI-based editing of other Destinations.

**Fix**: In LogStream 3.3.1.

{{< id `CRIBL-8352` >}}

### 2022-02-17 – v.3.2.2-3.3.x – REST Collector pagination fails on duplicate/nested attribute names [CRIBL-8352]

**Problem**: The [REST Collector](/stream/collectors-rest) does not consistently differentiate between multiple (nested) attributes that share the same name. This can break pagination that relies on a `Response Body Attribute`.

**Workaround**: If possible, select a different [Pagination](/stream/collectors-rest#pagination) method, or specify a unique attribute name.

**Fix**: In Cribl Stream 3.4.

{{< id `CRIBL-8346` >}}

### 2022-02-17 – v.3.3.x – Can't upgrade Workers to 3.3.x via UI [CRIBL-8346]

**Problem**: Attempting to upgrade on-prem Workers to version 3.3.x via the UI can fail, with an error of the form: `Group <group‑name> is a managed group`.

**Workaround**: Edit the Leader's `$CRIBL_HOME/local/cribl/groups.yml` file to delete all instances of the key-value pair `onPrem: false`. Then, resave the file and reload the Leader.

**Fix**: In Cribl Stream 3.4.

{{< id `CRIBL-8210` >}}

### 2022-02-10 – All versions, 3.2.1+ – Syslog Source's previewed data doesn't proceed through Routes [CRIBL-8210]

**Problem**: The Syslog Source's **General Settings** > **Input ID** field's default filter expression is `__inputId.startsWith('syslog:in_syslog:')`. The same filter appears in the **Preview Full** tab's **Entry Point** drop-down, except without the final colon. This means that data sent using **Preview Full**'s version of the filter will not go through Routes that use the **Input ID** version.

**Workaround**: When sending data from **Preview Full** to a Route, if both use the `syslog:in_syslog` filter, edit the Route's filter to remove the final colon. This will make the filter identical in both places, routing data correctly.

{{< id `CRIBL-8205` >}}

### 2022-02-10 – v.3.3.0-3.5.x – Slow-Mo Notification Notifications [CRIBL-8205]

**Problem**: After you [create a Notification](notifications#configuring-health) on a Destination, the new Notification might take up to several minutes to appear on the Destination config modal's **Notifications** tab.

**Details**: This delay has not been reproduced consistently. Some Notifications appear immediately.

**Fix**: In Cribl Stream 4.0.

{{< id `CRIBL-8132` >}}

### 2022-02-08 – v.3.3.x – Missing event from Datadog Agent v.7.33.0+ [CRIBL-8132]

**Problem**: If ingesting metrics from Datadog Agent 7.33.0 or later (i.e., you have set the `DD_DD_URL` environment variable or the `dd_url` YAML config key), LogStream's [Datadog Agent Source](sources-datadog-agent) will not ingest `agent_metadata` events. This is due to a breaking change in Datadog Agent 7.33.0, which split out part of the prior `/intake/` endpoint into a new `/api/v1/metadata` endpoint.

**Workaround**: Use Datadog Agent v.7.32.4 or earlier.

**Fix**: In Cribl Stream 3.4.

{{< id `CRIBL-8115` >}}

### 2022-02-08 – All versions through 3.3.x – Syslog and Metrics Sources' default filter expression needs correction [CRIBL-8115]

**Problem**: In the Syslog and Metrics Sources's **Logs** tab, the auto-generated filter expression (`channel =='input:in_syslog'`) is not specific enough and yields no results, because it doesn't address these Sources' two channels (`input:in_syslog:tcp` and `input:in_syslog:udp`). 

**Workaround**: Replace this default filter expression with `channel.startsWith('input:<input_ID>')`. For example: `channel.startsWith('input:in_syslog:')`.

**Fix**: In Cribl Stream 3.4.

{{< id `CRIBL-8105` >}}

### 2022-02-07 – v.3.2.2-3.4.0 – Okta integration fails after upgrading from v.3.1.3 to v.3.2.2 [CRIBL-8105]

**Problem**: Okta (OpenID Connect) authentication on Cribl Stream fails behind a proxy, when using environment variables (`http_proxy` or `https_proxy`).

**Workaround**: Configure the proxy to work in transparent mode, to bypass the `http*_proxy` variables. If the proxy is required, leave the `User Info URL` empty, and Cribl Stream can obtain user details from the `JWT token`.

**Fix**: In 3.4.1.

{{< id `CRIBL-8043` >}}

### 2022-02-04 – v.3.2.2-3.4.0 – Mask Function slows down post-processing Pipeline [CRIBL-8043]

**Problem**: A post-processing Pipeline with a Mask Function can slow down drastically, showing high CPU usage on Workers.

**Workaround**: If practical, promote the same Mask Function to a pre-processing or processing Pipeline. 

**Fix**: In Cribl Stream 3.4.1.

{{< id `CRIBL-8013` >}}

### 2022-2-03 – All versions though 3.4.0 – JSON Serialize Function in post-processing Pipeline won't process [CRIBL-8013]

**Problem**: A JSON Serialize Function in a post-processing Pipeline will throw errors of the form: `Failed to process event in Function: Maximum call stack size exceeded`.

**Workaround**: Promote the Serialize Function to a processing Pipeline, upstream from the Destination.

**Fix**: In 3.4.1.

{{< id `CRIBL-7998` >}}

### 2022-02-02 – v.3.0.0 through Current – Upgrading a Pack overwrites Lookup and sample-data customizations [CRIBL-7998]

**Problem**: Upgrading a [Pack](packs) overwrites any customizations you've made to the Pack's original/default Lookup tables and sample-data files.  

**Workaround**: Duplicate the Pack's default Lookup tables and sample-data files, and customize your duplicate copies. Upgrading the Pack will not overwrite your local copies.

**Fix**: Version TBD.

{{< id `CRIBL-7731` >}}

### 2022-01-18 – v.3.2.0-3.4.0 – Diagnostics > System Info page omits some entries [CRIBL-7731]

**Problem**: In current Cribl Stream versions, the **Diagnostics > System Info** page/tab omits certain statistics (`sysctl`, `ulimits`, `network stats`, etc.) that were previously displayed below the `cpus` and `nets` elements.

**Workaround**: The hidden information is nevertheless available within [diagnostic bundles](/stream/diagnosing).

**Fix**: In Cribl Stream 3.4.1. 

{{< id `CRIBL-7728` >}}

### 2022-01-18 – v.3.2.2 – Usernames containing certain letters cause some API requests to fail [CRIBL-7728]

**Problem**: Usernames containing certain letters can cause some API requests to fail with an `Invalid character in header content ["cribl-user"]` error. This can happen even when the username is valid for an Identity Provider.

**Workaround**: In your usernames, make sure that the letters you use are limited to the letters in Latin alphabet no. 1 as defined in the [ISO/IEC 8859-1](https://en.wikipedia.org/wiki/ISO/IEC_8859-1) standard.

**Fix**: In LogStream 3.3. 



{{< id `CRIBL-7638` >}}

### 2022-01-11 – v.3.2.2 – Git Collapse Actions disaggregates [CRIBL-7638]

**Problem**: With **Git Settings** > **Collapsed actions** enabled, the combined **Commit & Push** button appears in the modal as expected, but then saving a new change splits it back into two separate buttons: **Commit** and **Deploy**. 

**Workaround**: Disable the **Collapse actions** setting, then commit and deploy separately.

**Fix**: In LogStream 3.3.





{{< id `CRIBL-7513` >}}

### 2022-01-04 – v.3.2.x – Enabling GitOps deletes LogStream's `bin`, `logs`, and `pid`subdirectories [CRIBL-7513]

**Problem**: Enabling [GitOps](/stream/gitops) with LogStream 3.2.x, via either CLI or UI, can cause the deletion of the `bin`, `logs`, and `pid`subdirectories. This disables LogStream. The root cause is that `git` versions 2.13.1 and lower did not fully respect paths listed in `.gitignore`.

**Workaround**: Upgrade your `git` client to v.2.13.2 or higher, to correct the underlying behavior.

**Fix**: In LogStream 3.3.



{{< id `CRIBL-7445` >}}

### 2021-12-21 – v.3.2.2-4.0.x – Chain Function degrades CPU load and throughput [CRIBL-7445]

**Problem**: Chaining Pipelines via the Chain Function can increase CPU load, and can signficantly slow down data throughput.

**Workaround**: Consolidate all Functions – per processing scenario – into a single Pipeline.

**Fix**: In Cribl Stream 4.1.

{{< id `CRIBL-7364` >}}

### 2021-12-15 – v.3.2.1-3.2.2 – Backpressure when writing to S3 and other file-based Destinations [CRIBL-7364]

**Problem**: On file-based Destinations, enabling the **Remove staging dirs** toggle can trigger a race condition when inbound events per second reach a high rate. This leads to backpressure.

**Workaround**: Disable LogStream's native **Remove staging dirs** option. Instead, as the user running LogStream, set up a cron job like this:  
`crontab -e`  
`0 1 * * * find <stagingdir> -type d -empty -mtime +1 -delete`

**Fix**: In LogStream 3.3.

{{< id `CRIBL-7347` >}}

### 2021-12-13 – v.2.4.4-3.1.1 – Upgrades via UI delete sample files [CRIBL-7347]

**Problem**: Using the UI to upgrade LogStream versions prior to 3.1.2 will silently delete sample data files created by users. This affects using a Leader's UI (any version) to upgrade Workers running LogStream version 3.1.1 or earlier. It also affects using the UI to upgrade the Leader itself, or a Single-Instance deployment, from v.3.1.1 or earlier to any newer version.

**Workarounds**: 1. Upgrade pre-3.1.2 versions [via the filesystem](/stream/upgrading#distributed). 2. If you choose to upgrade pre-3.1.2 versions via the UI, first [save to the filesystem](/stream/data-preview#save-submenu) any custom sample files that you want to preserve. (Or, to copy directly from the filesystem, LogStream stores sample files internally at `$CRIBL_HOME/data/samples`, and stores an index of all sample files in `/$CRIBL_HOME/local/cribl/samples.yml`.)

**Fix**: Does not affect Workers running Cribl Stream (LogStream) 3.1.2 or higher. Does not affect self-upgrades of Leaders or Single Instances running v.3.1.2 or higher.

### 2021-12-10 – All Versions – API Reference is temporarily unstyled

**Problem**: Our documentation's [API Reference](/api-reference) has temporarily lost some visual styling, while we swap in a new rendering component.

**Workaround**: Manually expand accordions, as needed.

**Fix**: Published as of 12/20/2021.

{{< id `CRIBL-7293` >}}

### 2021-12-09 – v.3.2.1 – File-based Destinations display an unavailable Persistent Queue option [CRIBL-7293]

**Problem**: File-based Destinations' **Backpressure behavior** drop-down misleadingly displays a `Persistent Queue` option, which is not really available on these Destinations. (Applies to Filesystem and some Amazon, Azure, and Google Cloud Destinations.) Selecting this option displays an error, and the configuration cannot be saved.

**Workaround**: Don't fall for clicking that fake `Persistent Queue` option.

**Fix**: In LogStream 3.2.2.



### 2021-12-07 – v.3.2.1 – System Metrics Source's documentation doesn't open in Help drawer

**Problem**: After clicking the System Metrics Source's Help link, the drawer displays the error: "Unable to load docs. Please check LogStream's online documentation instead." This Source's documentation is also missing from the 3.2.1 docs PDF.

**Workaround**: Go to the live [System Metrics](sources-system-metrics) docs page.

**Fix**: In LogStream 3.2.1, as duplicate of CRIBL-6319.

{{< id `CRIBL-7268` >}}

### 2021-12-07 – v.3.1.2-3.1.3 – Increasing CPU load over time [CRIBL-7268]

**Problem**: CPU load increases over time, with a slow increase in memory usage. Restarting LogStream temporarily resolves these symptoms. The root cause is needlessly collecting a high volume of full-fidelity metrics from the CriblMetrics source. 

**Workaround**: Disable the CriblMetrics Source.

**Fix**: Upgrade to LogStream 3.2.1, which provides an option to disable the CriblMetrics Source's **Full Fidelity** toggle.

{{< id `CRIBL-7113` >}}

### 2021-11-24 – v.3.2.0 through Current – With processing Pipelines in QuickConnect, Preview Full doesn't show Functions' results [CRIBL-7113]

**Problem**: If a processing Pipeline has been inserted between a QuickConnect Source and Destination, selecting the **Pipelines** page, and then selecting **Preview Full** with a data sample, doesn't show the Pipeline's effects on the **OUT** tab. This affects only Pipelines inserted in a QuickConnect connection line. (Attaching a pre-processing Pipeline to a QuickConnect Source doesn't inhibit Preview.)

**Workarounds**: 1. Remove the Pipeline from QuickConnect, and re-create the same Source ‑> Pipeline ‑> Destination architecture under **Routing** > **Data Routes**. 2. Rely on **Preview Simple**.

**Fix**: Version TBD.

{{< id `CRIBL-7109` >}}

### 2021-11-24 – v3.2.0 – Data from Collectors and Collector-based Sources isn't reaching Routes [CRIBL-7109]

**Problem**: Routes are not recognizing data from [Collectors](/stream/collectors) and from the following Collector-based [Sources](sources): Prometheus Scraper, Office 365 Activity, Office 365 Services, and Office 365 Message Trace. This data will not flow through Routes, but **will** be sent to the default Destination(s).

**Workarounds**: 1. In Collectors' config modals, open the **Result Routing** tab, Disable the **Send to Routes** default, and directly specify a **Pipeline** and **Destination**. (This option is not available in Prometheus Scraper or in the Office 365 Sources.) 2. Skip v.3.2.0.

**Fix**: In LogStream 3.2.1.

{{< id `CRIBL-7053` >}}

### 2021-11-17 – v.2.1.0 through Current – Re-enabling a Function group mistakenly re-enables all its Functions [CRIBL-7053]

**Problem**: When you change a Function group from disabled to enabled, all of its Functions are enabled, regardless of their individual enabled/disabled states when the group was disabled.

**Workaround**: Avoid disabling and re-enabling Functions as a group (e.g., for testing or stepwise debugging purposes).

**Fix**: Version TBD.

{{< id `CRIBL-7049` >}}

### 2021-11-17 – v.3.2.0 – TLS certs that use passphrases won't decrypt private keys [CRIBL-7049]

**Problem**: Sources whose TLS config uses a (known good) passphrase will fail to decrypt private keys. You will see a connect error, or an error of the form: `TLS validation error, is passphrase correct?`

**Workarounds**: 1. Edit the Leader's or single instance's `inputs.yml` file to insert a plaintext TLS passphrase. (See paths [here](configuration-files).) Then commit and deploy the new config (distributed mode), or reload or restart the LogStream server (single instance). 2. Use a cert and key that do not require a passphrase. 3. Skip v.3.2.0.

**Fix**: In LogStream 3.2.1.

{{< id `CRIBL-7044` >}}

### 2021-11-17 – v.3.2.0 – Only one Chain Function works per Pipeline [CRIBL-7044]

**Problem**: If you add more than one Chain Function to a Pipeline, only the first will take effect. Chain Functions lower in the stack will simply pass the data down to the next Function.

**Workaround**: Design your data flow to require at most one Chain Function per Pipeline.

**Fix**: In 3.2.1.

{{< id `CRIBL-7043` >}}

### 2021-11-17 – v.3.2.0 – Exporting a Pack to another Group requires a Leader restart [CRIBL-7043]

**Problem**: Exporting a Pack to a different Worker Group via the UI succeeds, but opening the Pack on the target Group fails with a `Cannot read property...undefined` error.

**Workaround**: To resolve the error and make the target Pack accessible, restart the Leader. To prevent the error, export and import the Pack as a file.

**Fix**: In LogStream 3.2.1.

{{< id `CRIBL-7013` >}}
{{< id `CRIBL-7047` >}}

### 2021-11-16 – v.3.2.0 – QuickConnected Source goes to wrong Destination [CRIBL-7013, CRIBL-7047]

**Problem**: Some QuickConnect connections send data to an unintended Destination. We've observed this in single-instance deployments that include the Cribl-supplied `cribl_metrics_rollup` Pipeline, or include other Pipelines/Packs with stateful Functions like Rollup Metrics or Aggregations. Data will either flow through Routes instead of your specified QuickConnect Destination, or will continue flowing to the original QuickConnect Destination after you drag the connection to a different Destination.

**Workaround**: Use the **Data Routes** interface to manage the Pipeline and stateful Functions indicated above. If your QuickConnect data doesn't oblige a changed QuickConnect Destination, restart LogStream. This will stop data flow to the unintended Destination, and redirect it to the intended Destination.

**Fix**: In Cribl Stream 3.2.1.

{{< id `CRIBL-6961` >}}

### 2021-11-10 – v.3.2.0 – GitOps `Push` workflow displayed unsupported Revert button [CRIBL-6961]

**Problem**: When a GitOps `Push` workflow has placed the UI in read-only mode, the Commit UI displayed a **Revert** button, even though reverting changes was not supported. Pressing the button simply triggered an error message.

**Fix**: In Cribl Stream/LogStream 3.2.1.



{{< id `CRIBL-6674` >}}

### 2021-10-22 – v.3.2.x through Current – Kafka with Kerberos causes Workers' continuous restart [CRIBL-6674]

**Problem**: Configuring the Kafka Source or Destination with Kerberos authentication can send LogStream Workers into a continuous loop of restarts.

**Workaround (multi-step)**:  
1. In `krb5.conf`, set `dns_lookup_realm = false` and `dns_lookup_kdc = false`.  
2. From `krb5.conf`'s `[realms]` section, extract the realm name (from the element name) and the FQDN (from the `kdc` key's value, stripping any port number).  
3. Run `nslookup` on either or both of the realm name and the FQDN. E.g.: `nslookup mysubdomain.confluent.io` and `nslookup kdc.kerberos-123.local`.  
4. Add at least one of the returned IP addresses (which might be identical) to the `etc/hosts` file, as values in the format your platform expects.

**Fix**: Version TBD.

{{< id `CRIBL-6440` >}}

### 2021-10-11 – v.2.x-3.2.1 – Collectors do not filter correctly by date/time range [CRIBL-6440]

**Problem**: Collectors can retrieve data from undesired time ranges, instead of from the range specified. If paths are organized by date/time, this can also cause Collectors to retrieve data from the wrong paths. The root cause is that in the Collector's Run and Schedule configuration modals, time ranges are set based on the browser's time zone, whereas LogStream's backend assumes UTC.

**Workaround**: Offset the **Earliest** and **Latest** date/time values based on your browser's offset from UTC.

**Fix**: In Cribl Stream/LogStream 3.2.2 and later, the Run and Schedule modals provide a **Range Timezone** drop-down to prevent these errors.

{{< id `CRIBL-6412` >}}

### 2021-10-06 – v.3.1.2 – Monitoring page omits Collector Sources' data [CRIBL-6412]

**Problem**: With functioning Office 365 Activity and Office 365 Services Sources, the Job Inspector reports data being retrieved, and **Monitoring** > **Data** > **Destinations** reports data being sent out. However, **Monitoring** > **Data** > **Sources** falsely reports no data being received. The problem is isolated to this **Sources** page. It might also affect the Office 365 Message Trace and Prometheus Scraper Sources.

**Workaround**: Use the Source modal's **Live Data** tab, the **Monitoring** > **System** > **Job Inspector** page, and/or the **Monitoring** > **Data** > **Destinations** page to monitor throughput.

**Fix**: In LogStream 3.1.3.

{{< id `CRIBL-6319` >}}

### 2021-09-30 – v.3.1.2 – High CPU load with CriblMetrics Source [CRIBL-6319]

**Problem**: Enabling the CriblMetrics Source causes high event count and high CPU load.

**Workaround**: Adjust the `cribl_metrics_rollup`
pre-processing Pipeline to roll up a wider **Time Window** of metrics.

**Fix**: Upgrade to LogStream 3.2.1, which provides an option to disable the CriblMetrics Source's **Full Fidelity** toggle. 

{{< id `CRIBL-6310` >}}

### 2021-09-29 – v.3.0.2-3.1.2 – Memory leak with multiple Collectors [CRIBL-6310]

**Problem**: Configuring multiple Collectors can lead to gradual but cumulative memory leaks. Due to a caching error, the memory can be recovered only by restarting LogStream.

**Workaround**: Restart LogStream on the affected Worker Nodes.

**Fix**: In LogStream 3.1.3.

{{< id `CRIBL-6236` >}}

### 2021-09-21 – v.3.1.1 – Azure Blob Storage Source/Destination authentication error on upgrade from v.3.0.4 [CRIBL-6236]

**Problem**: Upon upgrading the Azure Blob Source and/or Destination from v.3.0.4 to v.3.1.1, you might receive an error of the form: `Unable to extract accountName with provided information.`

**Workaround**: Change the key, and then reset it back to your desired connection string.

**Fix**: Not observed as of Cribl LogStream 3.1.1.

{{< id `CRIBL-6182` >}}

### 2021-09-15 – v.3.0.0–3.1.3 – High CPU usage with Google Cloud Pub/Sub Source [CRIBL-6182]

**Problem**: The Google Cloud Pub/Sub Source substantially increases CPU usage, which stays high even after data stops flowing. This causes throughput degradation, and more-frequent `failed to acknowledge` errors.

**Workaround**: Configure the Google Cloud Pub/Sub Source's **Advanced Settings** > **Max backlog** to `1000`. 

**Fix**: In 3.2.0, which defaults the **Max backlog** to `1000`, and also relaxes the retry interval from 10 seconds to 30 seconds.

{{< id `CRIBL-6166` >}}

### 2021-09-14 – v.3.1.1 – Modifying Collector > Preview > Capture settings can break Capture elsewhere [CRIBL-6166]

**Problem**: Modifying the capture settings in a Collector's Run > Preview > Capture modal can improperly modify the Filter expression in Capture modals for other Collectors, Sources, and Routes/Pipelines.

**Fix**: In LogStream 3.1.2.

{{< id `CRIBL-6142` >}}

### 2021-09-10 – v.3.1.1 – Worker Group certificate name drop-down shows certificates from the Leader [CRIBL-6142]

**Problem**: When configuring certificates at **Groups** > `<group‑name>` > **Settings** > **API Server Settings** > **TLS**, certificates configured on the Leader incorrectly appear on the **Certificate name** drop-down.

**Fix**: In LogStream 3.1.2.

{{< id `CRIBL-6134` >}}

### 2021-09-10 – v.3.1.1 – Git Collapsed Actions broken in 3.1.1 [CRIBL-6134]

**Problem**: With **Collapsed Actions** enabled, clicking the **Commit & Push** button has no effect. (The **Commit & Deploy** button works properly.)

**Workaround**: Disable **Collapsed Actions**, to restore separate **Commit** and **Git Push** buttons.

**Fix**: In LogStream 3.1.2.

{{< id `CRIBL-6106` >}}



{{< id `CRIBL-6106` >}}
{{< id `CRIBL-6115` >}}

### 2021-09-08 – All versions through v.3.1.1 – UDP support is currently IPv4-only [CRIBL-6106, CRIBL-6115]

**Problem**: Where Sources and Destinations connect over UDP, they currently support IPv4 only, not IPv6. This applies to Syslog, Metrics, and SNMP Trap Sources; and to Syslog, SNMP Trap, StatsD, StatsD Extended, and Graphite Destinations.

**Workaround**: Integrate via IPv4 if possible.

**Fix**: UDP Sources and Destinations gained IPv6 support in LogStream 3.1.2.

{{< id `CRIBL-6086` >}}

### 2021-09-02 – v.3.1.0-3.1.1 – Google Pub/Sub authentication via proxy environment variable fails [CRIBL-6086]

**Problem**: When LogStream's Google Cloud Pub/Sub Source and Destination attempt authentication through a proxy, using the `https_proxy` environment variable, they send an HTTP request to http://www.googleapis.com:443/oauth2/v4/token. This request fails with 504/502 errors. The root cause is a mismatch in dependency libraries, whose correction has been identified, but requires broader testing.

**Workaround**: Configure the proxy in `transparent` mode, to avoid relying on environment variables.

**Fix**: In 3.1.2.

{{< id `CRIBL-6039` >}}
{{< id `CRIBL-6044` >}}

### 2021-08-24 – v.3.1.0 – High CPU load with LogStream version 3.1.0 [CRIBL-6039, CRIBL-6044]

**Problem**: LogStream 3.1.0 added code-execution safeguards that inadvertently increased CPU load, and decreased throughput, with several Functions and most expressions.

**Workaround**: Downgrade to version 3.0.4. 

**Fix**: Upgrade to version 3.1.1 or later.

{{< id `CRIBL-5977` >}}

### 2021-08-18 – v.3.1.0 – Clone Collector option broken [CRIBL-5977]

**Problem**: Clicking a 3.1.0 Collector modal's **Clone Collector** button simply closes the modal. (If you have unsaved changes, you'll first be challenged to confirm closing the parent modal – but the expected cloned modal won't open.)

**Workaround**: Click **+ Add New** to re-create your original Collector's config from scratch, adding any desired modifications.

**Fix**: In LogStream 3.1.1.

{{< id `CRIBL-5972` >}}

### 2021-08-18 – All versions through 3.1.0 – Tabbed code blocks broken on in-app docs [CRIBL-5972]

**Problem**: When docs with tabbed code blocks are opened in the Help drawer, the default (leftmost) tab seizes focus. Other tabs will not display when clicked.

**Workaround**: Click the blue/linked page title atop the Help drawer to open the same page on [docs.cribl.io](https://docs.cribl.io/), where all tabs can be selected.

**Fix**: In LogStream 3.1.1.

{{< id `CRIBL-5940` >}}

### 2021-08-14 – v.3.1.0 – Splunk Load Balanced Destination does not migrate auth type [CRIBL-5940]

**Problem**: In a Splunk Load Balanced Destination with **Indexer discovery** enabled and a corresponding **Auth token** defined, upgrading to LogStream 3.1.0 corrupts the **Auth token** field's value.

**Workaround**: Set the **Authentication method** to **Manual** and resave the token's value.

**Fix**: In 3.1.1.

{{< id `CRIBL-5926` >}}

### 2021-08-11 – v.3.1.0 – `C.Secret()` values are undefined in Collectors [CRIBL-5926]

**Problem**: Calling the [C.Secret()](/stream/3.1/cribl-reference#secret) internal method within a [Collector](/stream/collectors) field resolves incorrectly to an `undefined` substring. E.g., in URL fields, `C.Secret()` values will resolve to `/undefined/` path substrings.

**Workarounds**: 1. Use `C.vars` and a Global Variable, instead of using this method. 2. Root cause is that `C.Secret()` in Collectors and Pipeline Functions has access only to secrets that were created before the last restart. Therefore, restart Worker Processes to refresh the method's access.

**Fix**: In 3.1.1.

{{< id `CRIBL-5909` >}}

### 2021-08-10 – v.3.1.0 – Pre-processing Pipelines break Flows display [CRIBL-5909]

**Problem**: Attaching a pre-processing Pipeline to a Source breaks the **Monitoring** > **Flows (beta)** page's display. Attempting to remove Sources/Destinations from that page's selectors throws a cryptic `Sankey` error.

**Workaround**: Temporarily detach pre-processing Pipelines if you want to check Flows.

**Fix**: Upgrade to version 3.1.1 or later.

{{< id `CRIBL-5774` >}}

### 2021-07-29 – v.3.0.2-3.0.4 – Upgrades via UI require broader permissions [CRIBL-5774]

**Problem**: Upgrading from v.3.0.x via the UI requires the `cribl` user to be granted write permission on the parent directory above `$CRIBL_HOME`. The symptom is an error message of the form: `Upgrade failed: EACCES: permission denied, mkdir '/opt/unpack.xxxxxxx.tmp'`.

**Workaround**: Either adjust permissions, or upgrade via the filesystem. For complete instructions, see [Upgrading](/stream/3.0.4/upgrading#CRIBL-5774).

**Fix**: Does not affect LogStream 3.1 or higher.

{{< id `CRIBL-5738` >}}

### 2021-07-26 – v.3.0.x–3.1.0 – Packs with orphaned lookups block access to Worker Groups [CRIBL-5738]

**Problem**: If a Pack references a lookup file that's missing from the Pack, pushing the Pack to a Worker Group will block access to the Group's UI. You will see an error message of the form: "The Config Helper service is not available because a configuration file doesn't exist... Please fix it and restart LogStream."

**Workaround**: On the Leader Node, review the config helper logs (`$CRIBL_HOME/log/groups/<group>/*.log`) to see which references are broken. (In a single-instance deployment, see `$CRIBL_HOME/log/*.log`.) Then manually resolve these references in the Pack's configuration.

**Fix**: Upgrade to version 3.1.1 or later.

{{< id `CRIBL-5706` >}}

### 2021-07-20 – v.3.0.3-3.4.0 – Can't add Functions to a Pipeline named `config` [CRIBL-5706]

**Problem**: You cannot add Functions to a Pipeline if the Pipeline is named `config`, because this name conflicts with the reserved route for the **Create Pipeline** dialog.

**Workarounds**: Don'tcha name your Pipelines `config`.

**Fix**: In Cribl Stream 3.4.1.

{{< id `CRIBL-5611` >}}

### 2021-07-06 – All versions through Current – Duplicate Workers/​Worker GUIDs [CRIBL-5611]

**Problem**: Multiple Workers have identical GUIDs. This creates problems in [Monitoring](/stream/monitoring), [upgrading](upgrading) and versioning, etc., because all Workers show up as one.

**Cause**: This is caused by configuring one Worker and then copying its `cribl/` directory to other Workers, to quickly bootstrap a deployment.

**Workaround**: Don't do this! Instead, use the [Bootstrap Workers from Leader](/stream/deploy-workers) endpoint.

**Fix**: Version TBD.

{{< id `CRIBL-5595` >}}

### 2021-07-02 – v.3.0.2–3.0.3 – Sample file's last line not displayed upon upload [CRIBL-5595]

**Problem**: When uploading (attaching) a sample data file, the file's final line is not displayed in the **Add Sample Data** modal.

**Workarounds**: This is a UI bug only. LogStream correctly processes the complete sample data, which should show up when viewing the sample afterwards (e.g., within a Pipeline's preview pane).

**Fix**: In LogStream 3.1.0.

{{< id `CRIBL-5594` >}}

### 2021-07-02 – All versions through 3.0.x – Date fields misleadingly preview with string symbol [CRIBL-5594]

**Problem**: In [Preview or Capture](data-preview), incoming events like `_raw` will be displayed in the right pane with an `α` symbol that indicates string data. However, calling `new Date()` and then `C.Time.strptime()` [methods](cribl-reference#time) in an [Eval](eval-function) Function will return `null` on the OUT tab.

**Cause**: Due to the nature of JSON serialization, the incoming event's `Date` field is misleadingly subsumed under the event's `α` string symbol. It's actually a structured type, not a string...yet.

**Workaround**: If you see unexpected `null` results, stringify the datetime field as you extract it, e.g.: `new Date().toISOString()`. Feeding the resulting field to Time methods should return datetime strings as expected.

**Fix**: In LogStream 3.1.0.

{{< id `CRIBL-5534` >}}

### 2021-06-22 – v.3.0.0–3.0.3 – Internal `C.Text.relativeEntropy()` method – broken typeahead and preview [CRIBL-5534]

**Problem**: The `C.Text.relativeEntropy()` internal method is missing from JavaScript expressions' typeahead drop-downs. You can manually type or paste in the method, and save your Function and Pipeline, but LogStream's right Preview pane will (misleadingly) always show the method returning `0`.

**Workarounds**: Use other means (such as the **Live** button) to preview and verify that the method is (in fact) returning valid results.

**Fix**: In LogStream 3.1.0.

{{< id `CRIBL-5311` >}}
{{< id `CRIBL-5766` >}}

### 2021-05-20 – v.3.0.0 – Multiple Functions Break LogStream 3.0 Pipelines [CRIBL-5311, CRIBL-5766]

**Problem**: After upgrade to LogStream 3.0.0, including any of the following Functions in a Pipeline can break the Pipeline: GeoIP, Redis, DNS Lookup, Reverse DNS, Tee. Symptom is an error of the form: `Pipeline process timeout has occurred.` Less seriously, including these Functions in a Pipeline can suppress Preview's display of fields/values.

**Workarounds**: If you use these Functions in your Pipelines, stay with (or restore) a pre-3.0 version until LogStream 3.0.1 is available.

**Fix**: CRIBL-5311 fixed in LogStream 3.0.1. CRIBL-5766 fixed in LogStream 3.2.1.



### 2021-05-19 – v.3.0.0 – Leader's Changes fly-out stays open after Commit

**Problem**: In the Leader's left nav, the Changes fly-out remains stuck open after you commit pending changes.

**Workarounds**: Hover or click away. Then hover or click back to reopen the fly-out.

**Fix**: In LogStream 3.0.1.



### 2021-05-18 – v.3.0.0 – Packs > Export in "Merge" mode omits schemas and custom Functions

**Problem**: [Exporting a Pack](packs#export) with the export mode set to `Merge` omits schemas and custom Functions configured within the Pack's **Knowledge > Schemas**.

**Workarounds**:  1. Change the export mode to `Merge safe`, and export again. 2. If that doesn't preserve the schema and Functions, revert to `Merge` export mode; install the resulting Pack onto its target(s); and then manually copy/paste the schema(s) and Functions from the source Pack's UI to the target Pack's UI.

**Fix**: In LogStream 3.0.1.

{{< id `CRIBL-5274` >}}

### 2021-05-17 – v.3.0.0 through Current – Can't Enable KMS on Worker Group after installing license

**Problem**: Enabling HashiCorp Vault or AWS KMS on a Worker Group, after installing a LogStream license on the same Group, fails with a spurious `External KMS is prohibited by the current license` error message.

**Workaround**: On the Leader, navigate to **Settings** > **Worker Processes**. Restart the affected Worker Group's `CONFIG_HELPER` process. Then return to that Worker Group's **Security** > **KMS** Settings, re-enter the same KMS configuration, and save.

**Fix**: Version TBD.



### 2021-05-10 – v.2.4.5-3.0.3 – Elasticsearch Destination, with Auto version discovery, doesn't send Authorization header

**Problem**: When the Elasticsearch Destination has Basic Authentication enabled, and its **Elastic version** field specifies `Auto` version discovery, LogStream fails to send the configured username and password credentials along with its API initial request. Elasticsearch responds with an HTTP 401 error.

**Workaround**: Explicitly set the **Elastic version** to either `7.x` or `6.x` (depending on your Elasticsearch cluster's version); then stop and restart LogStream to pick up this configuration change.

**Fix**: In LogStream 3.1.0.

{{< id `CRIBL-5116` >}}

{{< id `CRIBL-5249` >}}

### 2021-05-04 – v.2.4.5-3.0.1 – Office 365 Message Trace Source skips events

**Problem**: The [Event Breaker](event-breakers) Rule provided for the [Office 365 Message Trace](/stream/sources-office365-msg-trace) Source mistakenly presets the **Default timezone** to `ETC/GMT‑0`. This setting causes LogStream to discover events but not collect them.

**Workaround**: Reset the Rule's **Default timezone** to `UTC`, then click **OK** and resave the Ruleset.

**Fix**: In 3.0.2.

{{< id `CRIBL-5102` >}}

### 2021-05-03 – v.2.4.4–3.0.1 – Rollup Function suppresses `sourcetype` metrics 

**Problem**: `sourcetype` metrics can be suppressed when the Cribl Internal > CriblMetrics Source is enabled and the `cribl_metrics_rollup` pre-processing Pipeline is attached to a Source.

**Workarounds**: Disabling the pre-processing pipeline restores `sourcetype` and any other missing data. However, without the rollup, a much higher data volume will be sent to the indexing tier.

**Fix**: In LogStream 3.0.2.



### 2021-04-20 – v.2.4.3–2.4.5 – Orphaned S3 staging directories

**Problem**: Using the S3 Destination, defining a partitioning expression with high cardinality can proliferate a large number (up to millions) of empty directories. This is because LogStream cleans up staged files, but not staging directories.

**Workaround**: Programmatically or manually delete stale staging directories (e.g., those older than 30 days).

**Fix**: In LogStream 3.0.2.



### 2021-04-12 – v.2.4.4-3.0.0 – Splunk Sources do not support multiple-metric events

**Problem**: LogStream's Splunk Sources do not support multiple-measurement metric data points. (LogStream's Splunk Load Balanced Destination does.)

**Fix**: In LogStream 3.0.1.



### 2021-04-07 – v.2.4.2–2.4.5 – Google Cloud Storage Destination fails to upload files > 5 MB

**Problem**: The Google Cloud Storage Destination might fail to put objects into GCS buckets. This happens with files larger than 5 MB, and causes the Google Cloud API to report a vague `Invalid argument` error.

**Workaround**: Set the **Max file size (MB)** to 5 MB. Also, reduce the **Max file open time (sec)** limit from its default `300` (5 minutes) to a shorter interval, to prevent files from growing to the 5 MB threshold. (Tune this limit based on your observed rate of traffic flow through the Destination.)

**Fix**: In LogStream 3.0.0.



### 2021-03-31 – v.2.4.4-2.4.5 – Local login option visible even when disabled

**Problem**: The **Log in with local user** option is displayed to users even when you have disabled **Settings > Authentication > Allow local auth** for an OpenID Connect identity provider.

**Workaround**: Advise users to ignore this button. Although visible, it will not function.

**Fix**: In LogStream 3.0.0.



### 2021-03-31 – v.2.4.0–2.4.4 – Splunk TCP and LB Destinations' Workers trigger OOM errors and restart

**Problem**: With a Splunk TCP or Splunk Load Balanced Destination created after upgrading to LogStream 2.4.x, Workers' memory consumption may grow without bound, leading to out-of-memory errors. The API Process will restart the Workers, but there might be temporary outages.

**Workaround**: Toggle the Destination's **Advanced Settings > Minimize in‑flight data loss** slider to `No`. This will preserve Processes killed by OOM conditions.

**Fix**: In LogStream 2.4.5.



### 2021-03-31 – v.2.4.4-2.4.5 – OpenID Connect authentication always shows local-auth fallback

**Problem**: Even if OpenID Connect external authentication is [configured](/stream/authentication#sso) to disable **Allow local auth**, LogStream's login page displays a **Log in with local user** button.

**Workaround**: Do not click that button.

**Fix**: In LogStream 3.0.0.



### 2021-03-31 – v.2.4.4 – Authentication options mistakenly display Cribl Cloud

**Problem**: The **Settings > Authentication > Type** drop-down offers a `Cribl Cloud` option, which is not currently functional. Attempting to configure and save this option could lock the `admin` user out of LogStream.

**Workaround**: Do not select, configure, or save that option.

**Fix**: In LogStream 2.4.5.



### 2021-03-30 – v.2.4.4 – Can't disable some Sources from within their config modals

**Problem**: In configuration modals for the Azure Blob Storage and Office 365 Message Trace Sources, the **Enabled** slider cannot be toggled off, and its tooltip doesn't appear.

**Workaround**: Disable your configured Source (where required) from the **Manage Blob Storage Sources** or the **Manage Message Trace Sources** page.

**Fix**: In LogStream 2.4.5.



### 2021-03-29 – v.2.4.x – SpaceOut Destination is broken

**Problem**: Within the SpaceOut game, you cannot shoot, and your player is immortal.

**Workaround**: There are other video games. After we defeat COVID, you'll even be able to buy a PS5.

**Fix**: Restored in LogStream 2.4.5.



### 2021-03-24 – v.2.4.x – Cribl App for Splunk blocks admin password changes, configuration changes, and Splunk-based authentication

**Problem**: Attempting to change the admin password via the UI triggers a 403/Forbidden message. You can reset the password by [editing `users.json`](/stream/authentication#manual-password-replacement), but can't save configuration changes to Settings, Pipelines, etc., because RBAC Roles are not properly applied.

**Workaround**: Using a [2.3.x version](https://cribl.io/download/logstream-past-releases/) of the App enables **local** authentication and enables changes to Cribl/LogStream passwords and configuration/settings. 

**Fix**: In LogStream 2.4.4.



### 2021-03-22 – v.1.7.0-2.4.3 – Azure Event Hubs Destination: Compression must be manually disabled

**Problem**: LogStream's [Azure Event Hubs](destinations-azure-event-hubs) Destination provides a **Compression** option that defaults to `Gzip`. However, compressed Kafka messages are not yet supported on Azure Event Hubs.

**Workaround**: Manually reset **Compression** to `None`, then resave Azure Event Hubs Destinations.

**Fix**: In LogStream 2.4.4.



### 2021-03-17 – v.2.4.2-2.4.3 – Parser Function > List of Fields copy/paste fails

**Problem**: When copying/pasting **List of Fields** contents between Parser Functions via the Copy button, the paste operation inserts unintended metadata instead of the original field references.

**Workaround**: Manually re-enter the second Parser Function's **List of Fields**.

**Fix**: In LogStream 2.4.4.



### 2021-03-13 – v.2.4.3 – UI can't find valid TLS .key files, blocking Master restarts and Worker reconfiguration

**Problem**: After upgrading to v.2.4.3, the UI fails to recognize valid TLS `.key` files, displaying spurious error messages of the form: 
"File does not exist: `$CRIBL_HOME/local/cribl/auth/certs/<keyname>key`." 
An affected Master will not restart. Affected Workers will restart, but will not apply changes made through the UI.

**Workaround**: Ideally, specify an absolute path to each key file, rather than relying on environment variables. If you're locked out of the UI, you'll need to manually edit the referenced paths within these configuration files in LogStream subdirectories: `local/cribl/cribl.yml` (General > API Server TLS settings) and/or `local/_system/instance.yml` (Distributed > TLS settings). Contact Cribl Support if you need assistance. A more drastic workaround is to disable TLS for the affected connections.

**Fix**: In LogStream 2.4.4.



### 2021-03-12 – v.2.4.2-2.4.5 – Redis Function with specific username can't authenticate against Redis 6.x ACLs

**Problem**: The [Redis](redis-function) Function, when used with a specific username and Redis 6.x's [Access Control List](https://redis.io/topics/acl) feature, fails due to authentication problems.

**Workaround**: In the Function's **Redis URL** field, point to the Redis `default` account, either with a password (e.g., `redis://default:Password1@192.168.1.20:6379`) or with no password (redis://192.168.1.20:6379). Do not specify a user other than `default`.

**Fix**: In LogStream 3.0.



### 2021-03-09 – v.2.4.3 – Splunk Destinations' in-app docs mismatch UI's current field order

**Problem**: For the Splunk Single Instance and Splunk Load Balanced Destinations, the in-app documentation omits the UI's **Advanced Settings** section. Some fields are documented out-of-sequence, or are omitted.

**Workaround**: Refer to the UI's tooltips, to the corrected [Splunk Single Instance](destinations-splunk) and [Splunk Load Balanced](destinations-splunk-lb) online docs, and/or to the corrected [PDF](https://cdn.cribl.io/dl/2.4.3/cribl-docs-2.4.3-20210310054833.pdf).

**Fix**: In LogStream 2.4.4.



### 2021-03-08 – v.2.4.3 – Enabling Git Collapse Actions breaks Commit & Deploy

**Problem**: After enabling **Settings > Distributed Settings > Git Settings > General > Collapse Actions**,  selecting **Commit & Deploy** throws a 500 error.

**Workaround**: Disable the **Collapse Actions** setting, then commit and deploy separately.

**Fix**: In LogStream 2.4.4.



### 2021-03-08 – v.2.4.3 – S3 Collector lacks options to reuse HTTP connections and allow-self signed certs

**Problem**: As of v.2.4.3, LogStream's AWS-related Sources & Destinations provide options to reuse HTTP connections, and to establish TLS connections to servers with self-signed certificates. However, the S3 Collector does not yet provide these options.



**Fix**: In LogStream 2.4.4.



### 2021-03-04 – v.2.4.2 – Esc key closes both Event Breaker Ruleset modals

**Problem**: After adding a rule to a **Knowledge > Event Breaker Ruleset**, pressing `Esc` closes the parent Ruleset modal along with the child Rule modal.

**Workaround**: Close the Rule modal by clicking either its **Cancel** button or its close box.

**Fix**: In LogStream 2.4.3.



### 2021-03-04 – v.2.4.2 – Aggregations Function in post-processing Pipeline addresses wrong Destination

**Problem**: An Aggregations Function, when used in a post-processing Pipeline, sends data to LogStream's Default Destination rather than to the Pipeline's attached Destination.

**Workaround**: If applicable, use the Function in a processing or pre-processing Pipeline instead.

**Fix**: In LogStream 2.4.3.



### 2021-02-25 – v.2.4.2 – On Safari, Event Breaker shows no OUT events

**Problem**: When viewing an Event Breaker's results on Safari, no events are displayed on the Preview pane's **OUT** tab.

**Workaround**: Use another supported browser.

**Fix**: In LogStream 2.4.3.



### 2021-02-22 – v.2.4.3 – Collection jobs UI errors

**Problem**: Collection jobs are missing from the **Monitoring > Sources** page, even though they are returned by metric queries. Also, the **Job Inspector > Live** modal displays an empty, unintended **Configure** tab.

**Workaround**: Use the Job Inspector to access collection results. Ignore the **Configure** tab.

**Fix**: In LogStream 2.4.4.



### 2021-02-19 – v.2.4.2 – Upon upgrade, Git remote repo setting breaks, blocking Worker Groups

**Problem**: If a Git remote repo was previously configured, upgrading to LogStream v.2.4.2 throws errors of this form upon startup: `Failed to initialize git repository. Config versioning will not be available...Invalid URL...`. The Master cannot commit or deploy to any Worker Group.

**Workarounds**: 1. Downgrade back to v.2.4.1 (or your previous working version). 2. Switch from Basic authentication to SSH authentication against the repo, to remove the username from requests. (The apparent root cause is Basic/http auth using a valid URL and username, but missing a password.)

**Fix**: In LogStream 2.4.3.



### 2021-02-19 – v.2.4.0-2.4.2 – Splunk (S2S) Forwarder access control blocks upon upgrade to LogStream 2.4.x

**Problem**: If Splunk indexers have forwarder tokens enabled, and worked with LogStream 2.3.x before, upgrading to LogStream 2.4.x causes data to stop flowing.

**Workaround**: If you encounter this problem, [rolling back](https://cribl.io/download/logstream-past-releases/) to your previously installed LogStream version (such as v.2.3.4) resolves it.

**Fix**: In LogStream 2.4.3.



### 2021-02-10 – v.2.4.0-2.4.1 – With Splunk HEC Source, JSON payloads containing embedded objects trigger high CPU usage

**Problem**: Splunk HEC JSON payloads containing nested objects trigger high CPU usage, due to a flaw in JSON parsing. 

**Workaround**: If you encounter this problem, [rolling back](https://cribl.io/download/logstream-past-releases/) to your previously installed LogStream version (such as v.2.3.4) resolves it.

**Fix**: In LogStream 2.4.2.



### 2021-01-30 – v.2.4.0 – Worker Nodes cannot connect to Master

**Problem**: Worker Nodes cannot connect to the Master after the Master is upgraded to v.2.4.0.

**Workaround**: Disable compression on the Workers. You can do so through the Workers' UI at **System Settings > Distributed Settings > Master Settings > Compression**, or by commenting out this line in each Worker's `cribl.yml` config file: 
```
compression: gzip
```

**Fix**: In LogStream 2.4.1.



### 2021-01-25 – v.2.4.0 – S3 collection stops working due to auth secret key issues.

**Problem**: S3 collection stops after upgrade to 2.4.0 due to secret key re-encryption. 

**Workaround**: Re-configure S3, save and re-deploy.

**Fix**: In LogStream 2.4.1.



### 2021-01-14 – v.2.4.0 – Google Cloud Storage Destination Needs Extra Endpoint to Initialize

**Problem**: The [Google Cloud Storage](destinations-google-cloud-storage) Destination fails to initialize, displaying an error of the form: `Bucket does not exist!`

**Workaround**: In the `outputs.yml` file, under your `cribl-gcp-bucket` key endpoint, add:  `https://storage.googleapis.com`. (in a single-instance deployment, locate this file at `$CRIBL_HOME/local/cribl/outputs.yml`. In a distributed deployment, locate it at `$CRIBL_HOME/groups/<group name>/local/cribl/outputs.yml`.)

**Fix**: In LogStream 2.4.1.



### 2021-01-14 – v.2.4.0 – Worker Groups' Settings > Access Management Is Absent from UI

**Problem**: In this release, the **Worker Groups > <group‑name> > System Settings** UI did not display the expected **Access Management**, **Authentication**, and **Local Users** sections.

**Workaround**: [Manually edit](/stream/authentication#local-authentication) the `users.json` file.

**Fix**: In LogStream 2.4.1.



### 2021-01-13 – v.2.4.0 – Route Filters Aren't Copied to Capture Modal

**Problem**: On the **Routes** page, selecting **Capture New** in the right pane does not copy custom **Filter** expressions to the resulting **Capture Sample Data** modal. That modal's **Filter Expression** field always defaults to `true`.

**Workarounds**: 1. Bypass the **Capture New** button. Instead, from the Route's own ••• (Options) menu, select **Capture**. This initiates a capture with the **Filter Expression** correctly populated. 2. Copy/paste the expression into the **Capture Sample Data** modal's **Filter Expression** field. Or, if the expression is displayed in that field's history drop-down, retrieve it.

**Fix**: In LogStream 2.4.1.



### 2021-01-13 – v.2.4.0 – Destinations' Documentation Doesn't Render from UI

**Problem**: Clicking the **Help** ![](page/help-button.png) link in a Destination's configuration modal displays the error message: "Unable to load docs. Please check LogStream's online documentation instead."

**Workarounds**: 1. Go directly to the online Destinations docs, starting [here](destinations). 2. Follow the UI link to the docs landing page, click through to open or download the current PDF, and scroll to its Destinations section.

**Fix**: In LogStream 2.4.1.



### 2021-01-13 – v.2.4.0 – `Esc` Key Doesn't Consistently Close Modals

**Problem**: Pressing `Esc` with focus on a modal's drop-down or slider doesn't close the modal as expected. (Pressing `Esc` with focus on a free-text field, combo box, or nothing does close the modal – displaying a confirmation dialog first, if you have unsaved changes.)

**Workarounds**: Click the **X** close box at upper right, or click **Cancel** at lower right.

**Fix**: In LogStream 2.4.1.



### 2020-12-17 – v.2.3.0-2.4.0 – Free-License Expiration Notice, Blocked Inputs

**Problem**: LogStream reports an expired Free license, and blocks inputs, even though Free licenses in v.2.3.0 do not expire. 

**Workaround**: This is caused by time-limited Free license key originally entered in a LogStream version prior to 2.3.0. Go to **Settings > Licensing**, click to select and expand your expired Free license, and click **Delete license**. LogStream will recognize the new, permanent Free license, and will restore throughput.

**Fix**: In LogStream 2.4.1.

### 2020-11-14 – v.2.3.3 – Null Fields Redacted in Preview, but Still Forwarded

**Problem**: Where event fields have null values, LogStream (by default) displays them as struck-out in the right Preview pane. The preview is misleading, because the events are still sent to the output. 

**Workaround**: If you do want to prevent fields with null values from reaching the output, use an [Eval](eval-function) Function, with an appropriate Filter expression, to remove them.

**Fix**: Preview corrected in LogStream 2.3.4.



### 2020-10-27 – v.2.3.2 – Cannot Name or Save New Event Breaker Rule

**Problem**: After clicking **Add Rule** in a new or existing Event Breaker Ruleset, the **Event Breaker Rule** modal's **Rule Name** field is disabled. Because **Rule Name** is mandatory field, this also disables saving the Rule via the **OK** button.

**Fix**: In LogStream 2.3.3.

### 2020-10-12 – v.2.3.1 – Deleting One Function Deletes Others in Same Group

**Problem**: After inserting a new Function into a group and saving the Pipeline, deleting the Function also deletes other Functions lower down in the same group.

**Fix**: In LogStream 2.3.2.

**Workaround**: Move the target Function out of the group, resave the Pipeline, and only then delete the Function.



### 2020-09-27 – v.2.3.1 – Enabling Boot Start as Different User Fails

**Problem**: When a root user tries to enable [boot-start](/stream/deploy-single-instance#enabling-start-on-boot) as a different user (e.g., using `/opt/cribl/bin/cribl boot-start enable -u <some‑username>`), they receive an error of this form:

```shell
error: found user=0 as owner for path=/opt/cribl, expected uid=NaN. 
Please make sure CRIBL_HOME and its contents are owned by the uid=NaN by running: 
[sudo] chown -R NaN:[$group] /opt/cribl 
```


**Fix**: In LogStream 2.3.2.

**Workaround**: Install LogStream 2.2.3 (which you can download [here](https://cdn.cribl.io/dl/2.2.3/cribl-2.2.3-90f21f14-linux-x64.tgz)), then upgrade to 2.3.1.

### 2020-09-17 – v.2.3.0 – Worker Groups menu tab hidden after upgrade to LogStream 2.3.0

**Problem**: Upon upgrading an earlier, licensed LogStream installation to v.2.3.0, the **Worker Groups** tab might be absent from the Master Node's top menu.

**Fix**: In LogStream 2.3.1.

**Workaround**: Click the **Home > Worker Groups** tile to access Worker Groups.



### 2020-09-17 – v.2.3.0 – Cannot Start LogStream 2.3.0 on RHEL 6, RHEL 7

**Problem**: Upon upgrading to v.2.3.0, LogStream might fail to start on RHEL 6 or 7, with an error message of the following form. This occurs when the user running LogStream doesn't match the LogStream binary's owner. LogStream 2.3.0 applies a restrictive permissions check using `id -un <uid>`, which does not work with the version of `id` that ships with these RHEL releases.

```shell
id: 0: No such user
ERROR: Cannot run command because user=root with uid=0 does not own executable 
```


**Fix**: In LogStream 2.3.1.

**Workaround**: Update your RHEL environment's `id` version, if possible.

### 2020-09-17 – v.2.3.0 – Cannot Start LogStream 2.3.0 with OpenId Connect

**Problem**: Upon upgrading an earlier LogStream installation to v.2.3.0, OIDC users might be unable to restart the LogStream server.

**Fix**: In LogStream 2.3.1.

**Workaround**: Edit `$CRIBL_HOME/default/cribl/cribl.yml` to add the following lines to its the `auth` section:

```shell
filter_type: email_whitelist
scope: openid profile email
```

### 2020-06-11 – v.2.1.x – Can't switch from Worker to Master Mode

**Problem**: In a Distributed deployment, attempting to switch Distributed Settings from Worker to Master Mode blocks with a spurious "Git not available...Please install and try again" error message.

**Fix**: In LogStream 2.3.0.

**Workaround**: To initialize `git`, switch first from Worker to Single mode, and then from Single to Master mode.

### 2020-05-19 – v.2.1.x – Login page blocks

**Problem**: Entering valid credentials on the login page (e.g., `http://localhost:9000/login`) yields only a spinner.

**Fix**: In LogStream 2.3.0.

**Workaround**: Trim `/login` from the URL.

### 2020-02-22 – v.2.1.x – Deleting resources in `default/`

**Problem**: In a Distributed deployment, deleting resources in `default/` causes them to reappear on restart. 

**Workaround/Fix**: In progress. 

### 2019-10-22 – v.2.0.0 – In-product upgrade issue on v2.0

**Problem**: Using in-product upgrade feature in v.1.7 (or earlier) fails to upgrade to v.2.0, due to package-name convention change. 

**Workaround/Fix**: Download the new version and upgrade per steps laid out [here](/stream/upgrading).

### 2019-08-27 – v.1.7 – In-product upgrade issue on v.1.7

**Problem**: Using in-product upgrade feature in v.1.6 (or earlier) fails to upgrade to v.1.7 due to package name convention change. 

**Workaround/Fix**: Download the new package and upgrade per steps laid out [here](/stream/upgrading).

### 2019-03-21 – v.1.4 – S3 stagePath issue on upgrade to v.1.4+

**Problem**: When upgrading from v.1.2 with a S3 output configured, `stagePath` was allowed to be undefined. In v.1.4+, `stagePath` is a required field. This might causing schema violations when upgrading older configs.

**Workaround/Fix**: Reconfigure the output with a valid `stagePath` filesystem path.
