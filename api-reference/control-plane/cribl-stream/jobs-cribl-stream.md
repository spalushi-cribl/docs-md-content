
<h1 id="cribl-stream-api-jobs-cribl-stream-">Cribl Stream API - Jobs (Cribl Stream) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-jobs-cribl-stream--jobs-cribl-stream-">Jobs (Cribl Stream)</h1>

## Run or schedule a job

<a id="opIdcreateJobs"></a>

> Code samples

`POST /jobs`

Run or schedule a job

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "type": "string",
    "conf": {},
    "destructive": false,
    "encoding": "string"
  },
  "input": {
    "type": "collection",
    "breakerRulesets": [
      "string"
    ],
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true,
      "command": "string",
      "args": [
        "string"
      ]
    },
    "throttleRatePerSec": "0",
    "metadata": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "pipeline": "string",
    "output": "string"
  }
}
```

<h3 id="run-or-schedule-a-job-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SavedJob](#schemasavedjob)|true|SavedJob object|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    "string"
  ]
}
```

<h3 id="run-or-schedule-a-job-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="run-or-schedule-a-job-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get info on jobs

<a id="opIdgetJobs"></a>

> Code samples

`GET /jobs`

Get info on jobs

<h3 id="get-info-on-jobs-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|offset|query|integer|false|Pagination offset|
|limit|query|integer|false|Maximum number of items to return|
|runType|query|[JobRunTypes](#schemajobruntypes)|false|Filter by job run type|
|state|query|string|false|Filter by current job state, e.g. "running"|
|id|query|string|false|Filter by job id, e.g. "id=1608713335.3&id=1608713326.1"|
|collectorId|query|string|false|Filter by collector id, e.g. "collectorId=Prometheus-in"|
|groupId|query|string|false|Filter by worker group id, e.g. "defaultHybrid"|

#### Enumerated Values

|Parameter|Value|
|---|---|
|runType|adhoc|
|runType|scheduled|
|runType|system|

> Example responses

> 200 Response

```json
{
  "offset": 0,
  "limit": 0,
  "items": [
    {
      "args": {
        "id": "string",
        "description": "string",
        "type": "collection",
        "ttl": "4h",
        "ignoreGroupJobsLimit": false,
        "removeFields": [],
        "resumeOnBoot": false,
        "environment": "string",
        "schedule": {
          "enabled": true,
          "skippable": true,
          "resumeMissed": false,
          "cronSchedule": "*/5 * * * *",
          "maxConcurrentRuns": 1,
          "run": {
            "type": "collection",
            "rescheduleDroppedTasks": true,
            "maxTaskReschedule": 1,
            "logLevel": "error",
            "jobTimeout": "0",
            "mode": "list",
            "timeRangeType": "relative",
            "earliest": 0,
            "latest": 0,
            "timestampTimezone": null,
            "timeWarning": {},
            "expression": "true",
            "minTaskSize": "1MB",
            "maxTaskSize": "10MB"
          }
        },
        "streamtags": [],
        "workerAffinity": false,
        "collector": {
          "type": "string",
          "conf": {},
          "destructive": false,
          "encoding": "string"
        },
        "input": {
          "type": "collection",
          "breakerRulesets": [
            "string"
          ],
          "staleChannelFlushMs": 10000,
          "sendToRoutes": true,
          "preprocess": {
            "disabled": true,
            "command": "string",
            "args": [
              "string"
            ]
          },
          "throttleRatePerSec": "0",
          "metadata": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "pipeline": "string",
          "output": "string"
        },
        "run": {
          "rescheduleDroppedTasks": true,
          "maxTaskReschedule": 1,
          "logLevel": "error",
          "jobTimeout": "0",
          "mode": "list",
          "timeRangeType": "absolute",
          "earliest": 0,
          "latest": 0,
          "timestampTimezone": "UTC",
          "timeWarning": {},
          "expression": "true",
          "minTaskSize": "1MB",
          "maxTaskSize": "10MB",
          "discoverToRoutes": false,
          "capture": {
            "duration": 60,
            "maxEvents": 100,
            "level": 0
          }
        }
      },
      "id": "string",
      "keep": true,
      "stats": {
        "property1": 0,
        "property2": 0
      },
      "status": {
        "reason": {},
        "state": 0
      }
    }
  ]
}
```

<h3 id="get-info-on-jobs-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of JobInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-info-on-jobs-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» offset|integer|false|none|pagination offset|
|» limit|integer|false|none|number of items present in the items array|
|» items|[[JobInfo](#schemajobinfo)]|false|none|the pre-limited items in the list of results|
|»» args|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobCollection](#schemarunnablejobcollection)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|false|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»»» collector|object|true|none|none|
|»»»»» type|string|true|none|The type of collector to run|
|»»»»» conf|object|true|none|none|
|»»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»»» input|object|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»»» preprocess|object|false|none|none|
|»»»»»» disabled|boolean|true|none|none|
|»»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»»» pipeline|string|false|none|Pipeline to process results|
|»»»»» output|string|false|none|Destination to send results to|
|»»»» run|object|true|none|none|
|»»»»» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|»»»»» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|»»»»» logLevel|string|false|none|Level at which to set task logging|
|»»»»» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|
|»»»»» mode|string|true|none|Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.|
|»»»»» timeRangeType|string|false|none|none|
|»»»»» earliest|number|false|none|Earliest time to collect data for the selected timezone|
|»»»»» latest|number|false|none|Latest time to collect data for the selected timezone|
|»»»»» timestampTimezone|string|false|none|Timezone to use for Earliest and Latest times|
|»»»»» timeWarning|object|false|none|none|
|»»»»» expression|string|false|none|A filter for tokens in the provided collect path and/or the events being collected|
|»»»»» minTaskSize|string|false|none|Limits the bundle size for small tasks. For example,<br>        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.|
|»»»»» maxTaskSize|string|false|none|Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,<br>        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.|
|»»»»» discoverToRoutes|boolean|false|none|Send discover results to Routes|
|»»»»» capture|object|false|none|none|
|»»»»»» duration|number|false|none|Amount of time to keep capture open, in seconds|
|»»»»»» maxEvents|number|false|none|Maximum number of events to capture|
|»»»»»» level|integer|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobExecutor](#schemarunnablejobexecutor)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|false|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» executor|object|true|none|none|
|»»»»» type|string|true|none|The type of executor to run|
|»»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»»» conf|object|false|none|none|
|»»»» run|object|true|none|none|
|»»»»» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|»»»»» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|»»»»» logLevel|string|false|none|Level at which to set task logging|
|»»»»» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobScheduledSearch](#schemarunnablejobscheduledsearch)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|true|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» savedQueryId|string|true|none|Identifies which search query to run|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» keep|boolean|false|none|none|
|»» stats|object|true|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» **additionalProperties**|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» status|[JobStatus](#schemajobstatus)|true|none|none|
|»»» reason|object|false|none|none|
|»»» state|integer|true|none|State of the Job|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|
|mode|list|
|mode|preview|
|mode|run|
|timeRangeType|absolute|
|timeRangeType|relative|
|level|0|
|level|1|
|level|2|
|level|3|
|type|collection|
|type|executor|
|type|scheduledSearch|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|
|type|collection|
|type|executor|
|type|scheduledSearch|
|state|0|
|state|1|
|state|2|
|state|3|
|state|4|
|state|5|
|state|6|
|state|7|
|state|8|
|state|9|

<aside class="success">
This operation does not require authentication
</aside>

## Remove job from job inspector by instance id

<a id="opIddeleteJobsById"></a>

> Code samples

`DELETE /jobs/{id}`

Remove job from job inspector by instance id

<h3 id="remove-job-from-job-inspector-by-instance-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job instance id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="remove-job-from-job-inspector-by-instance-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="remove-job-from-job-inspector-by-instance-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Cancel a job by instance id

<a id="opIdupdateJobsCancelById"></a>

> Code samples

`PATCH /jobs/{id}/cancel`

Cancel a job by instance id

<h3 id="cancel-a-job-by-instance-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job instance id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "state": "string"
    }
  ]
}
```

<h3 id="cancel-a-job-by-instance-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of JobState objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="cancel-a-job-by-instance-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[JobState](#schemajobstate)]|false|none|none|
|»» id|string|true|none|none|
|»» state|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Task errors for a job by id

<a id="opIdgetJobsErrorsByIdAndGroup"></a>

> Code samples

`GET /jobs/{id}/errors/{group}`

Get Task errors for a job by id

<h3 id="get-task-errors-for-a-job-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Instance id of the job to get results for|
|group|path|string|true|Group the job belongs to|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="get-task-errors-for-a-job-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-task-errors-for-a-job-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## prevent job from being deleted automatically

<a id="opIdupdateJobsKeepById"></a>

> Code samples

`PATCH /jobs/{id}/keep`

prevent job from being deleted automatically

<h3 id="prevent-job-from-being-deleted-automatically-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job Instance id|
|groupId|query|string|false|Group ID|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "args": {
        "id": "string",
        "description": "string",
        "type": "collection",
        "ttl": "4h",
        "ignoreGroupJobsLimit": false,
        "removeFields": [],
        "resumeOnBoot": false,
        "environment": "string",
        "schedule": {
          "enabled": true,
          "skippable": true,
          "resumeMissed": false,
          "cronSchedule": "*/5 * * * *",
          "maxConcurrentRuns": 1,
          "run": {
            "type": "collection",
            "rescheduleDroppedTasks": true,
            "maxTaskReschedule": 1,
            "logLevel": "error",
            "jobTimeout": "0",
            "mode": "list",
            "timeRangeType": "relative",
            "earliest": 0,
            "latest": 0,
            "timestampTimezone": null,
            "timeWarning": {},
            "expression": "true",
            "minTaskSize": "1MB",
            "maxTaskSize": "10MB"
          }
        },
        "streamtags": [],
        "workerAffinity": false,
        "collector": {
          "type": "string",
          "conf": {},
          "destructive": false,
          "encoding": "string"
        },
        "input": {
          "type": "collection",
          "breakerRulesets": [
            "string"
          ],
          "staleChannelFlushMs": 10000,
          "sendToRoutes": true,
          "preprocess": {
            "disabled": true,
            "command": "string",
            "args": [
              "string"
            ]
          },
          "throttleRatePerSec": "0",
          "metadata": [
            {
              "name": "string",
              "value": "string"
            }
          ],
          "pipeline": "string",
          "output": "string"
        },
        "run": {
          "rescheduleDroppedTasks": true,
          "maxTaskReschedule": 1,
          "logLevel": "error",
          "jobTimeout": "0",
          "mode": "list",
          "timeRangeType": "absolute",
          "earliest": 0,
          "latest": 0,
          "timestampTimezone": "UTC",
          "timeWarning": {},
          "expression": "true",
          "minTaskSize": "1MB",
          "maxTaskSize": "10MB",
          "discoverToRoutes": false,
          "capture": {
            "duration": 60,
            "maxEvents": 100,
            "level": 0
          }
        }
      },
      "id": "string",
      "keep": true,
      "stats": {
        "property1": 0,
        "property2": 0
      },
      "status": {
        "reason": {},
        "state": 0
      }
    }
  ]
}
```

<h3 id="prevent-job-from-being-deleted-automatically-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of JobInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="prevent-job-from-being-deleted-automatically-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[JobInfo](#schemajobinfo)]|false|none|none|
|»» args|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobCollection](#schemarunnablejobcollection)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|false|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»»» collector|object|true|none|none|
|»»»»» type|string|true|none|The type of collector to run|
|»»»»» conf|object|true|none|none|
|»»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»»» input|object|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»»» preprocess|object|false|none|none|
|»»»»»» disabled|boolean|true|none|none|
|»»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»»» name|string|true|none|none|
|»»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»»» pipeline|string|false|none|Pipeline to process results|
|»»»»» output|string|false|none|Destination to send results to|
|»»»» run|object|true|none|none|
|»»»»» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|»»»»» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|»»»»» logLevel|string|false|none|Level at which to set task logging|
|»»»»» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|
|»»»»» mode|string|true|none|Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.|
|»»»»» timeRangeType|string|false|none|none|
|»»»»» earliest|number|false|none|Earliest time to collect data for the selected timezone|
|»»»»» latest|number|false|none|Latest time to collect data for the selected timezone|
|»»»»» timestampTimezone|string|false|none|Timezone to use for Earliest and Latest times|
|»»»»» timeWarning|object|false|none|none|
|»»»»» expression|string|false|none|A filter for tokens in the provided collect path and/or the events being collected|
|»»»»» minTaskSize|string|false|none|Limits the bundle size for small tasks. For example,<br>        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.|
|»»»»» maxTaskSize|string|false|none|Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,<br>        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.|
|»»»»» discoverToRoutes|boolean|false|none|Send discover results to Routes|
|»»»»» capture|object|false|none|none|
|»»»»»» duration|number|false|none|Amount of time to keep capture open, in seconds|
|»»»»»» maxEvents|number|false|none|Maximum number of events to capture|
|»»»»»» level|integer|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobExecutor](#schemarunnablejobexecutor)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|false|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» executor|object|true|none|none|
|»»»»» type|string|true|none|The type of executor to run|
|»»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»»» conf|object|false|none|none|
|»»»» run|object|true|none|none|
|»»»»» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|»»»»» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|»»»»» logLevel|string|false|none|Level at which to set task logging|
|»»»»» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|[RunnableJobScheduledSearch](#schemarunnablejobscheduledsearch)|false|none|none|
|»»»» id|string|false|none|Unique ID for this Job|
|»»»» description|string|false|none|none|
|»»»» type|string|true|none|none|
|»»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»»» run|object|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» savedQueryId|string|true|none|Identifies which search query to run|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» id|string|true|none|none|
|»» keep|boolean|false|none|none|
|»» stats|object|true|none|none|
|»»» **additionalProperties**|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|object|false|none|none|
|»»»»» **additionalProperties**|number|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» status|[JobStatus](#schemajobstatus)|true|none|none|
|»»» reason|object|false|none|none|
|»»» state|integer|true|none|State of the Job|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|
|mode|list|
|mode|preview|
|mode|run|
|timeRangeType|absolute|
|timeRangeType|relative|
|level|0|
|level|1|
|level|2|
|level|3|
|type|collection|
|type|executor|
|type|scheduledSearch|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|
|type|collection|
|type|executor|
|type|scheduledSearch|
|state|0|
|state|1|
|state|2|
|state|3|
|state|4|
|state|5|
|state|6|
|state|7|
|state|8|
|state|9|

<aside class="success">
This operation does not require authentication
</aside>

## Pause a job by instance id

<a id="opIdupdateJobsPauseById"></a>

> Code samples

`PATCH /jobs/{id}/pause`

Pause a job by instance id

<h3 id="pause-a-job-by-instance-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job instance id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "state": "string"
    }
  ]
}
```

<h3 id="pause-a-job-by-instance-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of JobState objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="pause-a-job-by-instance-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[JobState](#schemajobstate)]|false|none|none|
|»» id|string|true|none|none|
|»» state|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Resume a job by instance id

<a id="opIdupdateJobsResumeById"></a>

> Code samples

`PATCH /jobs/{id}/resume`

Resume a job by instance id

<h3 id="resume-a-job-by-instance-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job instance id|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "state": "string"
    }
  ]
}
```

<h3 id="resume-a-job-by-instance-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of JobState objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="resume-a-job-by-instance-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[JobState](#schemajobstate)]|false|none|none|
|»» id|string|true|none|none|
|»» state|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get results for a discover job by instance id

<a id="opIdgetJobsResultsByIdAndGroup"></a>

> Code samples

`GET /jobs/{id}/results/{group}`

Get results for a discover job by instance id

<h3 id="get-results-for-a-discover-job-by-instance-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Instance id of the job to get results for|
|maxFiles|query|number|false|Maximum files to get job results|
|group|path|string|true|Group the job belongs to|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="get-results-for-a-discover-job-by-instance-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-results-for-a-discover-job-by-instance-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Error">Error</h2>
<!-- backwards compatibility -->
<a id="schemaerror"></a>
<a id="schema_Error"></a>
<a id="tocSerror"></a>
<a id="tocserror"></a>

```json
{
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|message|string|false|none|Error message|

<h2 id="tocS_SavedJob">SavedJob</h2>
<!-- backwards compatibility -->
<a id="schemasavedjob"></a>
<a id="schema_SavedJob"></a>
<a id="tocSsavedjob"></a>
<a id="tocssavedjob"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "type": "string",
    "conf": {},
    "destructive": false,
    "encoding": "string"
  },
  "input": {
    "type": "collection",
    "breakerRulesets": [
      "string"
    ],
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true,
      "command": "string",
      "args": [
        "string"
      ]
    },
    "throttleRatePerSec": "0",
    "metadata": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "pipeline": "string",
    "output": "string"
  }
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|

<h2 id="tocS_JobInfo">JobInfo</h2>
<!-- backwards compatibility -->
<a id="schemajobinfo"></a>
<a id="schema_JobInfo"></a>
<a id="tocSjobinfo"></a>
<a id="tocsjobinfo"></a>

```json
{
  "args": {
    "id": "string",
    "description": "string",
    "type": "collection",
    "ttl": "4h",
    "ignoreGroupJobsLimit": false,
    "removeFields": [],
    "resumeOnBoot": false,
    "environment": "string",
    "schedule": {
      "enabled": true,
      "skippable": true,
      "resumeMissed": false,
      "cronSchedule": "*/5 * * * *",
      "maxConcurrentRuns": 1,
      "run": {
        "type": "collection",
        "rescheduleDroppedTasks": true,
        "maxTaskReschedule": 1,
        "logLevel": "error",
        "jobTimeout": "0",
        "mode": "list",
        "timeRangeType": "relative",
        "earliest": 0,
        "latest": 0,
        "timestampTimezone": null,
        "timeWarning": {},
        "expression": "true",
        "minTaskSize": "1MB",
        "maxTaskSize": "10MB"
      }
    },
    "streamtags": [],
    "workerAffinity": false,
    "collector": {
      "type": "string",
      "conf": {},
      "destructive": false,
      "encoding": "string"
    },
    "input": {
      "type": "collection",
      "breakerRulesets": [
        "string"
      ],
      "staleChannelFlushMs": 10000,
      "sendToRoutes": true,
      "preprocess": {
        "disabled": true,
        "command": "string",
        "args": [
          "string"
        ]
      },
      "throttleRatePerSec": "0",
      "metadata": [
        {
          "name": "string",
          "value": "string"
        }
      ],
      "pipeline": "string",
      "output": "string"
    },
    "run": {
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "absolute",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": "UTC",
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB",
      "discoverToRoutes": false,
      "capture": {
        "duration": 60,
        "maxEvents": 100,
        "level": 0
      }
    }
  },
  "id": "string",
  "keep": true,
  "stats": {
    "property1": 0,
    "property2": 0
  },
  "status": {
    "reason": {},
    "state": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|args|[RunnableJob](#schemarunnablejob)|true|none|none|
|id|string|true|none|none|
|keep|boolean|false|none|none|
|stats|object|true|none|none|
|» **additionalProperties**|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» **additionalProperties**|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|status|[JobStatus](#schemajobstatus)|true|none|none|

<h2 id="tocS_JobRunTypes">JobRunTypes</h2>
<!-- backwards compatibility -->
<a id="schemajobruntypes"></a>
<a id="schema_JobRunTypes"></a>
<a id="tocSjobruntypes"></a>
<a id="tocsjobruntypes"></a>

```json
"adhoc"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|adhoc|
|*anonymous*|scheduled|
|*anonymous*|system|

<h2 id="tocS_JobState">JobState</h2>
<!-- backwards compatibility -->
<a id="schemajobstate"></a>
<a id="schema_JobState"></a>
<a id="tocSjobstate"></a>
<a id="tocsjobstate"></a>

```json
{
  "id": "string",
  "state": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|state|string|true|none|none|

<h2 id="tocS_SavedJobCollection">SavedJobCollection</h2>
<!-- backwards compatibility -->
<a id="schemasavedjobcollection"></a>
<a id="schema_SavedJobCollection"></a>
<a id="tocSsavedjobcollection"></a>
<a id="tocssavedjobcollection"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "type": "string",
    "conf": {},
    "destructive": false,
    "encoding": "string"
  },
  "input": {
    "type": "collection",
    "breakerRulesets": [
      "string"
    ],
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true,
      "command": "string",
      "args": [
        "string"
      ]
    },
    "throttleRatePerSec": "0",
    "metadata": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "pipeline": "string",
    "output": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|true|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|collector|object|true|none|none|
|» type|string|true|none|The type of collector to run|
|» conf|object|true|none|none|
|» destructive|boolean|false|none|Delete any files collected (where applicable)|
|» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|input|object|false|none|none|
|» type|string|false|none|none|
|» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|» preprocess|object|false|none|none|
|»» disabled|boolean|true|none|none|
|»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»» args|[string]|false|none|Arguments to be added to the custom command|
|» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|» metadata|[object]|false|none|Fields to add to events from this input|
|»» name|string|true|none|none|
|»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|» pipeline|string|false|none|Pipeline to process results|
|» output|string|false|none|Destination to send results to|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|

<h2 id="tocS_SavedJobExecutor">SavedJobExecutor</h2>
<!-- backwards compatibility -->
<a id="schemasavedjobexecutor"></a>
<a id="schema_SavedJobExecutor"></a>
<a id="tocSsavedjobexecutor"></a>
<a id="tocssavedjobexecutor"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "executor": {
    "type": "string",
    "storeTaskResults": true,
    "conf": {}
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|true|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|executor|object|true|none|none|
|» type|string|true|none|The type of executor to run|
|» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|» conf|object|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|

<h2 id="tocS_SavedJobScheduledSearch">SavedJobScheduledSearch</h2>
<!-- backwards compatibility -->
<a id="schemasavedjobscheduledsearch"></a>
<a id="schema_SavedJobScheduledSearch"></a>
<a id="tocSsavedjobscheduledsearch"></a>
<a id="tocssavedjobscheduledsearch"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "savedQueryId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|true|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|

<h2 id="tocS_RunnableJob">RunnableJob</h2>
<!-- backwards compatibility -->
<a id="schemarunnablejob"></a>
<a id="schema_RunnableJob"></a>
<a id="tocSrunnablejob"></a>
<a id="tocsrunnablejob"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "type": "string",
    "conf": {},
    "destructive": false,
    "encoding": "string"
  },
  "input": {
    "type": "collection",
    "breakerRulesets": [
      "string"
    ],
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true,
      "command": "string",
      "args": [
        "string"
      ]
    },
    "throttleRatePerSec": "0",
    "metadata": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "pipeline": "string",
    "output": "string"
  },
  "run": {
    "rescheduleDroppedTasks": true,
    "maxTaskReschedule": 1,
    "logLevel": "error",
    "jobTimeout": "0",
    "mode": "list",
    "timeRangeType": "absolute",
    "earliest": 0,
    "latest": 0,
    "timestampTimezone": "UTC",
    "timeWarning": {},
    "expression": "true",
    "minTaskSize": "1MB",
    "maxTaskSize": "10MB",
    "discoverToRoutes": false,
    "capture": {
      "duration": 60,
      "maxEvents": 100,
      "level": 0
    }
  }
}

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[RunnableJobCollection](#schemarunnablejobcollection)|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[RunnableJobExecutor](#schemarunnablejobexecutor)|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[RunnableJobScheduledSearch](#schemarunnablejobscheduledsearch)|false|none|none|

<h2 id="tocS_JobStatus">JobStatus</h2>
<!-- backwards compatibility -->
<a id="schemajobstatus"></a>
<a id="schema_JobStatus"></a>
<a id="tocSjobstatus"></a>
<a id="tocsjobstatus"></a>

```json
{
  "reason": {},
  "state": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|reason|object|false|none|none|
|state|integer|true|none|State of the Job|

#### Enumerated Values

|Property|Value|
|---|---|
|state|0|
|state|1|
|state|2|
|state|3|
|state|4|
|state|5|
|state|6|
|state|7|
|state|8|
|state|9|

<h2 id="tocS_RunnableJobCollection">RunnableJobCollection</h2>
<!-- backwards compatibility -->
<a id="schemarunnablejobcollection"></a>
<a id="schema_RunnableJobCollection"></a>
<a id="tocSrunnablejobcollection"></a>
<a id="tocsrunnablejobcollection"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "type": "string",
    "conf": {},
    "destructive": false,
    "encoding": "string"
  },
  "input": {
    "type": "collection",
    "breakerRulesets": [
      "string"
    ],
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true,
      "command": "string",
      "args": [
        "string"
      ]
    },
    "throttleRatePerSec": "0",
    "metadata": [
      {
        "name": "string",
        "value": "string"
      }
    ],
    "pipeline": "string",
    "output": "string"
  },
  "run": {
    "rescheduleDroppedTasks": true,
    "maxTaskReschedule": 1,
    "logLevel": "error",
    "jobTimeout": "0",
    "mode": "list",
    "timeRangeType": "absolute",
    "earliest": 0,
    "latest": 0,
    "timestampTimezone": "UTC",
    "timeWarning": {},
    "expression": "true",
    "minTaskSize": "1MB",
    "maxTaskSize": "10MB",
    "discoverToRoutes": false,
    "capture": {
      "duration": 60,
      "maxEvents": 100,
      "level": 0
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|false|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|collector|object|true|none|none|
|» type|string|true|none|The type of collector to run|
|» conf|object|true|none|none|
|» destructive|boolean|false|none|Delete any files collected (where applicable)|
|» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|input|object|false|none|none|
|» type|string|false|none|none|
|» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|» preprocess|object|false|none|none|
|»» disabled|boolean|true|none|none|
|»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»» args|[string]|false|none|Arguments to be added to the custom command|
|» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|» metadata|[object]|false|none|Fields to add to events from this input|
|»» name|string|true|none|none|
|»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|» pipeline|string|false|none|Pipeline to process results|
|» output|string|false|none|Destination to send results to|
|run|object|true|none|none|
|» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|» logLevel|string|false|none|Level at which to set task logging|
|» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|
|» mode|string|true|none|Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.|
|» timeRangeType|string|false|none|none|
|» earliest|number|false|none|Earliest time to collect data for the selected timezone|
|» latest|number|false|none|Latest time to collect data for the selected timezone|
|» timestampTimezone|string|false|none|Timezone to use for Earliest and Latest times|
|» timeWarning|object|false|none|none|
|» expression|string|false|none|A filter for tokens in the provided collect path and/or the events being collected|
|» minTaskSize|string|false|none|Limits the bundle size for small tasks. For example,<br>        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.|
|» maxTaskSize|string|false|none|Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,<br>        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.|
|» discoverToRoutes|boolean|false|none|Send discover results to Routes|
|» capture|object|false|none|none|
|»» duration|number|false|none|Amount of time to keep capture open, in seconds|
|»» maxEvents|number|false|none|Maximum number of events to capture|
|»» level|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|
|mode|list|
|mode|preview|
|mode|run|
|timeRangeType|absolute|
|timeRangeType|relative|
|level|0|
|level|1|
|level|2|
|level|3|

<h2 id="tocS_RunnableJobExecutor">RunnableJobExecutor</h2>
<!-- backwards compatibility -->
<a id="schemarunnablejobexecutor"></a>
<a id="schema_RunnableJobExecutor"></a>
<a id="tocSrunnablejobexecutor"></a>
<a id="tocsrunnablejobexecutor"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "executor": {
    "type": "string",
    "storeTaskResults": true,
    "conf": {}
  },
  "run": {
    "rescheduleDroppedTasks": true,
    "maxTaskReschedule": 1,
    "logLevel": "error",
    "jobTimeout": "0"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|false|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|executor|object|true|none|none|
|» type|string|true|none|The type of executor to run|
|» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|» conf|object|false|none|none|
|run|object|true|none|none|
|» rescheduleDroppedTasks|boolean|false|none|Reschedule tasks that failed with non-fatal errors|
|» maxTaskReschedule|number|false|none|Maximum number of times a task can be rescheduled|
|» logLevel|string|false|none|Level at which to set task logging|
|» jobTimeout|string|false|none|Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|logLevel|error|
|logLevel|warn|
|logLevel|info|
|logLevel|debug|
|logLevel|silly|

<h2 id="tocS_RunnableJobScheduledSearch">RunnableJobScheduledSearch</h2>
<!-- backwards compatibility -->
<a id="schemarunnablejobscheduledsearch"></a>
<a id="schema_RunnableJobScheduledSearch"></a>
<a id="tocSrunnablejobscheduledsearch"></a>
<a id="tocsrunnablejobscheduledsearch"></a>

```json
{
  "id": "string",
  "description": "string",
  "type": "collection",
  "ttl": "4h",
  "ignoreGroupJobsLimit": false,
  "removeFields": [],
  "resumeOnBoot": false,
  "environment": "string",
  "schedule": {
    "enabled": true,
    "skippable": true,
    "resumeMissed": false,
    "cronSchedule": "*/5 * * * *",
    "maxConcurrentRuns": 1,
    "run": {
      "type": "collection",
      "rescheduleDroppedTasks": true,
      "maxTaskReschedule": 1,
      "logLevel": "error",
      "jobTimeout": "0",
      "mode": "list",
      "timeRangeType": "relative",
      "earliest": 0,
      "latest": 0,
      "timestampTimezone": null,
      "timeWarning": {},
      "expression": "true",
      "minTaskSize": "1MB",
      "maxTaskSize": "10MB"
    }
  },
  "streamtags": [],
  "savedQueryId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|Unique ID for this Job|
|description|string|false|none|none|
|type|string|true|none|none|
|ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|» Items|string|false|none|List of fields to remove from Discover results|
|resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|schedule|object|false|none|Configuration for a scheduled job|
|» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|» cronSchedule|string|false|none|A cron schedule on which to run this job|
|» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|» run|object|false|none|none|
|streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|

