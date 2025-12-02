# Job Limits



You can configure global limits that optimize the execution of all Collectors and scheduled jobs (including Cribl Stream system tasks). You  access and set these limits as follows.
- [Single-instance deployments](/stream/deploy-single-instance): **Settings** > **System** > **General Settings** > **Limits** > **Jobs**.

    These limits apply to all Worker Processes.
- [Distributed deployments](/stream/deploy-distributed): From the Leader, [select a Worker Group] > **Group Settings** > **System** > **General Settings** > **Limits** > **Jobs**. 

    These limits are applied at the Worker Group level (except where noted), and trickle down to individual Worker Processes in the Group.

![Group Settings > Job Limits](job-limits-settings.ab74f9faa1.png)
{scale="60%" border="true"}

## Limits Available

The following controls are available at the UI locations listed above.

### Job Limits

**Concurrent Job Limit**: The total number of jobs that can run concurrently. Defaults to `10`.

**Concurrent System Job Limit**: The total number of **system** jobs that can run concurrently. Defaults to `10`. Minimum `1`.

**Concurrent Scheduled Job Limit**: The total number of **scheduled** jobs that can run concurrently. This limit is set as an offset relative to the **Concurrent Job Limit**. Defaults to `-2`.

> Skipped jobs indicate that a Group's **Concurrent Job Limit** has been reached or exceeded. Increase this limit to reduce the number of skippable jobs. For resource-intensive jobs, this might require deploying more Worker Nodes.
>
{.box .info}

### Task Limits

**Concurrent Task Limit**: The total number of tasks that a Worker Process can run concurrently. Defaults to `2`. Minimum `1`.

**Concurrent System Task Limit**: The number of system tasks that a Worker Process can run concurrently. Defaults to `1`. Minimum `1`.

**Max Task Usage Percentage**: Value, between `0` and `1`, representing the percentage of total tasks on a Worker Process that any single job may consume. Defaults to `0.5` (i.e., 50%).

**Task Poll Timeout**: The number of milliseconds that a Worker's task handler will wait to receive a task, before retrying a request for a task. Defaults to `60000` (i.e., 60 seconds). Minimum `10000` (10 seconds).

### Completion Limits

**Artifact Reaper Period**: Interval on which Cribl Stream attempts to reap jobs' stale disk artifacts. Defaults to `30m`.

**Finished Job Artifacts Limit**: Maximum number of finished job artifacts to keep on disk. Defaults to `100`. Minimum `0`.

**Finished Task Artifacts Limit**: Maximum number of finished task artifacts to keep on disk, per job, on each Worker Node. Defaults to `500`. Minimum `0`.

### Task Manifest and Buffering Limits

**Manifest Flush Period**: The rate (in milliseconds) at which a job's task manifest should be refreshed. Defaults to `100` ms. Minimum `100`, maximum `10000`.

**Manifest Max Buffer Size**: The maximum number of tasks that the task manifest can hold in memory before flushing to disk. Defaults to `1000`. Minimum `100`, maximum `10000`.

**Manifest Reader Buffer Size**: The number of bytes that the task manifest reader should pull from disk. Defaults to `4kb`.

**Job Dispatching**: The method by which tasks are assigned to Worker Processes. Defaults to `Least In‑Flight Tasks`, to optimize available capacity. `Round Robin` is also available.

**Job Timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Task Heartbeat Period**: The heartbeat period (in seconds) for tasks to report back to the Leader/API. Defaults to `60` seconds. Minimum `60`.




