# Fabric Real-Time Intelligence


> If the **Cribl** data source is not yet available in Microsoft Fabric, follow the instructions in [Fabric Eventstreams and the Fabric Real-Time Intelligence Destination](destinations-eventstreams-fabric-real-time-intelligence) instead of this page.
>
{.box .warning}

Cribl Edge supports sending data to [Eventstreams](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/overview) in [Microsoft Fabric Real-Time Intelligence](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/overview).

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes
>
> Before you create a Fabric Real-Time Intelligence Destination in Cribl Edge, [create a **Cribl** data source in Microsoft Fabric](https://aka.ms/criblrtidoc). Microsoft Fabric will then provision the connection values (like the bootstrap server) that you must use when you configure a Fabric Real-Time Intelligence Destination in Cribl Edge.
>
{.box .info}

## Configuring Cribl Edge to Output to Microsoft Fabric Real-Time Intelligence {#configure-fabric-dest}

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**.
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: A unique name to identify this Fabric Real-Time Intelligence Destination. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Bootstrap server**: The value of the **Bootstrap server** setting for the **Cribl** data source in Microsoft Fabric.
   - **Topic name**:  The value of the **Topic name** setting for the **Cribl** data source in Microsoft Fabric. Can be overwritten using the [`__topicOut` field](#internal-fields).
3. Next, you can configure the following **Optional Settings**:
   - **Acknowledgments**:  Control the number of required acknowledgments. Defaults to `Leader`. Setting this control to `Leader` or `All` may significantly slow throughput. If the impact is too great for your use case, set this control to `None`.
   - **Record data format**: Format to use to serialize events before writing. Defaults to `JSON`.
     - `JSON` serializes the entire event from your Pipeline, including all metadata and enriched fields, into a single JSON object.
     - `Field _raw` sends only the contents of the `_raw` field as the payload. This option provides maximum throughput and minimal overhead. If using raw data, send the data in CSV or JSON format.
   - **Backpressure behavior**: Whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`.
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [TLS](#tls), [Authentication](#auth), [Processing](#processing), [Retries](#retries) and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### TLS Settings (Client Side) {#tls}

 **Enabled**: Defaults to toggled on.

 **Validate server certs**: Defaults to toggled on.

### Authentication {#auth}

Authentication parameters to use when connecting to Microsoft Fabric. Using [TLS](#tls_settings) is highly recommended.

**SASL mechanism**: SASL (Simple Authentication and Security Layer) authentication mechanism to use. Defaults to `PLAIN`, which exposes [Basic Authentication](#plain) options from the **SASL mechanism** settings in the **Cribl** data source in Microsoft Fabric. Select `OAUTHBEARER` to enable [OAuth Authentication](#oauthbearer) via a different set of options.

#### Basic Authentication {#plain}

Selecting the `PLAIN` SASL mechanism provides the options listed in this section.

**SASL JASS username**: The value is `$ConnectionString` (immutable).

**SASL JASS password**: The stored secret that references the **SASL JASS password-primary** or **SASL JASS password-secondary** in the **Cribl** data source in Microsoft Fabric. The secret can reside in Cribl Edge's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need a new secret.

#### OAuth Authentication {#oauthbearer}

Selecting the `OAUTHBEARER` SASL mechanism provides the options listed in this section.

**Microsoft Entra ID authentication endpoint**: Specifies the Microsoft Entra ID endpoint from which to acquire authentication tokens. Defaults to `https://login.microsoftonline.com`. You can instead select `https://login.microsoftonline.us` or `https://login.partner.microsoftonline.cn`.

**Client ID**: Enter the `client_id` to pass in the OAuth request parameter.

**Tenant identifier**: Enter your Microsoft Entra ID subscription's directory ID (tenant ID).

**Scope**: Enter the scope to pass in the OAuth request parameter. This will be of the form: `https://<Bootstrap-Server-Hostname>/.default`. For example, for a bootstrap server `myfabricserver.example.com:9093`, **Scope** would be `https://myfabricserver.example.com/.default`.

**Authentication method**: Use the buttons to select one of these options:
- **Secret**: Exposes a **Client secret (text secret)** drop-down, in which you can select a stored [secret](securing-secrets) that references the `client_secret`. A **Create** link is available to define a new secret.
- **Certificate**: Exposes a **Certificate** drop-down, in which you can select a stored [certificate](securing-ssl). A **Create** link is available to define a new cert.

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_group` - Cribl Edge Fleet that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_mode` - Distributed mode of the instance that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}

These settings provide flexibility in handling retries for failed messages, allowing you to balance between quick retries and avoiding excessive load on the system.

The default configuration starts with a 300 milliseconds retry interval, doubles the interval after each retry, and caps the maximum retry interval at 30 seconds. The system will attempt to retry the request up to five times before considering it a failure.

**Retry limit**: Maximum number of times to retry a failed request before the message fails. Defaults to `5`. Enter `0` to not retry at all. For example, if set to `5`, the system will attempt to retry the request up to five times before considering it a failure.

**Initial retry interval (ms)**: Initial value used to calculate the retry interval, in milliseconds. This value determines the starting point for the retry delay. The default (and minimum) is `300` ms (three seconds). The maximum allowed value is `600000` ms (10 minutes). For example, if set to `1000` ms, the first retry will occur after one second.

**Backoff multiplier**: Set the backoff multiplier (`2-20`) to control the retry frequency for failed messages. The multiplier is used in an exponential backoff formula to increase the delay between retries. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. For example, with an initial retry interval of `1000` ms and a multiplier of `2`, the retry intervals will be 1,000 ms, 2,000 ms, 4,000 ms, and so on. See the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.

**Backoff limit (ms)**: The maximum wait time for a retry, in milliseconds. This setting caps the exponential backoff delay to prevent excessively long wait times. The default (and minimum) value is `30000` ms (30 seconds), and the maximum is `180000` ms (180 seconds). For example, if the calculated retry interval exceeds 180,000 ms, the retry will occur after 180,000 ms instead.

### Advanced Settings {#advanced-tab}

**Record size limit (KB, uncompressed)**: Maximum size (KB) of each record batch before compression. Setting should be < `message.max.bytes` settings in bootstrap server. Defaults to `768`.

**Events-per-batch limit**: Maximum number of events in a batch before forcing a flush. Defaults to `1000`.

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Connection timeout (ms)**: Maximum time to wait for a successful connection. Defaults to `10000` ms (10 seconds). Valid range is `1000` to `3600000` ms (1 second to 1 hour).

**Request timeout (ms)**: Maximum time to wait for Kafka to respond to a successful request. Defaults to `60000` ms (1 minute).

**Authentication timeout (ms)**: Maximum time to wait for Kafka to respond to an authentication request. Defaults to `10000` (10 seconds).

**Reauthentication threshold (ms)**: Time window during which Cribl Edge can reautheticate if needed, measured backward from the moment when credentials are set to expire. Defaults to `10000` (10 seconds).

A small value for this setting, combined with high network latency, might prevent the Destination from reauthenticating before the bootstrap server closes the connection.

A large value might cause the Destination to send reauthentication messages too soon, wasting bandwidth.

The Kafka setting `connections.max.reauth.ms` controls the reuthentication threshold on the Kafka side.

## Working with Event Timestamps {#timestamps}

By default, when an incoming event reaches this Destination, the underlying Kafka JS library adds a `timestamp` field set to the current time at that moment. The library gets the current time from the `Date.now()` JavaScript method. Events that this Destination sends to downstream Kafka receivers include this `timestamp` field.

Some production situations require the `timestamp` value to be the time an incoming event was originally created by an upstream sender, not the current time of its arrival at this Destination. For example, suppose the events were originally created over a wide time range, and arrive at the Destination hours or days later. In this case, the original creation time might be important to retain in events Cribl Edge sends to downstream Kafka receivers.

Here is how to satisfy such a requirement:

1. In a Pipeline that routes events to this Destination, use an Eval Function to add a field named `__kafkaTime` and write the incoming event's original creation time into that field. The value of `__kafkaTime` **must** be in UNIX epoch time ([either seconds or milliseconds since the UNIX epoch](https://developer.mozilla.org/en-US/docs/Glossary/Unix_time) – both will work). For context, see [this explanation](/stream/sources-msk#how-criblstream-handles-the-_time-field) of the related fields `_time` and `__origTime`.
2. This Destination will recognize the `__kafkaTime` field and write its value into the `timestamp` field. This is similar to the way you can use the `__topicOut` field to overwrite a topic setting, as described [above](#general-settings).

If the `__kafkaTime` field is **not** present, the Destination will apply the default behavior (using `Date.now()`) described previously.

## Fabric Workspace Permissions

To [add a **Cribl** data source in Microsoft Fabric](https://aka.ms/criblrtidoc) and retrieve the values necessary to [configure](#configure-fabric-dest) the Fabric Real-Time Intelligence Destination in Cribl Edge, you must have **Contributor** (or higher) Fabric workspace permissions.

To use [`OAUTHBEARER` authentication](#oauthbearer), you need **Member** (or higher) Fabric workspace permissions so that you can add a service principal app to the workspace with **Contributor** (or higher) permissions.

See [Permission model](https://learn.microsoft.com/en-us/fabric/security/permission-model) in the official Microsoft documentation for more information.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in forwarding data to a Destination.

Fields for this Destination:

  * `__topicOut`
  * `__key`
  * `__headers`
  * `__keySchemaIdOut`
  * `__valueSchemaIdOut`

## Troubleshooting {#troubleshooting}

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.
