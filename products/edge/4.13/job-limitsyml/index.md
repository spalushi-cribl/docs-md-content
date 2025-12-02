# job-limits.yml


`job-limits.yml` maintains parameters for Collection jobs and system tasks. In the UI, you can configure these per Fleet at **Fleet Settings** > **General Settings** > **Limits** > **Jobs**.

Where we do not specify or validate a minimum or maximum value, Cribl provides a prudent default. Adjust this initial value to match your architecture and needs.

```yaml {title="job-limits.yml"}
disableTasks: # [boolean] Disable jobs/tasks - When jobs/tasks are disabled, Worker and Edge Nodes on version 4.5.0 and newer won't poll the Leader for tasks. Nodes running 4.4.4 and older will honor the Jobs settings even if jobs/tasks are disabled
concurrentJobLimit: # [number; default: 10, minimum: 1] Concurrent job limit - The total number of jobs that may run concurrently.
concurrentSystemJobLimit: # [number; default: 10, minimum: 1] Concurrent system job limit - The total number of system jobs that may run concurrently.
concurrentScheduledJobLimit: # [number, default: -2] Concurrent scheduled job limit - The total number of scheduled jobs that may run concurrently. Limit is relative to concurrentJobLimit, and cannot exceed that value.
concurrentTaskLimit: # [number; default: 2, minimum: 1] Concurrent task limit - The total number of tasks that a worker process may run concurrently
concurrentSystemTaskLimit: # [number; default: 1, minimum: 1] Concurrent system task limit - The number of system tasks that a worker process may run concurrently.
maxTaskPerc: # [number; default: 0.5, minimum: 0, maximum: 1] Max task usage percentage - Value from 0 to 1 representing the percentage of total tasks within the system a job may consume.
taskPollTimeoutMs: # [number; default: 60000, minimum: 10000] Task poll timeout - The number of milliseconds the worker's task handler will wait to receive a task before retrying the request for a task.
jobArtifactsReaperPeriod: # [string; default: 30 min.] Artifact reaper period - Time interval on which the system attempts to reap stale disk artifacts belonging to the jobs.
finishedJobArtifactsLimit: # [number; default: 100 items, minimum: 0] Finished job artifacts limit - Maximum number of finished job artifacts to keep on disk.
finishedTaskArtifactsLimit: # [number; default: 500 items, minimum: 0] Finished task artifacts limit - Maximum number of finished task artifacts to keep on disk per job on each Worker Node.
taskManifestFlushPeriodMs: # [number; default: 100, minimum: 100, maximum: 10000] Manifest flush period - The rate at which a job's task manifest should be refreshed, in milliseconds.
taskManifestMaxBufferSize: # [number; default: 1000, minimum: 100, maximum: 10000] Manifest max buffer size - The maximum number of tasks the task manifest can hold in memory before flushing to disk.
taskManifestReadBufferSize: # [string; default: 4 KB] Manifest reader buffer size - The number of bytes the task manifest reader should pull from disk.
schedulingPolicy: # [string; default: LeastInFlight] Job dispatching - The method by which tasks are assigned to worker processes to complete the work. Can also be set to: RoundRobin.
jobTimeout: # [string; default: 0] Job timeout - Maximum time a job is allowed to run. Defaults to 0, for unlimited time. Units are seconds if not specified. Sample entries: 30, 45s, 15m.
taskHeartbeatPeriod: # [number; default: 60, minimum: 60] Task heartbeat period - The heartbeat period (in seconds) for tasks to report back to the leader/API.
```
