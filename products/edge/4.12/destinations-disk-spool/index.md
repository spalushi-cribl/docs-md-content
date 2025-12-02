# Disk Spool Destination


The **Disk Spool** Destination writes recent events to disk, so you can spool
incoming event data and save it in a predefined file path location. This
consistent path can help you find data when you use generic Cribl Search Datasets.

> Type: **Internal** | TLS Support: **N/A** | PQ Support: **No**
>
{.box .info}

## Configure Cribl Edge to Spool Event Data

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Disk Spool Destination definition. 
   - **Description**: Optionally, enter a description.
3. Next, you can configure the following **Optional Settings**: 
   - **Bucket time span**: The amount of time that data is held in each bucket before
   it’s written to disk. The default is 10 minutes (`10m`). Consider the volume of
   the data source, as high-volume logs can fill a disk up quickly if you enter a
   long time span.
   - **Data size limit**: This is the maximum amount of disk space that spooled data
is allowed to consume. Once data reaches this amount, Edge will delete data
starting with the oldest buckets.
   - **Data age limit**: The duration of time for which Cribl Edge will retain data.
Edge will delete data that exceeds the max age entered in this field. Example
values are `2h`, `4d`. The default value is 24 hours (`24h`).
   - **Compression**: The codec that Edge uses to compress the data. The default is
`gzip`.
   - **Partitioning expression**: The JavaScript expression that defines how Edge
partitions and organizes files into time bucket directories. The default is
date-based. If this field is left blank, Cribl Edge will fall back to the
event’s `__partition` field value (if present), or otherwise to the root
directory of the Output Location and Staging Location.
   - **Tags**: Add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren’t added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Processing](#processing) and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 


> **Use Case Example**: When you partition incoming events, Search can find them
more efficiently. Imagine you've configured three Sources, each sending
event data to the Disk Spool Destination. Enter the partitioning expression
`__inputId`. Now you can send data from the three different Sources to individual
subdirectories under the spool's time bucket directories.
>
{.box .success}

For help with writing partitioning expressions, check out [Improving Partitioning Expressions](/stream/usecase-s3-better-practices#expressions).


### Processing Settings {#processing}
#### Post‑Processing

**Pipeline**: The Pipeline that will process the data before sending it out.

**System fields**: A list of fields to automatically add to events that use this
output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline
that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

**Environment**: If you’re using GitOps, use this field to specify a single Git
branch on which to enable this configuration. If empty, the config will be
enabled everywhere.

## Collect Event Data

Once your Destination is configured, you can begin sending data to it from any
Source. We recommend only connecting Sources that do not support disk spooling
at the Source level. When you have disk spooling enabled at the Source _and_ you
connect the Source to the Disk Spool Destination, event data is duplicated and
sent into two different spools.

When you have the Disk Spool Destination configured and Sources connected to it,
check that data is flowing using the **Live Data** tab in the Disk Spool
Destination.

Next, use Cribl Search to locate spooled data.

### Searching for Spooled Data

Disk spooling allows you to store recent events to disk, and makes them
available to find with Cribl Search. Typically, data from Edge is sent to and
stored directly in a Destination. By caching data in a spool, you can perform
data queries in real time without needing to connect to the data's final
destination.

For example, let's say you're an IT administrator who is troubleshooting issues
with a specific Edge Node. Rather than navigating away from Edge to access the
software that's hosting all the Edge data and looking through logs, you can stay
in Cribl Edge and run a Search query, returning data for a specific Node over a
duration of time you define.

To find spooled data, make sure you have at least one Source connected to the
Disk Spool Destination, then open Cribl Search.

1. In the search query box, type `dataset="cribl_edge_spool"` to return all events
   currently in the spool.
2. Use statements, scope, and processing operators to refine your spooled data.
   Check out [Build a Search](/search/build-a-search) for more information and
   examples.
