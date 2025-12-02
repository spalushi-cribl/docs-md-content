# Cribl TCP


The internal Cribl TCP Source is available only in [distributed deployments](/stream/deploy-distributed). It is provided to facilitate sending data between Edge Nodes that are connected to the same Leader. You'll find this Source especially valuable in a [hybrid Cloud deployment](/stream/deploy-cloud#hybrid). 

> Type: **System and Internal**  |  TLS Support: **YES** | Event Breaker Support: **No**
>
{.box .info}
 
You might choose this Source over the [Cribl HTTP](sources-cribl-http) Source in certain circumstances, such as when a firewall or proxy allows raw TCP egress. In [single‑instance mode](/stream/deploy-single-instance) or for testing, you can substitute the [TCP JSON](sources-tcp-json) Source. (However, this substitution will not provide the single-billing benefits described in the next section.)

## How It Works

You can use the Cribl TCP Source to transfer data between Workers. If the Cribl TCP Source receives data from its [Cribl TCP Destination](destinations-cribl-tcp) counterpart, you're billed for ingress only once – when Cribl first receives the data. All data subsequently transferred to other Workers via the Cribl TCP Destination/Source pair is not charged.

This use case is common in hybrid Cribl.Cloud deployments, where a customer-managed (on-prem) Worker or Edge Node sends data to a Worker in Cribl.Cloud for additional processing and routing to Destinations. However, the Cribl TCP Destination/Source pair can similarly reduce your metered data ingress in other scenarios, such as on-prem Edge to on-prem Stream.

As one usage example, assume that you want to send data from one Edge Node deployed on-prem, to another that is deployed in [Cribl.Cloud](/stream/deploy-cloud). You could do the following:

- Create an on-prem Filesystem Collector (or whatever Collector or Source is suitable) for the data you want to send to Cribl.Cloud.
- Create an on-prem Cribl TCP Destination.
- Create a Cribl TCP Source, on the targetFleet in Cribl.Cloud.
- For an on-prem Edge Node configure a Filesystem Collector to send data to the Cribl TCP Destination, and from there to the Cribl TCP Source in Cribl.Cloud.
- On Cribl-managed Cribl.Cloud Edge Nodes, make sure that TLS is either disabled on both the Cribl TCP Destination and the Cribl TCP Source it's sending data to, or enabled on both. Otherwise, no data will flow. On Cribl.Cloud instances, the Cribl TCP Source ships with TLS enabled by default.

## Configuration Requirements {#reqs}

The key points about configuring this architecture are:
- The Cribl TCP Destination must be on a Edge Node that is connected to the same Leader as the Cribl TCP Source.
- You must specify the same Leader Address on the Edge Nodes that host both the Destination and Source. Otherwise, token verification will fail – breaking the connection and preventing data flow.
- To get the Leader Address specifically for Cribl.Cloud hybrid Workers, see [Hybrid Cribl HTTP/​Cribl TCP Configuration](deploy-cloud#hybrid-pairs).
- To configure the Leader Address via the UI, log directly into each Edge Node's UI. Then select **Settings** > **Global Settings** > **Distributed Settings** > **Leader Settings** > **Address**.
- To configure the Leader Address via the [instance.yml](instanceyml) file, the `host` values on the connecting Edge Nodes must be identical. In this example, both Edge Nodes must point to `cribl-leader`:

  ```yaml
  distributed:
    mode: master
    master:
      host: cribl-leader
      port: 4200
  ```

- When you configure the Cribl TCP Destination, its **Cribl endpoint** field must point to the **Address** and **Port** you've configured on the Cribl TCP Source.
- Cribl 3.5.4 was a breakpoint in Cribl TCP Leader/Worker communications. Edge Nodes running the Cribl TCP Source on Cribl Edge 3.5.4 and later can send data only to Edge Nodes running v.3.5.4 and later. Edge Nodes running the Cribl TCP Source on Cribl Edge 3.5.3 and earlier can send data only to Edge Nodes running v.3.5.3 and earlier. 
- Finally, it's important to understand the special way the Cribl TCP Source handles [internal fields](#internal-fields).

## Configuring the Cribl TCP Source

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **Cribl TCP**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **Cribl TCP**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Cribl TCP Source definition. 

**Address**: Enter hostname/IP to listen for TCP JSON data. E.g., `localhost` or `0.0.0.0`.

**Port**: Enter the port number to listen on, e.g., `10300`.

#### Additional Settings

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Server Side)

**Enabled** defaults to `No`. When toggled to `Yes`:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Defaults to `No`. When toggled to `Yes`:

- **Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `No`.

- **Common name**: Regex matching subject common names in peer certificates allowed to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. E.g., to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

**Enable Persistent Queue** defaults to `No`. When toggled to `Yes`:

**Mode**: Choose a mode from the drop-down:
* With `Smart` mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. 
* With `Always On` mode, PQ will always write events directly to the queue before forwarding them to the processing engine.

**Max buffer size**: Maximum number of events to hold in memory before dumping them to disk. Defaults to `1000`.

**Commit frequency**: Number of events to send before committing that Cribl Edge has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space the queue is allowed to consume. Once this limit is reached, Cribl Edge stops queueing and applies the fallback **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. 

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Edge will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre–Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields {#internal-fields}

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

The Cribl TCP Source (and the Cribl HTTP Source) treat internal fields differently than other Sources do. That's because of the difference in the way that incoming data originates.

Other Sources ingest data that's not coming from Cribl Edge or Stream, meaning that no Cribl internal fields can be present in that data when it arrives at the Source, and the Source is free to add internal fields without clobbering (overwriting) anything that existed already.

By contrast, the Cribl TCP Source and the Cribl HTTP Source ingest data that's coming from a Cribl TCP or Cribl HTTP Destination. That data **can** contain internal fields when it arrives at the Source. This means that if the Source adds internal fields, those could potentially clobber what existed before.

To avoid this problem, the Cribl TCP Source and the Cribl HTTP Source add a unique `__forwardedAttrs` (i.e., "forwarded attributes") field. The nested structure of the `__forwardedAttrs` field contains any of the following fields that are present in the arriving data:

| Internal Fields |
|--|
| `__srcIpPort` |
| `__inputId` |
| `__outputId` |

| Other Fields |
|--|
| `cribl_breaker` |
| `cribl_pipe` |

These fields are **copied** into `__forwardedAttrs`, not moved there. As the data (apart from `__forwardedAttrs`) moves through the Source and any Pipelines, the values of these fields can be overwritten. But the copies of these fields in `__forwardedAttrs` remain unchanged, so you can retrieve them as necessary.
