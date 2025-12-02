# File System/NFS



Cribl Stream supports collecting data from a locally mounted filesystem location that is available on all Worker Nodes.  

## Configuring a File System Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **File System** from the **Manage Sources** page's tiles or left nav. Click **Add Collector** to open the **File System** > **New Collector** modal, which provides the following options and fields.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

### Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. E.g., `DysonV11Roomba960`.

**Auto-populate from**: Select a Destination with which to auto-populate Collector settings. Useful when replaying data.

**Directory**: The directory from which to collect data. Templating is supported (e.g., `/myDir/${host}/${year}/${month}/`). You can also use templating to specify (e.g.) a Splunk bucket from which to collect. Symlinks will not be followed. More on [templates and Filters](collectors-schedule-run#tokens).

### Optional Settings

**Path extractors**: Extractors allow using template tokens as context for expressions that enrich discovery results. 

Click **Add Extractor** to add each extractor as a key-value pair, mapping a **Token** name on the left (of the form `/<path>/${<token>}`) to a custom JavaScript **Extractor expression** on the right (e.g., `{host: value.toLowerCase()}`). 

Each expression accesses its corresponding `<token>` through the `value` variable, and evaluates the token to populate event fields. Here is a complete example:

Token | Expression | Matched Value | Extracted Result
--- | --- | --- | ---
`/var/log/${foobar}` | `{program: value.split('.')[0]}` | `/var/log/syslog.1`| `{program: syslog, foobar: syslog.1}`




**Recursive**: If set to `Yes` (the default), data collection will recurse through subdirectories. 

**Max batch size (files)**: Maximum number of lines written to the discovery results files each time. Defaults to `10`. To override this limit in the Collector's Schedule/Run modal, use [Advanced Settings > Upper task bundle size](collectors-schedule-run#advanced-settings).

**Destructive**:  If set to `Yes`, the Collector will delete files after collection. Defaults to `No`.



**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

> Cribl Stream automatically detects gzip compression where a file name ends in `.gz`.
>
{.box .success}

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

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

Advanced Settings enable you to customize post-processing and administrative options.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields** : List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

## How the Collector Pulls Data

When you run a Filesystem/NFS Collector in Discovery mode, the first available Worker returns the list of available files to the Leader Node.

In Full Run mode, the Leader distributes the list of files to process across 1..*N* Workers as evenly as possible, based on file size. These Workers then stream in their assigned files from the filesystem location.

## Internal Fields {#internal-fields}

Cribl Stream uses a set of internal fields to assist in data handling. These "meta" fields are **not** part of an event, but they are accessible, and you can use them in [Functions](functions) to make processing decisions.

Relevant fields for this Collector:

* `__collectible` – This object's nested fields contain metadata about each collection job.
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


