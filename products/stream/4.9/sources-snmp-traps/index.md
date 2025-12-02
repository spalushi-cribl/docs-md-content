# SNMP Trap Source


Cribl Stream supports receiving data from SNMP Traps.

> Type: **Push**  |  TLS Support: **NO** | Event Breaker Support: **No**
>
{.box .info}

## Configure Cribl Stream to Receive SNMP Traps

> Cribl Stream, except in Cribl.Cloud, ships with an SNMP Trap Source preconfigured to listen on port `9162`. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

In Cribl Stream, configure SNMP Traps:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this Source definition. The default Source is prefilled with the value `in_snmp_trap`. which can't be changed via the UI.  If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
    - **Description**: Optionally, enter a description.
    - **Address**: Address to bind on. Defaults to `0.0.0.0` (all addresses).
    - **UDP Port**: Port on which to receive SNMP traps. Defaults to `162`.
        > Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
        > This results from restrictions of the UDP protocol.
        >
        > To minimize the risk of data loss, deploy a customer-managed (hybrid) Stream Worker Group
        > with Worker Nodes as close as possible to the UDP senders.
        > Cribl also recommends tuning the OS UDP buffer size.
        >
        {.box .cloud}
3. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#auth), [Persistent Queue Settings](#pq), [Processing](#processing) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### Authentication Settings {#auth}

SNMPv3 authentication provides secure access to devices through optional user authentication and decryption of incoming data packets. Cribl Stream lets you choose any of the following levels of security:

- User name checking without authentication.
- User authentication without privacy.
- User authentication with privacy.

**SNMPv3 authentication**: Toggle to `Yes` to enable SNMPv3 authentication and reveal authentication parameters. Defaults to `No`.

**Allow unmatched traps**: Toggle to `Yes` to pass through traps that don't match any of the configured users. Cribl Stream will not attempt to authenticate or decrypt these traps. When toggled to `No` (default), Cribl Stream drops traps without a correctly configured user name.

**v3 Name**: Enter the SNMPv3 user name (required). Multiple users are supported.

You must configure at least one user to enable SNMPv3 authentication. If desired, you can enter separate user credentials for each SNMPv3 user. The authentication protocol, authentication key, privacy protocol, and privacy key can be unique for each user.

For authentication to succeed, the user name and authentication protocol and key must match in the Source and the sending device configuration.

For decryption to succeed, the user name, authentication protocol and key, and the privacy protocol and key must match in the Source and the sending device configuration.

**Authentication protocol**: Select the authentication protocol required for your use case. If you select `None`, Cribl Stream will only check the user name without authenticating the user or decrypting the data. Otherwise, Cribl Stream uses the authentication protocol you specify and the key you provide to authenticate the user.

Options include:
- `None`
- `MD5`
- `SHA1`
- `SHA224`
- `SHA256`
- `SHA384`
- `SHA512`

**v3 authentication key**: Enter the authentication key.

Choosing an authentication protocol also allows you to select an optional privacy protocol to decrypt incoming packets.

Cribl Stream logs authentication failures at the `debug` level.

**Privacy protocol**: Select the privacy protocol required for your use case. If you select `None`, Cribl Stream authenticates the user without decrypting the incoming data. Otherwise, Cribl Stream uses the privacy protocol you specify and the key you provide to decrypt the data.

Options include:
- `None`
- `AES128`
- `AES256b (Blumenthal)`
- `AES2556r (Reeder)`

For every successfully decrypted trap, Cribl Stream adds `__didDecrypt: true` to the Event. Cribl Stream logs decryption failures at the `debug` level.

**v3 privacy key**: Enter the privacy key.

**Add user**: Click to add a new set of user credentials for SNMPv3 authentication.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.9/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Fields

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality.

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**Include varbind types**: Toggle on to include varbind types in the `varbinds` field on each event. This is required for the [SNMP Trap Serialize Function](snmp-trap-serialize-function) to serialize compliant events into SNMP traps before sending them to an SNMP Trap Destination. The field is parsed to an array of objects like, `[{oid: string, value: any, type: number}]`. Off by default.

**IP allowlist regex**: Regex matching IP addresses that are allowed to send data. Defaults to `.*`, that is, all IPs.

**Max buffer size (events)**: Maximum number of events to buffer when downstream is blocking. Defaults to `1000`.

**UDP socket buffer size (bytes)**: Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Min: `256`. Max: `4294967295`.

It may also be necessary to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source's data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__didDecrypt`: Set to `true` for every trap that is successfully decrypted.
* `__final`
* `__inputId`
* `__snmpRaw`: Buffer containing Raw SNMP packet
* `__snmpVersion`: Acceptable values are `0`, `2` , or `3`. These respectively indicate SNMP v1, v2c, and v3.
* `__srcIpPort` : In this particular Source, this field uses a pipe (`|`) symbol to separate the source IP address and the port, in this format:
`event.__srcIpPort = ${rInfo.address}|${rInfo.port};`
* `_time`

## Considerations for Working with SNMP Trap Data

* It's possible to work with SNMP metadata (that is, we'll decode the packet). Options include dropping, routing, and so on.

* SNMP packets can be forwarded to other SNMP destinations. However, the contents of the incoming packet **cannot** be modified – that is, we'll forward the packets verbatim as they came in.

* SNMP packets can be forwarded to non-SNMP destinations (for example, Splunk, Syslog, S3, and so on).

* Non-SNMP input data **cannot** be sent to SNMP destinations.

## Troubleshooting

[Snippet not found: content/shared/4.9/snippets/_sources-troubleshooting.md]
