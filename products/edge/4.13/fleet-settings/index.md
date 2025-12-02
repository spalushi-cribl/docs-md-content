# Configure Fleet Settings


Configure teleporting, throughput throttling, logging, and security at the Edge Fleet level.

---

> Required Permission: **Admin** 
>
{.box .info}

In Cribl Edge, you can use **Fleet Settings** to configure Node teleporting, throughput
throttling, logging, upgrading, and security at the Fleet level.

To edit Fleet Settings:

1. Select the Fleet, then select **Fleet Settings**. 
1. Configure the following settings per Fleet.

## General Settings {#general}

### Fleet Configuration

You can configure the following general Fleet options on the **Overview** tab:

|Fleet Configuration Setting|Description|
|---------------------------|-----------|
|**Enable teleporting to Nodes**|Toggle on to enable authenticated access to the Edge Node UI from the Leader. Toggle off to disable. For more information about teleporting, see [Teleport into an Edge Node](managing-edge-nodes#teleport-into-an-edge-node).|
|**Description**|Optionally, add or edit the Fleet description.|
|**Tags**|Use tags to organize Fleets into logical categories. Then, you can search by tags on the **View all Fleets** interface. This search filters the list of Fleets, showing only those with the tag you entered.|
|**Time to keep disconnected Nodes**|Configure how long the Fleet should retain information about a Node that disconnects from the Leader. Format examples: 8h, 5d, 1w (format defaults to days if you only specify a number). Default: 1 day.|
|**Inherited configuration**|Optionally, select an existing Fleet that this Fleet will inherit configurations from.|

> Settings configured on the **Overview** tab are not inherited by Subfleets.
{.box .success}

### API Server Settings

#### General

You can set the following options for the API server:

|API Server Setting|Description|
|------------------|-----------|
|**Host**|The hostname or IP address you want to bind the API server to.<ul><li>Defaults to `127.0.0.1` for standalone Edge Nodes and Edge Fleets.</li><li>You can override the value with the `CRIBL_API_HOST` [environment variable](environment-variables).</li></ul>|
|**Port**|API port to listen to.<ul><li>Defaults to `9420` for standalone Edge Nodes and Edge Fleets.</li><li>You can override the value with the `CRIBL_API_PORT` [environment variable](environment-variables).</li></ul>|

#### TLS

For information on TLS options, see the documentation for any Source or
Destination that supports TLS.

#### Advanced

You can set the following advanced options for the API server:

|<div style="width: 100px">API Advanced Server Setting</div>|Description|
|---------------------------|-----------|
|**Retry count**|The number of times to retry binding to the API port. Default is `120`.|
|**Retry period**|The period between consecutive retries for API port binding, in seconds. Default is `5`.|
|**URL base path**|The URL base path from which to serve all assets. Setting a URL base path may be useful when operating behind a proxy server.|
|**Listen on port**:|Toggle on (default) to expose the API service to the network on the configured API port. When toggled off, Cribl Edge doesn't listen on the port (`9420` by default).|
|**Local UI access**|Toggle on (default) to allow direct browser access to the UI for Edge Nodes.|
|**Logout on roles change**|If [role-based access control](roles) is enabled, toggle on to automatically log out users when their assigned Roles change. Default is toggled on.|
|**Auth-token TTL**|Authentication tokens' valid lifetime, in seconds. Default is `3600` (60 minutes = 1 hour); minimum is `1`.|
|**Session idle time limit**|How long to observe no user interaction before invalidating users' session tokens, in seconds. Default is `3600` (60 minutes = 1 hour); minimum is `60`.|
|**Login rate limit**|The number of login attempts allowed over the specified unit of time. For example, to limit login attempts to 50 per minute, specify the login rate limit `50/minute`. Valid units of time are `second`, `minute`, `hour`, and `day`. Default is `2/second`.|
|**SSO/SLO callback rate limit**|The number of requests to SSO and SLO callback endpoints allowed over the specified unit of time. For example, to limit requests to 10 per minute, specify `10/minute`. Valid units of time are `second`, `minute`, `hour`, and `day`.|
|**HTTP headers**|One or more custom HTTP headers to send with every response.|
|**Enable API cache**|Toggle on to enable browser caching of frequent API requests. Default is toggled on. Toggling off can slow the the UI response time.|

### Default TLS Settings

See the [System-wide TLS Settings Including Ciphers](securing-tls-overview#ciphers).

### Limits

The **Limits** tab provides access to controls for metrics, storage, metadata,
jobs, the Redis cache and connections to it, and CPU settings.

#### Metrics

[Snippet not found: content/shared/4.13/snippets/_internal-metrics.md]

For more information about each of the settings above, go to [Controlling Metrics Volume](monitoring#control-metrics-volume).

#### Storage {#storage}

You can configure the following options for storage:

|<div style="width: 100px">Storage Setting</div>|Description|
|---------------------------|-----------|
|**Sample size limit**| Maximum file size, in binary units (`KB`, `MB`), for sample data files. Maximum: `3 MB`. Default: `256 KB`.|
|**Min free disk space**| The minimum amount of disk space on the host before various features take measures to prevent disk usage (KB, MB, etc.). Default: `5 GB`. To avoid the ["Unable to write to the filesystem, disk space low" error](/stream/common-errors/#unable-to-write-to-the-filesystem-disk-space-low), ensure that the available disk space remains above this threshold by monitoring disk usage and configuring alerts.|
|**Worker Process PQ size limit**| Highest accepted value for the **Queue size limit** option used in individual Sources' and Destinations' persistent queues. Default: `1 TB`. Consult Cribl Support before increasing beyond this value.|

#### Metadata

**Event metadata sources**: List of event metadata sources to enable. No sources
are enabled by default.

#### Jobs and Tasks {#job-limits}

|<div style="width: 100px">Job Limits Setting</div>|Description|
|---------------------------|-----------|
**Disable jobs/tasks**| When enabled, the Edge Nodes won't poll the Leader for jobs/tasks. The job limits settings below will not affect Edge Nodes on version 4.5.0 and newer. Edge Nodes running 4.4.4 and older still use these jobs settings even if jobs/tasks are disabled here.|
|**Concurrent job limit**|The total number of jobs that can run concurrently. Defaults to `10`.|
|**Concurrent system job limit**|The total number of **system** jobs that can run concurrently. Defaults to `10`. Minimum `1`.|
|**Concurrent scheduled job limit**|The total number of **scheduled** jobs that can run concurrently. This limit is set as an offset relative to the **Concurrent job limit**. Defaults to `-2`.|

> Skipped jobs indicate that a Fleet's **Concurrent job limit** has been reached
> or exceeded. Increase this limit to reduce the number of skippable jobs. For
> resource-intensive jobs, this might require deploying more Edge Nodes.
>
{.box .info}

|<div style="width: 100px">Task Limits Setting</div>|Description|
|---------------------------|-----------|
|**Concurrent task limit**| The total number of tasks that a Worker Process can run concurrently. Defaults to `2`. Minimum `1`.|
|**Concurrent system task limit**| The number of system tasks that a Worker Process can run concurrently. Defaults to `1`. Minimum `1`.|
|**Task usage percentage limit**|Value, between `0` and `1`, representing the percentage of total tasks on a Worker Process that any single job may consume. Defaults to `0.5` (50%).|
|**Task poll timeout**|The number of milliseconds that a Worker's task handler will wait to receive a task, before retrying a request for a task. Defaults to `60000` (60 seconds). Minimum `10000` (10 seconds).|

### Completion Limits

|<div style="width: 100px">Completion Limits Setting</div>|Description|
|---------------------------|-----------|
|**Artifact reaper period**|Interval on which Cribl Edge attempts to reap jobs' stale disk artifacts. Defaults to `30m`.|
|**Finished job artifacts limit**|Maximum number of finished job artifacts to keep on disk. Defaults to `100`. Minimum `0`.|
|**Finished task artifacts limit**| Maximum number of finished task artifacts to keep on disk, per job, on each Edge Node. Defaults to `500`. Minimum `0`.|

### Task Manifest and Buffering Limits

|<div style="width: 100px">Task Manifest and Buffering Setting</div>|Description|
|---------------------------|-----------|
|**Manifest flush period**|The rate (in milliseconds) at which to refresh the task manifest of a job. Defaults to `100` ms. Minimum `100`, maximum `10000`.|
|**Manifest buffer size limit**|The maximum number of tasks that the task manifest can hold in memory before flushing to disk. Defaults to `1000`. Minimum `100`, maximum `10000`.|
|**Manifest reader buffer size**|The number of bytes that the task manifest reader should pull from disk. Defaults to `4kb`.|
|**Job dispatching**| The method by which tasks are assigned to Worker Processes. Defaults to `Least In‑Flight Tasks`, to optimize available capacity. `Round Robin` is also available.|
|**Job timeout**|Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.|
|**Task heartbeat period**|The heartbeat period (in seconds) for tasks to report back to the Leader/API. Defaults to `60` seconds. Minimum `60`.|

#### Redis 

##### Cache

|<div style="width: 100px">Redis Cache Setting</div>|Description|
|---------------------------|-----------|
|**Key TTL in seconds**|Maximum time to live of a key in the cache (seconds). `0` indicates no limit. Defaults to `10 minutes`.|
|**Max # of keys**| Maximum number of keys to retain in the cache. `0` indicates no limit. Defaults to `0`.|
|**Max cache size (bytes)**| Maximum number of bytes to retain in the cache. `0`indicates no limit. Defaults to `0`.|
|**Service period (seconds)**| Frequency of cache limit enforcement. Defaults to every `30 seconds`.|
|**Server assisted**|Default is toggled off. When toggled on, the following control appears.|
|**Client tracking mechanism**:| Mechanism for invalidation message delivery. In default mode, the server remembers which keys a client has requested and only sends invalidations for those, using more Redis server memory. In broadcast mode, it sends all invalidations, requiring more processing by Cribl Edge.|

##### Connections {#redis-connections-limit}

|<div style="width: 100px">Redis Connections Setting</div>|Description|
|---------------------------|-----------|
|**Reuse Redis connections**|Toggle on if you want Cribl Edge to try to reuse Redis connections when multiple [Redis Functions](redis-function) (or references to them) are present. When enabled, displays the following additional control:<ul><li>**Connection limit**: The maximum number of identical connections allowed before Cribl Edge tries to reuse connections. Defaults to `0`, meaning unlimited connections are allowed (equivalent to leaving **Reuse Redis connections** toggled off). Setting a non-zero integer value forces Cribl Edge to try to reuse connections for each individual Worker Process (**not** to reuse connections **among** Worker Processes).</li></ul>|

To understand why and when to employ these controls, see [Reusing Redis Connections](redis-function#managing-redis-connections).

#### Other {#other}

|<div style="width: 100px">Other Setting</div>|Description|
|---------------------------|-----------|
|**CPU profile TTL**|The time-to-live for collected CPU profiles.|
|**Default managed node heartbeat period**|How many seconds a managed Node will wait to send back a heartbeat to the Cribl control plane.|
|**Config bundle download timeout**|How many seconds a Cribl Stream Worker will wait for a successful Leader connection before canceling a download of a new configuration bundle. This timeout helps prevent Workers from hanging indefinitely when there are network issues or other delays during the download process. A `0` value means wait indefinitely, which could cause Workers to hang.|
**Use Windows tools to collect process info**|Enable PowerShell to collect process information instead of using the native API. <br>This legacy setting will be removed in a future release. We highly recommend keeping this option disabled and using the newer native capabilities which are faster and more reliable.|

> When using PowerShell to collect process information, the environment variables displayed in the [**Explore** > **Processes**](explore-edge-win#processes) tab might be inaccurate.
> They will reflect the environment of the PowerShell process itself, not the actual environment of the process being viewed.
{.box .warning}

### Proxy Settings

**Use proxy env vars**: Honors the `HTTP_PROXY/HTTPS_PROXY` environment variables. Defaults to toggled on.

Cribl prioritizes environment variables for proxy settings in this order: Process, User, and System. 

If your Cribl service is managed by a service manager other than systemd (such as upstart or init), the `Use proxy env vars` toggle might not behave as expected because Cribl might prioritize environment variables set by the service manager instead of using the proxy settings you intended.

### Sockets

**Directory**: Holds sockets for inter-process communication (IPC), such as communications between a load-balancing process and a Worker Process. Defaults to `/tmp` (your system's temp directory).

### Shutdown Settings {#shutdown}

**Drain timeout (sec)**: Determines how long a Cribl server will wait for writes to complete before the server shuts down on individual Worker Processes. If you notice that Workers are under-ingesting available data upon shutdown or restart, increase the `10`–second default. Acceptable range of values: minimum `1` second, maximum `600` seconds (10 minutes). 

## Worker Processes {#processes}

|<div style="width: 150px">Worker Processes Setting</div>|Description|
|---------------------------|-----------|
|**Process count**|**This setting only applies to the [Kubernetes Logs Source](sources-kubernetes-logs)**. Indicates the number of Worker Processes to spawn. Positive numbers specify an absolute number of Workers. Negative numbers specify a number of Workers relative to the number of CPUs in the system, for example: {<`number of CPUs available`> minus <`this setting`>}. The default in Cribl Edge is 1.<ul><li>You can use **Process count** to adjust the number of Worker Processes **only for the Kubernetes Logs Source**. This is necessary when running the Kubernetes Logs Source with load balancing enabled. Load balancing increases performance when you have a higher event ingest volume than one Worker Process can handle.</li><li>Cribl Edge will provision at least the **Minimum process count** set below, but no more than the number of CPUs the host has available.</li></ul>|
|**Minimum process count**|Indicates the minimum number of Worker Processes to spawn. Overrides the lowest Process count result. Cribl Edge interprets a 0 entry as “default,” which here yields 1 Process.|
|**Memory (MB)**|Amount of heap memory available to each Worker Process, in MB. The OS allocates heap memory dynamically as the Process requests it up to the amount dictated by this setting. The Memory (MB) setting does not govern external (ext) memory.|
|**Enable heap snapshots**|Default is toggled off. Toggle on for Cribl Edge to automatically create memory snapshots for Worker Processes when they approach or exceed memory limits. Only the two most recent heap snapshots are retained. Older snapshots are automatically deleted. This behavior cannot be modified.|

> The **Enable heap snapshots** is available for hybrid or on-prem deployments only. This setting should only be enabled if recommended by support or if you are experiencing out-of-memory issues. Be aware that enabling this setting may impact system performance and storage usage. 
>
{.box .warning}

> #### Non-configurable Settings
> These settings can't be configured in Cribl Edge, and instead have default values.
> 
> **Max connections at startup**: Maximum number of connections accepted at Worker Process startup. Defaults to `1`.
> 
> **Startup throttling duration (ms)**: Maximum time (in milliseconds) to continue throttling connections after  Worker Process startup. Defaults to `10000` ms (10 sec). 
> 
>**Load throttle %**: Sets a threshold to prevent overwhelming Workers. If 90% of a Worker Process' CPU utilization readings exceed this threshold over one minute, the process will reject new connections until the CPU load stabilizes. Another process that is below the threshold will accept the connection the next time it is established. Defaults to `0`% (no throttling). Enter a percentage between `1`–`100` to enable throttling.
{.box .info}

> You can configure the CPU saturation threshold, but the 90% sampling trigger is not configurable. Also, `_raw stats` > `cpuPerc` values might diverge from your **Load throttle %** threshold. This is because `cpuPerc` is sampled and averaged once per minute, whereas the **Load throttle %** is evaluated every second, with a rolling 1-minute lookback sample. (These intervals are also not configurable.)
>
{.box .info}

## Other Settings

This page's remaining options work essentially the same way as their counterparts in **Settings** > **Global**. Use the following links for details about: [logging](monitoring#log_settings) levels/redactions, [access management](access-management), [security](securing-and-monitoring), [scripts](scripts), and [diagnostics](/stream/diagnosing). 

