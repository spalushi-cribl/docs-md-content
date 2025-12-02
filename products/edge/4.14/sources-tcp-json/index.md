# TCP JSON Source


Cribl Edge can receive [newline-delimited JSON](#format) data over TCP.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
>
{.box .info}

## Configure Cribl Edge to Receive TCP JSON Data

> Cribl Edge ships with a TCP JSON Source preconfigured to listen on Port 10070. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
      - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. Configure the following under **General Settings**:
    - **Input ID**: Enter a unique name. The default Source is prefilled with the value `in_tcp_json`, which can't be changed via the UI. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter hostname/IP to listen for TCP JSON data. For example, `localhost` or `0.0.0.0`.
    - **Port**: Enter the port number to listen on.
3. Under **Authentication**, select an **Authentication method** from the dropdown:
   - **Manual**: Use this default option to enter the shared secret that clients must provide in the `authToken` header field. Exposes an **Auth token** field for this purpose. (If left blank, unauthorized access will be permitted.) A **Generate** link is available if you need a new secret.
   - **Secret**: This option exposes an **Auth token (text secret)** drop-down, in which you can select a stored secret that references the `authToken` header field value described above. The secret can reside in Cribl Edge's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need a new secret.
4. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [TLS](#tls), [Persistent Queue Settings](#pq), [Processing](#processing) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**.


### TLS Settings (Server Side) {#tls}

**Enabled**: Defaults to toggled off. When toggled on:

**Certificate**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Default is toggled off. When toggled on:

- **Validate client certificates**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (for example, the system's CA). Default is toggled on.

- **Common name**: Regex that a peer certificate's subject attribute must match in order to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. (For example, to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.) If the subject attribute contains Subject Alternative Name (SAN) entries, the Source will check the regex against all of those but ignore the Common Name (CN) entry (if any). If the certificate has no SAN extension, the Source will check the regex against the single name in the CN.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.14/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Fields

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**Enable proxy protocol**: Toggle on if the connection is proxied by a device that supports [proxy protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

**Enable load balancing** (Cribl Stream only): Toggle on to distribute the data from incoming TCP connections across multiple Worker Processes. This improves performance compared to the default of having all data from an incoming connection processed on a single Worker Process.
* This spins up an extra Worker Process (named `wLB` in **Settings** > **Processes**) on the Worker Node, which handles splitting data from incoming TCP connections across all other Worker Processes.
* When enabled, the option to include common fields in the JSON header is not supported. This means fields specified under `fields` in the JSON header are not automatically added to all events.

> Load balancing is available only in Cribl Stream Distributed deployments.
>
{.box .warning}

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (such as, all IPs).

**Active connection limit**: Maximum number of active connections allowed per Worker Process. Defaults to `1000`. Set a lower value if connection storms are causing the Source to hang. Set to `0` for unlimited connections.

**Socket idle timeout (seconds)**: The duration that Cribl Edge will wait for activity on an idle TCP socket before closing the connection. Disabled when set to `0`, the default.

**Forced socket termination timeout (seconds)**: The extra time the server waits before forcibly closing a socket that has been idle (**TCP socket idle timeout**) or exceeded its maximum lifespan (**TCP socket max lifespan**) but has not yet properly closed. This prevents resource leaks caused by unresponsive clients or network issues. Configure based on network latency and client behavior. Default: `30` seconds. Set to `0` to disable.

**Socket max lifespan (seconds)**: The duration that a socket is allowed to remain open, regardless of activity. This setting prevents resource exhaustion (such as TCP pinning) by limiting the lifespan of connections. Configure based on expected connection durations and resource availability. Disabled when set to `0`, the default.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the configuration will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Field for this Source:

* `__inputId`
* `__srcIpPort`

## Format {#format}

Cribl Edge expects TCP JSON events in [newline-delimited JSON](https://github.com/ndjson/ndjson-spec) format:

1. A header line. Can be empty – for example, `{}`. If **authToken** is enabled (see above) it should be included here as a field called `authToken`. When `authToken` is **not** set, the header line is **optional**. In this case, the first line will be treated as an event if does not look like a header record.

  In addition, if events need to contain common fields, they can be included here under `fields`. In the example below, `region` and `AZ` will be automatically added to all events.

2. A JSON event/record per line.

  ```json {title="Sample TCP JSON Events"}
  {"authToken":"myToken42", "fields": {"region": "us-east-1", "AZ":"az1"}}

  {"_raw":"this is a sample event ", "host":"myHost", "source":"mySource", "fieldA":"valueA", "fieldB":"valueB"}
  {"host":"myOtherHost", "source":"myOtherSource", "_raw": "{\"message\":\"Something informative happened\", \"severity\":\"INFO\"}"}
  ```

### TCP JSON Field Mapping to Splunk

If a TCP JSON Source is routed to a Splunk destination, fields within the JSON payload are mapped to Splunk fields. Fields that do not have corresponding (native) Splunk fields become index-time fields. For example, let's assume we have a TCP JSON event as below:

`{"_time":1541280341, "host":"myHost", "source":"mySource", "_raw":"this is a sample event ",
 "fieldA":"valueA"}`

Here, `_time`, `host`, and `source` become their corresponding fields in Splunk. The value of `_raw` becomes the actual body of the event, and `fieldA` becomes an index-time field (`fieldA`::`valueA``).

## Examples

### Testing TCP JSON In

This first example simply tests that data is flowing in through the Source:

1. Configure Cribl Edge to listen on port `10001` for TCP JSON. Set `authToken` to `myToken42`.
2. Create a file called `test.json` with the payload above.
3. Send it over to your Cribl Edge host: `cat test.json | nc <myCriblHost> 10001`

### Cribl Edge to Cribl.Cloud

This second example demonstrates using TCP JSON to send data from one Cribl Edge instance to a downstream Cribl.Cloud instance. We assume that the downstream Cloud instance uses Cribl.Cloud's **default** TCP JSON Source configuration.

So all the configuration happens on the upstream instance's [TCP JSON Destination](destinations-tcp-json). Replace the `<organizationId>` placeholder with the Org ID from your Cribl.Cloud portal.

#### TCP JSON Destination Configuration

On the upstream Cribl Edge instance's Destination, set the following field values to match the target Cloud instance's defaults:

##### General Settings

**Address**: `default.main.<organizationId>.cribl.cloud` – you can simply copy/paste your Cribl.Cloud portal's **Ingest Endpoint** here. With a Cribl.Cloud Enterprise plan, generalize the  `default.main` substring in this URL to `<groupName>.main` when sending to other Fleets.

**Port**: `10070`

##### TLS Settings (Client Side)

**Enabled**: Toggle on

**Validate server certs**: Toggle on

## Periodic Logging {#logging}

Cribl Edge logs metrics about incoming requests and ingested events once per minute.

These logs are stored in the `metrics.log` file. To view them in the UI, open the Source's **Logs** tab and choose **Worker Process X Metrics** from the drop-down, where **X** is the desired Worker Process.

This kind of periodic logging helps you determine whether a Source is in fact still healthy even when no data is coming in.

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_sources-troubleshooting.md]
