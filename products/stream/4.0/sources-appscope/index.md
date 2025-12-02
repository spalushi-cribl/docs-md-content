# AppScope


AppScope is an open-source instrumentation utility from Cribl. It offers visibility into any Linux command or application, regardless of runtime, with no code modification. For details about configuring the AppScope CLI, loader, and library, see: https://appscope.dev/docs.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
{.box .info}

## Configuring Cribl Stream to Receive AppScope Data

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **AppScope**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **AppScope**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this AppScope Source definition.

**UNIX domain socket**: When toggled to `Yes`, exposes the following two fields to specify a file-backed UNIX domain socket connection to listen on.
- **UNIX socket path**: Path to the UNIX domain socket. Defaults to `$CRIBL_HOME/state/appscope.sock`.
- **UNIX socket permissions**: Permissions to set for this socket, e.g., `777`. If empty, Cribl Stream will use the runtime user's default permissions.  

When **UNIX domain socket** is set to `No` (the default), you instead see the following two fields to specify a network host and port.
- **Address**: Enter the hostname/IP on which to listen for AppScope data. (E.g., `localhost`.) Defaults to `0.0.0.0`, meaning all addresses.
- **Port**: Enter the port number to listen on.

### Authentication Settings  {#authentication-settings}

Use the **Authentication method** buttons to select one of these options:

- **Manual**: Use this default option to enter the shared secret clients must provide in the `authToken` header field. Click **Generate** if you need a new auth token. If empty, unauthenticated access will be permitted. 

- **Secret**: This option exposes an **Auth token (text secret)** drop-down, in which you can select a stored secret that references the auth token described above. The secret can reside in Cribl Stream's [internal secrets manager](kms-config) or (if enabled) in an external KMS. Click **Create** if you need to configure a new secret.

### Optional Settings {#optional}

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Server Side) {#tls}

This left tab is displayed only when the [Optional Settings](#optional) > **UNIX domain socket** slider is set to `No`. It provides the following options.

**Enabled** defaults to `No`. When toggled to `Yes`:

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.  

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Defaults to `No`. When toggled to `Yes`:

- **Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `No`.

- **Common name**: Regex matching subject common names in peer certificates allowed to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. E.g., to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### AppScope Filter Settings {#filter}

Using the controls below, you can optionally configure filters to pass to your AppScope Source upon startup. Define an **Allow filter** list by linking to an existing AppScope config for each process. Define a **Deny filter** list to exclude processes from being scoped.   

> This tab is currently unavailable on Cribl-managed Cribl.Cloud Workers.
> 
> You use filtered entries to match aspects of a scoped process. When a scoped process matches the filter(s), the settings defined in the AppScope config element override previously defined settings.
>
{.box .info}

**Filter enabled**: When toggled to `Yes`, exposes this tab's remaining options.

**Deny filter**: Click **+ Add filter** to exclude processes from being scoped. Each row of the resulting table provides the following fields.

  - **Process name**: Matches if the string value you enter corresponds to the basename of the scoped process.

  - **Process argument**: Matches if the string value you enter appears as a substring anywhere in the scoped process' full command line (including options and arguments). 

**Allow filter**: Click **+ Add filter** to include processes to scope, and to link to an AppScope config. If you define no filters here, all processes can be scoped. 

  - **Process name**: Matches if the string value you enter corresponds to the basename of the scoped process.

  - **Process argument**: Matches if the string value you enter appears as a substring anywhere in the scoped process' full command line (including options and arguments).

  - **AppScope config**: Select an existing AppScope config. When the filter matches. this config's settings will override previously defined settings.

**Transport override**: Enter a URL to override aspects of the transport configuration, such as the hostname, port, or TLS settings. For details, see [Transport Override Details](#override).

**Extract directory**: The AppScope binary's contents will be extracted into the directory that you specify here. Cribl Stream will create the directory if it doesn't already exist. Defaults to `$TEMP/appscope`.

#### Transport Override Details {#override}

In scenarios like the following, use the **Transport override** option to extend the defaults in AppScope's transport configuration:   

- When this Source is set to **TCP** mode, it typically listens on the default address `0.0.0.0`. Scoped clients will need a specific `IP` or `hostname` to connect. In these cases, set **Transport override** URL to a specific IP/hostname (example format: `tcp://my.host.name`). This Source will parse the URL and look for the `hostname` and `port`, then use those values to override what would otherwise be sent to the `scope start` command. 

- When this Source is set to **UNIX domain socket**, it listens by default on `$CRIBL_HOME/state/appscope.sock`. The socket is often created on a mounted volume in a container. The path to that socket might be different outside the container, or when mounted into another container. In these cases, set the **Transport override** URL to specify an alternative path (example format: `unix:///some/other/volume/appscope.sock`).

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Smart`: This default option will engage PQ only when the Source detects backpressure from the Cribl Stream data processing engine.
- `Always On`: This option will always write events into the persistent queue, before forwarding them to the Cribl Stream data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Stream applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Stream will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Stream to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> Setting the PQ **Mode** to `Always On` can degrade throughput performance. Select this mode only if you want guaranteed data durability. As a trade-off, you might need to either accept slower throughput, or provision more machines/faster disks.
>
{.box .warning}

### Processing Settings

#### Event Breakers

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Disk Spooling

**Enable disk persistence**: Whether to save metrics to disk. When set to `Yes`, exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Stream will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Stream will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Optionally compress the data before sending. Defaults to `gzip` compression. Select `none` to send uncompressed data.

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/appscope.sock`.

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Enable proxy protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection.

**Max active connections**: Maximum number of active connections allowed per Worker Process. Defaults to `1000`; enter `0` to allow unlimited connections.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## AppScope with Edge on Kubernetes 

When Cribl Edge detects that a `scope`'d process is running inside a Kubernetes container, it reports the Kubernetes metadata as `kube_**` properties. It adds these to the incoming events and metrics, and you can view the combined events and metrics on the AppScope Source's **Live Data** tab. 

> For Cribl Edge to detect that it's running in a Kubernetes Pod, you must first set the `CRIBL_K8S_POD` [environment variable](environment-variables).
>
{.box .info}

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
  
  * `__inputId`
  * `__srcIpPort`

## Examples 

### Cribl.Cloud – TLS

An `in_appscope` TLS Source is preconfigured for you on Cribl.Cloud, using port `10090`. You can send it AppScope data using this command:

```shell
./scope run -c in.main-default-<Your-Org-ID>.cribl.cloud:10090 -- ps -ef
```

### Cribl.Cloud – TCP

Cribl.Cloud reserves port `10091` to ingest AppScope data over TCP. 

Add a new AppScope Source, leave **UNIX domain socket** toggled off, assign it **Port**: `10091`, and save it. 

You can now send it AppScope data using this command:

```shell
./scope run -c tcp://in.main-default-<Your-Org-ID>.cribl.cloud:10091 \
>     -- curl -so /dev/null https://wttr.in/94105
```