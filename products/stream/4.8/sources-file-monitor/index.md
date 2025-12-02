# File Monitor Source


The File Monitor Source generates events from text files, compressed, archived, and (some) binary log files, based on lines and records extracted from the content.

> Type: **System**  |  TLS Support: **N/A** | Event Breaker Support: **Yes**
>
{.box .info}

> This Source is available in Cribl.Cloud on customer-managed [hybrid Workers](cloud-enterprise#hybrid), but not on Cribl-managed Workers in Cribl.Cloud.
>
{.box .cloud}

As of v.4.2.x, this Source can process the following file formats:
- **Compressed/archived**: `zip`, `gzip`, `zstd`, and`tar`. Cribl Stream only
  supports DEFLATE compression for `zip` files.
- **ASCII**: `text`.
- **Non-ASCII binary**.

Binary files are broken into base64-encoded chunks and streamed bypassing [Event Breakers](#events). Text files are processed normally through Event Breakers. (Before v.4.2.x, the File Monitor Source could handle only text files.) Also as of v.4.2.x, the File Monitor Source no longer excludes `*.gz` files by default.  

## File Monitoring and One-Time Ingestion

Cribl Edge offers two options for ingesting file data:
- **File Monitor Source**: This is ideal for ongoing log file monitoring. It can be configured to prioritize processing the latest data in newly discovered files upon startup ("Collect from end"). Subsequently, it automatically monitors and ingests new content as it's added.

> To automatically discover and ingest new log files, Cribl Edge requires privileged access for file scanning and reading. For alternative approaches, see [Running Edge as an Unprivileged User](/edge/deploy-runtime-user#cap).
>
{.box .warning}
- **One-time File Ingest**: This feature complements the File Monitor by allowing a [one-time ingest](/edge/explore-edge#ingest) of any file. It's useful for quickly processing specific files or initial data loads. If **Explore** is enabled, as it might be restricted in some environments for security reasons.

## Discovering and Filtering Files to Monitor

To produce its initial list of files to monitor, the File Monitor Source runs a discovery procedure at a configurable **Polling interval**. The Source then applies an **Allowed list** to filter the initial list down into its final form. Then, for each file on the list, the Source compares current state with previously-stored state. This comparison determines whether the File Monitor Source will actually watch a given file for a given polling interval, or just ignore the file.

In the simplest case, the Source discovers a file for which it has no stored state. This means that the file has just been created, and needs to be monitored. See the [Examples](#file-monitor-examples) for other possibilities, along with a description of the **Status** tab, which displays state information for all files being monitored.

How does the File Monitor Source discover files in the first place? You have the choice of two **Discovery Modes**: **Auto** or **Manual**.
 
- In **Auto** mode, the Source automatically discovers files that running processes have open for writing. Auto mode collects logs from (the discovered) active files that match the path/allowlist. 
 
  Auto mode is useful to detect which files are being written to. For example, if you enter an allowlist of `*log`, Auto mode will find all the active logs within that directory, no matter what's writing to it, and it will exclude any rotated logs.

> Cribl Edge currently supports Auto mode only when running on Linux, not on Windows.
>
{.box .info}

- In **Manual** mode, the Source discovers files based on the specified directory and depth.

   **Manual** mode is often used to collect logs in a folder where the process that creates them rotates them when they get large. For example, if you specify a path of `/var/log` and an allowlist of `*/messages*`, Manual mode will capture the current `/var/log/messages`, as well as older logs from `/var/log/messages*`.

> If you are using a tool like `rsync` to transfer files in chunks, the File Monitor Source might start collecting files before the transfer is complete. To prevent this, configure your tool to transfer files serially. (For example, use: `rsync --append`.)
>
{.box .info}

## Configuring a File Monitor Source

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

### Configure via QuickConnect

1. To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). 
1. Click **Add Source** at left. From the resulting drawer's tiles, select **System and Internal** > **File Monitor**. 
1. Click either **Add Destination** or (if displayed) **Select Existing**. 

The **General Settings** drawer will open.
 
### Configure via [Routing](routes) 

1. Click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). 
1. From the resulting page's tiles or left nav, select **System and Internal** > **File Monitor**.
1. Click **Add Source** to open the **New Source** modal.


### General Settings

**Enabled**: Toggle to `Yes` to enable the Source. 

**Input ID**: Enter a unique name to identify this File Monitor Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.

**Discovery Mode**: Use the buttons to select one of these options:
- **Auto**: Tells the Source to automatically discover files that running processes have open for writing.
- **Manual**: Tells the Source to discover the files within the **Search path** (a directory) that you specify, down to the **Max depth**. You must enter an absolute path (no wildcards) for the **Search path**, as File Monitor searches files in a single directory in Manual mode. The defaults are:
  - **Search path** is set to the `/var/log` directory 
  - **Allowlist** of `*/log/*` and `*log` 
  - **Max depth** of 4

 If you leave the **Max depth** field empty, the Source will search subdirectories, and their subdirectories, and so on, without limit. If you specify `0`, the Source will discover only the top-level files within the **Search path**. For `1`, the Source will discover files one level down.

Both modes allow you to set the **Polling interval**, which otherwise defaults to 10 seconds.

### Optional Settings

**Filename allowlist**: Wildcard syntax, and the exclamation mark (`!`) for negation, are allowed. For example, you can use `!*cribl*access.log` to prevent the Source from discovering Cribl Stream's own log files. The default filters are `*/log/*` and `*log`.

> Always think through how your **Search path** and **Filename allowlist** settings interact: If you're not careful, you can inadvertently create overlapping monitor groups that deliver duplicates of some files. See [Using Allowlists Effectively](#using-allowlists) below.
>
{.box .warning} 

**Max age duration**: Optionally, specify a maximum age of files to monitor. This Source will filter events with timestamps newer than the configured duration. (Where files don't have a parsable timestamp, it will set their events' timestamp to `Now`.) Enter a duration in a format like `30s`, `4h`, `3d`, or `1w`. Defaults to an empty field, which applies no age filters.

**Check file modification times**: If toggled to `Yes`, this Source will skip files with modification times older than the configured **Max age duration**.

> When you set a **Max age duration** threshold, the Source will open every file and read through all its contents to seek a timestamp. For best performance, also enable the **Check file modification times** toggle to skip older files.
>
{.box .info} 

**Collect from end**: If toggled to `Yes`, this Source will prioritize processing the latest data by jumping to the end of newly discovered files on Cribl Stream's startup. This is ideal if you want to focus on new content in existing log files. However, this behavior only applies during the first encounter with a file.  For existing files after the initial scan and all subsequent discoveries, Cribl Stream will switch to its normal processing mode, reading from the beginning to capture ongoing updates.

**Enable binary files**: If toggled to `Yes`, the Source will report the file as binary and stream it in Base64-encoded chunks. By default, the Source ignores binary (non-text) files such as JPEG images, MP3 audio files, or some binary data files.

**Force text format**: If toggled to `Yes`, the Source displays ingested data as text in the event (when the data isn't in a compressed or archived file). This option is helpful if you have files that contain binary mixed with text where the data should be streamed as all text (for example, AuditD logs can contain this type of data).

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Processing Settings

#### Event Breakers {#events}

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied to the input data stream before the data is sent through the Routes. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec.), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Idle timeout**: Time, in seconds, before an idle file is closed. Defaults to `300` sec. (5 minutes). 

**Hash length**: How many file header bytes to use in a hash for identifying a file uniquely. Defaults to `256` bytes. For details, see [Configuring Hash Lengths](#hash).

#### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

> This Source defaults to [QuickConnect](quickconnect).  
>
{.box .info}



## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are not part of an event, but they are accessible, and Functions can use them to make processing decisions.

For the File Monitor Source, you can use internal fields to enrich events with container and host metadata. The following internal fields are available:

- `__baseFilename`
- `__source` / `source`
- `__raw`
- `__inputId`
- `container_id`
- `container_path`
- `host_path`

## Monitoring Renamed Files' State

The File Monitor Source uses a simple scheme to find "backlog" files corresponding to a matching file: Tell it to tail `foo.log`, and it will look for `foo.log.[0-9]` and scrape those, too. 

This Source keeps hashes that correspond to the start point within the file, and to the last-read point. This way, if Cribl Stream is stopped, files are rotated, and Cribl Stream is restarted, the File Monitor Source can find the resume point in those backlog files. 

Similarly, if you rename the file **within the same backlog scheme** (for example, `foo.log` to `foo.log.0`), the File Monitor Source can resume at the right place. However, if you renamed `foo.log` to `bar.log` (a deviation from the expected naming scheme), this Source could not find its resume point. 



## Configuring Hash Lengths {#hash}

For files with identical first lines, configure the hash length to be longer than the first line to ensure that the header includes something unique. It is common for a batch of `CSV` files or `IIS` logs to have identical first lines listing columns that appear in subsequent lines. 

If you don't adjust the hash length, all files with headers larger than `256 bytes` will appear to File Monitor as one file. As long as the hash length is greater than the header length, and the subsequent lines include something unique, the File Monitor Source will identify them as different files.

## Using Allowlists Effectively {#using-allowlists}

Done properly, file monitoring collects exactly the files you need to see, without duplication. You want to avoid inadvertently creating overlapping monitor groups that deliver duplicates, because this can "run the meter" twice for affected files, potentially causing license issues.

The key is knowing how to craft allowlists.

### Crafting Allowlists: Principles

To get the results you want from file monitoring, always apply the following principles.

###### Write allowlists to explicitly include the full path of files you want to collect. 

The search path itself is **not** excluded from the match. For example, if you want to collect the file `/var/log/messages` when you've set your search path to `/var/log`, your filename allowlist needs to be either `/var/log/messages`, `/var/*/messages`, or `*/log/messages`.

Exclusions (`!`) also need to address the full file path. For example, if you want to collect everything from `/var/log/*` **except for** `/var/log/apache/*`, your allowlist must include one of the following:

* `!/var/log/apache/*`
* `!*/apache/*`
* `!*/log/apache/*`

Wildcards (`*`) match **any** part of the file path:

* `*log` matches `/var/foo/bar/myfile.log`.
* `*.log` matches anything ending in `.log`.
* `/var/log/*` matches anything in `/var/log`, subject to the **Max Depth** setting.

###### Put explicit matches (and exclusions) at the beginning of the allowlist, ahead of wildcard matches.

For example, an allowlist of `/var/log/*` followed by `!/var/log/apache/*` will fail to exclude the file `/var/log/apache/foo` because that file matches the first allowlist entry. 

In allowlists, order matters!

###### Be as specific as possible, whenever possible, to prevent accidental matches. 

Each of the following examples shows how to avoid matching unwanted files within the directories that you're monitoring.

To collect `/var/log/boot.log` (and older versions of `boot.log.*`) as well as `/var/log/messages`:

**Search path** | **Filename allowlist**
--- | ---
`/var/log` | `/var/log/boot.log`, `/var/log/messages`

To collect everything in `/var/log` while excluding `/var/log/apache/*`:

**Search path** | **Filename allowlist**
--- | ---
`/var/log` | `!/var/log/apache/*`, `/var/log/*`

### Examples of Allowlists {#file-monitor-examples}

Below are typical scenarios where good allowlist technique will save you from duplication headaches. 

#### Example: Excluding the System's Own Logs

Recall that you'll often create a set of File Monitor Sources, each of which monitors one file. This is **not** the right approach in an example like this one, where **Discovery Mode** is set to **Auto**. Using **Auto** mode in a file monitor along with literally any other file monitor can cause unintended, duplicate collection. Use **Auto** mode carefully!

Here's the example:

Suppose you added a File Monitor Source on a Linux machine, set your **Discovery Mode** to **Auto**, and specified your home directory as the **Search path** (for example, `/home/bogart/`).
 
You do not want to monitor Cribl Stream's own log files, or any log files generated by Chrome, the web browser you're using. To exclude them, you add `!*cribl*access.log` and `!*chrome*` to your **Allowlist**. You **do** want to monitor any other log files that the Source can discover, so you also add `*log`.

After awhile, the Status tab looks like this:

![The Status tab](file-monitor-status.d822e0dc8a.png)
{border="true"}

#### Example: Fixing Overlapping Monitor Groups

Here's an example of the kind of overlapping monitor groups you should avoid:

**Search path** | **Max depth** | **Filename allowlist**
--- | --- | ---
`/var` | (none) | `*/log/*`
`/var/log/apache` | `0` | `*`

The above configuration would collect all the files in `/var/log/apache/` twice. A better solution would be the following:

**Search path** | **Max depth** | **Filename allowlist**
--- | --- | ---
`/var/log` | (none) | `!*/apache/*`
`/var/log/apache` | `0` | `*`
