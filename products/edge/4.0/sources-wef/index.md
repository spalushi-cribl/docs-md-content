# Windows Event Forwarder


Cribl Edge supports receiving Windows events from the Windows Event Forwarding mechanism built into modern versions of Microsoft Windows (including Windows 10, Windows Server 2012, and more-recent releases).

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No** | Uses Key-Value Store
>
{.box .info}

Currently, this Source supports only client certificate (mutual TLS) authentication. Cribl plans to add Kerberos authentication support in a future release.

> If you are running this Source on a Cribl Stream version earlier than v.4.3.0, consider delaying any planned upgrades to v.4.3.x or v.4.4.0 until CRIBL-20444 is fixed.
>
{.box .warning}

## Upstream Prerequisites {#prerequisites}

This page assumes that:
- You already have Windows Event Forwarding set up,
- Pointed to one or more Windows Event Collectors (WECs), 
- Using client certificate authentication over HTTPS. 

We also assume that you have – or will generate – a server certificate for this Source, issued by the same Certificate Authority as the client certs. 

> For details, see [Configuring Upstream Clients/Senders](#clients).
> 
> For a complete walk-through of generating certificates, setting permissions and policies, and ingesting Windows Event subscriptions and data through Cribl.Cloud, see our [Configuring WEF for Cribl Stream](/stream/usecase-wef-config) topic.
>
{.box .success}

### Monitoring Numerous EventIDs with WEF

A single XPath query within a Windows Event Forwarding (WEF) subscription is limited to approximately 22 `EventID`s. If your query exceeds this limit, WEF may fail to forward matching events to Cribl Edge.

To monitor a larger number of `EventID`s, the best practice is to use a single subscription containing multiple queries.

You can achieve this by editing the subscription's XML configuration to include several `<Query>` blocks within the `<QueryList>`. Each query can contain its own group of `EventID`s, allowing you to bypass the single-query limit efficiently.

Creating multiple subscriptions is not recommended, as it consumes more resources and goes against Microsoft's performance guidelines.

## Configuring Cribl Edge to Receive Windows Events

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Windows Event Forwarder**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Windows Event Forwarder**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Windows Event Forwarder Source definition. 

**Address**: Enter the hostname/IP on which to listen for Windows events data. (E.g., `localhost` or `0.0.0.0`.)

**Port**: Enter the port number. The default, 5986, is the port used by Windows Event Collector for HTTPS-based subscriptions.

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.  

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`. If multiple certificates are present in a `.pem`, each must directly certify the one preceding it in that file. This should match the order of traversing the chain from the host to the root CA cert.

> The CA certificate that generated the Cribl Edge server certificate must be the same root CA that signed all client certificates that will be forwarding Windows events to this Source. If the issuing CA is an intermediate CA, this CA must also match on the server and all client certs.
>
{.box .info}

**Common name**: Regex matching subject common names in peer certificates allowed to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. E.g., to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

**Verify certificate via OCSP**: If toggled to `Yes`, Cribl Edge will use an OCSP (Online Certificate Status Protocol) service to check that client certificates presented in incoming requests have not been revoked. Exposes this additional toggle:

- **Strict validation**: If enabled, Cribl Edge will fail checks on **any** OCSP error. Otherwise, Cribl Edge will fail checks only when a certificate is revoked, and will ignore other errors (such as `OCSP server is unavailable` errors).

### Optional Settings

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Edge. Use a tab or hard return between (arbitrary) tag names.

### Subscriptions

Click **+ Add Subscription** to define a new subscription. At least one subscription is required, and ha). the following options:

**Name**: A friendly name for the subscription, and is only used to help you identify it.

**Version**: A read-only field which will be populated when the Source is saved, and will be assigned a new value any time new changes are made to the subscription that affect how a client interprets the subscription.

**Format**: You can choose `Raw` to receive only XML data about the subscribed events, or `RenderedText` to additionally include amplifying information generated by the client about the event's contents.

**Heartbeat**: The maximum allowable time, in seconds, before the client will check in with Cribl Edge even if it has no new events to send.

**Batch timeout**: The maximum time, in seconds, that the client should aggregate new events before sending them to Cribl Edge.

> Windows Event Collector defines two delivery modes of "Event Delivery Optimization":
> - The "Minimize Bandwidth" mode corresponds to a **Heartbeat** and **Batch timeout** of 6 hours (21,600 seconds).
> - The "Minimize Latency" mode corresponds to a **Heartbeat** of 1 hour (3,600 seconds) and a **Batch timeout** of 30 seconds. 
>
{.box .info}

**Read existing events**: Control whether historical events should be sent by the client when it first connects. If set to `No`, only events generated by the client after the subscription is received will be forwarded. If set to `Yes`, the behavior depends on the **Use bookmarks** setting:

- If **Use bookmarks** is set to `No`, then each time a client *forcibly* renews its subscription (e.g., after group policy update), all events matching the query will be sent. Restarting Cribl Edge or restarting the client will not cause all events to be re-sent.
- If **Use bookmarks** is set to `Yes`, then when the client receives a subscription for the first time, it will send all historical events matching the query, but on subsequent (even forced) resubscriptions, it will only send events later than the saved bookmark.
- In either case, if a subscription is changed and saved, the saved bookmarks are no longer valid (they are tied to a specific subscription version), and all events matching the updated subscription will be sent.

**Use bookmarks**: If toggled to `Yes`, Cribl Edge will keep track of which events have been received, resuming from that point even if the client forcibly re-subscribes. If set to `No`, a client (re-)subscribing will either send all historical events, or send only events subsequent to the subscription, depending on the **Read existing events** setting.

**Compression**: Sets whether Windows clients compress the events sent to Cribl Edge, using the Streaming Lossless Data Compression (SLDC) algorithm. Defaults to `Yes`.

**Queries**: See the explanation in [Configuring Queries](#queries) below.
**Query builder mode**: See the explanation in [Configuring Queries](#queries) below.

**Targets**: Set the DNS names of the clients that should receive this subscription. Wildcard-matching is supported.

#### Configuring Queries {#queries}

Queries determine which events to send from the clients. **At least one query is required.** The format is derived from the XPath implementation used by Windows Event Collector. You have two **Query builder mode** options: **Simple** or **Raw XML**. 

Select **Raw XML** if you have an existing XPath query. Paste your query into the **XML query** field.

Select **Simple** if you need to manually build queries. Click **+ Add query** to define each new query. A query has the following properties:
- **Path**: Set this to the `Path` attribute of a `Select` XPath element. See the example below.
- **Query expression**: Set this to the value inside a `Select` XPath element. See the example below.

With either a **Simple** or **Raw XML** query, you can use the **Targets** field to enter the DNS names of the endpoints that should forward these events. Supports wildcards (e.g., `*.mydomain.com`). The default global wildcard (`*`) enables all endpoints.

> If you have both **Simple** and **Raw XML** queries defined within a given Subscription, the **Query builder mode** button that you select when saving this Source's configuration determines which query/queries Cribl Edge will send. To send both **Simple** and **Raw XML** queries, use the **+ Add Subscription** button to define multiple subscriptions.
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

To use this subscription in a Cribl Edge Windows Event Forwarder Source, you would either use the **Raw XML** option and paste the above XML, or use **Simple** mode and set the following query properties:
- **Path**: `Security`
- **Query expression**: `*[System[(Level=1  or Level=2 or Level=3) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]`

> You do not need to use the `<Query>` element's `Id` or `Path` attributes anywhere in the Cribl Edge subscription config.
>
{.box .success}

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Smart`: This default option will engage PQ only when the Source detects backpressure from the Cribl Edge data processing engine.
- `Always On`: This option will always write events into the persistent queue, before forwarding them to the Cribl Edge data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Edge will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Edge to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Edge will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> Setting the PQ **Mode** to `Always On` can degrade throughput performance. Select this mode only if you want guaranteed data durability. As a trade-off, you might need to either accept slower throughput, or provision more machines/faster disks.
>
{.box .warning}

### Processing Settings

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Allow MachineID mismatch**: Set this to `Yes` if you do not want to verify that the events sent by a client match the client's certificate Common Name (Subject CN). If set to `No`, events where the `MachineID` (by default the client's machine name, like `CLIENT1.domainName.com`) do not match the CN will be rejected.

**Enable proxy protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Max active requests**: Maximum number of active requests allowed for this Source, per Worker Process. Defaults to `256`. Enter `0` for unlimited.

**Capture request headers**: Toggle this to `Yes` to add request headers to events, in the `__headers` field.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Edge how long to wait for additional data before closing the socket connection. Defaults to `5` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

The longer the timeout, the more Cribl Edge will reuse connections. The shorter the timeout, the closer Cribl Edge gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, and mitigate the associated cost.

**CA fingerprint override**: Use this setting only if the first certificate in the configured CA chain does not match the SHA1 fingerprint that the WEF client expects. If that is the case, enter the expected SHA1 fingerprint.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__final`
* `__headers` – Added only when **Advanced Settings** > **Capture request headers** is set to `Yes`.
* `__inputId` 
* `__srcIpPort` – See details [below](#src-ip-port).
* `__subscriptionName`
* `__subscriptionVersion`
* `_raw`
* `_time`

### Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the WEF client sending data to this Source.

When any proxies (including load balancers) lie between the WEF client and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Enable proxy protocol** is set to `No`, the original client IP/port in this header will override the value of `__srcIpPort`. 

If **Enable proxy protocol** is set to `Yes`, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Key-Value Store

The Windows Event Forwarder Source uses key-value store for bookmark information when bookmarks are enabled.
`machineId` is used as keys.

## Configuring Upstream Clients/Senders {#clients}

This section summarizes how to configure Windows endpoints/senders to forward events to this Cribl Edge Source. You can find basic instructions for setting up WEF (in a traditional Windows environment) in [this Microsoft guide](https://docs.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription#setting-up-a-source-initiated-subscription-where-the-event-sources-are-not-in-the-same-domain-as-the-event-collector-computer). You can generally follow that guide's "non-domain" section to correctly configure the endpoints/senders.

> For a complete walk-through of generating certificates, setting permissions and policies, and ingesting Windows Event subscriptions and data through Cribl.Cloud, see Cribl's [Configuring WEF for Cribl Stream](/stream/usecase-wef-config) topic.
>
{.box .success}

1. Set up Cribl Edge's Windows Event Forwarder Source as outlined above. The CA certificate you use should be the issuing authority both for the server certificate, and for all client certs you plan to have forwarding events to this Source.

2. Ensure that your existing Windows Event Collector is receiving events correctly from whatever endpoints and subscriptions you already have in place.

3. Find the fingerprint of the CA cert you are using for this Source, via a tool like `certutil` or `certlm` on Windows, or `openssl` on other operating systems. You'll need this in the next step.

4. Edit the group policy for endpoints you want to forward to this Source:

   - Under **Computer Configuration > Administrative Templates > Windows Components > Event Forwarding**, modify the **Configure target Subscription Manager** setting.
  
   - Add a Subscription Manager with a value like: `Server=https://<cribl-instance>:<wef-source-port>/wsman/SubscriptionManager/WEC,Refresh=<desired refresh interval>,IssuerCA=<CA cert fingerprint from above>`
  
   - Note that the path portion is the same as required for a WEC subscription, and is not configurable in Cribl Edge.
  
   - Ensure that the protocol is `https`. (This Source does not currently support `http` with Kerberos.)
  
   - When complete, save the policy and apply it to affected endpoints.

> The path component within the **Subscription Target URL** set using Group Policy Objects (GPOs) is strictly case-sensitive. If the capitalization is incorrect, WEF will encounter errors and fail to forward events to Cribl Edge.
> 
> The following format exemplifies the proper capitalization for the Subscription Target URL path:
> `Server=<url>/SubscriptionManager/wsman/WEC,Refresh=<interval>`
> 
> Pay close attention to the capitalization of "SubscriptionManager" within the path. It must be exactly as shown in the correct format (with a capital "M") for WEF to function properly.
>
{.box .warning}

5. Check that events are flowing into Cribl Edge now according to the configured subscriptions. If they are not:

   - Verify that the clients can reach the Cribl Edge instance through the network to port 5986 (or other configured port) via TLS/HTTP. If clients are connecting to Cribl Edge via a proxy, you may need to enable the Proxy Protocol setting in the **Advanced** section of the Source configuration. Ensure that the correct outbound firewall port is opened on the client.

   - Verify all of the following: that the certificate chain is correct; that the endpoints have a valid CRL encompassing the CA cert; that the CA cert is a trusted root on the clients; and that the server and client certs are issued by the same CA. The `CAPI2` Windows event log might reveal any errors here.

   - Check Cribl Edge for any errors, as well as the `EventForwarding-Plugin` and `Windows Remote Management` event logs on the clients.

   ## Troubleshooting {#troubleshooting}

   When using auto-enroll, the Eventlog-ForwardingPlugin Operational logs might display an error of this form:  
   `If Kerberos mechanism is used, verify that the client computer and the destination computer are joined to a domain.`

   To resolve this: In your auto-enroll template's **Client‑Server Authentication Properties**, make sure the **Subject Name format** is set to **Common name**.