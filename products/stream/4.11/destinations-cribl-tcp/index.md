# Cribl TCP Destination


The Cribl TCP Destination sends data to a [Cribl TCP Source](sources-cribl-tcp) connected to the same Leader. Relaying data between a Cribl TCP Destination and Cribl TCP Source pair prevents double-billing: You'll be charged no additional charges or credits after initial ingest to the first Worker Group. 

It’s common to send data from Cribl Edge to a Cribl Stream Worker Group using this kind of pairing.

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
>
>For guidance on when to choose this Source versus the [Cribl HTTP](destinations-cribl-http) Destination, see [Interoperation Protocols](/reference-architectures/reference-arch-full-suite/#protocols).
{.box .info}

Within Cribl Stream, the Cribl TCP Destination is available only in [Distributed deployments](/stream/deploy-distributed). In [single‑instance mode](/stream/deploy-single-instance), or for testing, you can use the [TCP JSON](destinations-tcp-json) Destination instead. However, this substitution will not facilitate sending [all internal fields](#internal-fields), as described below.

## How It Works

You can use the Cribl TCP Destination to transfer data between Worker Nodes and Workers. If the Cribl TCP Destination sends data to its [Cribl TCP Source](sources-cribl-tcp) counterpart on another Worker, you're billed for ingress only once – when Cribl first receives the data. All data subsequently relayed to other Workers via a Cribl TCP Destination/Source pair is not charged.

This use case is common in hybrid Cribl.Cloud deployments, where a customer-managed (hybrid) Worker Node sends data to a Worker in Cribl.Cloud for additional processing and routing to Destinations. However, the Cribl TCP Destination/Source pair can similarly reduce your metered data ingress in other scenarios, such as on-prem Cribl Edge to on-prem Cribl Stream.

As one usage example, assume that you want to send data from one Node deployed on-prem, to another that is deployed in [Cribl.Cloud](deploy-cloud). You could do the following:

- Create an on-prem File System Collector (or whatever Collector or Source is suitable) for the data you want to send to Cribl.Cloud.
- Create an on-prem Cribl TCP Destination.
- Create a Cribl TCP Source, on the target Cribl Stream Worker Group or Cribl Edge Fleet in Cribl.Cloud.
- For an on-prem Node configure a File System Collector to send data to the Cribl TCP Destination, and from there to the Cribl TCP Source in Cribl.Cloud.
- On Cribl-managed Nodes in Cribl.Cloud, make sure that TLS is either disabled on both the Cribl TCP Destination and the Cribl TCP Source it's sending data to, or enabled on both. Otherwise, no data will flow. On Cribl.Cloud instances, the Cribl TCP Source ships with TLS enabled by default.


## Configuration Requirements {#reqs}

The key points about configuring this architecture are:
- The Cribl TCP Destination must be on a Node that is connected to the same Leader as the Cribl TCP Source.
- When you configure the Cribl TCP Destination, its **Address** and **Port** values must point to the **Address** and **Port** you've configured on the Cribl TCP Source.
- Cribl 3.5.4 was a breakpoint in Cribl TCP Leader/Worker communications. Nodes running the Cribl TCP Source on Cribl 3.5.4 and later can send data only to Nodes running v.3.5.4 and later. Nodes running the Cribl TCP Source on Cribl 3.5.3 and earlier can send data only to Nodes running v.3.5.3 and earlier.

## Configure a Cribl TCP Destination

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
    - **Output ID**: Enter a unique name to identify this Destination definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**.
    - **Description**: Optionally, enter a description.
    - **Load balancing**:  When toggled on, see [Load Balancing Settings](#lb) below. 

        > The following two fields appear **only** if **Load balancing** is toggled off (default), and must match the **Address** and **Port** you've configured on the peer Cribl TCP Source to which you're sending.
        >
        {.box .info}
    - **Address**: Hostname of the receiver.
    - **Port**: Port number to connect to on the host, for example, `10300`.
3. Next, you can configure the following **Optional Settings**: 
    - **Compression**: Codec to use to compress the data before sending. Defaults to `Gzip`.
    - **Throttling**: Throttle rate, in bytes per second. Defaults to `0`, meaning no throttling. Multiple-byte units such as KB, MB, and so forth, are also allowed – for example, `42 MB`. When throttling is engaged, your **Backpressure behavior** selection determines whether Cribl Stream will handle excess data by blocking it, dropping it, or queueing it to disk. When used in conjunction with **Max connections**, the throttling rate is applied per connection. For instance, if **Max connections** is set to `2` and throttling is set to `3 MB`, each of the two connections can send data at a rate of up to 3 MB per second, resulting in a total potential throughput of 6 MB per second across both connections.
    - **Backpressure behavior**: Specifies whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`. See [Persistent Queue Settings](#pq) below.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [TLS settings](#tls), [Persistent Queue](#pq), [Timeout](#timeout), [Processing](#processing), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Load Balancing Settings {#lb}

Enabling the **Load balancing** toggle displays the following controls:

##### Exclude Current Host IPs

Displays under **Optional Setting** when **Load balancing** is toggled on. This toggle determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Default is toggled off, which keeps the current host available for load balancing.

##### Destinations

The **Destinations** table is where you specify a set of receivers on which to load-balance data. Click **Add Destination** to specify more receivers on new rows. Each row provides the following fields:

**Address**: Hostname of a receiver. Optionally, you can paste in a comma-separated list, in `<host>:<port>` format.
 
**Port**: Port number to send data to on the host.

> Each **Address**/**Port** combination must match the **Address** and **Port** configured on a peer TCP Source to which you're sending.
>
{.box .info}

**TLS**: Whether to inherit TLS configs from group setting, or disable TLS. Defaults to `inherit`.

**TLS servername**: Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP); otherwise, uses the global TLS settings. 

**Load weight**: Set the relative traffic-handling capability for each connection by assigning a weight (> `0`). This column accepts arbitrary values, but for best results, assign weights in the same order of magnitude to all connections. Cribl Stream will attempt to distribute traffic to the connections according to their relative weights.

The final column provides an `X` button to delete any row from the table.

For details on configuring all these options, see [About Load Balancing](load-balancing).

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.11/snippets/_destinations-persistent-queue-settings.md]

### TLS Settings (Client Side) {#tls}

**Use TLS**: Defaults to toggled off. When toggled on:

**Autofill?**: This setting is experimental.

**Validate server certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (for example, the system's CA). Default is toggled off.

**Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate name**: The name of the predefined certificate.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

### Timeout Settings {#timeout}

**Connection timeout**: Amount of time (in milliseconds) to wait for the connection to establish before retrying. Defaults to `10000`.

**Write timeout**: Amount of time (in milliseconds) to wait for a write to complete before assuming connection is dead. Defaults to `60000`.

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

**Auth Token TTL minutes**: The number of minutes before the internally generated authentication token expires, valid values between 1 and 60.

**Exclude fields**: Fields to exclude from the event. This Destination excludes
`__kube_*`, `__metadata`, and `__winEvent` by default and forwards all other
[Internal Fields](#internal-fields). For more information about `__winEvent`, see
the [Windows Event Logs Source](/edge/sources-windows-event-logs#optional-settings) topic.

> If you are running Cribl Stream 4.0.x (or earlier) and are using the [Kubernetes Metrics Source](/edge/sources-kubernetes-metrics) with this Destination, consider excluding the following fields to reduce event size: 
> 
> `!__inputId`,`!__outputId`,`!__criblMetrics,__* `. 
> 
> The contents of `__raw` are often redundant with the `_raw` field's contents. Where they are identical, consider excluding one of the two.
>
{.box .info}

**Log failed requests to disk**: Toggling on makes the payload of the most recently failed request available for inspection. See [Inspect Payload to Troubleshoot Closed Connections](#log-payload) below.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

The following options are added if you enable the [General Settings](#general-settings) tab's **Load balancing** option:

**DNS resolution period (seconds)**: Re-resolve any hostnames after each interval of this many seconds, and pick up destinations from A records. Defaults to `600` seconds.

**Load balance stats period (seconds)**: Lookback traffic history period. Defaults to `300` seconds.  (Note that If multiple receivers are behind a hostname – i.e., multiple A records – all resolved IPs will inherit the weight of the host, unless each IP is specified separately. In Cribl Stream load balancing, IP settings take priority over those from hostnames.)

**Max connections**: Constrains the number of concurrent receiver connections, per Worker Process, to limit memory utilization. If set to a number &gt; `0`, then on every DNS resolution period, Cribl Stream will randomly select this subset of discovered IPs to connect to. Cribl Stream will rotate IPs in future resolution periods – monitoring weight and historical data, to ensure fair load balancing of events among IPs.

#### Inspect Payload to Troubleshoot Closed Connections {#log-payload}

When a downstream receiver closes connections from this Destination (or just stops responding), inspecting the payload of the most recently failed request can help you find the cause. For example:

* Suppose you send an event whose size is larger than the downstream receiver can handle. 
* Suppose you send an event that has a `number` field, but the value exceeds the highest number that the downstream receiver can handle.

When **Log failed requests to disk** is enabled, you can inspect the last failed request payload. Here is how:

1. In the Destination UI, navigate to the **Logs** tab.
2. Find a log entry with a `connection error` message. 
3. Expand the log entry. 
4. If the message includes the phrase `See payload file for more info`, note the path in the `file` field on the next line.

Now you have the path to the directory where Cribl Stream is storing the payload from the last failed request.

## Internal Fields Loopback to Sources {#internal-fields}

The Cribl TCP and [Cribl HTTP](destinations-cribl-http) Destinations differ from all other Destinations in the way they handle internal fields: They normally send data back to their respective Cribl Sources – where Cribl internal fields, metrics, and sender-generated fields can all be useful. 

These Destinations forward all internal fields by default, except for any that you exclude in **Advanced Settings** > **Exclude fields**.

As examples, if the following fields are present on an event forwarded by a Cribl HTTP or Cribl TCP Destination, they'll be accessible in the ingesting Cribl HTTP/TCP Source:`__criblMetrics`, `__srcIpPort`, `__inputId`, and `__outputId`.
