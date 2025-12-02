# Kafka


Cribl Stream supports receiving data records from a [Kafka](https://kafka.apache.org/) cluster. This Source automatically detects compressed data in `Gzip`, `Snappy`, or `LZ4` format, and automatically decompresses the data upon ingesting it.
.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **No**
> 
> Kafka uses a binary protocol over TCP. It does not support HTTP proxies, so Cribl Stream must receive events directly from senders. You might need to adjust your firewall rules to allow this traffic.
>
{.box .info}

## Configuring Cribl Stream to Receive Data from Kafka Topics 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select  [**Pull** > ] **Kafka**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select  [**Pull** > ] **Kafka**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Source definition.

**Brokers**: List of Kafka brokers to use, e.g., `localhost:9092`.

**Topics**: Enter the name(s) of topics to subscribe to. Press `Enter`/`Return` between multiple entries.

> To optimize performance and prevent excessive rebalancing, Cribl recommends subscribing each Kafka Source to only one topic. To subscribe to multiple topics, consider creating a dedicated Kafka Source for each one. 
> 
> For the same reason, the **Group ID** (below) should be unique for each of your Kafka, Confluent Cloud, and Azure Event Hubs Sources. For details, see [Controlling Rebalancing](#controlling-rebalancing).
>
{.box .warning}

### Optional Settings

**Group ID**: The name of the consumer group to which this Cribl Stream instance belongs.

**From beginning**: Whether to start reading from the earliest available data. Relevant only during initial subscription. Defaults to `Yes`.   

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Client Side) {#tls_client}

**Enabled:** defaults to `No`. When toggled to `Yes`:

**Autofill?**: This setting is experimental.

**Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `No`.

**Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

**Certificate name**: The name of the predefined certificate.

**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.

**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.

**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 

**Passphrase**: Passphrase to use to decrypt private key.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Authentication

This section governs SASL (Simple Authentication and Security Layer) authentication to use when connecting to brokers.

**Enabled**: Defaults to `No`. When toggled to `Yes`:
 
**SASL mechanism**:  Use this drop-down to select the SASL authentication mechanism to use. The mechanism you select determines the controls displayed below.

#### PLAIN, SCRAM-256, or SCRAM-512 

With any of these authentication mechanisms, select one of the following buttons:

**Manual**: Displays **Username** and **Password** fields to enter your Kafka credentials directly.

**Secret**: This option exposes a **Credentials secret** drop-down in which you can select a [stored text secret](/stream/securing-and-monitoring#secrets) that references your Kafka credentials. A **Create** link is available to store a new, reusable secret.

#### GSSAPI/Kerberos 

Selecting Kerberos as the authentication mechanism displays the following options:

**Keytab location**: Enter the location of the keytab file for the authentication principal.

**Principal**: Enter the authentication principal, e.g.: `kafka_user@example.com`.

**Broker service class**: Enter the Kerberos service class for Kafka brokers, e.g.: `kafka`.

### Schema Registry

This section governs Kafka Schema Registry authentication for [Avro‑encoded](https://www.confluent.io/blog/avro-kafka-data/)  data with a schema stored in the Confluent Schema Registry.

**Enabled:** defaults to `No`. When toggled to `Yes`, displays the following controls:

**Schema registry URL**:  URL for access to the Confluent Schema Registry. (E.g., `http://<hostname>:8081`.)

**TLS enabled**: When toggled to `Yes,` displays the following TLS settings for the Schema Registry.

> These have the same format as the [TLS Settings (Client Side)](#tls_client) above.
>
{.box .info}

* **Validate server certs**: Require client to reject any connection that is not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to **No**.

* **Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

* **Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

* **Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

* **Certificate name**: The name of the predefined certificate.

* **CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.

* **Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.

* **Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 

* **Passphrase**: Passphrase to use to decrypt private key.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.


### Advanced Settings

Use these settings to fine-tune Cribl Stream's integration with Kafka topics. If you are unfamiliar with these parameters, contact Cribl Support to understand the implications of changing the defaults.

**Heartbeat interval (ms)**: Expected time between heartbeats to the consumer coordinator when using Kafka's group management facilities. Value must be lower than `sessionTimeout`, and typically should not exceed 1/3 of the `sessionTimeout` value. Defaults to `3000` ms (3 seconds). For details, see the [Kafka documentation](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms).

**Session timeout (ms)**: Timeout used to detect client failures when using Kafka's group management facilities. If the client sends the broker no heartbeats before this timeout expires, the broker will remove this client from the group, and will initiate a rebalance. Value must be between the broker's configured `group.min.session.timeout.ms` and `group.max.session.timeout.ms`. Defaults to `30000` ms (30 seconds). For details, see the [Kafka documentation](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms).

**Rebalance timeout (ms)**: Maximum allowed time for each worker to join the group after a rebalance has begun. If the timeout is exceeded, the coordinator broker will remove the worker from the group.  Defaults to `60000` ms (1 minute). For details, see the [Kafka documentation](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms).

**Connection timeout (ms)**: Maximum time to wait for a successful connection. Defaults to `10000` ms (10 seconds). Valid range is `1000` to `3600000` ms (1 second to 1 hour). For details, see the [Kafka documentation](https://kafka.apache.org/documentation/#brokerconfigs_sasl.login.connect.timeout.ms).

**Request timeout (ms)**: Maximum time to wait for a successful request. Defaults to `60000` ms (1 minute). For details, see the [Kafka documentation](https://kafka.apache.org/documentation/#brokerconfigs_offsets.commit.timeout.ms).

**Max retries**: Maximum number of times to retry a failed request before the message fails. Defaults to `5`; enter `0` to not retry at all.

**Authentication timeout (ms)**: Maximum time to wait for Kafka to respond to an authentication request. Defaults to `1000` (1 second).

**Reauthentication threshold (ms)**: If the broker requires periodic reauthentication, this setting defines how long before the reauthentication timeout Cribl Stream initiates the reauthentication. Defaults to `10000` (10 seconds).

A small value for this setting, combined with high network latency, might prevent the Source from reauthenticating before the Kafka broker closes the connection.

A large value might cause the Source to send reauthentication messages too soon, wasting bandwidth.

The Kafka setting `connections.max.reauth.ms` controls the reuthentication threshold on the Kafka side.

**Offset commit interval (ms)**: How often, in milliseconds, to commit offsets. If both this field and the **Offset commit threshold** are empty, Cribl Stream will commit offsets after each batch. If both fields are set, Cribl Stream will commit offsets when either condition is met.

**Offset commit threshold**: The number of events that will trigger an offset commit. If both this field and the **Offset commit interval** are empty, Cribl Stream will commit offsets after each batch. If both fields are set, Cribl Stream will commit offsets when either condition is met.

**Max bytes per partition**: The maximum amount of data that the server will return per partition. Must equal or exceed the maximum message size the server allows. (Otherwise, the producer will be unable to send messages larger than the consumer can fetch.) If not specified, defaults to `1048576`.

**Max bytes**: Maximum amount of bytes to accumulate in the response. The default is `10485760 (10 MB)`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

> If you observe an excessive number of group rebalances, and/or you observe consumers not regularly pulling messages, try increasing the values of **Heartbeat interval**, **Session timeout**, and **Rebalance timeout**.
>
{.box .success}

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__final`
* `__headers` (when present in the record)
* `__inputId`
* `__key` (when using Schema Registry)
* `__keySchemaIdIn` (when using Schema Registry)
* `__origTime`
* `__partition`
* `__raw`
* `__schemaId` (when using Schema Registry)
* `__topicIn` (indicates the Kafka topic that the event came from; see `__topicOut` in our [Kafka Destination](destinations-kafka) documentation)
* `__valueSchemaIdIn` (when using Schema Registry) 
* `_raw`
* `_time`

## How Cribl Stream Pulls Data

Kafka treats all the Worker Nodes as members of a Consumer Group, and Kafka manages each Node’s data load. By default, Workers will poll every 5 seconds. In the case of Leader failure, Worker Nodes will continue to receive data as normal.



## How Cribl Stream Handles the `_time` Field

Events that the Kafka Source emits always contain a `_time` field, and sometimes also an `__origTime` field. Here's how Cribl Stream determines what to send:

* If the incoming Kafka message contains no `_time` field, the message's timestamp becomes the value of the emitted event's `_time` field.
* If the incoming Kafka message contains a `_time` field whose value is a timestamp in UNIX epoch time, that timestamp becomes the value of the emitted event's `_time` field.
* If the incoming Kafka message contains a `_time` field whose value is **not** a timestamp in UNIX epoch time (e.g., an ISO or UTC timestamp), that becomes the value of the emitted event's `__origTime` field, **and** the message's timestamp becomes the value of the emitted event's `_time` field.

## Controlling Rebalancing {#controlling-rebalancing}

When you configure multiple Sources that subscribe to different topics, but all belong to the same consumer group, a state change affecting any Source in this consumer group will affect all the other Sources. Examples of state changes include: deploying new configs, adding or removing Worker Processes, or Worker Processes crashing.

Here’s an example – three Sources, three different topics, all in one consumer group:
- `Source_1 - Topic_1 - ConsumerGroup1`
- `Source_2 - Topic_2 - ConsumerGroup1`
- `Source_3 - Topic_3 - ConsumerGroup1`

Imagine that Source 1 undergoes a state change event, such as a Worker Process crash. Source 2 and Source 3 will rebalance – stopping data flow until the rebalance completes. 

#### Shared Worker Group Mitigation

If Sources that share a consumer group all deploy as part of the same Worker Group, changes will have smaller side effects than when Sources are spread across different Worker Groups. (Conversely, imagine a configuration where deploying new configs for Worker Group 1 caused rebalancing of topics in worker Worker Group 2. This spillover would be especially undesirable.)

#### Bottom Line

Changes to any member of a consumer group affect all other members of that consumer group. To prevent this undesired behavior, make sure to use a unique **Group ID** for each Kafka, Confluent Cloud, and Azure Event Hubs Source.