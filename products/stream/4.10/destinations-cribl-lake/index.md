# Cribl Lake Destination




The **Cribl Lake** Destination sends data to [Cribl Lake](/lake/)
and automatically selects a partitioning scheme that works well with Cribl Search.

> Type: Non-Streaming | TLS Support: Yes | PQ Support: No
{.box .info} 

## Requirements

Cribl Lake Destination is available only in [Cribl.Cloud](deploy-cloud).

Cribl Lake Destination can send data to both Cribl-managed [Cloud](cloud-workers) 
and customer-managed [hybrid](worker-groups) Stream Worker Groups.
Hybrid Worker Groups must be running version 4.8 or higher.

## Configure a Cribl Lake Destination

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations**. Select the Destination you want. Next, select **Add Destination**.
2. In the Destination modal, configure the following under **General Settings**:
    - **Output ID**: Enter a unique name to identify this Cribl Lake Destination.
    - **Lake dataset**: Select Cribl Lake Dataset to send data to.
        > You can't target the built-in `cribl_logs` and `cribl_metrics` Datasets with this Destination.
       {.box .info}
3. Next, you can configure the following **Optional Settings** that you'll find across many Cribl Destinations:
    - **Backpressure behavior**: Whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Post-Processing](#processing) settings outlined in the below sections.
5. Click **Save**, then **Commit & Deploy**.
6. Verify that data is [searchable](/search/search-cribl-lake) in Cribl Lake.

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports `c*` wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
