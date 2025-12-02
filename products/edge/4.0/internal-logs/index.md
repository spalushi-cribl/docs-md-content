# Internal Logs


Distributed deployments emit a larger set of logs than single-instance deployments. We'll describe the distributed set first. 

You can display and [export](/stream/monitoring#export) all internal logs from the UI at **Monitoring** > **Logs** (Stream) or **Manage** > **Logs** (Edge). Logs' persistence depends on event volume, not time – for details, see [Log Rotation and Retention](monitoring#retention).

> Several logs listed on this page are exposed only in customer-managed (on-prem) deployments. In Cribl.Cloud, Leaders support Cribl Stream [Worker Node logs](/stream/internal-logs#node) on [hybrid](deploy-cloud#enterprise) Workers.
> 
>  However, Organization Members who have the Admin Permission on [Cribl Search](/search/) can use that product to search the `cribl_internal_logs` Dataset for additional details about the Leader and its Cribl-managed Workers.
>
{.box .cloud}

## Leader Node Logs (Distributed)

The `API/main` process emits the following logs into the Leader Node's `$CRIBL_HOME/log/` directory.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| `cribl.log`   | Principal log in Cribl Edge.  Includes telemetry/license-validation logs. Corresponds to top-level `cribl.log` on [Diag](/stream/diagnosing) page.  | **Leader** > **API Process** |
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

## Worker/Edge Node Logs (Distributed)

The API Process emits the following log in `$CRIBL_HOME/log/`.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------|-------|
| `cribl.log`   |  Messages about the Worker/Edge Node communicating with the Leader Node (i.e., with its API Process) and other API requests, e.g., sending metrics,  reaping job artifacts.  | **GROUPNAME** > **Worker:HOSTNAME** > **API Process**  |

Each Worker Process emits the following log in `$CRIBL_HOME/log/worker/N/`, where `N` is the Worker/Edge Node Process ID.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| `cribl.log`   |  Messages about the Worker/Edge Node processing data.  | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |

For convenience, the UI aggregates the Worker/Edge Node Process logs as follows.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------| -------|
| N/A   |  Aggregation of all the `Worker Process N` logs and the API Process log.  | **GROUPNAME** > **WORKER_NAME**  |

> In Cribl Stream, the logs listed above are currently available only on customer-managed [hybrid](deploy-cloud#hybrid) Workers. The single-instance logs listed below are not relevant to Cribl.Cloud.
>
{.box .cloud}

## Single‑Instance Logs

The `API/main` process emits the same logs as it does for a distributed deployment, in`$CRIBL_HOME/log/`:

* `cribl.log`  
* `access.log`  
* `audit.log`   
* `notifications.log`
* `ui-access.log`    

Each Worker/Edge Node Process emits the following log in `$CRIBL_HOME/log/worker/N/`, where `N` is the Worker/Edge Node Process ID.

| Logfile Name |  Description  | Equivalent on [Logs](monitoring#log_search) page |
|-----|--------|-------|
| `cribl.log`   |  Messages about the Worker/Edge Node processing data.  | **GROUPNAME** > **Worker:HOSTNAME** > **Worker Process N**  |

## `_raw stats` Event Fields

Each Worker/Edge Node Process logs this information at a 1-minute frequency. So each event's scope covers only that Worker/Edge Node Process, over a 1‑minute time span defined by the `startTime` and `endTime` fields.

### Sample Event

`{"time":"2021-11‑24T15:12:05.713Z","cid":"w1","channel":"server","level":"info",​"message":"_raw stats","inEvents":31237,"outEvents":44791,"inBytes":7820263,​"outBytes":14701001,"starttime":1637766660,"endtime":1637766720,"activeCxn":0,"openCxn":136,"closeCxn":135,"pqInEvents":0,"pqOutEvents":0,"pqInBytes":0,"pqOutBytes":0,"pqTotalBytes":0,"droppedEvents":1113,"activeEP":41,"blockedEP":5,"cpuPerc":23.34,"mem":{"heap":119,"ext":79,"rss":380}}`


### Field Descriptions

Field | Description
--- | ---
`inEvents` | Number of events received from all inputs after Event Breakers are applied. This can be larger than `outEvents` if events are dropped via Drop, Aggregation, Suppression, Sampling, or similar Functions. 
`outEvents` | Number of events sent out to all Destinations. This can be larger than `inEvents` due to creating event clones or entirely new unique events. (E.g., when using the Aggregation Function.)
`inBytes` | Number of bytes received from all Sources (based only off `_raw`).
`outBytes` | Number of bytes sent to all Destinations (based only off `_raw`).
`startTime` | The beginning of the timespan represented by these metrics.
`endTime` | The end of the timespan represented by these metrics. (Will always be 60 seconds after `startTime`.)
`activeCxn` | Number of TCP connections newly opened at the time the `_raw` stats are logged. (This is a gauge when exported in internal metrics, and can otherwise be ignored as an instantaneous measurement. Only some application protocols count toward this; e.g., any HTTP-based Source does not count.)
`openCxn` | Same as `activeCxn`, but tracked as a counter rather than a gauge. So `openCxn` will show all connections newly opened each minute, and is more accurate than using `activeCxn`. 
`closeCxn` | Number of TCP connections that were closed.
`pqInEvents` | Number of events that were written to Persistent Queues, across all Destinations.
`pqOutEvents` | Number of events that were flushed from Persistent Queues, across all Destinations.
`pqInBytes` | Number of bytes that were written to Persistent Queues, across all Destinations.
`pqOutBytes` | Number of bytes that were flushed from Persistent Queues, across all Destinations.
`pqTotalBytes` | Amount of data currently stored in Persistent Queues, across all Destinations.
`droppedEvents` | This is equivalent to the `total.dropped_events` [metric](internal-metrics). Drops can occur from Pipeline Functions, from Destination Backpressure, or from other errors. Any event not sent to a Destination is considered dropped.
`activeEP` | Number of currently active event processors (EPs). EPs are used to process events through Breakers and Pipelines as the events are received from Sources and sent to destinations. EPs are typically created per TCP connection (such as for HTTP).
`blockedEP` | Number of currently blocked event processors (caused by blocking Destinations).
`cpuPerc` | CPU utilization from the combined user and system activity over the last 60 seconds.
`mem.heap` | Heap section of process memory, in MB.
`mem.ext` | External section of process memory, in MB.
`mem.rss` | Resident set size section of process memory, in MB.