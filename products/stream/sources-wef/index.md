# Windows Event Forwarder Source


Cribl Stream supports receiving Windows events from the Windows Event Forwarding (WEF) mechanism built into modern versions of Microsoft Windows (including Windows 10, Windows Server 2012, and more-recent releases).

The integration between WEF and Cribl works by generating events on Windows systems, which are collected by Windows Event Collectors (WECs). These events are then forwarded to Cribl using WEF, secured by either mutual TLS or Kerberos authentication. Cribl receives the events through the Windows Event Forwarder Source, processes them according to configured pipelines, and sends the processed events to designated Destinations.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No** | Uses Key-Value Store
>
{.box .info}

## Prerequisites {#prerequisites}

* WEF is set up and enabled.
* WEF client(s) are pointed to WECs.
* You've configured either [client certificate](sources-wef-client) or [Kerberos](sources-wef-kerberos) authentication.

> On Cribl Edge, Kerberos authentication for this Source is supported only for Edge Nodes running on x86_64 Linux.
{.box .info}

## Configure Cribl Stream to Receive Windows Events

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
    - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.

1. In the Source modal, configure the following under **General Settings**:
   * **Input ID**: A unique name that identifies this Windows Event Forwarder Source.
   * **Address**: The hostname/IP on which to listen for Windows events data. Example: `localhost` or `0.0.0.0`.
   * **Port**: The port number. The default, `5986`, is the port used by Windows Event Collector for HTTPS-based subscriptions.
   * **Authentication method**: The method for accepting incoming client connections, either `Client certificate` or `Kerberos`.
    > Kerberos authentication is not supported for Cribl-managed Workers in Cribl.Cloud, but it is enabled for customer-managed Worker Groups, whether on-prem or hybrid.
    >
    {.box .cloud}
   * For **Client Certificate**:
     * **Certificate name**: Choose the certificate you created.
     * **Private key path**: Server path containing the private key (in PEM format) to use. For Cribl.Cloud, the default path is `/opt/criblcerts/criblcloud.key` for a Cribl-managed Worker Group in Cribl.Cloud. Path can reference `$ENV_VARS`.
     * **Passphrase**: Passphrase to use to decrypt private key.
     * **Certificate path**: Server path containing certificates (in PEM format) to use. For Cribl.Cloud, the default path is `/opt/criblcerts/criblcloud.crt` for a Cribl-managed Worker Group in Cribl.Cloud. Path can reference `$ENV_VARS`.
     * **CA certificate path**: Server path containing [CA certificates](https://en.wikipedia.org/wiki/Certificate_authority) (in PEM format) to use. Path can reference `$ENV_VARS`. If multiple certificates are present in a `.pem`, the first certificate in the chain should be the issuing CA certificate. If the first certificate is not the issuing CA, use the **CA fingerprint override** setting to specify the expected SHA1 fingerprint. This should match the order of traversing the chain from the host to the root CA cert.
     > The CA certificate that generated the Cribl Stream server certificate must match the root CA that signed all client certificates that will be forwarding Windows events to this Source. If the issuing CA is an intermediate CA, this CA must also match on the server and all client certs.
     >
     {.box .info}
     * **Common name**: Regex that a peer certificate's subject attribute must be able to connect. Defaults to `.*`.
       * Matches on the substring after `CN=`.
       * As needed, escape regular expression (regex) tokens to match literal characters. For example, to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.
       * If the subject attribute contains Subject Alternative Name (SAN) entries, the Source will check the regex against all of those but ignore the Common Name (CN) entry (if any). If the certificate has no SAN extension, the Source will check the regex against the single name in the CN.
     * Optionally, select the **Minimum TLS version** and **Maximum TLS version** to accept from connections.
     * **Verify certificate via OCSP**: If toggled on, Cribl Stream will use an OCSP (Online Certificate Status Protocol) service to check that client certificates presented in incoming requests have not been revoked. Exposes the **Strict validation** toggle.
     * **Strict validation**: If enabled, Cribl Stream will fail checks on **any** OCSP error. Otherwise, Cribl Stream will fail checks only when a certificate is revoked, and will ignore other errors (such as `OCSP server is unavailable` errors).
   * For **Kerberos**: Provide the SPN and keytab file location that you set in [Keytab file](sources-wef-kerberos#keytab-export).
     * **Service Principal Name**: The Service Principal Name (SPN) in this format: `HTTP/<fully qualified domain name>@REALM`. This identifies the service in the Kerberos realm. The service pricipal name is case-sensitive, and must be exactly in the form used to export the keytab.
     * **Keytab location**: The path to the keytab file containing the service principal credentials. Cribl Stream uses `/etc/krb5.keytab` by default. This file contains the keys used for Kerberos authentication.
   * Optionally, add **Tags** that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

1. **Subscriptions** define which logs to collect and how to filter them. You need to add at least one subscription. For comprehensive details on subscription settings and the query builder, refer to the [subscriptions](#subscriptions) section.

1. Optionally, configure any [Persistent Queue](#pq), [Processing](#processing), and [Advanced](#advanced) settings outlined in the below sections.

1. Click **Save**, then **Commit & Deploy**.

### Subscriptions {#subscriptions}

Subscriptions control which events are collected from Windows endpoints and sent to the Cribl Stream Windows Event Forwarder Source. They filter the types of events to monitor, like security logs or application logs.

> Due to the way subscription bookmarks are stored, Leader CPU usage is directly impacted by the number of subscriptions created for the Source and the number of clients receiving each subscription. To optimize performance, prefer to minimize the number of subscriptions by combining them where possible.
{.box .info}

You can tailor your event collection to focus on the data that matters most to your Organization's security and monitoring needs. This helps reduce noise, save storage space, and ensure critical events are captured and analyzed.

Select **Add Subscription** and fill out the following fields to define at least one new, required subscription.

**Subscription name**: A friendly name for the subscription, which is only used to help you identify it.

**Version**: A read-only field that automatically updates whenever you save changes to the subscription. This version number reflects the specific configuration of the subscription and helps ensure that clients correctly interpret and process the subscription data.

**Format**: You can choose `Raw` to receive only XML data about the subscribed events, or `RenderedText` to additionally include amplifying information generated by the client about the event's contents.

**Heartbeat**: The maximum allowable time, in seconds, before the client will check in with Cribl Stream even if it has no new events to send.

**Batch timeout**: The maximum time, in seconds, that the client should batch new events before sending them to Cribl Stream.

>Windows Event Collector defines two delivery modes of "Event Delivery Optimization":
>
> - The "Minimize Bandwidth" mode corresponds to a **Heartbeat** and **Batch timeout** of 6 hours (21,600 seconds).
> - The "Minimize Latency" mode corresponds to a **Heartbeat** of 1 hour (3,600 seconds) and a **Batch timeout** of 30 seconds.
>
{.box .info}

**Locale**: The language code to use for the events sent by the client, in RFC-3066 format (for example, `en-US`). The language code uses [ISO 639](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes), and the country code uses [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). Defaults to `en-US` when left blank.

**Read existing events**: Controls whether the client should send historical events when it first connects. The behavior of **Read existing events** is influenced by the **Use bookmarks** setting. See the table below for a complete overview.

**Use bookmarks**: Keeps track of which events it has received.

Setting|Behavior
-----|-----
**Read existing events** toggled off| Only new events generated after it receives the subscription are sent.
**Read existing events** toggled on and **Use bookmarks** toggled off| All historical events are sent on the initial subscription and each time a client *forcibly* renews its subscription (for example, after group policy update), all events matching the query will be sent. Restarting Cribl Stream or restarting the client will not cause all events to be re-sent.
**Read existing events** toggled on and **Use bookmarks** toggled on| Historical events are sent on the initial subscription, but only new events (later than the saved bookmark) are sent on subsequent (even forced) subscription renewals.

**Important Considerations**

* **Subscription changes**: If you change and save a subscription, the saved bookmarks are no longer valid (they are tied to a specific subscription version), and all events matching the updated subscription will be sent.
  
* **Leader and Worker Node interactions**: Worker Nodes continue to accept new events even when the Leader is down. They sync with the Leader when it's back up. However, bookmarks stored in memory can be lost if a Worker Node restarts while the Leader is down.

* **Client balancing and bookmarks**: If a client balances to a different Worker Node while the Leader is down, it might receive a stale bookmark. Bookmarks are only sent to the client when it requests a subscription, not when it delivers events. This typically happens on the `Refresh=XX` interval from the GPO.
  * If the Leader is down when a client requests a subscription, it will receive the last known bookmark from the Worker Node, which could be outdated.
  * If a Worker Node restarts while the Leader is down, the client might not receive any bookmark at all, potentially leading to duplicate events.

**Compression**: Sets whether Windows clients compress the events sent to Cribl Stream, using the Streaming Lossless Data Compression (SLDC) algorithm. Defaults to toggled on.

**Queries**: See the explanation in [Configuring Queries](#queries) below.

**Query builder mode**: See the explanation in [Configuring Queries](#queries) below.

**Targets**: Set the DNS names of the clients that should receive this subscription. This field supports wildcard matching.

**Fields**: Use this setting to add fields exclusively to the events that WEF delivers **for the subscription you are defining**, using [Eval](eval-function)-like functionality. To add fields to all incoming events regardless of subscription, use [**Processing Settings** > **Fields**](#processing-fields) instead:

Optionally, you can create the field such that its **Value** is the value of a subscription metadata element. (See the available [subscription metadata elements](#subscription-metadata) listed below.)

To do this, specify a **Value** of `__subscription.<metadata_element>`. The Source will then find the metadata element you specified in the special object `__subscription` and set the value to that.

For example, if you wanted all events for the subscription to be tagged with its **Locale**, you could set the **Name** to `Locale` (or any arbitrary name) and **Value** to `__subscription.locale`.

* **Field Name**: Unique name for the field you're adding.

* **Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Subscription Metadata Elements {#subscription-metadata}

| Metadata Element | Type | Corresponding UI Setting |
|---|---|---|
|`version`| string | **Version** |
|`subscriptionName`| string | **Name** |
|`contentFormat`|`Raw` or `RenderedText`|**Format**|
|`readExistingEvents`|boolean|**Read existing events**|
|`heartbeatInterval`|number|**Heartbeat**|
|`batchTimeout`|number|**Batch timeout**|
|`sendBookmarks`|boolean|**Use bookmarks**|
|`compress`|boolean|**Compression**|
|`queries`|array| see [About the Query builder mode](#query-builder) below|

#### Change Subscriptions

If you change a subscription that is in use, Windows Event Forwarder will discover the new subscription ID. Once the `Refresh=XX` interval has elapsed, if any events were sent to the now-invalid previous subscription ID, Windows will resend them to the new one (assuming that these events remain valid under the updated subscription parameters). The [Receiver Refuses to Accept Delivery of Events](#receiver-refuses) warnings you'll see in this scenario are normal.

##### About Query Builder Mode {#query-builder}

This setting produces the `queries` array in one of two different forms, depending on whether you select its **Simple** button or its **Raw XML** button.

**Simple** produces an array whose elements each contain `path`, a string that corresponds to **Queries** > **Path**, and `queryExpression`, a string that corresponds to **Queries** > **Query expression**.

**Raw XML** produces an array whose elements each contain only the `xmlQuery` metadata element. This is the XML query you specified, in the form of a string, that corresponds to the **XML query** setting.

To access any element in the array, use array indexing. For example:
 * `__subscription.queries[0].path` will access the path to the first query in an array created by **Query builder mode** > **Simple**.
 * `__subscription.queries[0].xmlQuery` will access the first query in an array created by **Query builder mode** > **Raw XML**.

#### Configure Queries {#queries}

Queries determine which events to send from the clients. **At least one query is required.** The format is derived from the XPath implementation used by Windows Event Collector.

> While a single XPath query in a WEF subscription is limited to approximately 22 EventIDs, you can monitor more by creating a structured XML query with multiple `<Query>` elements within a single subscription. This is the recommended approach for performance and resource efficiency.
{.box .warning}

You have two **Query builder mode** options: **Simple** or **Raw XML**.

* **Raw XML** allows you to enter your XPath query into the **XML query** field.

* **Simple** allows you to manually build queries. Click **Add Query** to define each new query. A query has the following properties:
  *  **Path**: Set this to the `Path` attribute of a `Select` XPath element. See the example below.
  *  **Query expression**: Set this to the value inside a `Select` XPath element. See the example below.

With either a **Simple** or **Raw XML** query, you can use the **Targets** field to specify the DNS names of the endpoints that should forward these events. Supports wildcards (for example, `*.mydomain.com`). The default global wildcard (`*`) enables all endpoints.

> If you have both **Simple** and **Raw XML** queries defined within a given Subscription, the **Query builder mode** button that you select when saving this Source's configuration determines which query/queries Cribl Stream will send. To send both **Simple** and **Raw XML** queries, use the **Add Subscription** button to define multiple subscriptions.
>
{.box .success}

#### Example

When creating a subscription in a Windows Event Collector, you can view the generated XML/XPath query. Consider a subscription that returns all events from the Security log that are of severities Critical, Error, or Warning, and that occurred within the last 24 hours. This subscription would generate XML like this:

```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*[System[(Level=1  or Level=2 or Level=3) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select>
  </Query>
</QueryList>
```

To use this subscription in a Cribl Stream Windows Event Forwarder Source, you would either use the **Raw XML** option and paste the above XML, or use **Simple** mode and set the following query properties:
- **Path**: `Security`
- **Query expression**: `*[System[(Level=1  or Level=2 or Level=3) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]`

> You do not need to use the `<Query>` element's `Id` or `Path` attributes anywhere in the Cribl Stream subscription config.
>
{.box .success}

### Persistent Queue Settings {#pq}

In the **Persistent Queue Settings** tab, you can optionally specify persistent queue storage, using the following controls. Persistent queue buffers and preserves incoming events when a downstream Destination has an outage or experiences backpressure.

Before enabling persistent queue, learn more about persistent queue behavior and how to optimize it with your system:

- [About Persistent Queues](/persistent-queues)
- [Optimize Source Persistent Queues (sPQ)](/persistent-queues-sources)

> On Cribl-managed [Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable persistent queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process.
>
> The 1 GB limit is on uncompressed inbound data, and the queue does not perform any compression. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Enable persistent queue**: Default is toggled off. When toggled on:

**Mode**: Select a condition for engaging persistent queues.

- `Always On`: This default option will always write events to the persistent queue, before forwarding them to the Cribl Stream data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from the Cribl Stream data processing engine.

> `Smart` mode only engages when necessary, such as when a downstream Destination becomes blocked *and* the **Buffer size limit** reaches its limit. When persistent queue is set to `Smart` mode, Cribl attempts to flush the queue when every new event arrives. The only time events stay in the buffer is when a downstream Destination becomes blocked.
>
{.box .info}

**Buffer size limit**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. This buffer is for all connections, not just per Worker Process. For that reason, this can dramatically expand memory usage. Connections share this limit, which may result in slightly lower throughput for higher numbers of connections. For higher numbers of connections, consider increasing the limit.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**File size limit**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, and so forth. If not specified, Cribl Stream applies the default `1 MB`.

**Queue size limit**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Source will stop queueing data and block incoming data. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, and so forth. Can be set as high as `1 TB`, unless you've [configured](group-settings#storage) a different **Worker Process PQ size limit** in Group or Fleet settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file closes. Defaults to `None`; `Gzip` is also available.

> In Cribl Stream 4.1 and newer, the Source persistent queue default **Mode** is `Always on`, to best ensure events' delivery. For details on optimizing this selection, see [Optimize Source Persistent Queues (sPQ)](persistent-queues-sources).
>
> You can optimize Workers' startup connections and CPU load at **Group/Fleet settings** > [Worker Processes](group-settings#processes).
>
{.box .info}

### Processing Settings {#processing}

#### Fields {#processing-fields}

Use this setting to add fields to **all events coming into the Source**, using [Eval](eval-function)-like functionality. (To add fields on a per-subscription basis, use [**Subscriptions** > **Fields**](#subscriptions) instead.)

**Field Name**: Unique name for the field you're adding.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced}

**Allow MachineID mismatch** (client certificate only): Toggle on if you do not want to verify that the events sent by a client match the client's certificate Common Name (Subject CN). If toggled off, events where the `MachineID` (by default the client's machine name, like `CLIENT1.domainName.com`) do not match the CN will be rejected.

This setting applies only to client certificate authentication. If you use Kerberos authentication, this option will not appear and will be treated as if it is toggled off.

**Show originating IP**: Toggle on when clients are connecting through a proxy that supports the `X-Forwarded-For` header to keep the client's original IP address on the event instead of the proxy's IP address. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Capture request headers**: Toggle on to add request headers to events, in the `__headers` field.

**Health check endpoint**: Toggle on to enable a health check endpoint specific to this Source: `http(s)://<host>:<port>/cribl_health`. A `200` HTTP response code is returned when the Source is healthy. Otherwise, two errors you could receive are:
  * `ECONNRESET` where the Source failed to initialize due to not having listeners on the port.
  * `503` or `Server is busy, max active connections reached` indicate there are too many connections per Worker Process.

**Log CA fingerprint mismatch warning** (client certificate only): Toggle on to log a warning message when the client certificate's issuing Certificate Authority (CA) fingerprint doesn't match the expected CA fingerprint configured in the Windows Event Forwarding Source. This mismatch prevents proper authentication and event forwarding. The log message is `Mismatched fingerprint` and includes both the expected `acceptedFingerprint` and actual `issuerFingerprint` fingerprints. Toggled off by default.

**Active request limit**: Maximum number of active requests allowed for this Source, per Worker Process. Defaults to `256`. Enter `0` for unlimited.

> Raising this limit can increase throughput by allowing more concurrent data requests, but increases resource usage and load on both your Cribl Stream infrastructure and on downstream Destinations. Before raising this limit, ensure:
>
> - Your Cribl Stream deployment has sufficient capacity to support higher request concurrency, including CPU, memory, or number of Worker Processes.
> - Downstream Destinations are correctly sized and tuned to accept the higher data ingest rate, preventing backpressure. See [Manage Backpressure](manage-backpressure) for more information.
>
> Improper sizing on either side can result in dropped events, delayed processing, or overall system instability.
{.box .warning}

**Requests-per-socket limit**: The maximum number of requests Cribl Stream should allow on one socket before instructing the client to close the connection. Defaults to `0` (unlimited). See [Balancing Connection Reuse Against Request Distribution](#max-requests-per-socket) below.

**Socket timeout (seconds)**: How long Cribl Stream should wait before assuming that an inactive socket has timed out. The default `0` value means wait forever. For Kerberos authentication, see how to [adjust timeouts](#adj-timeouts).

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Stream how long to wait for additional data before closing the socket connection. Defaults to `90` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

The longer the timeout, the more Cribl Stream will reuse connections. The shorter the timeout, the closer Cribl Stream gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, which mitigates the associated cost.

For Kerberos authentication, see how to [adjust timeouts](#adj-timeouts).

**IP allowlist regex**: Grants access to requests originating from specific IP addresses that match a defined pattern. Unmatched requests are rejected with a 403 (Forbidden) status code. Defaults to `.*` (allow all).

**IP denylist regex**: Blocks requests originating from specific IP addresses that match a defined pattern, even if they would be allowed by default. Rejected requests receive a 403 (Forbidden) status code. Defaults to `^$` (allow all).

**CA fingerprint override** (client certificate only): Use this setting only if the first certificate in the configured CA chain does not match the SHA1 fingerprint that the WEF client expects, such as an intermediate certificate. If that is the case, enter the expected SHA1 fingerprint. The thumbprint specified here must match the `IssuerCA` parameter in the Subscription Manager [template](sources-wef-client#subtemplate).

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Balancing Connection Reuse Against Request Distribution {#max-requests-per-socket}

**Requests-per-socket limit** allows you to limit the number of HTTP requests an upstream client can send on one network connection. Once the limit is reached, Cribl Stream uses HTTP headers to inform the client that it must establish a new connection to send any more requests. (Specifically, Cribl Stream sets the HTTP `Connection` header to `close`.) After that, if the client disregards what Cribl Stream has asked it to do and tries to send another HTTP request over the existing connection, Cribl Stream will respond with an HTTP status code of `503 Service Unavailable`.

Use this setting to strike a balance between connection reuse by the client, and distribution of requests among one or more Worker Node processes by Cribl Stream:

* When a client sends a sequence of requests on the same connection, that is called connection reuse. Because connection reuse benefits client performance by avoiding the overhead of creating new connections, clients have an incentive to **maximize** connection reuse.

* Meanwhile, a single process on that Worker Node will handle all the requests of a single network connection, for the lifetime of the connection. When receiving a large overall set of data, Cribl Stream performs better when the workload is distributed across multiple Worker Node processes. In that situation, it makes sense to **limit** connection reuse.

There is no one-size-fits-all solution, because of variation in the size of the payload a client sends with a request and in the number of requests a client wants to send in one sequence. Start by estimating how long connections will stay open. To do this, multiply the typical time that requests take to process (based on payload size) times the number of requests the client typically wants to send.

If the result is 60 seconds or longer, set **Requests-per-socket limit** to force the client to create a new connection sooner. This way, more data can be spread over more Worker Node processes within a given unit of time.

For example: Suppose a client tries to send thousands of requests over a very few connections that stay open for hours on end. By setting a relatively low **Requests-per-socket limit**, you can ensure that the same work is done over more, shorter-lived connections distributed between more Worker Node processes, yielding better performance from Cribl Stream.

A final point to consider is that one Cribl Stream Source can receive requests from more than one client, making it more complicated to determine an optimal value for **Requests-per-socket limit**.

#### Timeout Considerations for Kerberos Authentication {#adj-timeouts}

If you use Kerberos authentication, there are some trade-offs you should consider when you select settings in **Subscriptions** > **Batch timeout** and **Advanced Settings** > **Keep-alive timeout (seconds)**.

Windows expects socket connections to be long-lived for Kerberos authentication so it can avoid the overhead of performing the two-step Kerberos authentication on each new batch delivery.

Set **Subscriptions** > **Batch timeout**, **Subscriptions** > **Heartbeat**, or both to be less than or equal to **Keep-alive timeout** (or the lowest **Batch timeout** if several subscriptions are used), especially when a load balancer is in front of the Worker Nodes.

Set **Socket timeout** higher than **Batch timeout** (or the lowest **Batch timeout** if several subscriptions are used). This ensures that connections aren't prematurely closed before batches are sent. If the **Socket timeout** has elapsed between event deliveries or heartbeats, Cribl Stream will still handle it gracefully, but incur an additional overhead of two network round trips.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Event Handling {#kvstore}

The Windows Event Forwarder Source uses internal fields and a key-value store to manage and process events efficiently. Internal fields are metadata added to events for internal processing and should be treated as read-only to avoid unintended consequences. Key fields include:

* `__final`
* `__headers` – Added only when **Advanced Settings** > **Capture request headers** is toggled on.
* `__inputId`
* `__srcIpPort` – See details [below](#src-ip-port).
* `__subscriptionName`
* `__subscriptionVersion`
* `_raw`
* `_time`

The key-value store uses the `machineId` as keys to store bookmark information when bookmarks are enabled. This ensures that events are processed correctly and efficiently by maintaining the state of event processing.

Together, the key-value store and internal fields work to ensure accurate and efficient event handling. The key-value store maintains the state of event processing, while internal fields provide necessary metadata for internal operations, enabling Cribl Stream to process and route events effectively.

## Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the WEF client sending data to this Source.

In the WEF Source configuration, there's an option to handle the `X-Forwarded-For` header, which is commonly used by load balancers to pass the original client's IP address. When any proxies (including load balancers) lie between the WEF client and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Show originating IP** is toggled off, the original client IP/port in this header will override the value of `__srcIpPort`.

If **Show originating IP** is toggled on, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Troubleshoot {#troubleshooting}

The Source's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](sources#capture-source-data) to see real-time events as they are ingested. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the ingestion process, including any errors or warnings that may have occurred.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify ingestion issues. Analyze the graphs showing events and bytes in/out over time.

### Common Issues

#### High CPU Usage on Leader Node API Process

**Problem**: Experiencing high CPU usage on your Leader Node's API Process is often a symptom of having too many subscriptions (thousands). This can lead to performance issues and instability.

**Solutions**: First attempt to reduce or consolidate your subscriptions. This can often alleviate the problem without the need for additional configurations. However, since the process of consolidating subscriptions can vary significantly between environments, there is no one-size-fits-all approach.

If you are unable to reduce the number of subscriptions sufficiently, consider enabling Write-Ahead Log (WAL) mode for the [key-value store](#kvstore) (KV Store). We recommend working with [Cribl Support](/support/) to incorporate the new **KV Store** settings effectively. You can find these settings under **Settings** > **Global Settings** > **General Settings** > **Limits** or in the [limits.yml](limitsyml) file.

#### Security Event Log Forwarding Fails

**Error**:
    `Error "0x138C" Windows Event Forward plugin can't read any event from the query since the query returns no active channel.`

**Solution**: Enable the Network Service to read the security logs by [configuring access for the Event Log Service](https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/security-event-log-forwarding-fails-error-0x138c-5004#resolution).

> When configuring a Group Policy Object (GPO) to add NETWORK SERVICE permissions, carefully review existing permissions to prevent unintended removal. Preserve all necessary permissions to avoid application issues.
>
{.box .warning}

#### Cannot Find the Certificate

**Error**:
   `The forwarder is having a problem communicating with subscription manager at address… The WS-Management service cannot find the certificate that was requested.`

For example:

```shell
The forwarder is having a problem communicating with subscription manager at address https://<stream_worker>:5986/wsman/SubscriptionManager/WEC.

Error code is 2150858882 and Error Message is <f:WSManFault xmlns:f=http://schemas.microsoft.com/wbem/wsman/1/wsmanfault Code="2150858882" Machine="DELMD2273309.na.blkint.com"><f:Message>The WS-Management service cannot find the certificate that was requested. </f:Message></f:WSManFault>.
```

**Resolution**:

* The `NETWORK SERVICE` user does not have permissions to use the private key of the certificate for authentication.
* Add the `NETWORK SERVICE` user to the list of users allowed to use the private key.

#### Receiver Refuses to Accept Delivery of Events {#receiver-refuses}

**Warning**:
   `The receiver refuses to accept delivery of events and requests that the subscription be canceled.`

If you make a change to a subscription that is in use, **and** if the Windows client tries to send events before the `Refresh=XX` interval has elapsed, the result is that these events are sent to the old subscription, which is now invalid because its query is outdated or because other subscription parameters have changed.

The warning message tells the WEF client to stop sending events to the now-invalid old subscription.

**Resolution**:

Windows Event Forwarder handles this scenario gracefully, and you do not need to take any action to resolve it:

* Once the `Refresh=XX` interval has elapsed, the WEF client will ask for and receive the updated subscription, and from then on should be able to successfully deliver events against that new subscription.

* Any events that were refused under the old/invalid subscription ID will be resent under the new one, assuming that they remain valid according to the new subscription parameters.

#### Subscription Cannot be Created

**Error**:
   `The subscription <subscription> can not be created. The error code is 5004.`

The error appears in the `Microsoft-Windows-Eventlog-ForwardingPlugin/Operational` log.

**Resolution**:
1. Apply the `O:BAG:SYD:...` [permissions](sources-wef-client#3-set-client-certificate-permissions) to make the log file readable.
2. Reboot the machine.

#### Verify that Client and Destination are Joined to a Domain

**Error**:
   `If Kerberos mechanism is used, verify that the client computer and the destination computer are joined to a domain.`

When you are using auto-enroll, the `Eventlog-ForwardingPlugin Operational` logs might display an error of this form.

**Resolution**:

In your auto-enroll template's **Client‑Server Authentication Properties**, make sure the **Subject Name format** is set to **Common name**.

## Resources {#resources}

For processing Pipelines that you can import and adapt to your needs, examine these resources on the [Cribl Packs Dispensary](https://packs.cribl.io/):

- [Microsoft Windows Events](https://packs.cribl.io/packs/cribl-windows-events) – Shape events into JSON or Key=Value format, and dramatically reduce event sizes.

- [Cribl Edge Windows for Common Schema](https://packs.cribl.io/packs/cribl-edge-windows-source) – Transform Windows events into a Common Schema.

- [Exabeam Pack for Windows](https://packs.cribl.io/packs/cribl_exabeam_windows) – Forward specific Windows Event IDs to Exabeam.

- [QRadar Pack for Windows Events](https://packs.cribl.io/packs/cribl-qradar-windows-events) – Optimize Windows events for delivery to QRadar.

## Further Reading

For more context and greater detail, see ATA Learning's [WEF tutorial](https://adamtheautomator.com/windows-event-collector/).

For more about WEF subscriptions, including sample subscription queries, see:

* Microsoft's [Use Windows Event Forwarding to Help with Intrusion Detection](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection) topic.

* The NSA Cybersecurity Directorate's [WEF Guidance](https://github.com/nsacyber/Event-Forwarding-Guidance/tree/master/Subscriptions/samples).

* Palantir's [WEF Guidance](https://github.com/palantir/windows-event-forwarding).
