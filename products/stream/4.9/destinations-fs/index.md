# Filesystem/NFS Destination


**Filesystem** is a non-streaming Destination type that Cribl Stream can use to output files to a local file system or a network-attached file system (NFS).

> Type: Non-Streaming | TLS Support: N/A | PQ Support: N/A
>
{.box .info}  

> In Cribl.Cloud, the Filesystem Destination is only available on customer-managed [hybrid](cloud-enterprise#hybrid) Worker Nodes.
{.box .cloud}

## Configure Cribl Stream to Output to Filesystem Destinations

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Filesystem definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Output location**: Final destination for the output files. 
   - **Data format**: The output data format defaults to `JSON`. `Raw` and `Parquet` are also available. Selecting `Parquet` (supported only on Linux, not Windows) exposes a [**Parquet Settings**](#parquet-settings) left tab, where you **must** configure certain options in order to export data in Parquet format.
3. Next, you can configure the following **Optional Settings**: 
    - **Staging location**: Local filesystem location in which to buffer files before compressing and moving them to the final destination. Cribl recommends that this location be stable and high-performance.

      > The **Staging location** field is not displayed or available on Cribl.Cloud-managed Worker Nodes.
      >
      {.box .cloud}

    - **Partitioning expression**: JavaScript expression that defines how files are partitioned and organized. Default is date-based. If blank, Cribl Stream will fall back to the event's `__partition` field value (if present); or otherwise to the root directory of the **Output Location** and  **Staging Location**.

    - **Compress**: Data compression format used before moving to final destination. Defaults to `gzip` (recommended). This setting is not available when **Data format** is set to `Parquet`.

    - **File name prefix expression**: The output filename prefix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `CriblOut`.

   - **File name suffix expression**: The output filename suffix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `` `.${C.env["CRIBL_WORKER_ID"]}.${__format}${__compression === "gzip" ? ".gz" : ""}` ``, where `__format` can be `json` or `raw`, and `__compression` can be `none` or `gzip`.

      > To prevent files from being overwritten, Cribl appends a random sequence of 6 characters to the end of their names. This also applies to prefix and suffix expressions in file names.
      > 
      > For example, if you set the **File name prefix expression** to `CriblExec` and set the **File name suffix expression** to `.csv`, the file name might display as `CriblExec-adPRWM.csv` with `adPRWM` appended.
      >
      {.box .info}

    - **Backpressure Behavior**: Select whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.

    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Processing](#processing), [Parquet](#parquet-settings), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### Processing Settings {#processing}
 
#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Parquet Settings {#parquet-settings} 

To write out Parquet files, note that:
- On Linux, you can use the Cribl Stream CLI's `parquet` [command](cli-reference#parquet) to view a Parquet file, its metadata, or its schema.
- Cribl Edge Workers support Parquet only when running on Linux, not on Windows.
- See [Working with Parquet](parquet-schemas) for pointers on how to avoid problems such as data mismatches.

**Automatic schema**: Toggle on to automatically generate a Parquet schema based on the events of each Parquet file that Cribl Stream writes. When toggled off (the default), exposes the following additional field: 

* **Parquet schema**: Select a schema from the drop-down. 

> If you need to modify a schema or add a new one, follow the instructions in our [Parquet Schemas](parquet-schemas#creating-parquet-schemas) topic. These steps will propagate the freshest schema back to this drop-down.
>
{.box .warning}


**Parquet version**: Determines which data types are supported, and how they are represented. Defaults to `2.6`; `2.4` and `1.0` are also available.

**Data page version**: Serialization format for data pages. Defaults to `V2`. If your toolchain includes a Parquet reader that does not support `V2`, use `V1`.

**Group row limit**: The number of rows that every group will contain. The final group can contain a smaller number of rows. Defaults to `10000`.

**Page size**: Set the target memory size for page segments. Generally, set lower values to improve reading speed, or set higher values to improve compression. Value must be a positive integer smaller than the **Row group size** value, with appropriate units. Defaults to `1 MB`.

**Log invalid rows**: Toggle to `Yes` to output up to 20 unique rows that were skipped due to data format mismatch. Log level must be set to `debug` for output to be visible.

**Write statistics**: Leave toggled on (the default) if you have Parquet tools configured to view statistics – these profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc.

**Write page indexes**: Leave toggled on (the default) if your Parquet reader uses statistics from Page Indexes to enable [page skipping](https://github.com/apache/parquet-format/blob/master/PageIndex.md). One Page Index contains statistics for one data page.

**Write page checksum**: Toggle on if you have configured Parquet tools to verify data integrity using the checksums of Parquet pages.

**Metadata (optional)**: The metadata of files the Destination writes will include the properties you add here as key-value pairs. For example, one way to tag events as belonging to the [OCSF category](https://schema.ocsf.io/1.0.0/categories?extensions=) for security findings would be to set **Key** to `OCSF Event Class` and **Value** to `2001`.

### Advanced Settings {#advanced-tab}

**Max file size (MB)**: Maximum uncompressed output file size. Files of this size will be closed and moved to final output location. Defaults to `32`.

**Max file open time (sec)**: Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location. Defaults to `300`. 

**Max file idle time (sec)**: Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location. Defaults to `30`.

**Max open files**: Maximum number of files to keep open concurrently. When exceeded, the oldest open files will be closed and moved to final output location. Defaults to `100`.

> Cribl Stream will close files when **either** of the `Max file size (MB)` or the `Max file open time (sec)` conditions are met.
>
{.box .info}

**Compression level**: Specifies the level of compression to apply before moving files to the final destination. Higher compression levels reduce file size but increase CPU usage. Choose from `Best Speed`, `Normal`, or `Best Compression`. The default level, `Best Speed` will prioritize faster compression over smaller file sizes.

**Writing high watermark (KB)**: Sets the high watermark for writing files, in kilobytes. This determines the buffer size for file writes, impacting performance and memory usage. Enter a numeric value representing the buffer size in kilobytes. The default size, `64` will use a 64 KB buffer for file writes.

**Header line**: If set, this line will be written to the beginning of each output file, followed by a newline character. This can be useful for adding a header row to CSV files.

**Disk space protection**: Specifies whether to `Block` (default) or `Drop` incoming events when the disk space falls below the globally defined [**Min free disk space**](settings-group-fleet#storage) amount.

**Add Output ID**: When set to `Yes` (the default), adds the **Output ID** field's value to the staging location's file path. This ensures that each Destination's logs will write to its own bucket.

> For a Destination originally configured in a Cribl Stream version below 2.4.0, the **Add Output ID** behavior will be switched **off** on the backend, regardless of this toggle's state. This is so that upon Cribl Stream upgrade and restart, any files pending in the original staging directory will not be lost. To enable this option for such Destinations, Cribl's recommended migration path is:
> 
> - Clone the Destination.
> - Where Routes reference the original Destination, redirect them to instead reference the new, cloned Destination. 
> 
> This way, the original Destination will process pending files (after an idle timeout), and the new, cloned Destination will process newly arriving events with **Add output ID** enabled.
>
{.box .warning}

**Remove staging dirs**: Toggle this on to  delete empty staging directories after moving files. This prevents the proliferation of orphaned empty directories. When enabled, exposes this additional option:
  - **Staging cleanup period**: How often (in seconds) to delete empty directories when **Remove staging dirs** is enabled. Defaults to `300` seconds (every 5 minutes). Minimum configurable interval is `10` seconds; maximum is `86400` seconds (every 24 hours).



**Enable dead-lettering**: Toggle this on to set a maximum number of retries, and to move files to a designated directory when write failures exceed that limit. This prevents data flow blockage and excessive error logging due to undeliverable files. When enabled, exposes two additional fields:
   - **Dead-letter location**: Specify the storage location for undeliverable files. Defaults to `$CRIBL_HOME/state/outputs/dead-letter`.
   - **Maximum retry limit**: Configure the retry limit for failed file deliveries. This setting defines how many times the system will attempt to move a file to its intended location before it is deemed undeliverable and placed in the dead-letter directory. Defaults to `20`.
  

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

* `__partition`

> To export events from an intermediate stage **within a Pipeline** to a file, see the [Tee Function](tee-function).
>
{.box .info}

## Troubleshooting

[Snippet not found: content/shared/4.9/snippets/_destinations-troubleshooting.md]