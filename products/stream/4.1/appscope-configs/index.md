# AppScope Configs


You can obtain high-fidelity application data from any Linux command or application using [AppScope](https://appscope.dev/), an [open‑source](https://github.com/criblio/appscope) tool from Cribl. This includes data that's [difficult or impossible](https://cribl.io/blog/the-appscope-origin-story/) to obtain any other way.

AppScope is included in your Cribl Stream installation. The `in_appscope` [AppScope Source](sources-appscope) is enabled right out-of-the-box; it is configured to send data to Cribl Edge or Cribl Stream on the same host.  

If you need one or more reusable AppScope configurations, create and save them in the AppScope Configs UI described below. They will then be available in the AppScope Config Library.

AppScope Configs ship with a `sample_config` file that contains default settings. You can customize this `sample_config`. Once it's customized, a **Restore Default** button appears at the config modal's lower left; click this if you need to restore the sample file's default settings. 

> ##### Rolling Back a `sample_config` Filter {{< id `rollback` >}}
>
> When the `sample_config` file has been assigned to an [AppScope Source's filter](sources-appscope#filter), you can't restore its default settings. (An error message will state that the AppScope Config file is currently in use.) To resolve this:
> 
> On the AppScope Source's **AppScope Filter Settings** tab, delete the `sample_config` from the **Allowlist**. Then, use the AppScope Configs UI to restore the `sample_config`'s default settings. You can now add the refreshed `sample_config` back to the AppScope Source's Allowlist.
>
{.box .warning}

## Creating AppScope Configs {#creating}

You access the **AppScope Configs** UI from Cribl Stream's top nav:
* In Cribl Stream, select  **Processing** > **Knowledge** > **AppScope Configs**.
* In Cribl Edge, select **More** > **Knowledge** > **AppScope Configs**. 

Click **Add AppScope config** to open a modal to begin creating an AppScope config, using the options below.

Once you've created AppScope configs, you can edit, delete, search, and tag them as desired.

### General Settings

**ID**: Enter a unique name to identify this AppScope Config.

**Description**: Optionally, enter a description that will remind you what the config is for when you see it listed in the library.

**Tags**: Optionally, enter any desired tags.

#### Transports 

> AppScope offers several options for routing data. See [Data Routing](https://appscope.dev/docs/data-routing/) in the AppScope docs.
>
{.box .info}

**Use AppScope Source's IP/port or UNIX domain socket?**: The default`Yes` setting applies the transport options defined in the associated [AppScope Source](sources-appscope). Toggling this to `No` exposes the following fields in this section.

**Cribl authentication token**: Optionally, set a value to add to the `config-event` header (as a top-level `authToken` property) when AppScope establishes a connection.  

**Send metrics and events over the same connection?**: Defaults to `Yes`, which locks some settings into preconfigured values. Toggle to `No` if you prefer to route metrics and events independently.

**Connection type**: From the drop-down, choose one of the connection types described [below](#connections), then enter values in whatever fields the UI exposes.
* When **Send metrics and events over the same connection?** is toggled to `Yes`, you configure a single connection by choosing `TCP`, `Edge` (the default), or `Unix`.
* When routing metrics and events independently, you configure one connection for metrics and another for events; the drop-down offers two additional connection types: `UDP` and `File`.

#### Transports and Connection Types {#connections}

At the heart of any AppScope config are the transports you define. In AppScope, a transport specifies something to send, how to send it, and where to send it. The things to send can be metrics, events, or logs.

Depending on the choices you make in the **Transport** settings, the UI will make some or all of the following types of connections available.

**UDP**: The destination is the network server whose hostname or IP address you specify in the **Host** field, along with a UDP **Port** to listen on.

**TCP**: The destination is the network server whose hostname or IP address you specify in the **Host** field, along with a TCP **Port** to listen on. The **Enable TLS** toggle defaults to `No`.

**Unix**: The destination is a Unix domain server listening on the Unix domain **Socket path** you specify. 

**File**: The destination is the file whose directory location you specify in the **File path** field, along with a **Buffering** method.

**Edge**: The destination is Cribl Edge, listening on a preconfigured Unix domain socket. 

##### TLS Settings (TCP Only)

When the **Connection type** is `TCP`, the UI exposes the **Enabled** toggle, which defaults to `No`.

If you toggle **Enabled** to `Yes`:
* Set **Validate server certs** to `true` if you want the connection to fail when the TLS server certificate cannot be validated. Defaults to `No`.

* Leave **CA certificate path** blank if you toggled the **Validate server certs** to `true` and are using the local OS-provided trusted CA certificates to validate the server's certificate. Otherwise, specify the server path containing CA certificates, which must be in PEM format.

### Logs Settings

Configure where and how AppScope should send its own logs.

**Connection type**: From the drop-down, choose one of the connection types described [above](#connections), then enter values in whatever fields the UI exposes.

**Log level**: Set logging verbosity. When **Send metrics and events over the same connection?** is toggled to `Yes`, **Log level** is locked at `warning`.

**File path**: Specify a directory in which to store logs. If not specified, defaults to `/tmp/scope.log`.

**Buffering**: Set this method to`Line` if there's a chance that multiple scoped processes will be writing to the same file. This prevents the log file from getting scrambled, with interleaved lines. However, in single-writer scenarios, selecting `Full` often improves performance.

### Metrics Settings {#metrics}

The [Schema Reference](https://appscope.dev/docs/schema-reference) in the AppScope docs lists and fully describes every metric that AppScope can emit.

> Process (`proc.*`) metrics periodically report information about resource usage at a point in time. All other metrics report on actions taken by a scoped process. For examples, see [Metrics and Events in AppScope](https://appscope.dev/docs/working-with#events-and-metrics) in the AppScope documentation.
>
{.box .info}

**Should AppScope emit metrics?**: Defaults to `Yes`, which exposes the following options.

**Format**: AppScope can emit metrics in either of two formats: `NDJSON` (the default), or `StatsD`. If you choose `StatsD`, the UI exposes the following fields:
- **StatsD prefix**: Optionally, enter a prefix for AppScope to add to metrics. 
- **StatsD max length**: Optionally, enter a maximum length, in bytes, for StatsD-formatted metrics. AppScope will truncate any metric which exceeds the maximum length. (Defaults to `512`.)

**Verbosity**: Set a value between `0` and `9`. (Defaults to `4`.) Verbosity controls both the cardinality of metrics that AppScope emits, and which kinds of metrics AppScope aggregates. For details, see [Metrics and Events in AppScope](https://appscope.dev/docs/working-with#events-and-metrics) in the AppScope documentation. But the general principle is:
* At low verbosity, metrics summarize multiple actions over a reporting period.
* At higher verbosity, metrics provide details about single actions in real time.

**Reporting period**: Set the metric summary interval in seconds. Defaults to `10`. 

**Watch list**: Select categories of metrics to emit. Defaults to the following categories: `StatsD`, `File System`, `Network`, `HTTP`, `DNS`, `Process`. 

### Events Settings

The [Schema Reference](https://appscope.dev/docs/schema-reference) in the AppScope docs lists and fully describes every event that AppScope can emit.

**Should AppsScope emit events?**: Defaults to `Yes`, which exposes the following controls in this section.

**Event-rate limiter (events per second)**: AppScope discards events generated in excess of the limiter setting, which defaults to `10000`. To disable the limiter, set it to `0`. 

**Enhance filesystem event data**: When toggled to `Yes` (the default), AppScope enhances `fs.open` events by adding the `uid`, `gid`, and `mode` fields.

**Watch list**: Specify categories of events to emit, and configure their content and structure. Defaults to the following categories: `Console`, `File`, `Filesystem`, `Network`, `DNS`, and `HTTP`. For details, see [Categories](#categories).

#### Events Categories {#categories}

The **Watch list** is a table providing a row for each enabled category of events. The **Type** column contains the category names. The other columns (**Name**, **Field**, and **Value**) contain regular expressions. 

At the right edge of the **Name**, **Field**, and **Value** fields is a Copy button, a Flag button to append Regex flags, and an Expand button to open a validation modal. 

AppScope emits an event when its values match all the regexes in the corresponding category's row. For **HTTP**, request and response events must also match the headers listed in the **Header** column.

**Allow binary** is a toggle, available only for **Console** events. When toggled to `Yes` (the default), **Console** events will include binary data along with text data. If you prefer to redact binary data, toggle to `No`.

The **Type** column enumerates any or all of the following event category names. Toggle **Enabled** to `Yes` to include a category. 
- **Console**: Includes writes to standard out and error. Use this to monitor console output, especially in containerized environments where logging to files isn't commonly done. 
- **File**: Includes writes to files. Use this to monitor log files and/or any other files of interest.
- **Filesystem**: Includes filesystem operations like `open`, `close`, and `delete`.
- **Network**: Includes `open` and `close` events on network connections.
- **DNS**: Includes DNS request and response events. 
- **HTTP**: Includes HTTP request and response events.
- **Granular**: Seldom used, and not emitted by default. Can be useful when tracking down a problem. When this type is enabled, AppScope sends more events than it otherwise would, giving you a more granular view of the activity of the scoped process.

Some usage examples:
* For `fs.*` events, AppScope normally emits `fs.open` and `fs.close` events. With **Granular** enabled, every time a read or write occurs, AppScope emits `fs.read` or `fs.write`, respectively.
* For `net.*` events, AppScope normally emits `net.open`, `net.close`, and `net.app`.With **Granular** enabled, every time a read or write is done on the socket, AppScope emits `net.rx` or `net.tx`, respectively.

### Payloads {#payloads}

> AppScope never sends encrypted payloads to disk, to Cribl Stream, or to Cribl Edge.
>
{.box .success}

**Capture all payloads**: Enable capturing all payloads. Defaults to `No`. Enable this setting with caution: when scoping I/O-intensive programs, it can produce large amounts of data.

**Protocol detection**: Click **Add Protocol** to specify a protocol (e.g., StatsD, HTTP, Redis) for AppScope to detect in network payloads, as well as what AppScope should do upon detecting that protocol. AppScope checks the first packet on a channel against the regular expression for each protocol until it finds a match. Then AppScope treats the matching protocol as the detected protocol for its channel, and ignores any others listed. See [Sample Protocols](#protocols) for regexes that detect common protocols.

**Name**: Enter a short, descriptive name to define the protocol-detect event. AppScope inserts this value into the header for payloads sent to Cribl. Since AppScope sends this as JSON, you cannot include double quotes or backslashes.

**Regex**: Enter a regular expression to detect the desired protocol.

**Generate protocol-detect-events**: Toggle to `Yes` to generate protocol-detect events. Defaults to `No`. 

**Capture payload**: Toggle to `Yes` to send payloads from matching streams to the configured destinations. Defaults to `No`.

**Convert to hex**: When toggled to `Yes` (the default), AppScope converts part of the payload to a hex-string before applying the regex. Otherwise, AppScope applies the regex to the binary payload.

**Bytes to convert**: The number of bytes to convert to hex before applying the regex. Defaults to `256`.

**Payload directory**: Specify the directory where AppScope should store payload files. Consider using a performant filesystem to reduce I/O performance impacts. Defaults to `tmp`. When **General Settings** > **Send metrics and events over the same connection?** is toggled to `Yes`, AppScope ignores this field and sends payloads to the default destination.

### Advanced Settings

**Send process-start message?**: When toggled to `Yes` (the default), enables the [process-start message](https://appscope.dev/docs/working-with#the-processstart-message). This message is the first one AppScope sends upon establishing a connection. It describes AppScope's configuration for that session, as well as the application being scoped. 

**Command directory**: If you want to dynamically change AppScope configuration while scoping a running process, save a `scope.<pid>` file to a **command directory** of your choice, where `<pid>` is the PID of the scoped process, and the file contains lines of the form `<ENV_VAR>=<value>`. Once per reporting period, AppScope looks for a `scope.<pid>` file in the **command directory**, which defaults to `/tmp`. You can configure reporting period in the **Metrics** [tab](#metrics).

**Metadata**: If you want to add a field to events and metrics that AppScope emits, click **Add Metadata** and define it as a key-value pair. Key/value definitions cannot include environment variables.

**Per-process configs**: Optionally, define a config that, given a process that fits a particular pattern, will partly or completely override the config you have defined already.  

Click **Add config** and configure the **Filters** relevant to the process(es) you want to match:

- **Process name**: Enter a string to match the basename of the scoped process.
- **Process argument**: Enter an expression to match the scoped process's full command line including options and arguments.
- **Hostname**: Enter a string to match the hostname of the machine where the scoped process is running.
- **Username**: Enter a string to match the username for the scoped process's UID.
- **Environment**: To match when an environment variable is set, enter its name alone (i.e., `FOO`). To match when the variable is set with a particular value, enter both (i.e., `FOO=bar`).
- **Ancestor**: Enter a string to match the basename of the scoped process's parent, parent's parent, etc.

Then, in the **Config** column, click the **Edit** button. This takes you through the settings, beginning with **General Settings**, but this time you'll be specifying what in the previously-defined settings to override when a process matches your **Filters**.

## Protocol Detection Examples for Payloads {#protocols}

Here are some example definitions for [detecting protocols](#payloads) in payloads.

#### Redis 
This regex detects the plain-text Redis serialization protocol ([RESP](https://redis.io/docs/reference/protocol-spec/)).

**Name**: `Redis`

**Regex**: `"^[*]\\d+|^[+]\\w+|^[$]\\d+"`

#### Mongo 

This regex detects the [MongoDB wire protocol](https://www.mongodb.com/docs/manual/reference/mongodb-wire-protocol/). Since it's binary, we convert to hex.

**Name**: `Mongo`

**Regex**:`^240100000000000000000000d407`

**Convert to hex**: `Yes`

**Convert length**: `14`

#### HTTP 

By default, AppScope uses an internally-defined protocol detector for HTTP similar to this example.

**Name**: `HTTP`

**Regex**:`"HTTP\\/1\\.[0-2]|PRI \\* HTTP\\/2\\.0\r\n\r\nSM\r\n\r\n"`

#### StatsD

By default, AppScope uses an internally-defined protocol detector for StatsD similar to this example.

**Name**: `STATSD`

**Regex**:` "^([^:]+):([\\d.]+)\\|(c|g|ms|s|h)"`

#### TLS
By default, AppScope uses an internally-defined protocol detector for TLS/SSL similar to this example.

**Name**: `TLS`

**Regex**: `"^(?:(?:16030[0-3].{4})|(?:8[0-9a-fA-F]{3}01))"`

**Convert to hex**: `Yes`

**Convert length**: `5`
