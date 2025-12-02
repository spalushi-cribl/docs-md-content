# job-limits.yml


`job-limits.yml` maintains parameters for collection jobs and system tasks. In the UI, you can configure these at **Group Settings** > **General Settings** > **Limits** > **Jobs**.

```yaml {title="$CRIBL_HOME/default/cribl/job-limits.yml"}
concurrentJobLimit: # [number] Concurrent job limit - The total number of jobs that may run concurrently
concurrentSystemJobLimit: # [number] Concurrent system job limit - The total number of system jobs that may run concurrently
concurrentScheduledJobLimit: # [number] Concurrent scheduled job limit - The total number of scheduled jobs that may run concurrently. Limit is relative to concurrent job limit.
concurrentTaskLimit: # [number] Concurrent task limit - The total number of tasks that a worker process may run concurrently
concurrentSystemTaskLimit: # [number] Concurrent system task limit - The number of system tasks that a worker process may run concurrently
maxTaskPerc: # [number] Max task usage percentage - Value from 0 to 1 representing the percentage of total tasks within the system a job may consume
taskPollTimeoutMs: # [number] Task poll timeout - The number of milliseconds the worker's task handler will wait to receive a task before retrying the request for a task.
jobArtifactsReaperPeriod: # [string] Artifact reaper period - Time period at which the system attempts to reap stale disk artifacts belonging to the jobs
finishedJobArtifactsLimit: # [number] Finished job artifacts limit - Maximum number of finished job artifacts to keep on disk.
finishedTaskArtifactsLimit: # [number] Finished task artifacts limit - Maximum number of finished task artifacts to keep on disk per job on each worker node.
taskManifestFlushPeriodMs: # [number] Manifest flush period - The rate at which a job's task manifest should be refreshed in milliseconds.
taskManifestMaxBufferSize: # [number] Manifest max buffer size - The maximum number of tasks the task manifest can hold in memory before flushing to disk.
taskManifestReadBufferSize: # [string] Manifest reader buffer size - The number of bytes the task manifest reader should pull from disk.
schedulingPolicy: # [string] Job dispatching - The method by which tasks are assigned to worker processes to complete the work
jobTimeout: # [string] Job timeout - Maximum time a job is allowed to run. Defaults to 0, for unlimited time. Units are seconds if not specified. Sample entries: 30, 45s, 15m.
taskHeartbeatPeriod: # [number] Task heartbeat period - The heartbeat period (in seconds) for tasks to report back to the leader/API.
```
