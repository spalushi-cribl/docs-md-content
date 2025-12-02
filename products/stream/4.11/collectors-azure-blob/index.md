# Azure Blob Storage Collector


Cribl Stream supports collecting data, and replaying specific events, both from [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction), and from [Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-namespace), which implements a hierarchical namespace over blob data. This page covers how to configure the Collector.

> For configuration examples, see [Resources](#resources) below.
>
{.box .success}

## Configure an Azure Blob Storage Collector 

1. Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to **Data** > **Sources**. Choose the Source and select **Add Collector**. 
2. In the **New Collector** modal, configure the following under **Collector Settings**:
    - **Collector ID**: Unique ID for this Collector. For example, `azure_42-a`. If you clone this Collector, Cribl Stream will add `-CLONE` to the original **Collector ID**.
    - **Description**: Optionally, enter a description.
    - **Auto-populate from**: Optionally, select a predefined Destination that will be used to auto-populate Collector settings. Useful when replaying data.
    - **Container name**: Container to collect from. This value can be a constant, or a JavaScript expression that can be evaluated only at init time. For example, referencing a Global Variable: `myBucket-${C.vars.myVar}`.

> Container names can include only lowercase letters, numbers, and/or hyphens (`-`). This restriction is imposed by Azure.
>
{.box .info}

3. Under **Authentication**, select an **Authentication method** from the dropdown:
     -  **Manual**: Use this default option to enter your Azure Storage connection string directly. Exposes a **Connection string** field for this purpose. (If left blank, Cribl Stream will fall back to `env.AZURE_STORAGE_CONNECTION_STRING`.)
     -  **Secret**: This option exposes a **Connection string (text secret)** drop-down, in which you can select a stored secret that references an Azure Storage connection string. The secret can reside in Cribl Stream's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need to generate a new secret.
     -  **Client secret**: This option allows you to use the Azure service principal's client secret to authenticate, and exposes [Azure service principal settings](#service-principal-settings).
     -  **Certificate**: This option allows you to use a certificate registered with the Azure service principal to authenticate, and exposes [Azure service principal settings](#service-principal-settings).
4. Next, you can configure the following **Optional Settings**: 
    - **Path**: The directory from which to collect data. Templating is supported (for example, `myDir/${datacenter}/${host}/${app}/`). Time-based tokens are also supported (for example, `myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/`). More on [templates and Filters](collectors-schedule-run#tokens).
   - **Path extractors**: Extractors allow using template tokens as context for expressions that enrich discovery results. 
    - Select **Add Extractor** to add each extractor as a key-value pair, mapping a **Token** name on the left (of the form `/<path>/${<token>}`) to a custom JavaScript **Extractor expression** on the right (for example, `{host: value.toLowerCase()}`). See an example of the [Extractor Expression](#expression) below. 
    - **Recursive**: Toggle on (default) if you want data collection to recurse through subdirectories. 
    - **Include metadata**: Toggle on (default) if you want Cribl Stream to include Azure Blob metadata in collected events (at `__collectible.metadata`).
    - **Include tags**: Toggle on (default) if you want Cribl Stream to include Azure Blob tags in collected events (at `__collectible.tags`). To prevent [errors](#error), toggle off when using a Shared Access Signature connection string, especially on storage accounts that do not support Azure Blob index tags.
    - **Max batch size (objects)**: Maximum number of metadata objects to batch before recording as results. Defaults to `10`. To override this limit in the Collector's Schedule/Run modal, use [Advanced Settings > Upper task bundle size](collectors-schedule-run#advanced-settings).
    - **Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for Parquet files. UTF-16LE and Latin-1 are also supported.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
  5. Optionally, configure any [Result](#result-settings), [Result Routing](#result-routing), and [Advanced](#advanced-settings) settings outlined in the sections below.
  6. Select **Save**, then **Commit & Deploy**. 
  7. To verify that the Collector actually collects data, you can start a single run
in the [Preview](/stream/collectors-schedule-run#mode/) mode.
  
> The sections described below are spread across several tabs.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
> 
> Cribl Stream supports data collection and replay from Azure's **hot** and **cool** access tiers, but not from the **archive** tier – whose stated retrieval lag, up to several hours, cannot guarantee data availability.
>
{.box .info}

### Authentication Options   

This section provides additional details to configure access to the Azure Blog Storage.
 
#### Connection String Format

**Manual** and **Secret** authentication methods use an Azure Storage connection string in this format:
`DefaultEndpointsProtocol=[http|https];AccountName=<your‑account‑name>;AccountKey=<your‑account‑key>`

A fictitious example, using Microsoft's recommended HTTPS option, is:
`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=12345678...32`

#### Service Principal Settings

**Client secret** and **Certificate** authentication methods expose the following additional settings:

- **Storage account name**: Enter the name of your Azure Storage Account.
- **Tenant ID**: Enter the service principal's tenant ID.
- **Client ID**: Enter the service principal's client ID.
- **Client secret (text secret)** (with **Client secret** method selected): Select a text secret containing the client secret, or create a new one.
- **Certificate** (with **Certificate** method selected): Select the certificate you registered as credentials for your app in the Azure portal, or create a new one.
- **Endpoint suffix**: (Optional) Enable connectivity to Azure Blob Storage in different regions, for instance, Azure China with `core.chinacloudapi.cn`. Defaults to `core.windows.net`.

#### Using Shared Access Signature

You can authenticate for Azure Blob Storage using a [shared access signature](https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview) token as a connection string.

To employ it successfully, go to your storage account's **Shared access signature** settings
and make sure that **Allowed blob index permissions** enables both **Read/Write** and **Filter**.

If you are using Azure Premium storage, you can't set blob index permissions.
In such a case, in your Collector's setting, enable the **Include metadata** option.
This will allow you to use shared access signature,
but as a side effect will strip defined tags for downloaded blobs.

### Extractor Expression Example {#expression}

Each expression accesses its corresponding `<token>` through the `value` variable, and evaluates the token to populate event fields. Here is a complete example:

Token | Expression | Matched Value | Extracted Result
--- | --- | --- | ---
`/var/log/${foobar}` | `{program: value.split('.')[0]}` | `/var/log/syslog.1`| `{program: syslog, foobar: syslog.1}`

### Result Settings {#result-settings}

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

- **Enabled**: Defaults to off. Toggle on to enable the custom command.

- **Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

- **Arguments**: Select **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

- **Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

- **Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

[Snippet not found: content/shared/4.11/snippets/_sources-add-fields.md]

#### Result Routing {#result-routing}

**Send to Routes**: Toggle on (default) if you want Cribl Stream to send events to normal routing and event processing. Toggle off to select a specific Pipeline/Destination combination. Toggling off exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

Toggling on (default) exposes this field: 
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, and so on. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might toggle **Send to Routes** off when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

### Advanced Settings {#advanced-settings}

Advanced Settings enable you to customize post-processing and administrative options.

- **Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

- **Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.
- **Remove Discover fields**: List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs UI. You can specify wildcards (such as `aws*`).
- **Resume job on boot**: Toggle on to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

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

## Resources {#resources}

For examples of configuring Cribl Stream to interoperate with Azure services, see these guides:

- [SSO with Microsoft Entra ID and OIDC (On-Prem)](usecase-azure-ad).

- [SSO with Microsoft Entra ID and SAML (Cribl.Cloud)](saml-azure-setup).

- [SSO with Microsoft Entra ID and SAML (On-Prem)](usecase-sso-saml-entra).

Also see these Packs on the [Cribl Packs Dispensary](https://packs.cribl.io/), which provide processing Pipelines to transform and optimize incoming Azure data. You can directly import these Pipelines and adapt them to your needs:

- [Microsoft Azure NSG Source](https://packs.cribl.io/packs/cribl-azure-nsg-source) to Common Schema.

- [Azure NSG Flow Logs](https://packs.cribl.io/packs/cribl-azure-nsg-flow-logs).

- [Azure NSG Flow Logs to OCSF](https://packs.cribl.io/packs/cribl-Azure-NSG-OCSF-Flow-Logs).

- [Azure Audit Logs to OCSF](https://packs.cribl.io/packs/azure-audit-log-ocsf).

- [Microsoft Office Activity & Azure Logs](https://packs.cribl.io/packs/microsoft-activity).

## Troubleshooting

When permissions are correct on the object store, and events are reaching the Collector, the Preview pane will show events and the Job Inspector will show an **Events collected** count. 

However, if previewing returns no events and throws no error, first check your Filter expression by previewing without it (for example, simplify the Filter expression to `true`). Then check the Job Inspector: If the **Total size** is greater than `0`, and the **Received size** is `NA` or `0`, make sure you have list and read permissions on the object store.

### Common Error {#error}

The error `This request is not authorized to perform this operation using this permission.` can occur when the Azure Blob Storage Collector lacks permissions to access Azure Blob Index Tags.

- If your storage account is Premium Storage, disable **Include tags** in the Azure Blob Storage Collector **Optional Settings** as Premium Storage does not support Azure Blob Index Tags.
- For other storage account types, ensure **Allow blob index permissions** is enabled for your Azure Blob Storage account to support the **Include tags** functionality. If the **Include tags** option is enabled, the Azure App will need `Owner Access`. If tags are not needed, you can disable the **Include tags** option to avoid requiring `Owner Access`.