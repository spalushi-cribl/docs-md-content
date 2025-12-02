# Internal Logs


Distributed deployments emit a larger set of logs than single-instance deployments. We'll describe the distributed set first. 

You can display and [export](/stream/monitoring#export) all internal logs from the UI at **Monitoring** > **Logs** (Stream) or **Manage** > **Logs** (Edge). Logs' persistence depends on event volume, not time – for details, see [Log Rotation and Retention](monitoring#retention).

> Several logs listed on this page are exposed only in customer-managed (on-prem) deployments. In Cribl.Cloud, Leaders support Cribl Stream [Worker Node logs](/stream/internal-logs#node) on [hybrid](cloud-enterprise#hybrid) Workers.
> 
>  However, Organization Members who have the Admin Permission on [Cribl Search](/search/) can use that product to search the `cribl_internal_logs` Dataset for additional details about the Leader and its Cribl-managed Workers.
>
{.box .cloud}

## Leader Node Logs (Distributed) {#leader}

The `API/main` process emits the following logs into the Leader Node's `$CRIBL_HOME/log/` directory.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| `cribl.log`   | Principal log in Cribl Stream.  Includes telemetry/license-validation logs. Corresponds to top-level `cribl.log` on [Diag](/stream/diagnosing) page.  | **Leader** > **API Process** |
| `access.log`   |  API calls, e.g., `GET /api/v1/version/info`.  | **Leader** > **Access** |
| `audit.log`    | Actions pertaining to files, e.g., `create`, `update`, `commit`, `deploy`, `delete`.   | **Leader** > **Audit** |
| `notifications.log` |  Messages that appear in the Notification list in the UI. | **Leader** > **Notifications** |
| `ui-access.log`    |  Interactions with different UI components described as URLs, e.g., `/settings/apidocs`, `/dashboard/logs`. |  **Leader** > **UI Access** |

The `API/main` process emits the following service logs into the Leader Node's `$CRIBL_HOME/log/service/` directory. Each service includes a `cribl.log` file that logs the service’s internal telemetry and an `access.log` file that logs which API calls the service has handled.

| SERVICE NAME | DESCRIPTION | EQUIVALENT ON [Logs](monitoring#log_search) PAGE|
|---|---|---|
| Connections Service | Handles all worker connections and communication, including heartbeats, bundle deploys, teleporting, restarting, etc. Workers are assigned to connection processes using a round-robin algorithm. | **Leader** > **Connections Service** |
| Lease Renewal Service  | Handles lease renewal for the primary Leader Node. | **Leader** > **Lease Renewal Service**|
|Metrics Service |Handles in-memory metrics, merging of incoming packets, metrics persistence and rehydration, and UI queries for metrics. |**Leader** > **Metrics Service** |
|Notifications Service|Triggers Notifications based on its configuration.|**Leader** > **Notifications Service**  |

The Config Helper process for each Worker Group/Fleet emits the following log in `$CRIBL_HOME/log/group/GROUPNAME`.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| `cribl.log`   |  Messages about config maintenance, previews, etc.  | **GROUPNAME** > **Config helper**  |

## Worker Node Logs (Distributed) {#node}

The API Process emits the following log in `$CRIBL_HOME/log/`.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------|-------|
| `cribl.log`   |  Messages about the Worker/Edge Node communicating with the Leader Node (i.e., with its API Process) and other API requests, e.g., sending metrics,  reaping job artifacts.  | **GROUPNAME** > **Worker:HOSTNAME** > **API Process**  |

Each Worker Process emits the following logs in `$CRIBL_HOME/log/worker/N/`, where `N` is the Worker/Edge Node Process ID. (The `metrics.log` file is written only when [HTTP-based Destinations](destinations-backpressure-triggers#http-based-destinations) exist.)

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| `cribl.log`   |  Messages about the Worker/Edge Node processing data.  | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |
| `metrics.log`   |  Messages about the Worker/Edge Node's outbound HTTP request statistics. | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |

For convenience, the UI aggregates the Worker/Edge Node Process logs as follows.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| N/A   |  Aggregation of all the `Worker Process N` logs and the API Process log.  | **GROUPNAME** > **WORKER_NAME**  |

> In Cribl Stream, the logs listed above are currently available only on customer-managed [hybrid](/stream/cloud-enterprise#hybrid) Workers. The single-instance logs listed below are not relevant to Cribl.Cloud.
>
{.box .cloud}

## Single‑Instance Logs {#single}

The `API/main` process emits the same logs as it does for a distributed deployment, in`$CRIBL_HOME/log/`:

* `cribl.log`  
* `access.log`  
* `audit.log`   
* `notifications.log`
* `ui-access.log`    

Each Worker/Edge Node Process emits the following logs in `$CRIBL_HOME/log/worker/N/`, where `N` is the Worker/Edge Node Process ID. (The `metrics.log` file is written only when [HTTP-based Destinations](destinations-backpressure-triggers#http-based-destinations) exist.)

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------|-------|
| `cribl.log`   |  Messages about the Worker/Edge Node processing data.  | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |
| `metrics.log`   |  Messages about the Worker/Edge Node's outbound HTTP request statistics. | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |

## `_raw stats` Event Fields {#stats}

Each Worker/Edge Node Process logs this information at a 1-minute frequency. So each event's scope covers only that Worker/Edge Node Process, over a 1‑minute time span defined by the `startTime` and `endTime` fields.

### Sample Event {#sample}

``` json
{"time":"2022-11-17T16:54:05.349Z","cid":"w0","channel":"server","level":"info","message":"_raw stats","inEvents":307965,"outEvents":495848,"inBytes":52756162,"outBytes":83028013,"starttime":1668703980,"endtime":1668704040,"activeCxn":0,"openCxn":0,"closeCxn":0,"rejectCxn":0,"abortCxn":0,"pqInEvents":62000,"pqOutEvents":114591,"pqInBytes":12163896,"pqOutBytes":22481509,"pqTotalBytes":480467058,"droppedEvents":0,"tasksStarted":6,"tasksCompleted":6,"activeEP":9,"blockedEP":0,"cpuPerc":101.09,"eluPerc":97.81,"mem":{"heap":277,"heapTotal":287,"ext":46,"rss":453,"buffers":0}}
```

### Field Descriptions {#fields}

Field | Description
--- | ---
`abortCxn` | Number of TCP connections that were aborted.
`activeCxn` | Number of TCP connections newly opened at the time the `_raw` stats are logged. (This is a gauge when exported in internal metrics, and can otherwise be ignored as an instantaneous measurement. Only some application protocols count toward this; e.g., any HTTP-based Source does not count.)
`activeEP` | Number of currently active event processors (EPs). EPs are used to process events through Breakers and Pipelines as the events are received from Sources and sent to destinations. EPs are typically created per TCP connection (such as for HTTP).
`blockedEP` | Number of currently blocked event processors (caused by blocking Destinations).
`closeCxn` | Number of TCP connections that were closed.
`cpuPerc` | CPU utilization from the combined user and system activity over the last 60 seconds.
`droppedEvents` | This is equivalent to the `total.dropped_events` [metric](internal-metrics). Drops can occur from Pipeline Functions, from Destination Backpressure, or from other errors. Any event not sent to a Destination is considered dropped.
`eluPerc` | Event loop utilization. Represents the percentage of time over the last 60 seconds that the NodeJS runtime spent processing events within its event loop.
`endTime` | The end of the timespan represented by these metrics. (Will always be 60 seconds after `startTime`.)
`inBytes` | Number of bytes received from all Sources (based only off `_raw`).
`inEvents` | Number of events received from all inputs after Event Breakers are applied. This can be larger than `outEvents` if events are dropped via Drop, Aggregation, Suppression, Sampling, or similar Functions.
`mem.buffers` | Memory allocated for ArrayBuffers and SharedArrayBuffers.
`mem.ext` | External section of process memory, in MB.
`mem.heap` | Used heap section of process memory, in MB.
`mem.heapTotal` | Total heap section of process memory, in MB.
`mem.rss` | Resident set size section of process memory, in MB.
`openCxn` | Same as `activeCxn`, but tracked as a counter rather than a gauge. So `openCxn` will show all connections newly opened each minute, and is more accurate than using `activeCxn`.
`outBytes` | Number of bytes sent to all Destinations (based only off `_raw`).
`outEvents` | Number of events sent out to all Destinations. This can be larger than `inEvents` due to creating event clones or entirely new unique events (such as when using the Aggregation Function).
`pqInBytes` | Number of bytes that were written to Persistent Queues, across all Destinations.
`pqInEvents` | Number of events that were written to Persistent Queues, across all Destinations.
`pqOutBytes` | Number of bytes that were flushed from Persistent Queues, across all Destinations.
`pqOutEvents` | Number of events that were flushed from Persistent Queues, across all Destinations.
`pqTotalBytes` | Amount of data currently stored in Persistent Queues, across all Destinations.
`rejectCxn` | Number of TCP connections that were rejected.
`startTime` | The beginning of the timespan represented by these metrics.
`tasksCompleted` | The number of tasks the process has started and completed for all collection jobs for which it was executing tasks.
`tasksStarted` | The number of tasks the process started for all collection jobs for which it was executing tasks.

## Troubleshooting With the `cribl_stderr` Log {#cribl-stderr}

Rarely, the Node.js server that functions as the backend for Cribl Stream can fail with an OOM (out of memory) or other fatal error. When this happens, Cribl Stream will not be able to write to its usual logs, and instead creates a special `cribl_stderr.log` file.

This file is not viewable via the UI, and its main value is to help Cribl Support troubleshoot OOM and other rare scenarios. Starting in Cribl Stream v.4.6.1, each entry in `cribl_stderr.log` contains a timestamp in UTC.
