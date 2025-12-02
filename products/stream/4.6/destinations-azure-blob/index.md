# Azure Blob Storage Destination


Cribl Stream supports sending data to (and replaying specific events from) both [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction), and [Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-namespace), which implements a hierarchical namespace over blob data. This Destination can deliver data to Azure whether Cribl Stream is running on Azure, another cloud platform, or on-prem.

> Type: Non-Streaming | TLS Support: Yes | PQ Support: No
>
{.box .info} 

## Configuring Cribl Stream to Output to Azure Blob Storage

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical QuickConnect UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles, select **Azure** > **Blob Storage**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Azure** > **Blob Storage**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Destination definition. 

**Container name**: Specify the Azure Blob Storage container name. Containers organize blobs, similar to directories in a file system. Container names can include only lowercase letters, numbers, and hyphens. 
  - For dynamic container names, provide a JavaScript expression within quotes or backticks, for example, `` `myContainer-${C.env["CRIBL_WORKER_ID"]}` ``. This expression is evaluated at initialization, resolving environment or runtime variables. Only simple concatenations and references to global variables are supported.

**Blob prefix**: Root directory to prepend to path before uploading.

**Staging location**: Local filesystem location in which to buffer files before compressing and moving them to the final destination. Cribl recommends that this location be stable and high-performance.

> The **Staging location** field is not displayed or available on Cribl.Cloud-managed Worker Nodes.
>
{.box .cloud}

**Data format**: The output data format defaults to `JSON`. `Raw` and `Parquet` are also available. Selecting `Parquet` (supported only on Linux, not Windows) exposes a [**Parquet Settings**](#parquet-settings) left tab, where you **must** configure certain options in order to export data in Parquet format.

### Authentication Settings  {#authentication-settings}

Use the **Authentication method** drop-down to select one of these options:

- **Manual**: Use this default option to enter your Azure Storage connection string directly. Exposes a **Connection string** field for this purpose. (If left blank, Cribl Stream will fall back to `env.AZURE_STORAGE_CONNECTION_STRING`.)

- **Secret**: This option exposes a **Connection string (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-and-monitoring#secrets) that references an Azure Storage connection string. A **Create** link is available to store a new, reusable secret.

#### Connection String Format

Either authentication method uses an Azure Storage connection string in this format:

```
DefaultEndpointsProtocol=[http|https];AccountName=<your‑account‑name>;AccountKey=<your‑account‑key>;EndpointSuffix=<your-endpoint-suffix>
```

A fictitious example, using Microsoft's recommended HTTPS option, is:

```
DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=12345678...32;EndpointSuffix=core.windows.net
```

### Optional Settings

**Create container**:  Toggle to `Yes` to create the configured container in Azure Blob Storage if one does not already exist.

**Partitioning expression**: JavaScript expression that defines how files are partitioned and organized. Default is date-based. If blank, Cribl Stream will fall back to the event's `__partition` field value (if present); or otherwise to the root directory of the **Output Location** and  **Staging Location**.

**Compress**: Data compression format used before moving to final destination. Defaults to `gzip` (recommended). This setting is not available when **Data format** is set to `Parquet`. 

**File name prefix expression**: The output filename prefix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `CriblOut`.

**File name suffix expression**: The output filename suffix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `` `.${C.env["CRIBL_WORKER_ID"]}.${__format}${__compression === "gzip" ? ".gz" : ""}` ``, where `__format` can be `json` or `raw`, and `__compression` can be `none` or `gzip`.

> To prevent files from being overwritten, Cribl appends a random sequence of 6 characters to the end of their names. File name prefix and suffix expressions do not bypass this behavior.
> 
> For example, if you set the **File name prefix expression** to `CriblExec` and set the **File name suffix expression** to `.csv`, the file name might display as `CriblExec-adPRWM.csv` with `adPRWM` appended.
>
{.box .info}

**Backpressure behavior**: Whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.   

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Processing Settings 

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

### Advanced Settings

**Max file size (MB)**: Maximum uncompressed output file size. Files reaching this size will be closed and moved to the final output location. Defaults to `32`.

**Max file open time (sec)**: Maximum amount of time to write to a file. Files open for longer than this limit will be closed and moved to final output location. Defaults to `300`. 

**Max file idle time (sec)**: Maximum amount of time to keep inactive files open. Files open for longer than this limit will be closed and moved to final output location. Default: `30`. 

**Max open files**: Maximum number of files to keep open concurrently. When exceeded, the oldest open files will be closed and moved to final output location. Default: `100`.

> Cribl Stream will close files when **either** of the `Max file size (MB)` or the `Max file open time (sec)` conditions are met.
>
{.box .info}

**Max concurrent file parts**: Maximum number of parts to upload in parallel per file. A value of `1` tells the Destination to send one part at a time – that is, to upload the file's contents sequentially. Defaults to `1`; highest allowed value is `10`.

**Add Output ID**: When set to `Yes` (the default), adds the **Output ID** field's value to the staging location's file path. This ensures that each Destination's logs will write to its own bucket.

> For a Destination originally configured in a Cribl Stream version below 2.4.0, the **Add Output ID** behavior will be switched **off** on the backend, regardless of this toggle's state. This is to avoid losing any files pending in the original staging directory, upon Cribl Stream upgrade and restart. To enable this option for such Destinations, Cribl's recommended migration path is:
> 
> - Clone the Destination.
> - Redirect the Routes referencing the original Destination to instead reference the new, cloned Destination. 
> 
> This way, the original Destination will process pending files (after an idle timeout), and the new, cloned Destination will process newly arriving events with **Add output ID** enabled.
>
{.box .warning}

**Remove staging dirs**: Toggle to `Yes` to delete empty staging directories after moving files. This prevents the proliferation of orphaned empty directories. When enabled, exposes this additional option:
  - **Staging cleanup period**: How often (in seconds) to delete empty directories when **Remove staging dirs** is enabled. Defaults to `300` seconds (every 5 minutes). Minimum configurable interval is `10` seconds; maximum is `86400` seconds (every 24 hours).

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

  * `__partition`

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).
