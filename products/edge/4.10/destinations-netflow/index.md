# NetFlow Destination


Cribl Edge supports passing through [NetFlow](https://www.cisco.com/c/en/us/products/ios-nx-os-software/ios-netflow/index.html) v5 and v9 UDP traffic to NetFlow Collectors.

> Type: Non-Streaming | TLS Support: No | PQ Support: Yes
>
{.box .info} 

## Configurie Cribl Edge to Output to NetFlow

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this NetFlow definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **NetFlow Destinations**: Add the downstream NetFlow Collectors to which Cribl Edge should send data.
   - **Address:** Hostname or IP address of the Collector.
   - **Port:** Port number to connect to on the Collector. Defaults to `2055`, which is the standard port for NetFlow traffic.
3. Next, you can configure the following **Optional Settings**: 
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
3. Optionally, you can adjust the [Processing](#processing) and [Advanced](#advanced-tab) settings outlined in the sections below.
4. Select **Save**, then **Commit & Deploy**.
   
   
### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

### Advanced Settings {#advanced-tab}

**DNS resolution period (sec)**: Re-resolve any hostnames after each interval of this many seconds, and pick up destinations from records. Defaults to `0` seconds. A value of `0` means every datagram sent will incur a DNS lookup. A non-zero value improves performance but can reduce the overall reliability if the DNS records for the downstream Collectors change frequently.

**Environment**: If you’re using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

* `__netflowRaw` : Directly routes the raw NetFlow packet data to a downstream NetFlow Collector.

## Considerations for Working with NetFlow Data FlowSets

For both NetFlow v5 and v9, Cribl Edge:

* Can forwards NetFlow packets to other NetFlow Collectors. However, it cannot modify the the contents of the incoming packet. In other words, Cribl Edge forwards the packets verbatim as they came in.
* Only routes NetFlow packets from upstream Exporters and cannot generate its own NetFlow packets.
* Cannot send non-NetFlow input data to NetFlow Collectors.

## Troubleshooting

[Snippet not found: content/shared/4.10/snippets/_destinations-troubleshooting.md]
