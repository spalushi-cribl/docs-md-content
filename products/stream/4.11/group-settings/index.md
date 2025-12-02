# Configure Worker Group Settings


> Required Permission: **Admin** 
>
{.box .info}

Group Settings allows you to configure teleporting, throughput
throttling, logging, upgrading, and security at the Worker Group level.

To edit Group Settings:

1. Select the Worker Group, then select **Group Settings**. 
1. Configure the following settings per Worker Group.

## General Settings {#general}

### Worker Group Configuration

You can configure the following options on this tab:

**Description**: Optionally, add or edit the Worker Group description.

**Tags**: Use tags to organize Worker Groups into logical categories. Then, you can search by tags on the **View all Groups** interface. This search filters the list of Worker Groups, showing only those with the tag you entered.

**Enable teleporting to Workers**:Toggle on to enable authenticated access to Workers' UI from the Leader [Teleporting](setting-up-leader-and-worker-nodes#teleporting). Toggle off to disable.

### API Server Settings

#### General

You can set the following options for the API server.

**Host**: The hostname or IP address you want to bind the API server to.
Defaults to `0.0.0.0` for Leader, standalone Stream Workers, and Worker Groups.
 
You can override the value with the `CRIBL_API_HOST` [environment variable](environment-variables).

**Port**: API port to listen to.

Defaults to `9000` for Leader, standalone Stream Workers, and Worker Groups.

You can override the value with the `CRIBL_API_PORT` [environment variable](environment-variables).

#### TLS

For information on TLS options, see the documentation for any Source or
Destination that supports TLS.

#### Advanced

Set the following advanced options for the API server:

- **Retry count**: The number of times to retry binding to the API port. Default is `120`.

- **Retry period**: The period between consecutive retries for API port binding, in seconds. Default is `5`.

- **URL base path**: The URL base path from which to serve all assets. Setting a URL base path may be useful when operating behind a proxy server.

- **Local UI access**: Toggle on (default) to allow direct browser access to the UI for Worker Nodes.

[Snippet not found: content/shared/4.11/snippets/_api-server-settings-authentication.md]

- **Enable API cache**: Toggle on to enable browser caching of frequent API requests. Default is toggled on. Toggling off can slow the the UI response time.

### Default TLS Settings

