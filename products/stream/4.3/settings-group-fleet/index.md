# Worker Group Settings


By selecting **Settings** > **Group Settings** from Cribl Stream's top nav, you can configure the following settings per Worker Group.

## General Settings {#general}

You can configure the following options in **Worker Group Configuration**:

 **Description**: Optionally, add or edit a description of the Worker Group's purpose.
 
 **Enable teleporting to Workers**: Use this toggle to enable or disable authenticated access to Workers' UI from the Leader ([Stream](/stream/deploy-distributed#worker-access), [Edge](/edge/explore-edge#teleport)).

In Cribl Stream 4.1.2 and later, you can configure the following option in **Shutdown Settings**:

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

> You configure the CPU saturation threshold, but the 90% sampling trigger is not configurable. Also, `_raw stats` > `cpuPerc` values might diverge from your **Load throttle %** threshold. This is because `cpuPerc` is sampled and averaged once per minute, whereas the **Load throttle %** is evaluated every second, with a rolling 1-minute lookback sample. (These intervals are also not configurable.)
>
{.box .info}

**Single-threaded mode (V8)**: When enabled (the default), limits the engine's garbage collection to a single thread. This option does not alter the threading used for processing events.  Consult Cribl Support before toggling this off, which might degrade performance.

## Other Settings

This page's remaining options work essentially the same way as their **Global Settings** counterparts. Use the following links for details about: [logging](monitoring#log_settings) levels/redactions, [access management](access-management) [security](securing-and-monitoring), [scripts](scripts), and [diagnostics](/stream/diagnosing). 

