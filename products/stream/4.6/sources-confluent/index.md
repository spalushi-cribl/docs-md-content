# Confluent Cloud Source


Cribl Stream supports receiving [Kafka](https://kafka.apache.org/documentation/) topics from the [Confluent Cloud](https://docs.confluent.io/cloud/current/overview.html) managed Kafka platform. As of Cribl Stream v.3.3, this Source automatically detects compressed data in `Gzip`, `Snappy`, or `LZ4` format.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **No**
> 
> Confluent Cloud uses a binary protocol over TCP. It does not support HTTP proxies, so Cribl Stream must receive events directly from senders. You might need to adjust your firewall rules to allow this traffic.
> 
> Your Confluent Cloud permissions should include `Consumer Group: Read`.
>
{.box .info}

## Ingesting Kafka Topics from Confluent Cloud

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Confluent Cloud**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] **Confluent Cloud**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.

**Brokers**: List of Confluent Cloud brokers to use, for example, `myAccount.confluent.cloud:9092`.

**Topics**: Enter the name(s) of topics to subscribe to. Press `Enter`/`Return` between multiple entries.

> To optimize performance and prevent excessive rebalancing, Cribl recommends subscribing each Confluent Cloud Source to only one topic. To subscribe to multiple topics, consider creating a dedicated Confluent Cloud Source for each one. 
> 
> For the same reason, the **Group ID** (below) should be unique for each of your Confluent Cloud, Kafka, and Azure Event Hubs Sources. For details, see [Controlling Rebalancing](#controlling-rebalancing).
>
{.box .warning}

### Optional Settings

**Group ID**:  The name of the [consumer group](https://docs.confluent.io/platform/current/clients/consumer.html) to which this Cribl Stream instance belongs.

**From beginning**: Whether to start reading from the earliest available data. Relevant only during initial subscription. Defaults to `Yes`.   

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### TLS Settings (Client Side) {#tls_client}

**Enabled:** defaults to `No`. When toggled to `Yes`:

**Autofill?**: This setting is experimental.

**Validate client certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (such as the system's CA). Defaults to `Yes`.

**Server name (SNI)**: Leave this field blank. See [Connecting to Kafka](#connecting) below.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

**Certificate name**: The name of the predefined certificate.

**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.

**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.

**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 

**Passphrase**: Passphrase to use to decrypt private key.

### Authentication

This section documents the SASL (Simple Authentication and Security Layer) authentication settings to use when connecting to brokers. Using [TLS](#tls_client) is highly recommended.

**Enabled**: Defaults to `No`. When toggled to `Yes`:

* **SASL mechanism**:  Select the SASL (Simple Authentication and Security Layer) authentication mechanism to use. Defaults to `PLAIN`. `SCRAM‑SHA‑256`, `SCRAM‑SHA‑512`, and `GSSAPI/Kerberos` are also available. The mechanism you select determines the controls displayed below.

#### PLAIN, SCRAM-256, or SCRAM-512 

With any of these SASL mechanisms, select one of the following buttons:

**Manual**: Displays **Username** and **Password** fields to enter your Kafka credentials directly.

**Secret**: This option exposes a **Credentials secret** drop-down in which you can select a [stored text secret](/stream/securing-and-monitoring#secrets) that references your Kafka credentials. A **Create** link is available to store a new, reusable secret.

#### GSSAPI/Kerberos 

Selecting Kerberos as the authentication mechanism displays the following options:

**Keytab location**: Enter the location of the keytab file for the authentication principal.

**Principal**: Enter the authentication principal, for example: `kafka_user@example.com`.

**Broker service class**: Enter the Kerberos service class for Kafka brokers, for example: `kafka`.

> You will also need to set up your environment and configure the Cribl Stream host for use with Kerberos. See [Kafka Authentication with Kerberos](/stream/usecase-kafka-kerberos) for further detail.
>
{.box .success}

### Schema Registry

This section governs Confluent Schema Registry Authentication for [Avro-encoded](https://www.confluent.io/blog/avro-kafka-data/)  data with a schema stored in the Confluent Schema Registry.

**Enabled:** defaults to `No`. When toggled to `Yes`, displays the following controls:

**Schema registry URL**:  URL for access to the Confluent Schema Registry (for example, `http://<hostname>:8081`).

**TLS enabled**: defaults to `No`. When toggled to `Yes,` displays the following TLS settings for the Schema Registry (in the same format as the [TLS Settings (Client Side)](#tls_client) above):

* **Validate server certs**: Require client to reject any connection that is not authorized by a CA in the **CA certificate path**, or by another trusted CA (such as the system's CA). Defaults to `No`.

* **Server name (SNI)**: Leave this field blank. See [About Kafka and TLS](#kafka-tls) below.

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

> If you observe an excessive number of group rebalances, and/or you observe consumers not regularly pulling messages, try increasing the values of **Heartbeat interval**, **Session timeout**, and **Rebalance timeout**.
>
{.box .success}

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

### Connecting to Kafka {#connecting} 

> ##### Leave the **TLS Settings** > **Server name (SNI)** field blank
>
> In Cribl Stream's Kafka-based Sources and Destinations (including this one), the Kafka library that Cribl Stream uses manages SNI (Server Name Indication) without any input from Cribl Stream. Therefore, you should leave the **TLS Settings** > **Server name (SNI)** field blank.
> 
> Setting this field in the Cribl Stream UI can cause traffic to be routed to the wrong brokers, because it interferes with the Kafka library's operation. This is especially important for [Confluent Cloud Dedicated clusters](https://docs.confluent.io/cloud/current/clusters/cluster-types.html), which rely on SNI – as managed by the Kafka library – for routing.
>
{.box .warning}

Connecting to a Kafka cluster entails working with hostnames for **brokers** and **bootstrap servers**.

Brokers are servers that comprise the storage layer in a Kafka cluster. Bootstrap servers handle the initial connection to the Kafka cluster, and then return the list of brokers. A broker list can run into the hundreds. Every Kafka cluster has a `bootstrap.servers` [property](https://www.ibm.com/docs/en/iis/11.7?topic=kafka-setting-up-server), defined as either a single `hostname:port` K-V pair, or a list of them. If Cribl Stream tries to connect via one bootstrap server and that fails, Cribl Stream then tries another one on the list.

In the **General Settings** > **Brokers** list, you can enter either the hostnames of brokers that your Kafka server has been configured to use, or, the hostnames of one or more bootstrap servers. If Kafka returns a list of brokers that's longer than the list you entered, Cribl Stream keeps the full list internally. Cribl Stream neither saves the list nor makes it available in the UI. The connection process simply starts at the beginning whenever the Source or Destination is started.

Here's an overview of the connection process:

1. From the **General Settings** > **Brokers** list – where each broker is listed as a hostname and port – Cribl Stream takes a hostname and resolves it to an IP address.

2. Cribl Stream makes a connection to that IP address. Notwithstanding the fact that Cribl Stream resolved **one** particular hostname to that IP address, there may be **many** services running at that IP address – each with its own distinct hostname.

3. Cribl Stream establishes TLS security for the connection.

Although SNI is managed by the Kafka library rather than in the Cribl Stream UI, you might want to know how it fits into the connection process. The purpose of the SNI is to specify one hostname – that is, service – among many that might be running on a given IP address within a Kafka cluster. Excluding the other services is one way that TLS makes the connection more secure.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__inputId`
  * `__topicIn` (indicates the Confluent Cloud topic that the event came from; see `__topicOut` in our [Confluent Cloud Destination](destinations-Confluent Cloud) documentation)
  * `__partition`
  * ` __schemaId` (when using Schema Registry)

## How Cribl Stream Pulls Data

Confluent Cloud treats all the Worker Nodes as members of a Consumer Group, and Confluent Cloud manages each Node’s data load. By default, Workers will poll every 5 seconds. In the case of Leader failure, Worker Nodes will continue to receive data as normal.



## How Cribl Stream Handles the `_time` Field

Events that the Kafka Source emits always contain a `_time` field, and sometimes also an `__origTime` field. Here's how Cribl Stream determines what to send:

* If the incoming Kafka message contains no `_time` field, the message's timestamp becomes the value of the emitted event's `_time` field.
* If the incoming Kafka message contains a `_time` field whose value is a timestamp in UNIX epoch time, that timestamp becomes the value of the emitted event's `_time` field.
* If the incoming Kafka message contains a `_time` field whose value is **not** a timestamp in UNIX epoch time (for example, an ISO or UTC timestamp), that becomes the value of the emitted event's `__origTime` field, **and** the message's timestamp becomes the value of the emitted event's `_time` field.

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