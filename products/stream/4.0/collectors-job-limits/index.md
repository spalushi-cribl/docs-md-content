# Job Limits



You can configure global limits that optimize the execution of all Collectors and scheduled jobs (including Cribl Stream system tasks). You  access these limits as follows:
- Distributed deployments: [Select a Worker Group] > **Group/Fleet Settings** > **System** > **General Settings** > **Limits** > **Jobs**.
- Single-instance deployments: **Settings** > **Global Settings** > **System** > **General Settings** > **Limits** > **Jobs**.

![Job Limits settings](Job-Limits-Settings.ac52465d96.png)
{scale="70%"}

## Limits Available

The following controls are available at **Settings** > **Global Settings** > **General Settings > Job Limits**.

> In a [distributed deployment](/stream/deploy-distributed), these limits are set on, and deployed from, the Leader. They are applied at the Worker Group level (except where noted), and trickle down to individual Worker Processes in the group. Task limits are applied at the Worker Process level.
> 
> In a [single-instance deployment](deploy-single-instance), these limits are set on the single instance, and apply to all its Worker Processes.
>
{.box .success}

### Job Limits

**Concurrent Job Limit**: The total number of jobs that can run concurrently. Defaults to `10`.

> If you see jobs being skipped, this indicates that the **Concurrent Job Limit** for this Group has been reached or exceeded. Here, you need to increase this limit to reduce the number of skippable jobs. Note that, for resource-intensive jobs, this might trigger a need to deploy more Worker Nodes.
>
{.box .info}

**Concurrent System Job Limit**: The total number of **system** jobs that can run concurrently. Defaults to `10`.

**Concurrent Scheduled Job Limit**: The total number of **scheduled** jobs that can run concurrently. This limit is set as an offset relative to the **Concurrent Job Limit**. Defaults to `-2`.

### Task Limits

**Concurrent Task Limit**: The total number of tasks that a Worker Process can run concurrently. Defaults to `2`.

**Concurrent System Task Limit**: The number of system tasks that a Worker Process can run concurrently. Defaults to `1`.

**Max Task Usage Percentage**: Value, between `0` and `1`, representing the percentage of total tasks on a Worker Process that any single job may consume. Defaults to `0.5` (i.e., 50%).

**Task Poll Timeout**: The number of milliseconds that a Worker's task handler will wait to receive a task, before retrying a request for a task. Defaults to `60000` (i.e., 60 seconds).

### Completion Limits

**Artifact Reaper Period**: Interval on which Cribl Stream attempts to reap jobs' stale disk artifacts. Defaults to `30m`.

**Finished Job Artifacts Limit**: Maximum number of finished job artifacts to keep on disk. Defaults to `100`.

**Finished Task Artifacts Limit**: Maximum number of finished task artifacts to keep on disk, per job, on each Worker Node. Defaults to `500`.

### Task Manifest and Buffering Limits

**Manifest Flush Period**: The rate (in milliseconds) at which a job's task manifest should be refreshed. Defaults to `100` ms.

**Manifest Max Buffer Size**: The maximum number of tasks that the task manifest can hold in memory before flushing to disk. Defaults to `1,000`.

**Manifest Reader Buffer Size**: The number of bytes that the task manifest reader should pull from disk. Defaults to `4kb`.

**Job Dispatching**: The method by which tasks are assigned to Worker Processes. Defaults to `Least In‑Flight Tasks`, to optimize available capacity. `Round Robin` is also available.

**Job Timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Task Heartbeat Period**: The heartbeat period (in seconds) for tasks to report back to the Leader/API. Defaults to `60` seconds.