See the [System-wide TLS Settings Including Ciphers](securing-tls-overview#ciphers).

### Limits

The **Limits** tab provides access to controls for metrics, storage, metadata,
jobs, the Redis cache and connections to it, and CPU settings.

### Metrics

See [Controlling Metrics Volume](monitoring#volume).

#### Storage {#storage}

You can configure the following options for storage. In **General Settings**,
open **Limits** and then select **Storage**:

**Sample size limit**: Maximum file size, in binary units (`KB`, `MB`), for sample
data files. Maximum: `3 MB`. Default: `256 KB`.

**Min free disk space**: The minimum amount of disk space on the host before various features take measures to prevent disk usage (KB, MB, etc.). Default: `5 GB`. To avoid the ["Unable to write to the filesystem, disk space low" error](/stream/common-errors/#unable-to-write-to-the-filesystem-disk-space-low), ensure that the available disk space remains above this threshold by monitoring disk usage and configuring alerts.

**Worker Process PQ size limit**: Highest accepted value for the **Queue size limit**/**Max queue size** option used in individual Sources' and Destinations' persistent queues. Default: `1 TB`. Consult Cribl Support before increasing beyond this value.

#### Metadata

**Event metadata sources**: List of event metadata sources to enable. No sources
are enabled by default.


### Job Limits {#job-limits}

**Disable jobs/tasks**: Cribl Edge only. See [Fleet Settings](/edge/fleet-settings#job-limits)
for more information. 

**Concurrent job limit**: The total number of jobs that can run concurrently.
Defaults to `10`.

**Concurrent system job limit**: The total number of **system** jobs that can
run concurrently. Defaults to `10`. Minimum `1`.

**Concurrent scheduled job limit**: The total number of **scheduled** jobs that
can run concurrently. This limit is set as an offset relative to the
**Concurrent job limit**. Defaults to `-2`.

> Skipped jobs indicate that a Group's **Concurrent job limit** has been reached
> or exceeded. Increase this limit to reduce the number of skippable jobs. For
> resource-intensive jobs, this might require deploying more Worker Nodes.
>
{.box .info}

### Task Limits

**Concurrent task limit**: The total number of tasks that a Worker Process can
run concurrently. Defaults to `2`. Minimum `1`.

**Concurrent system task limit**: The number of system tasks that a
Worker Process can run concurrently. Defaults to `1`. Minimum `1`.

**Task usage percentage limit**: Value, between `0` and `1`, representing the
percentage of total tasks on a Worker Process that any single job may consume.
Defaults to `0.5` (50%).

**Task poll timeout**: The number of milliseconds that a Worker's task handler
will wait to receive a task, before retrying a request for a task. Defaults to
`60000` (60 seconds). Minimum `10000` (10 seconds).x

### Completion Limits

**Artifact reaper period**: Interval on which Cribl Stream attempts to reap jobs'
stale disk artifacts. Defaults to `30m`.

**Finished job artifacts limit**: Maximum number of finished job artifacts to
keep on disk. Defaults to `100`. Minimum `0`.

**Finished task artifacts limit**: Maximum number of finished task artifacts to
keep on disk, per job, on each Worker Node. Defaults to `500`. Minimum `0`.

### Task Manifest and Buffering Limits

**Manifest flush period**: The rate (in milliseconds) at which a job's task
manifest should be refreshed. Defaults to `100` ms. Minimum `100`, maximum
`10000`.

**Manifest buffer siz limite**: The maximum number of tasks that the task manifest
can hold in memory before flushing to disk. Defaults to `1000`. Minimum `100`,
maximum `10000`.

**Manifest reader buffer size**: The number of bytes that the task manifest
reader should pull from disk. Defaults to `4kb`.

**Job dispatching**: The method by which tasks are assigned to Worker Processes.
Defaults to `Least In‑Flight Tasks`, to optimize available capacity.
`Round Robin` is also available.

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for
unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`,
`15m`.

**Task heartbeat period**: The heartbeat period (in seconds) for tasks to report
back to the Leader/API. Defaults to `60` seconds. Minimum `60`.

#### Redis 

##### Cache

**Key TTL in seconds**: Maximum time to live of a key in the cache (seconds). `0` indicates no limit. Defaults to `10 minutes`.

**Max # of keys**: Maximum number of keys to retain in the cache. `0` indicates
no limit. Defaults to `0`.

**Max cache size (bytes)**: Maximum number of bytes to retain in the cache. `0`
indicates no limit. Defaults to `0`.

**Service period (seconds)**: Frequency of cache limit enforcement. Defaults to
every `30 seconds`.

**Server assisted**: Default is toggled off. When toggled on, the following
control appears.

**Client tracking mechanism**: Mechanism for invalidation message delivery. In
default mode, the server remembers which keys a client has requested and only
sends invalidations for those, using more Redis server memory. In broadcast
mode, it sends all invalidations, requiring more processing by Cribl Stream.

##### Connections {#redis-connections-limit}

**Reuse Redis connections**: Toggle on if you want Cribl Stream to try to reuse Redis connections when multiple [Redis Functions](redis-function) (or references to them) are present. 
When enabled, displays the following additional control:

* **Connection limit**: The maximum number of identical connections allowed before Cribl Stream tries to reuse connections.  Defaults to `0`, meaning unlimited connections are allowed (equivalent to leaving **Reuse Redis connections** toggled off). Setting a non-zero integer value forces Cribl Stream to try to reuse connections for each individual Worker Process (**not** to reuse connections **among** Worker Processes).

To understand why and when to employ these controls, see [Reusing Redis Connections](redis-function#managing-redis-connections).

#### Other {#other}

**CPU profile TTL**: The time-to-live for collected CPU profiles.

**Default managed node heartbeat period**: How many seconds a managed Worker will
wait to send back a heartbeat to the Cribl control plane.

**Config bundle download timeout**: How many seconds a Cribl Stream Worker will wait for a successful Leader connection before canceling a download of a new configuration bundle. This timeout helps prevent Workers from hanging indefinitely when there are network issues or other delays during the download process. A `0` value means wait indefinitely, which could cause Workers to hang.

### Proxy Settings

**Use proxy env vars**: Honors the `HTTP_PROXY/HTTPS_PROXY` environment variables. Defaults to toggled on.

Cribl prioritizes environment variables for proxy settings in this order: Process, User, and System. 

If your Cribl service is managed by a service manager other than systemd (such as upstart or init), the `Use proxy env vars` toggle might not behave as expected because Cribl might prioritize environment variables set by the service manager instead of using the proxy settings you intended.

### Sockets

**Directory**: Holds sockets for inter-process communication (IPC), such as communications between a load-balancing process and a Worker Process. Defaults to `/tmp` (your system's temp directory).

### Shutdown Settings {#shutdown}

**Drain timeout (sec)**: Determines how long a Cribl server will wait for writes to complete before the server shuts down on individual Worker Processes. If you notice that Workers are under-ingesting available data upon shutdown or restart, increase the `10`–second default. Acceptable range of values: minimum `1` second, maximum `600` seconds (10 minutes). 

## Worker Processes {#processes}

[Snippet not found: content/stream/4.11/snippets/_worker-processes.md]

The following controls are also available on this tab to optimize Worker Processes' throughput on startup.

**Max connections at startup**: Maximum number of connections accepted at Worker Process startup. Defaults to `1`. Enter a negative integer for unlimited connections.

**Startup throttling duration (ms)**: Maximum time (in milliseconds) to continue throttling connections after Worker Process startup. Defaults to `10000` ms (10 sec.) Enter `0` to disable throttling.

**Load throttle %**: Sets a threshold to prevent overwhelming Workers. If 90% of a Worker Process' CPU utilization readings exceed this threshold over one minute, the process will reject new connections until the CPU load stabilizes. Another process that is below the threshold will accept the connection the next time it is established. Defaults to `0`% (no throttling). Enter a percentage between `1`–`100` to enable throttling. 

> You can configure the CPU saturation threshold, but the 90% sampling trigger is not configurable. Also, `_raw stats` > `cpuPerc` values might diverge from your **Load throttle %** threshold. This is because `cpuPerc` is sampled and averaged once per minute, whereas the **Load throttle %** is evaluated every second, with a rolling 1-minute lookback sample. (These intervals are also not configurable.)
>
{.box .info}

**Enable heap snapshots**: Default is toggled off. Toggle on for Cribl Stream to automatically create memory snapshots for Worker Processes when they approach or exceed memory limits. Only the two most recent heap snapshots are retained. Older snapshots are automatically deleted. This behavior cannot be modified. 

> The **Enable heap snapshots** is available for hybrid or on-prem deployments only. This setting should only be enabled if recommended by support or if you are experiencing out-of-memory issues. Be aware that enabling this setting may impact system performance and storage usage. 
>
{.box .warning}

## Other Settings

This page's remaining options work essentially the same way as their counterparts in **Settings** > **Global**. Use the following links for details about: [logging](monitoring#log_settings) levels/redactions, [access management](access-management), [security](securing-and-monitoring), [scripts](scripts), and [diagnostics](/stream/diagnosing). 

