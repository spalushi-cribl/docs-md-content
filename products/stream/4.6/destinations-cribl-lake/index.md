# Cribl Lake Destination




The **Cribl Lake** Destination sends data to [Cribl Lake](/lake/)
and automatically selects a partitioning scheme that works well with Cribl Search.

> Type: Non-Streaming | TLS Support: Yes | PQ Support: No
{.box .info} 

> The Cribl Lake Destination is available only in Cribl.Cloud,
> and only in cloud-based Worker Groups.
{.box .cloud}

## Configure a Cribl Lake Destination

1. From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:
    - To configure via the graphical [QuickConnect](quickconnect) UI:
        1. Click **Routing**, then **QuickConnect**.
        1. Click **Add Destination** at right.
        1. From the resulting drawer's tiles, select **Data Lakes**, then **Cribl Lake**.
        1. Click either **Add Destination** or (if displayed) **Select Existing**.
    - Or, to configure via the [Routing](routes) UI:
        1. Click **Data**, then **Destinations**.
        1. From the resulting page's tiles or the **Destinations** left nav, select **Data Lakes**, then **Cribl Lake**.
        1. Click **Add Destination** to open a **New Destination** modal.

1. In the Destination modal, configure the following under **General Settings**:
    - **Output ID**: Enter a unique name to identify this Cribl Lake Destination. 
    - **Lake dataset**: Select Cribl Lake Dataset to send data to.
        > You can't target the built-in `cribl_logs` and `cribl_metrics` Datasets with this Destination.
        {.box .info}

1. Next, you can configure the following **Optional Settings** that you'll find across many Cribl Destinations:
    - **Backpressure behavior**: Whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

1. Optionally, configure any [Post-Processing](#processing) settings outlined in the below sections.
1. Click **Save**, then **Commit & Deploy**.
1. Verify that data is [searchable](/search/search-cribl-lake) in Cribl Lake.

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports `c*` wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
