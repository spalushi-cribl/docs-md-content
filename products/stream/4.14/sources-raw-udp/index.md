# UDP (Raw) Source



Cribl Stream supports receiving raw, unparsed data via UDP.

> Type: **Push**  |  TLS Support: **NO** | Event Breaker Support: **NO**
>
{.box .info}

## Configure Cribl Stream to Receive Raw UDP Data

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this raw UDP Source definition.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter the hostname/IP to listen for raw UDP data. For example: `localhost`, `0.0.0.0`, or `::`.
    - **Port**: Enter the port number.
3. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Persistent Queue Settings](#pq), [Processing](#processing), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
>
> To minimize the risk of data loss, deploy a customer-managed (hybrid) Stream Worker Group
> with Worker Nodes as close as possible to the UDP sender.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}


### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.14/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Fields

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-tab}

**Single msg per UDP**: Toggle on if each UDP message should be treated as an independent event. Toggle off (default) if the message should be broken on newlines to create multiple events.

**Ingest raw bytes**: Toggle on to add a `__rawBytes` field to each event containing an array of the bytes received as the UDP message.

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (i.e, all IPs).

**Buffer size limit (events)**: Maximum number of events to buffer when downstream is blocking. The buffer is only in memory.

**UDP socket buffer size (bytes)**: Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Min: `256`. Max: `4294967295`.

It may also be necessary to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## What Fields to Expect

The Raw UDP Source does minimal processing of the incoming UDP messages to remain consistent with the internal Cribl [event model](event-model). For each UDP message received on the socket, you can expect an event with the following fields:

  * `_raw`: Contains the UTF-8 representation of the entire message received (if **Single msg per UDP** is toggled on), or of the given line that was split out of the message.
  * `_time`: The UNIX timestamp (in seconds) at which the message was received by Cribl Stream.
  * `source`: A string in the form `udp|<remote IP address>|<remote port>`, indicating the remote sender.
  * `host`: The hostname of the machine running Cribl Stream that ingested this event.

Also, the internal fields listed below will be present.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [functions](functions) can use them to make processing decisions.

Fields accessible for this Source:

  * `__inputId`
  * `__srcIpPort`
  * `__rawBytes`: When **Ingest raw bytes** is toggled on, this field will be an array containing the bytes of the UDP message.

## UDP Tuning

Incoming UDP traffic is put into a buffer by the Linux kernel. Cribl will drain from that buffer as resources are available.
At lower throughput levels, and with plenty of available processes, this isn’t an issue.
As you scale up, however, the default size of that buffer may be too small.

You can check the current buffer size with:

```shell
$ sysctl net.core.rmem_max
```

A typical value of about 200 KB is far too small for an even moderately busy syslog server.
You can check the health of UDP with the following command. Check the `packet receive errors` line.

```shell
$ netstat -su
```

If `packet receive errors` is more than zero, you have lost events,
which is a particularly serious problem if the number of errors is increasing rapidly.
This means you need to increase your `net.core.rmem_max` setting (see earlier).

You can update the live settings, but you’ll also need to change the boot-time setting so next time you reboot everything is ready to roll.

Live change, setting to 25 MB:

```shell
$ sysctl -w net.core.rmem_max=26214400
net.core.rmem_max = 26214400
$ sysctl -w net.core.rmem_default=26214400
net.core.rmem_default = 26214400
```

For the permanent settings change, add the following lines to `/etc/sysctl.conf`:

```shell
net.core.rmem_max=26214400
net.core.rmem_default=26214400
```

We recommend you track a few things related to UDP receiving:

- The `netstat -su` command, watching for errors.
- The **Status** tab in the UDP (Raw) Source. In particular, watch for dropped messages.
  They could indicate you need a bigger buffer under **Advanced Settings** (default: 1000 events). They could also indicate your Worker is encountering pressure further down the Pipeline.
- Especially if you increase your kernel receive buffer as above, watch your Worker processes' memory usage.

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_sources-troubleshooting.md]
