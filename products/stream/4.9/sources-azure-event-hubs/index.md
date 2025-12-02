# Azure Event Hubs Source


Cribl Stream supports receiving data records from [Azure Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/).

> Type: **Pull**  |  TLS Support: **YES** (secure API) | Event Breaker Support: **No**
> 
> Azure Event Hubs uses a binary protocol over TCP. It does not support HTTP proxies, so Cribl Stream must receive events directly from senders. You might need to adjust your firewall rules to allow this traffic.
>
> For configuration examples, see [Resources](#resources) below.
>
{.box .info}

## Configure Cribl Stream to Receive Data from Azure Event Hubs

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**. 
    - **Description**: Optionally, enter a description.
    - **Brokers**: List of Event Hubs Kafka brokers to connect to – for example, `yourdomain.servicebus.windows.net:9093`. Get the hostname from the host portion of the primary or secondary connection string in Shared Access Policies. If omitted, the port will default to `9093`.
    - **Event Hub name**: The name of the Event Hub (a.k.a. Kafka Topic) to subscribe to.
3. Next, you can configure the following **Optional Settings**:
    - **Group ID**:  The name of the consumer group that includes this Cribl Stream instance. Defaults to `Cribl`.
    - **From beginning**: Whether to start reading from the earliest available data. Relevant only during initial subscription. Defaults to `Yes`.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the  [TLS](#tls), [Authentication](#authentication-settings), [Processing](#processing) and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

> To prevent excessive Kafka rebalancing and reduced throughput, each **Group ID** that you specify here should be subscribed to only one Kafka Topic – that is, only to the single Topic you specify in **Event Hub name**. This has a few implications:
> - The **Group ID** should be something other than `$Default`, especially if Event Hubs are stored In shared accounts, where the `$Default` group might be subscribed to other Topics.
> - The **Group ID** should also be unique for each of your Azure Event Hubs, Kafka, and Confluent Cloud Sources.
> - You should configure a **separate** Azure Event Hubs Source for each Group:Topic pair whose events you want to subscribe to.
> 
> For details, see [Controlling Rebalancing](#controlling-rebalancing).
>
{.box .warning}

### TLS Settings (Client Side) {#tls}

**Enabled**: Defaults to `Yes`. 

**Validate server certs**: Whether to reject connections to servers without signed certificates. Defaults to `Yes`.

### Authentication Settings {#authentication-settings}

**Enabled**: With the default `Yes` setting, this section's remaining settings are displayed, and all are required settings.

**SASL mechanism**: SASL (Simple Authentication and Security Layer) authentication mechanism to use with Kafka brokers. Defaults to `PLAIN`, which exposes [Basic Authentication](#plain) options that rely on Azure Event Hubs connection strings. Select `OAUTHBEARER` to enable [OAuth Authentication](#oauthbearer) via a different set of options.

#### Basic Authentication {#plain}

Selecting the `PLAIN` SASL mechanism provides the options listed in this section.

**Username**: The username for authentication. For Event Hubs, this should always be `$ConnectionString`.

**Authentication method**: Use the buttons to select one of these options:
- **Manual**: Use this default option to enter your Event Hubs connection string's primary or secondary key from the Event Hubs workspace. Exposes a **Password** field for this purpose. 
- **Secret**: This option exposes a **Password (text secret)** drop-down, in which you can select a stored secret that references an Event Hubs connection string. The secret can reside in Cribl Stream's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need a new secret.

##### Connection String Format

Either authentication method above uses an Azure Event Hubs connection string in this format:

`Endpoint=sb://<FQDN>/;SharedAccessKeyName=<your‑shared-access‑key-name>;SharedAccessKey=<your‑shared-access‑key-value>`

A fictitious example is:

`Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

#### OAuth Authentication {#oauthbearer}

Selecting the `OAUTHBEARER` SASL mechanism provides the options listed in this section.

**Microsoft Entra ID authentication endpoint**: Specifies the Microsoft Entra ID endpoint from which to acquire authentication tokens. Defaults to `https://login.microsoftonline.com`. You can instead select `https://login.microsoftonline.us` or `https://login.partner.microsoftonline.cn`.

**Client ID**: Enter the `client_id` to pass in the OAuth request parameter.

**Tenant identifier**: Enter your Microsoft Entra ID subscription's directory ID (tenant ID).

**Scope**: Enter the scope to pass in the OAuth request parameter. This will be of the form: `https://<Event‑Hubs‑Namespace-Host-name>/.default`. (For example, for an Event Hubs Namespace > Host name: `goatyoga.servicebus.windows.net`, **Scope**: `https://goatyoga.servicebus.windows.net/.default`.)

**Authentication method**: Use the buttons to select one of these options:
- **Manual**: This default option exposes a **Client secret** field, in which to directly enter the `client_secret` to pass in the OAuth request parameter.
- **Secret**: Exposes a **Client secret (text secret)** drop-down, in which you can select a stored [secret](securing-secrets) that references the `client_secret`. A **Create** link is available to define a new secret.
- **Certificate**: Exposes a **Certificate name** drop-down, in which you can select a stored [certificate](securing-ssl). A **Create** link is available to define a new cert.

### Processing Settings {#processing}

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Retries

These settings provide flexibility in handling retries for failed messages, allowing you to balance between quick retries and avoiding excessive load on the system.

The default configuration starts with a 300 milliseconds retry interval, doubles the interval after each retry, and caps the maximum retry interval at 30 seconds. The system will attempt to retry the request up to five times before considering it a failure.

**Max retries**: Maximum number of times to retry a failed request before the message fails. Defaults to `5`. Enter `0` to not retry at all. For example, if set to `5`, the system will attempt to retry the request up to five times before considering it a failure.

**Initial retry interval (ms)**: Initial value used to calculate the retry interval, in milliseconds. This value determines the starting point for the retry delay. The default (and minimum) is `300` ms (three seconds). The maximum allowed value is `600000` ms (10 minutes). For example, if set to `1000` ms, the first retry will occur after one second.

**Backoff multiplier**: Set the backoff multiplier (`2-20`) to control the retry frequency for failed messages. The multiplier is used in an exponential backoff formula to increase the delay between retries. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. For example, with an initial retry interval of `1000` ms and a multiplier of `2`, the retry intervals will be 1,000 ms, 2,000 ms, 4,000 ms, and so on. See the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.

**Backoff limit (ms)**: The maximum wait time for a retry, in milliseconds. This setting caps the exponential backoff delay to prevent excessively long wait times. The default (and minimum) value is `30000` ms (30 seconds), and the maximum is `180000` ms (180 seconds). For example, if the calculated retry interval exceeds 180,000 ms, the retry will occur after 180,000 ms instead.

### Advanced Settings {#advanced-tab}

Use these settings to fine-tune Cribl Stream's integration with Event Hubs Kafka brokers. For details, see Azure Event Hubs' [recommended configuration](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md) documentation. If you are unfamiliar with these parameters, contact Cribl Support to understand the implications of changing the defaults.

**Heartbeat interval (ms)**: Expected time between heartbeats to the consumer coordinator when using Kafka's group management facilities. (Corresponds to `heartbeat.interval.ms` in the Kafka domain.) Value must be lower than `sessionTimeout`, and typically should not exceed 1/3 of the `sessionTimeout` value. Defaults to `3000` ms (3 seconds).

**Session timeout (ms)**: Timeout used to detect client failures when using Kafka's group management facilities. (Corresponds to `session.timeout.ms` in the Kafka domain.) If the client sends the broker no heartbeats before this timeout expires, the broker will remove this client from the group, and will initiate a rebalance. Value must be lower than `rebalanceTimeout`. Defaults to `30000` ms (30 seconds).

**Rebalance timeout (ms)**: Maximum allowed time for each worker to join the group after a rebalance has begun. (Corresponds to `rebalance.timeout.ms` in the Kafka domain.) If this timeout is exceeded, the coordinator broker will remove the worker from the group. Defaults to `60000` ms (1 minute).

**Connection timeout (ms)**: Maximum time to wait for a successful connection. Defaults to `10000` ms (10 seconds). Valid range is `1000` to `3600000` ms (1 second to 1 hour).

**Request timeout (ms)**: Maximum time to wait for a successful request. Defaults to `60000` ms (1 minute).

**Authentication timeout (ms)**: Maximum time to wait for Kafka to respond to an authentication request. Defaults to `1000` (1 second).

**Reauthentication threshold (ms)**: If the broker requires periodic reauthentication, this setting defines how long before the reauthentication timeout Cribl Stream initiates the reauthentication. Defaults to `10000` (10 seconds).

A small value for this setting, combined with high network latency, might prevent the Source from reauthenticating before the Kafka broker closes the connection.

A large value might cause the Source to send reauthentication messages too soon, wasting bandwidth.

The Kafka setting `connections.max.reauth.ms` controls the reuthentication threshold on the Kafka side.

**Offset commit interval (ms)**: How often, in milliseconds, to commit offsets. If both this field and the **Offset commit threshold** are empty, Cribl Stream will commit offsets after each batch. If both fields are set, Cribl Stream will commit offsets when either condition is met.

**Offset commit threshold**: The number of events that will trigger an offset commit. If both this field and the **Offset commit interval** are empty, Cribl Stream will commit offsets after each batch. If both fields are set, Cribl Stream will commit offsets when either condition is met.

**Max bytes per partition**: The maximum amount of data that the server will return per partition. Must equal or exceed the maximum message size the server allows. (Otherwise, the producer will be unable to send messages larger than the consumer can fetch.) If not specified, defaults to `1048576`.

**Max bytes**: Maximum amount of bytes to accumulate in the response. The default is `10485760 (10 MB)`.

**Maximum number of errors per socket**: Maximum number of consecutive request errors that can occur on a single socket connection before the connection is discarded and reestablished. This mitigates issues with idle connections, particularly those used for sending heartbeats, thereby reducing the number of rebalances in a consumer group. Default is `0`, which disables this feature. Accepts values `1-100`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Minimize duplicates**: Optionally, toggle to `Yes` to start only one consumer for each topic partition. This reduces duplicates. 

> If you observe an excessive number of group rebalances, and/or you observe consumers not regularly pulling messages, try increasing the values of **Heartbeat interval**, **Session timeout**, and **Rebalance timeout**.
>
{.box .success}

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__inputId`
  * `__topicIn` (indicates the Kafka topic that the event came from)
  * `__partition`
  * ` __schemaId` (when using [Azure Schema Registry](https://docs.microsoft.com/en-us/azure/event-hubs/schema-registry-overview))
  * `__key` (when using Schema Registry)
  * `__headers` (when using Schema Registry)
  * `__keySchemaIdIn` (when using Schema Registry)
  * `__valueSchemaIdIn` (when using Schema Registry) 

## How Cribl Stream Pulls Data

Azure Event Hubs treat all the Worker Nodes as members of a Consumer Group, and each Worker gets its share of the load from Azure Event Hubs. This is the same process as normal Kafka. By default, Workers will poll every 5 seconds. In the case of Leader failure, Worker Nodes will continue to receive data as normal.



## Randomized Partition Assignment

In Cribl Stream v.4.8.2 and newer, partition assignment is randomized. The Worker Processes selected to consume data during partition assignment are selected randomly, reducing the possibility of uneven load distribution when there are fewer partitions than Worker Processes. Note that *random* assignment is not the same as *balanced* assignment, and even distribution of work is not guaranteed.

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

## Resources {#resources}

For examples of configuring Cribl Stream to interoperate with Azure services, see these guides:

- [Azure Event Hubs Integrations](/stream/usecase-azure-event-hubs/).

- [SSO with Microsoft Entra ID and OIDC (On-Prem)](/stream/usecase-azure-ad/).

- [SSO with Microsoft Entra ID and SAML (Cribl.Cloud)](/stream/saml-azure-setup/).

- [SSO with Microsoft Entra ID and SAML (On-Prem)](/stream/usecase-sso-saml-entra/).

Also see these Packs on the [Cribl Packs Dispensary](https://packs.cribl.io/), which provide processing Pipelines to transform and optimize incoming Azure data. You can directly import these Pipelines and adapt them to your needs:

- [Microsoft Azure NSG Source](https://packs.cribl.io/packs/cribl-azure-nsg-source) to Common Schema.

- [Azure NSG Flow Logs](https://packs.cribl.io/packs/cribl-azure-nsg-flow-logs).

-  [Azure NSG Flow Logs to OCSF](https://packs.cribl.io/packs/cribl-Azure-NSG-OCSF-Flow-Logs).

-  [Azure Audit Logs to OCSF](https://packs.cribl.io/packs/azure-audit-log-ocsf).

- [Microsoft Office Activity & Azure Logs](https://packs.cribl.io/packs/microsoft-activity).

## Troubleshooting

[Snippet not found: content/shared/4.9/snippets/_sources-troubleshooting.md]

### Common Issue

[Snippet not found: content/shared/4.9/snippets/_common-errors-kafka.md]