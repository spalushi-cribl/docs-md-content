# Azure Blob Storage Collector



Cribl Stream supports collecting data, and replaying specific events, both from [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction), and from [Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-namespace), which implements a hierarchical namespace over blob data. This page covers how to configure the Collector.

## Configuring an Azure Blob Storage Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **Azure Blob** from the **Manage Sources** page's tiles or left nav. Click **Add Collector** to open the **Azure Blob** > **New Collector** modal, which provides the following options and fields.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
> 
> Cribl Stream supports data collection and replay from Azure's **hot** and **cool** access tiers, but not from the **archive** tier – whose stated retrieval lag, up to several hours, cannot guarantee data availability.
>
{.box .info}

### Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. For example, `azure_42-a`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.

**Auto-populate from**: Optionally, select a predefined Destination that will be used to auto-populate Collector settings. Useful when replaying data.

**Container name**: Container to collect from. This value can be a constant, or a JavaScript expression that can be evaluated only at init time. E.g., referencing a Global Variable: `myBucket-${C.vars.myVar}`.

> Container names can include only lowercase letters, numbers, and/or hyphens (`-`). This restriction is imposed by Azure.
>
{.box .info}

### Authentication

Use the **Authentication method** drop-down to select one of these options:

- **Manual**: Use this default option to enter your Azure Storage connection string directly. Exposes a **Connection string** field for this purpose. (If left blank, Cribl Stream will fall back to `env.AZURE_STORAGE_CONNECTION_STRING`.)

- **Secret**: This option exposes a **Connection string (text secret)** drop-down, in which you can select a stored secret that references an Azure Storage connection string. The secret can reside in Cribl Stream's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need to generate a new secret.

- **Client secret**: This option allows you to use the Azure service principal's client secret to authenticate, and exposes [Azure service principal settings](#service-principal-settings).

- **Certificate**: This option allows you to use a certificate registered with the Azure service principal to authenticate, and exposes [Azure service principal settings](#service-principal-settings).

#### Service Principal Settings

**Client secret** and **Certificate** authentication methods expose the following additional settings:

- **Storage account name**: Enter the name of your Azure Storage Account.
- **Tenant ID**: Enter the service principal's tenant ID.
- **Client ID**: Enter the service principal's client ID.
- **Client secret (text secret)** (with **Client secret** method selected): Select a text secret containing the client secret, or create a new one.
- **Certificate** (with **Certificate** method selected): Select the certificate you registered as credentials for your app in the Azure portal, or create a new one.

#### Using Shared Access Signature

You can authenticate for Azure Blob Storage using a [shared access signature](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview) token as connection string.

To employ it successfully, go to your storage account's **Shared access signature** settings
and make sure that **Allowed blob index permissions** enables both **Read/Write** and **Filter**.

If you are using Azure Premium storage, you can't set blob index permissions.
In such a case, in your collector's setting, enable the **Include Metadata** option.
This will allow you to use shared access signature,
but as side effect will strip defined tags for downloaded blobs.

### Optional Settings {#optional-settings}

**Path**: The directory from which to collect data. Templating is supported (e.g., `myDir/${datacenter}/${host}/${app}/`). Time-based tokens are also supported (e.g., `myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/`). More on [templates and Filters](collectors-schedule-run#tokens).

**Path extractors**: Extractors allow using template tokens as context for expressions that enrich discovery results. 

Click **Add Extractor** to add each extractor as a key-value pair, mapping a **Token** name on the left (of the form `/<path>/${<token>}`) to a custom JavaScript **Extractor expression** on the right (e.g., `{host: value.toLowerCase()}`). 

Each expression accesses its corresponding `<token>` through the `value` variable, and evaluates the token to populate event fields. Here is a complete example:

Token | Expression | Matched Value | Extracted Result
--- | --- | --- | ---
`/var/log/${foobar}` | `{program: value.split('.')[0]}` | `/var/log/syslog.1`| `{program: syslog, foobar: syslog.1}`



**Recursive**: If set to `Yes` (the default), data collection will recurse through subdirectories. 

**Include metadata**: With the default `Yes` setting, Cribl Stream will include Azure Blob metadata in collected events (at `__collectible.metadata`).

**Include tags**: With the default `Yes` setting, Cribl Stream will include Azure Blob tags in collected events (at `__collectible.tags`). To prevent errors, toggle this to `No` when using a Shared Access Signature connection string, especially on storage accounts that do not support Azure Blob index tags.

**Max batch size (objects)**: Maximum number of metadata objects to batch before recording as results. Defaults to `10`. To override this limit in the Collector's Schedule/Run modal, use [Advanced Settings > Upper task bundle size](collectors-schedule-run#advanced-settings).

**Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for parquet files. UTF-16LE and Latin-1 are also supported.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

#### Connection String Format

**Manual** and **Secret** authentication methods use an Azure Storage connection string in this format:
`DefaultEndpointsProtocol=[http|https];AccountName=<your‑account‑name>;AccountKey=<your‑account‑key>`

A fictitious example, using Microsoft's recommended HTTPS option, is:
`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=12345678...32`

### Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to `No`. Toggle to `Yes` to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination. The `No` setting exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

### Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## Replay

See these resources that demonstrate how to replay data from object storage. Both are written around Amazon S3-compatible stores, but the general principles apply to Azure blobs as well:

- [Data Collection & Replay sandbox](https://sandbox.cribl.io/course/data-collection): Step-by-step tutorial, in a hosted environment, with all inputs and outputs preconfigured for you. Takes about 30 minutes.

- [Using S3 Storage and Replay](/stream/usecase-replay-s3): Guided walk-through on setting up your own replay.

## How the Collector Pulls Data

In the Discover phase, the first available Worker returns the list of files to the Leader Node. In the Collect phase, Cribl Stream spreads the list of files to process spread across 1..N Workers, based on file size, with the goal of distributing tasks as evenly as possible across Workers. These Workers then stream in their assigned files from the remote Azure Blob Storage location.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Collector:

* `__collectible` – This object's nested fields contain metadata about each collection job. 
  * As mentioned above in **Optional Settings**, this object will include Azure Blob's own metadata at `__collectible.metadata` and Azure Blob tags at `__collectible.tags`, unless you deselect the corresponding UI toggles.
  * `collectorType`: Indicates the type of Collector used for the job.
  * `collectorId`: Represents the **Collector ID** of the Collector, as configured during setup.
* `__inputId` – Uniquely identifies the origin of data for a [collection job](collectors-schedule-run). Its format varies depending on whether the job is ad hoc or scheduled:
  * Ad hoc jobs are formatted as `collection:<timestamp>.<randomId>.adhoc.<Collector ID>`.
    * `<timestamp>`: The Unix timestamp when the job was initiated.
    * `<randomId>`: A random identifier to ensure uniqueness.
    * `adhoc`: Indicates the job was manually triggered.
    * `<Collector ID>`: The ID of the Collector.
  * Scheduled jobs are formatted as `collection:<Collector ID>`.
    * `<Collector ID>`: The ID of the Collector.

## Troubleshooting

When permissions are correct on the object store, and events are reaching the Collector, the Preview pane will show events and the Job Inspector will show an **Events collected** count. 

However, if previewing returns no events and throws no error, first check your Filter expression by previewing without it (e.g., simplify the Filter expression to `true`). Then check the Job Inspector: If the **Total size** is greater than `0`, and the **Received size** is `NA` or `0`, make sure you have list and read permissions on the object store.
