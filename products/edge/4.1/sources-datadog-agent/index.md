# Datadog Agent {#datadog-agt-top}


[Datadog Agent](https://docs.datadoghq.com/agent/) is open-source software that monitors the host on which it runs. Acting as a [DogStatsD](https://docs.datadoghq.com/developers/dogstatsd/?tab=hostagent) server, Datadog Agent also aggregates metrics from other processes or containers on the host.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
>
{.box .info}

Cribl Edge can ingest the following from Datadog Agent, in [bundled](#what-where) form:

* Logs.
* Metrics (gauge, rate, counter, and histogram).
* Service checks.
* Agent metadata and other events emitted from the `/intake/` endpoint.

Datadog Agent also emits Application Performance Monitoring data, which it sends to Datadog. Cribl Edge does not currently support ingesting this APM data. 

On the system(s) that you want to monitor, you'll need to install Datadog Agent and [configure](#config-datadog) it to send data to Cribl Edge. Cribl Edge can parse, filter, and enrich that data, and then send it to any supported Destination, including a Cribl Edge [Datadog Destination](destinations-datadog). (By default, Datadog Agent sends data only to Datadog.)

For inbound log data, this Source supports gzip -compression when the `Content‑Encoding: gzip` connection header is set. For other data types, it assumes that all inbound data is compressed using `deflate`.

## Configuring Cribl Edge to Ingest Datadog Agent Output

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** >] **Datadog Agent**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** >] **Datadog Agent**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Datadog Agent Source definition. 

**Address**: Enter hostname/IP to listen for Datadog Agent data. E.g., `localhost` or `0.0.0.0`.

**Port**: Enter the port number to listen on.

### Optional Settings

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Edge. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Server Side)

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

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Edge will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Edge to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Edge will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> As of Cribl Edge 4.1, Source-side PQ's default **Mode** changed from `Smart` to `Always on`. This option more reliably ensures events' delivery, and the change does not affect existing Sources' configurations. However:
> - If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
> - If you enable `Always on`, this can reduce data throughput. As a trade-off for data durability, you might need to either accept slower throughput, or provision more machines/faster disks.
> - You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .warning}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Enable Proxy Protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Capture request headers**: Toggle this to `Yes` to add request headers to events, in the `__headers` field.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Max active requests**: Maximum number of active requests per worker process. Use `0` for unlimited.

**Extract metrics**: Toggle to `Yes` to extract each incoming metric to multiple events, one per data point. This works well when sending metrics to a `statsd`-type output. If sending metrics to DatadogHQ or any destination that accepts arbitrary JSON, leave toggled to `No` (the default).

**Forward API key validation requests**: Toggle to `Yes` to send key validation requests from Datadog Agent to the Datadog API. If toggled to `No` (the default), Stream handles key validation requests by always responding that the key is valid.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__agent_api_key`
* `__agent_event_type`
* `__final`
* `__headers` – Added only when **Advanced Settings** > **Capture request headers** is set to `Yes`.
* `__inputId`
* `__srcIpPort` – See details [below](#src-ip-port).
* `_time`

### Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the Datadog Agent sending data to this Source.

When any proxies (including load balancers) lie between the Datadog Agent and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Enable proxy protocol** is set to `No`, the original client IP/port in this header will override the value of `__srcIpPort`. 

If **Enable proxy protocol** is set to `Yes`, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Sending Datadog Agent Data to Cribl Edge {#config-datadog}

Before you begin this section, you should have Datadog Agent running on one or more hosts.

To enable Datadog Agent to send data to Cribl Edge, you'll set the following environment variables:

* `DD_DD_URL`: The URL or IP address and port of your Datadog Agent Source in Cribl Edge. 
* `DD_LOGS_CONFIG_LOGS_DD_URL`: The same value as above, assuming log collection is enabled on Datadog Agent.
* `DD_LOGS_CONFIG_USE_HTTP`: Set to `true`. Cribl Edge ingests Datadog Agent logs over HTTP only; ingesting logs over TCP is not supported.

Set these environment variables in one of two ways: (1) through a `docker run` command, or (2) by editing the `datadog.yaml` configuration file that your Datadog Agent uses.  	

### Using the `docker run` Command

Here's an example `docker run` that sets the environment variables described above, along with [others](https://docs.datadoghq.com/agent/docker/?tab=standard#environment-variables) required for Datadog Agent but not relevant to Cribl Edge. Replace the example values with values appropriate for your environment. 

```shell
docker run --rm --name dd-agent
  -e DD_API_KEY=6d1ephx1w55p978i6d1ephx1w55p978i \
  -e DD_SKIP_SSL_VALIDATION=true \ 
  -e DD_DD_URL="http://0.0.0.0:8080" \
  -e DD_LOGS_ENABLED=true \
  -e DD_LOGS_CONFIG_LOGS_DD_URL="0.0.0.0:8080" \
  -e DD_LOGS_CONFIG_USE_HTTP=true \
  -e DD_LOGS_CONFIG_LOGS_NO_SSL=true \
  -e DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true \
  -e DD_DOGSTATSD_NON_LOCAL_TRAFFIC="true" \
  gcr.io/datadoghq/agent:7
