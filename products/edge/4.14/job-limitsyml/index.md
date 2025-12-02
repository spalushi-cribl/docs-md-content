# job-limits.yml


`job-limits.yml` maintains parameters for Collection jobs and system tasks. In the UI, you can configure these per Fleet at **Fleet Settings** > **General Settings** > **Limits** > **Jobs**.

Where we do not specify or validate a minimum or maximum value, Cribl provides a prudent default. Adjust this initial value to match your architecture and needs.

```yaml {title="job-limits.yml"}
# Disable jobs/tasks - When jobs/tasks are disabled, Worker and Edge Nodes on version 4.5.0 and newer won't
# poll the Leader for tasks. Nodes running 4.4.4 and older will honor the Jobs settings even if jobs/tasks
# are disabled.
# [boolean; default: false]
disableTasks:
# Concurrent job limit - The total number of jobs that may run concurrently
# [number; min: 1; default: 10]
concurrentJobLimit:
# Concurrent system job limit - The total number of system jobs that may run concurrently
# [number; min: 1; default: 10]
concurrentSystemJobLimit:
# Concurrent scheduled job limit - The total number of scheduled jobs that can run concurrently. Limit is
# relative to concurrent job limit.
# [number; default: -2]
concurrentScheduledJobLimit:
# Concurrent task limit - The total number of tasks that a Worker Process can run concurrently
# [number; min: 0; default: 2]
concurrentTaskLimit:
# Concurrent system task limit - The number of system tasks that a Worker Process can run concurrently
# [number; min: 0; default: 1]
concurrentSystemTaskLimit:
# Task usage percentage limit - Value from 0 to 1 representing the percentage of total tasks within the
# system a job may consume
# [number; min: 0; max: 1; default: 0.5]
maxTaskPerc:
# Task poll timeout - The number of milliseconds the Worker's task handler will wait to receive a task
# before retrying the request for a task
# [number; min: 10000; default: 60000]
taskPollTimeoutMs:
# Artifact reaper period - Time period at which the system attempts to reap stale disk artifacts belonging
# to the jobs
# [string; default: 30m]
jobArtifactsReaperPeriod:
# Finished job artifacts limit - Maximum number of finished job artifacts to keep on disk
# [number; min: 0; default: 100]
finishedJobArtifactsLimit:
# Finished task artifacts limit - Maximum number of finished task artifacts to keep on disk per job on each
# Worker Node
# [number; min: 0; default: 500]
finishedTaskArtifactsLimit:
# Manifest flush period - The rate at which a job's task manifest should be refreshed in milliseconds
# [number; min: 100; max: 10000; default: 100]
taskManifestFlushPeriodMs:
# Manifest buffer size limit - The maximum number of tasks the task manifest can hold in memory before
# flushing to disk
# [number; min: 100; max: 10000; default: 1000]
taskManifestMaxBufferSize:
# Manifest reader buffer size - The number of bytes the task manifest reader should pull from disk
# [string; default: 4kb]
taskManifestReadBufferSize:
# Job dispatching - The method by which tasks are assigned to worker processes to complete the work
# One of: LeastInFlight | RoundRobin
# [string; default: LeastInFlight]
schedulingPolicy:
# Job timeout - Maximum time a job is allowed to run. Defaults to 0, for unlimited time. Units are seconds
# if not specified. Sample entries: 30, 45s, 15m.
# [string; default: 0]
jobTimeout:
# Task heartbeat period - The heartbeat period (in seconds) for tasks to report back to the Leader/API
# [number; min: 60; default: 60]
taskHeartbeatPeriod:
```
