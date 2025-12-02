# Worker Group Settings


By selecting **Settings** > **Group Settings** from Cribl Stream's top nav, you can configure the following settings per Worker Group.

## General Settings {#general}

### Worker Group Configuration

You can configure the following options on this tab:

 **Description**: Optionally, add or edit a description of the Worker Group's purpose.
 
 **Enable teleporting to Workers**: Use this toggle to enable or disable authenticated access to Workers' UI from the Leader ([Stream](/stream/deploy-distributed#worker-access), [Edge](/edge/explore-edge#teleport)).

In Cribl Stream 4.1.2 and later, you can configure the following option in [**Shutdown Settings**](#shutdown).

### API Server Settings

#### General

You can set the following options in **General Settings** > **API Server Settings** > **General**:

**Host**: The hostname or IP address you want to bind the API server to. Defaults to `0.0.0.0`.

**Port**: API port to listen to. Defaults to `9000`.

#### TLS

For information on TLS options, see the documentation for any Source or Destination that supports TLS.

#### Advanced

Set the following advanced options for the API server:

- **Retry count**: The number of times to retry binding to the API port. Default is `120`.

- **Retry period**: The period between consecutive retries for API port binding, in seconds. Default is `5`.

- **URL base path**: The URL base path from which to serve all assets. Setting a URL base path may be useful when operating behind a proxy server.

- **Local UI access**: Toggle on (default) to allow direct browser access to the UI for Worker Nodes.

[Snippet not found: content/shared/4.4/snippets/_api-server-settings-authentication.md]

- **Enable API cache**: Toggle on to enable browser caching of frequent API requests. Default is toggled on. Toggling off can slow the the UI response time.

### Default TLS Settings

See the [Securing and Monitoring topic](securing-and-monitoring#cyphers).

### Limits

The **Limits** tab provides access to controls for metrics, storage, metadata, jobs, the Redis cache, and CPU settings.

### Metrics

See [Controlling Metrics Volume](monitoring#volume).

#### Storage {#storage}

You can configure the following options in **General Settings** > **Limits** > **Storage**:

**Max sample size**: Maximum file size, in binary units (KB, MB), for sample data files. Maximum: `3 MB`. Default: `256 KB`.

**Min free disk space**: The minimum amount of disk space on the host before various features take measures to prevent disk usage (KB, MB, etc.). Default: `5 GB`.

**Max PQ size per Worker Process**: Highest accepted value for the **Max queue size** option used in individual Sources' and Destinations' persistent queues. Default: `1 TB`. Consult Cribl Support before increasing beyond this value.

#### Metadata

**Event metadata sources**: List of event metadata sources to enable. No sources are enabled by default.

#### Jobs

See [Job Limits](/stream/collectors-job-limits#job-limits).

#### Redis Cache

**Key TTL in seconds**: Maximum time to live of a key in the cache (seconds). `0` indicates no limit. Defaults to `10 minutes`.

**Max # of keys**: Maximum number of keys to retain in the cache. `0` indicates no limit. Defaults to `0`.

**Max cache size (bytes)**: Maximum number of bytes to retain in the cache. `0` indicates no limit. Defaults to `0`.

**Service period (seconds)**: Frequency of cache limit enforcement. Defaults to every `30 seconds`.

**Server assisted**: Defaults to `No`. When toggled to `Yes`, the following control appears.

**Client tracking mechanism**: Mechanism for invalidation message delivery. In default mode, the server remembers which keys a client has requested and only sends invalidations for those, using more Redis server memory. In broadcast mode, it sends all invalidations, requiring more processing by Cribl Stream.

#### Other

**CPU profile TTL**: The time-to-live for collected CPU profiles.

**Default managed node heartbeat period**: How many seconds a managed node will wait to send back a heartbeat to the Cribl control plane.

### Proxy Settings

**Use proxy env vars**: Honors the `HTTP_PROXY/HTTPS_PROXY` environment variables. Defaults to `Yes`.

> Cribl prioritizes environment variables for proxy settings in this order: Process, User, and System. If your Cribl service is managed by a service manager other than systemd (such as upstart or init), the `Use proxy env vars` toggle might not behave as expected because Cribl might prioritize environment variables set by the service manager instead of using the proxy settings you intended.
>
{.box .info}

### Shutdown Settings {#shutdown}

**Drain timeout (sec)**: Determines how long a Cribl server will wait for writes to complete before the server shuts down on individual Worker Processes. If you notice that Workers are under-ingesting available data upon shutdown or restart, increase the `10`–second default. Acceptable range of values: minimum `1` second, maximum `600` seconds (10 minutes). 

## Worker Processes {#processes}

For details about this left tab's **Process count**, **Minimum process count**, and **Memory (MB)** controls, see [Sizing and Scaling](/stream/scaling#worker-processes). 

> **Process count** and **Minimum process count** are not configurable on Cribl Edge, where each Edge Node is automatically allocated `1` Worker Process.
>
{.box .info}

The following controls are also available on this tab to optimize Worker Processes' throughput on startup.

**Max connections at startup**: Maximum number of connections accepted at Worker Process startup. Defaults to `1`. Enter a negative integer for unlimited connections.

**Startup throttling duration (ms)**: Maximum time (in milliseconds) to continue throttling connections after Worker Process startup. Defaults to `10000` ms (10 sec.) Enter `0` to disable throttling.

**Load throttle %**: Sets a threshold to prevent overwhelming Workers. If 90% of a Worker Process' CPU utilization readings over the preceding 1 minute exceed this threshold, Cribl Stream will reject new connections until the CPU load stabilizes. Defaults to `0`% (no throttling). Enter a percentage between `1`–`100` to enable throttling. 

> You can configure the CPU saturation threshold, but the 90% sampling trigger is not configurable. Also, `_raw stats` > `cpuPerc` values might diverge from your **Load throttle %** threshold. This is because `cpuPerc` is sampled and averaged once per minute, whereas the **Load throttle %** is evaluated every second, with a rolling 1-minute lookback sample. (These intervals are also not configurable.)
>
{.box .info}

## Other Settings

This page's remaining options work essentially the same way as their **Global Settings** counterparts. Use the following links for details about: [logging](monitoring#log_settings) levels/redactions, [access management](access-management) [security](securing-and-monitoring), [scripts](scripts), and [diagnostics](/stream/diagnosing). 

