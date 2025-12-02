
<h1 id="cribl-core-api-saved-jobs">Cribl Core API - Saved Jobs v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-saved-jobs-saved-jobs">Saved Jobs</h1>

## Get a list of SavedJob objects

<a id="opIdlistSavedJob"></a>

> Code samples

`GET /lib/jobs`

Get a list of SavedJob objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
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
  ]
}
```

<h3 id="get-a-list-of-savedjob-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-savedjob-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»» collector|object|true|none|none|
|»»»» type|string|true|none|The type of collector to run|
|»»»» conf|object|true|none|none|
|»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»» input|object|false|none|none|
|»»»» type|string|false|none|none|
|»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»» preprocess|object|false|none|none|
|»»»»» disabled|boolean|true|none|none|
|»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»» name|string|true|none|none|
|»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»» pipeline|string|false|none|Pipeline to process results|
|»»»» output|string|false|none|Destination to send results to|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» executor|object|true|none|none|
|»»»» type|string|true|none|The type of executor to run|
|»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»» conf|object|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|executor|
|type|scheduledSearch|

<aside class="success">
This operation does not require authentication
</aside>

## Create SavedJob

<a id="opIdcreateSavedJob"></a>

> Code samples

`POST /lib/jobs`

Create SavedJob

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

<h3 id="create-savedjob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SavedJob](#schemasavedjob)|true|New SavedJob object|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
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
  ]
}
```

<h3 id="create-savedjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-savedjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»» collector|object|true|none|none|
|»»»» type|string|true|none|The type of collector to run|
|»»»» conf|object|true|none|none|
|»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»» input|object|false|none|none|
|»»»» type|string|false|none|none|
|»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»» preprocess|object|false|none|none|
|»»»»» disabled|boolean|true|none|none|
|»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»» name|string|true|none|none|
|»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»» pipeline|string|false|none|Pipeline to process results|
|»»»» output|string|false|none|Destination to send results to|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» executor|object|true|none|none|
|»»»» type|string|true|none|The type of executor to run|
|»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»» conf|object|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|executor|
|type|scheduledSearch|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SavedJob

<a id="opIddeleteSavedJobById"></a>

> Code samples

`DELETE /lib/jobs/{id}`

Delete SavedJob

<h3 id="delete-savedjob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
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
  ]
}
```

<h3 id="delete-savedjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-savedjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»» collector|object|true|none|none|
|»»»» type|string|true|none|The type of collector to run|
|»»»» conf|object|true|none|none|
|»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»» input|object|false|none|none|
|»»»» type|string|false|none|none|
|»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»» preprocess|object|false|none|none|
|»»»»» disabled|boolean|true|none|none|
|»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»» name|string|true|none|none|
|»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»» pipeline|string|false|none|Pipeline to process results|
|»»»» output|string|false|none|Destination to send results to|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» executor|object|true|none|none|
|»»»» type|string|true|none|The type of executor to run|
|»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»» conf|object|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|executor|
|type|scheduledSearch|

<aside class="success">
This operation does not require authentication
</aside>

## Get SavedJob by ID

<a id="opIdgetSavedJobById"></a>

> Code samples

`GET /lib/jobs/{id}`

Get SavedJob by ID

<h3 id="get-savedjob-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
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
  ]
}
```

<h3 id="get-savedjob-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-savedjob-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»» collector|object|true|none|none|
|»»»» type|string|true|none|The type of collector to run|
|»»»» conf|object|true|none|none|
|»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»» input|object|false|none|none|
|»»»» type|string|false|none|none|
|»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»» preprocess|object|false|none|none|
|»»»»» disabled|boolean|true|none|none|
|»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»» name|string|true|none|none|
|»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»» pipeline|string|false|none|Pipeline to process results|
|»»»» output|string|false|none|Destination to send results to|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» executor|object|true|none|none|
|»»»» type|string|true|none|The type of executor to run|
|»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»» conf|object|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|executor|
|type|scheduledSearch|

<aside class="success">
This operation does not require authentication
</aside>

## Update SavedJob

<a id="opIdupdateSavedJobById"></a>

> Code samples

`PATCH /lib/jobs/{id}`

Update SavedJob

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

<h3 id="update-savedjob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SavedJob](#schemasavedjob)|true|SavedJob object to be updated|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
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
  ]
}
```

<h3 id="update-savedjob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SavedJob objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-savedjob-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[oneOf]|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobCollection](#schemasavedjobcollection)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» workerAffinity|boolean|false|none|If enabled, tasks are created and run by the same Worker Node|
|»»» collector|object|true|none|none|
|»»»» type|string|true|none|The type of collector to run|
|»»»» conf|object|true|none|none|
|»»»» destructive|boolean|false|none|Delete any files collected (where applicable)|
|»»»» encoding|string|false|none|Character encoding to use when parsing ingested data. When not set, @{product} will default to UTF-8 but may incorrectly interpret multi-byte characters.|
|»»» input|object|false|none|none|
|»»»» type|string|false|none|none|
|»»»» breakerRulesets|[string]|false|none|A list of event-breaking rulesets that will be applied, in order, to the input data stream|
|»»»» staleChannelFlushMs|number|false|none|How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines|
|»»»» sendToRoutes|boolean|false|none|Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.|
|»»»» preprocess|object|false|none|none|
|»»»»» disabled|boolean|true|none|none|
|»»»»» command|string|false|none|Command to feed the data through (via stdin) and process its output (stdout)|
|»»»»» args|[string]|false|none|Arguments to be added to the custom command|
|»»»» throttleRatePerSec|string|false|none|Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.|
|»»»» metadata|[object]|false|none|Fields to add to events from this input|
|»»»»» name|string|true|none|none|
|»»»»» value|string|true|none|JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)|
|»»»» pipeline|string|false|none|Pipeline to process results|
|»»»» output|string|false|none|Destination to send results to|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobExecutor](#schemasavedjobexecutor)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» executor|object|true|none|none|
|»»»» type|string|true|none|The type of executor to run|
|»»»» storeTaskResults|boolean|false|none|Determines whether or not to write task results to disk|
|»»»» conf|object|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[SavedJobScheduledSearch](#schemasavedjobscheduledsearch)|false|none|none|
|»»» id|string|false|none|Unique ID for this Job|
|»»» description|string|false|none|none|
|»»» type|string|true|none|none|
|»»» ttl|string|false|none|Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.|
|»»» ignoreGroupJobsLimit|boolean|false|none|When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.|
|»»» removeFields|[string]|false|none|List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.|
|»»»» Items|string|false|none|List of fields to remove from Discover results|
|»»» resumeOnBoot|boolean|false|none|Resume the ad hoc job if a failure condition causes Stream to restart during job execution|
|»»» environment|string|false|none|Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.|
|»»» schedule|object|false|none|Configuration for a scheduled job|
|»»»» enabled|boolean|false|none|Enable to configure scheduling for this Collector|
|»»»» skippable|boolean|false|none|Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits|
|»»»» resumeMissed|boolean|false|none|If Stream Leader (or single instance) restarts, run all missed jobs according to their original schedules|
|»»»» cronSchedule|string|false|none|A cron schedule on which to run this job|
|»»»» maxConcurrentRuns|number|false|none|The maximum number of instances of this scheduled job that may be running at any time|
|»»»» run|object|false|none|none|
|»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»» savedQueryId|string|true|none|Identifies which search query to run|

#### Enumerated Values

|Property|Value|
|---|---|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|collection|
|type|executor|
|type|scheduledSearch|
|type|collection|
|type|executor|
|type|scheduledSearch|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

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