```

### Using a Config File

Instead of using environment variables, you can set the Cribl Edge-related values in your Datadog Agent's `datadog.yaml` config file.

See the documentation links in the [example config file](https://github.com/DataDog/datadog-agent/blob/main/pkg/config/config_template.yaml) that Datadog maintains online; and, the [docs](https://docs.datadoghq.com/agent/guide/agent-configuration-files/?tab=agentv6v7) that describe filesystem locations relevant to getting Datadog Agent to configure itself with the desired `datadog.yaml` file.

### Managing What Data Goes Where {#what-where}

The different kinds of information that Datadog Agents emit - logs, metrics, service checks, agent metadata, and APM data - are not completely independent when you send them to Cribl Edge Datadog Agent Source. The reference to "bundling" near the [top](#datadog-agt-top) of this page introduced this situation in simplified form. The table below explains the relevant constraints for each type of information that Datadog Agents emit, along with the Datadog environment variables that control them.

Type(s) of Information | Dependencies and/or Limitations | Environment Variable
--- | --- | ---
Logs | Independent of other types. | `DD_LOGS_CONFIG_DD_URL`
Metrics, Events, Service Checks, and Metadata |  Always sent together. | `DD_DD_URL`
APM Data | Can only be sent to Datadog, e.g., `https://trace.agent.datadoghq.com`. <br/> Cannot be sent to Cribl Edge even when you **are** sending other types there. | `DD_APM_DD_URL`

## Managing API Keys {#api-keys-datadog}

Suppose your data flow runs from Datadog Agents, to Cribl Edge Datadog Agent Sources, to Cribl Edge Datadog Destinations, to Datadog accounts. You'll need to decide **how many** of each of these elements there are to define the data flow you want. You will also set (or override) **Datadog API keys** to support the desired data flow. For some data flows, you'll need the **General Settings** > **Allow API key from events** toggle in the Cribl Edge Datadog Destination.

Another possibility is that you want data to flow to some Destination other than a Cribl Edge Datadog Destination. If that's the case, try adapting the examples below to your use case. Please connect with us on the `Cribl Community` Slack if you have questions. 

### Data Flow Examples

The examples which follow start simple and become more complex in terms of desired data flow, and, consequently, of Cribl Edge configuration.

#### One Agent to One Account

* You're running one Datadog Agent on one host. 
* You want the output of the Agent sent to a single Datadog account. 
* You only need one API key. 
* You configure that single API key on your Cribl Edge Datadog Destination.

#### Many Agents to One Account

* You're running Datadog Agents on a fleet of hosts. 
* You want to consolidate all the output of the Agents into a single Datadog account. 
* You only need one API key. This is no different from the simpler one-to-one scenario above.
* You configure that single API key on your Cribl Edge Datadog Destination. 
* No matter which host the data originates from, your Cribl Edge Datadog Destination sends it to Datadog using that single API key.

#### Many Agents to Many Accounts

* You're running Datadog Agents on a fleet of hosts. 
* You want the output of the Agents from some hosts to flow into one Datadog account, and the output from other hosts to flow into a different Datadog account.
  * This may need to scale up to multiple accounts, if, for example, you are managing data from several different customers, or from several different organizations within your company.
* You need one API key **for each** Datadog account. 
* In the Cribl Edge Datadog Destination, toggle **General Settings** > **Allow API key from events** to `Yes`. 
* Here's what happens:
  * Datadog Agent always sends an API key when it communicates with the Cribl Edge Datadog Agent Source. Agents within different subgroups of hosts will send different API keys, as described above.
  * Cribl Edge Datadog Agent Source passes the API key from the Agent to the Cribl Edge Datadog Destination as an internal field. 
  * Cribl Edge Datadog Destination uses that API key to direct its output to the correct Datadog account. If need be, you can configure the Destination to override the passed-in API key with a different one. This comes in handy when you know that your Agent(s) are using invalid API keys. You can even override **all** passed-in API keys. 

## Verifying that Data is Flowing {#verifying}

Once you have configured your Datadog Agent(s) as described above, and they have begun sending data: 

* Try viewing the incoming data in the **Live Data** tab in your Cribl Edge Datadog Agent Source.
* If you have a Cribl Edge Datadog Destination set up, connect your Cribl Edge Datadog Agent Source to it via a **QuickConnect Passthru**, and verify that the same data shows up in the Destination's **Live Data** tab.
  * Another possibility is that you want your Cribl Edge Datadog Agent Source to connect to some Destination other than a Cribl Edge Datadog Destination. That's beyond the scope of this example, but verifying data flow should be similar. 
* If the Cribl Edge Datadog Destination is configured to send data to a Datadog instance, verify that the same data shows up there, in the appropriate form, according to the transformations you've configured in Cribl Edge.
* If you have a many-to-many data flow as described above, verify that output is being correctly split among the possible Datadog accounts at the end of the data flow.

Assuming that data is behaving as expected, you can proceed to the best part, namely adding one or more Pipelines between Datadog Agent Source and Datadog Destination, to transform the data however you wish. Given the varied nature of what Datadog Agents collect, there should be ample opportunities to make the data leaner and more usable.
