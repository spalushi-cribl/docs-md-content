
<h1 id="cribl-core-api-diagnostics-and-monitoring">Cribl Core API - Diagnostics and Monitoring v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-diagnostics-and-monitoring-diagnostics-and-monitoring">Diagnostics and Monitoring</h1>

## Remove diag bundle

<a id="opIddeleteSystemDiag"></a>

> Code samples

`DELETE /system/diag`

Remove diag bundle

<h3 id="remove-diag-bundle-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|path|query|string|true|Diag bundle path|

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

<h3 id="remove-diag-bundle-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="remove-diag-bundle-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get list of existing diag bundles

<a id="opIdgetSystemDiag"></a>

> Code samples

`GET /system/diag`

Get list of existing diag bundles

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "modTime": 0,
      "path": "string",
      "size": 0
    }
  ]
}
```

<h3 id="get-list-of-existing-diag-bundles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Diag objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-list-of-existing-diag-bundles-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Diag](#schemadiag)]|false|none|none|
|»» id|string|true|none|none|
|»» modTime|number|false|none|none|
|»» path|string|true|none|none|
|»» size|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Returns a diag bundle as a tar.gz archive

<a id="opIdgetDiagBundle"></a>

> Code samples

`GET /system/diag/download`

> Example responses

> 200 Response

<h3 id="returns-a-diag-bundle-as-a-tar.gz-archive-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A tar.gz file consisting all configuration files and recent logs|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

## Send a diag bundle (tar.gz archive) to Cribl

<a id="opIdcreateSystemDiagSend"></a>

> Code samples

`POST /system/diag/send`

Send a diag bundle (tar.gz archive) to Cribl

> Body parameter

```json
{
  "sendToCribl": false,
  "path": "string",
  "renameJs": true,
  "includeMetrics": true,
  "includeGit": true,
  "includeTopTalkers": false,
  "maxIncludeJobs": 0,
  "includeInstallLogs": false
}
```

<h3 id="send-a-diag-bundle-(tar.gz-archive)-to-cribl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SendDiagBundle](#schemasenddiagbundle)|true|SendDiagBundle object|
|» sendToCribl|body|boolean|false|Send diagnostic bundle to Cribl Support|
|» path|body|string|false|Existing bundle that will be sent to Cribl Support. Max 100MB.|
|» renameJs|body|boolean|false|Append .txt to JavaScript files|
|» includeMetrics|body|boolean|false|Disable to exclude metrics from the bundle|
|» includeGit|body|boolean|false|Disable to exclude the git audit from the bundle. Unavailable for worker, managed-edge, and outpost modes|
|» includeTopTalkers|body|boolean|false|Include data about your 10 highest-volume Sources, Destinations, Pipelines, Routes, and Packs in the diagnostic bundle|
|» maxIncludeJobs|body|number|false|Number of jobs, including all tasks that will be included in the bundle|
|» includeInstallLogs|body|boolean|false|Enable to include installation logs in the bundle (Windows only)|

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

<h3 id="send-a-diag-bundle-(tar.gz-archive)-to-cribl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="send-a-diag-bundle-(tar.gz-archive)-to-cribl-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get basic system information

<a id="opIdgetSystemInfo"></a>

> Code samples

`GET /system/info`

Get basic system information

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "BUILD": {},
      "apiPort": 0,
      "conf": {
        "confVersion": "string",
        "inputs": 0,
        "name": "string",
        "outputs": 0,
        "pipelines": 0,
        "routes": 0,
        "rules": 0
      },
      "configPath": "string",
      "cpus": [
        {
          "model": "string",
          "speed": 0,
          "times": {}
        }
      ],
      "diskUsage": {
        "bytesAvailable": 0,
        "bytesUsed": 0,
        "diskPath": "string",
        "totalDiskSize": 0
      },
      "distMode": "edge",
      "env": {},
      "guid": "string",
      "hasCloudWorkspace": true,
      "hostname": "string",
      "installPath": "string",
      "isCriblSandbox": true,
      "isFedRampEnabled": true,
      "isFipsEnabled": true,
      "license": {
        "email": "string",
        "isRegistered": true,
        "isSplunkApp": true,
        "limits": {
          "edge_groups": 0,
          "edge_procs": 0,
          "kms": 0,
          "lake_access_groups": 0,
          "lake_ddss": 0,
          "lake_metrics": 0,
          "lakehouse": 0,
          "leader_resiliency": 0,
          "max_executors_per_search": 0,
          "notifications": 0,
          "outpost": 0,
          "persistent_queue": 0,
          "projects": 0,
          "rbac": 0,
          "remote_auth": 0,
          "remote_git": 0,
          "s3_bundle": 0,
          "sds": 0,
          "search_acceleration": 0,
          "search_groups": 0,
          "system_email": 0,
          "worker_groups": 0,
          "worker_procs": 0
        },
        "type": "prod"
      },
      "limits": {
        "samples": {
          "maxSize": "string"
        }
      },
      "loadavg": [
        0
      ],
      "memory": {
        "free": 0,
        "total": 0
      },
      "messages": [
        {
          "id": "string",
          "severity": "info",
          "title": "string",
          "text": "string",
          "time": 0,
          "group": "string",
          "metadata": [
            {}
          ]
        }
      ],
      "net": {},
      "openssl": {
        "fipsProviderVersion": "string",
        "version": "string"
      },
      "os": {
        "arch": "string",
        "endianness": "string",
        "platform": "string",
        "release": "string",
        "type": "string"
      },
      "systemConf": {
        "installType": "string",
        "restart": "string",
        "upgrade": "string"
      },
      "uptime": 0,
      "version": "string",
      "workerProcesses": 0
    }
  ]
}
```

<h3 id="get-basic-system-information-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SystemInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-basic-system-information-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SystemInfo](#schemasysteminfo)]|false|none|none|
|»» BUILD|object|true|none|none|
|»» apiPort|number|true|none|none|
|»» conf|object|true|none|none|
|»»» confVersion|string|false|none|none|
|»»» inputs|number|true|none|none|
|»»» name|string|false|none|none|
|»»» outputs|number|true|none|none|
|»»» pipelines|number|true|none|none|
|»»» routes|number|true|none|none|
|»»» rules|number|true|none|none|
|»» configPath|string|true|none|none|
|»» cpus|[object]|true|none|none|
|»»» model|string|true|none|none|
|»»» speed|number|true|none|none|
|»»» times|object|true|none|none|
|»» diskUsage|object|true|none|none|
|»»» bytesAvailable|number|true|none|none|
|»»» bytesUsed|number|true|none|none|
|»»» diskPath|string|true|none|none|
|»»» totalDiskSize|number|true|none|none|
|»» distMode|string|true|none|none|
|»» env|object|true|none|none|
|»» guid|string|true|none|none|
|»» hasCloudWorkspace|boolean|false|none|none|
|»» hostname|string|true|none|none|
|»» installPath|string|true|none|none|
|»» isCriblSandbox|boolean|false|none|none|
|»» isFedRampEnabled|boolean|false|none|none|
|»» isFipsEnabled|boolean|false|none|none|
|»» license|[LicenseInfo](#schemalicenseinfo)|true|none|none|
|»»» email|string|false|none|none|
|»»» isRegistered|boolean|true|none|none|
|»»» isSplunkApp|boolean|false|none|none|
|»»» limits|[LicenseLimits](#schemalicenselimits)|true|none|none|
|»»»» edge_groups|number|false|none|none|
|»»»» edge_procs|number|false|none|none|
|»»»» kms|number|false|none|none|
|»»»» lake_access_groups|number|false|none|none|
|»»»» lake_ddss|number|false|none|none|
|»»»» lake_metrics|number|false|none|none|
|»»»» lakehouse|number|false|none|none|
|»»»» leader_resiliency|number|false|none|none|
|»»»» max_executors_per_search|number|false|none|none|
|»»»» notifications|number|false|none|none|
|»»»» outpost|number|false|none|none|
|»»»» persistent_queue|number|false|none|none|
|»»»» projects|number|false|none|none|
|»»»» rbac|number|true|none|none|
|»»»» remote_auth|number|true|none|none|
|»»»» remote_git|number|true|none|none|
|»»»» s3_bundle|number|false|none|none|
|»»»» sds|number|false|none|none|
|»»»» search_acceleration|number|false|none|none|
|»»»» search_groups|number|false|none|none|
|»»»» system_email|number|false|none|none|
|»»»» worker_groups|number|true|none|none|
|»»»» worker_procs|number|true|none|none|
|»»» type|string|true|none|none|
|»» limits|object|true|none|none|
|»»» samples|object|true|none|none|
|»»»» maxSize|string|true|none|none|
|»» loadavg|[number]|true|none|none|
|»» memory|object|true|none|none|
|»»» free|number|true|none|none|
|»»» total|number|true|none|none|
|»» messages|[[BulletinMessage](#schemabulletinmessage)]|true|none|none|
|»»» id|string|true|none|none|
|»»» severity|string|false|none|none|
|»»» title|string|false|none|none|
|»»» text|string|true|none|none|
|»»» time|number|false|none|none|
|»»» group|string|false|none|none|
|»»» metadata|[object]|false|none|none|
|»» net|object|true|none|none|
|»» openssl|[OpenSslInfo](#schemaopensslinfo)|false|none|none|
|»»» fipsProviderVersion|string|true|none|none|
|»»» version|string|true|none|none|
|»» os|object|true|none|none|
|»»» arch|string|true|none|none|
|»»» endianness|string|true|none|none|
|»»» platform|string|true|none|none|
|»»» release|string|true|none|none|
|»»» type|string|true|none|none|
|»» systemConf|[SystemConf](#schemasystemconf)|true|none|none|
|»»» installType|string|true|none|none|
|»»» restart|string|true|none|none|
|»»» upgrade|string|true|none|none|
|»» uptime|number|true|none|none|
|»» version|string|true|none|none|
|»» workerProcesses|number|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|distMode|edge|
|distMode|worker|
|distMode|single|
|distMode|master|
|distMode|managed-edge|
|distMode|outpost|
|distMode|search-supervisor|
|type|prod|
|type|trial|
|type|free|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<aside class="success">
This operation does not require authentication
</aside>

## Query raw internal system metrics

<a id="opIdgetSystemMetrics"></a>

> Code samples

`GET /system/metrics`

Query raw internal system metrics

<h3 id="query-raw-internal-system-metrics-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|wp|query|string|false|worker process to query, this would work only on a worker node|
|numBuckets|query|number|false|buckets in the past to include in the query results|
|earliest|query|any|false|earliest time to query against|
|latest|query|any|false|latest time to query against|
|metricNameFilter|query|any|false|can be a regex or an array of metric names|
|filterExpr|query|string|false|a js expression to apply against the metrics returned (can filter dimensions)|
|namespace|query|string|false|metric store namespace to retrieve data from|

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

<h3 id="query-raw-internal-system-metrics-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="query-raw-internal-system-metrics-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Enumerate all internal system metrics

<a id="opIdcreateSystemMetricsEnum"></a>

> Code samples

`POST /system/metrics/enum`

Enumerate all internal system metrics

> Body parameter

```json
{
  "dimKeyFilter": "string",
  "dimValueFilter": "string",
  "earliest": 0,
  "filterExpr": "string",
  "maxValues": 0,
  "metricNameFilter": "string",
  "namespace": "string"
}
```

<h3 id="enumerate-all-internal-system-metrics-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SystemMetricsEnum](#schemasystemmetricsenum)|true|SystemMetricsEnum object|
|» dimKeyFilter|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|[string]|false|none|
|» dimValueFilter|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|[string]|false|none|
|» earliest|body|number|false|none|
|» filterExpr|body|string|false|none|
|» maxValues|body|number|false|none|
|» metricNameFilter|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|[string]|false|none|
|» namespace|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "dims": [
        {
          "count": 0,
          "name": "string",
          "values": [
            "string"
          ]
        }
      ],
      "name": "string"
    }
  ]
}
```

<h3 id="enumerate-all-internal-system-metrics-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of MetricNameInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="enumerate-all-internal-system-metrics-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[MetricNameInfo](#schemametricnameinfo)]|false|none|none|
|»» dims|[object]|true|none|none|
|»»» count|number|true|none|none|
|»»» name|string|true|none|none|
|»»» values|[string]|true|none|none|
|»» name|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Aggregate raw internal system metrics

<a id="opIdcreateSystemMetricsQuery"></a>

> Code samples

`POST /system/metrics/query`

Aggregate raw internal system metrics

> Body parameter

```json
{
  "aggs": {
    "aggregations": [
      "string"
    ],
    "cumulative": true,
    "dotsAsLiterals": true,
    "flushEventLimit": 0,
    "flushMemLimit": 0,
    "hostname": "string",
    "idleTimeLimitSeconds": 0,
    "lagToleranceSeconds": 0,
    "metricsMode": true,
    "prefix": "string",
    "preserveSplitByStructure": true,
    "searchAggMode": "Coordinated",
    "splitBys": [
      "string"
    ],
    "sufficientStatsOnly": true,
    "timeWindowSeconds": 0
  },
  "alwaysBounds": true,
  "earliest": "string",
  "latest": "string",
  "metrics": {},
  "where": "string"
}
```

<h3 id="aggregate-raw-internal-system-metrics-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[MetricsAggOpts](#schemametricsaggopts)|true|MetricsAggOpts object|
|» aggs|body|[AggregationMgrOptions](#schemaaggregationmgroptions)|true|none|
|»» aggregations|body|[string]|true|none|
|»» cumulative|body|boolean|true|none|
|»» dotsAsLiterals|body|boolean|false|none|
|»» flushEventLimit|body|number|true|none|
|»» flushMemLimit|body|number|true|none|
|»» hostname|body|string|true|none|
|»» idleTimeLimitSeconds|body|number|false|none|
|»» lagToleranceSeconds|body|number|false|none|
|»» metricsMode|body|boolean|true|none|
|»» prefix|body|string|false|none|
|»» preserveSplitByStructure|body|boolean|false|none|
|»» searchAggMode|body|[SearchAggMode](#schemasearchaggmode)¦null|false|none|
|»» splitBys|body|[string]|false|none|
|»» sufficientStatsOnly|body|boolean|true|none|
|»» timeWindowSeconds|body|number|true|none|
|» alwaysBounds|body|boolean|false|none|
|» earliest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» latest|body|any|false|none|
|»» *anonymous*|body|string|false|none|
|»» *anonymous*|body|number|false|none|
|» metrics|body|[MetricsStore](#schemametricsstore)|false|none|
|» where|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» searchAggMode|Coordinated|
|»» searchAggMode|CoordinatedSuppressPreview|
|»» searchAggMode|DistributedCoordinated|
|»» searchAggMode|DistributedCoordinatedSuppressPreview|
|»» searchAggMode|Federated|

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

<h3 id="aggregate-raw-internal-system-metrics-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Response objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="aggregate-raw-internal-system-metrics-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of log files

<a id="opIdgetSystemLogs"></a>

> Code samples

`GET /system/logs`

Get a list of log files

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "path": "string",
      "size": 0
    }
  ]
}
```

<h3 id="get-a-list-of-log-files-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LogFileInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-log-files-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[LogFileInfo](#schemalogfileinfo)]|false|none|none|
|»» id|string|true|none|none|
|»» path|string|true|none|none|
|»» size|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get contents of the log file

<a id="opIdgetSystemJobsLogsByIdAndGroupId"></a>

> Code samples

`GET /system/jobs/logs/{id}/{groupId}`

Get contents of the log file

<h3 id="get-contents-of-the-log-file-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Job id|
|groupId|path|string|true|Group ID|
|limit|query|integer|false|Maximum number of log lines to retrieve starting from offset.|
|offset|query|integer|false|Offset in the current log file to fetch the log events from.|
|endOffset|query|integer|false|in the current log file to fetch the log events upto.|
|et|query|integer|false|Epoch timestamp of the earliest event (includes rolled files present on disk)|
|lt|query|integer|false|Epoch timestamp of the latest event (includes rolled files present on disk)|
|filter|query|string|false|Filter|

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

<h3 id="get-contents-of-the-log-file-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-contents-of-the-log-file-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Retrieve the latest metric values, optionally aggregated, for multiple time series

<a id="opIdcreateInsightsMetricsQuery"></a>

> Code samples

`POST /insights/metrics/query`

Retrieve the latest metric values, optionally aggregated, for multiple time series

> Body parameter

```json
{
  "bucketSize": 0,
  "dimensionFilters": [
    {
      "name": "string",
      "operator": "eq",
      "value": "string"
    }
  ],
  "dimensionSplits": [
    "string"
  ],
  "endTime": 0,
  "metricExpressions": [
    "string"
  ],
  "startTime": 0
}
```

<h3 id="retrieve-the-latest-metric-values,-optionally-aggregated,-for-multiple-time-series-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[InsightsQueryRequest](#schemainsightsqueryrequest)|true|InsightsQueryRequest object|
|» bucketSize|body|number|false|none|
|» dimensionFilters|body|[oneOf]|false|none|
|»» *anonymous*|body|object|false|none|
|»»» name|body|string|true|none|
|»»» operator|body|string|true|none|
|»»» value|body|string|true|none|
|»» *anonymous*|body|object|false|none|
|»»» name|body|string|true|none|
|»»» operator|body|string|true|none|
|»»» value|body|string|true|none|
|»» *anonymous*|body|object|false|none|
|»»» name|body|string|true|none|
|»»» operator|body|string|true|none|
|»»» value|body|boolean|true|none|
|» dimensionSplits|body|[string]|false|none|
|» endTime|body|number|false|none|
|» metricExpressions|body|[string]|true|none|
|» startTime|body|number|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» operator|eq|
|»»» operator|ne|
|»»» operator|exists|

> Example responses

> 200 Response

```json
{
  "timeSeries": [
    {
      "dimensions": {
        "property1": "string",
        "property2": "string"
      },
      "name": "string",
      "points": [
        {
          "endTime": 0,
          "startTime": 0,
          "value": 0
        }
      ]
    }
  ]
}
```

<h3 id="retrieve-the-latest-metric-values,-optionally-aggregated,-for-multiple-time-series-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|InsightsQueryResponse object|[InsightsQueryResponse](#schemainsightsqueryresponse)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of IoMetricsEntry objects

<a id="opIdlistIoMetricsEntry"></a>

> Code samples

`GET /system/iometrics`

Get a list of IoMetricsEntry objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "canDelete": true,
      "id": "string",
      "level": "minimal"
    }
  ]
}
```

<h3 id="get-a-list-of-iometricsentry-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of IoMetricsEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-iometricsentry-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[IoMetricsEntry](#schemaiometricsentry)]|false|none|none|
|»» canDelete|boolean|false|none|none|
|»» id|string|true|none|none|
|»» level|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|level|minimal|
|level|basic|
|level|detailed|
|level|comprehensive|

<aside class="success">
This operation does not require authentication
</aside>

## Get IoMetricsEntry by ID

<a id="opIdgetIoMetricsEntryById"></a>

> Code samples

`GET /system/iometrics/{id}`

Get IoMetricsEntry by ID

<h3 id="get-iometricsentry-by-id-parameters">Parameters</h3>

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
      "canDelete": true,
      "id": "string",
      "level": "minimal"
    }
  ]
}
```

<h3 id="get-iometricsentry-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of IoMetricsEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-iometricsentry-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[IoMetricsEntry](#schemaiometricsentry)]|false|none|none|
|»» canDelete|boolean|false|none|none|
|»» id|string|true|none|none|
|»» level|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|level|minimal|
|level|basic|
|level|detailed|
|level|comprehensive|

<aside class="success">
This operation does not require authentication
</aside>

## Update IoMetricsEntry

<a id="opIdupdateIoMetricsEntryById"></a>

> Code samples

`PATCH /system/iometrics/{id}`

Update IoMetricsEntry

> Body parameter

```json
{
  "canDelete": true,
  "id": "string",
  "level": "minimal"
}
```

<h3 id="update-iometricsentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[IoMetricsEntry](#schemaiometricsentry)|true|IoMetricsEntry object to be updated|
|» canDelete|body|boolean|false|none|
|» id|body|string|true|none|
|» level|body|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» level|minimal|
|» level|basic|
|» level|detailed|
|» level|comprehensive|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "canDelete": true,
      "id": "string",
      "level": "minimal"
    }
  ]
}
```

<h3 id="update-iometricsentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of IoMetricsEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-iometricsentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[IoMetricsEntry](#schemaiometricsentry)]|false|none|none|
|»» canDelete|boolean|false|none|none|
|»» id|string|true|none|none|
|»» level|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|level|minimal|
|level|basic|
|level|detailed|
|level|comprehensive|

<aside class="success">
This operation does not require authentication
</aside>

## Delete IoMetricsEntry

<a id="opIddeleteIoMetricsEntryById"></a>

> Code samples

`DELETE /system/iometrics/{id}`

Delete IoMetricsEntry

<h3 id="delete-iometricsentry-parameters">Parameters</h3>

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
      "canDelete": true,
      "id": "string",
      "level": "minimal"
    }
  ]
}
```

<h3 id="delete-iometricsentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of IoMetricsEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-iometricsentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[IoMetricsEntry](#schemaiometricsentry)]|false|none|none|
|»» canDelete|boolean|false|none|none|
|»» id|string|true|none|none|
|»» level|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|level|minimal|
|level|basic|
|level|detailed|
|level|comprehensive|

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

<h2 id="tocS_Diag">Diag</h2>
<!-- backwards compatibility -->
<a id="schemadiag"></a>
<a id="schema_Diag"></a>
<a id="tocSdiag"></a>
<a id="tocsdiag"></a>

```json
{
  "id": "string",
  "modTime": 0,
  "path": "string",
  "size": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|modTime|number|false|none|none|
|path|string|true|none|none|
|size|number|false|none|none|

<h2 id="tocS_SendDiagBundle">SendDiagBundle</h2>
<!-- backwards compatibility -->
<a id="schemasenddiagbundle"></a>
<a id="schema_SendDiagBundle"></a>
<a id="tocSsenddiagbundle"></a>
<a id="tocssenddiagbundle"></a>

```json
{
  "sendToCribl": false,
  "path": "string",
  "renameJs": true,
  "includeMetrics": true,
  "includeGit": true,
  "includeTopTalkers": false,
  "maxIncludeJobs": 0,
  "includeInstallLogs": false
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|sendToCribl|boolean|false|none|Send diagnostic bundle to Cribl Support|
|path|string|false|none|Existing bundle that will be sent to Cribl Support. Max 100MB.|
|renameJs|boolean|false|none|Append .txt to JavaScript files|
|includeMetrics|boolean|false|none|Disable to exclude metrics from the bundle|
|includeGit|boolean|false|none|Disable to exclude the git audit from the bundle. Unavailable for worker, managed-edge, and outpost modes|
|includeTopTalkers|boolean|false|none|Include data about your 10 highest-volume Sources, Destinations, Pipelines, Routes, and Packs in the diagnostic bundle|
|maxIncludeJobs|number|false|none|Number of jobs, including all tasks that will be included in the bundle|
|includeInstallLogs|boolean|false|none|Enable to include installation logs in the bundle (Windows only)|

<h2 id="tocS_SystemInfo">SystemInfo</h2>
<!-- backwards compatibility -->
<a id="schemasysteminfo"></a>
<a id="schema_SystemInfo"></a>
<a id="tocSsysteminfo"></a>
<a id="tocssysteminfo"></a>

```json
{
  "BUILD": {},
  "apiPort": 0,
  "conf": {
    "confVersion": "string",
    "inputs": 0,
    "name": "string",
    "outputs": 0,
    "pipelines": 0,
    "routes": 0,
    "rules": 0
  },
  "configPath": "string",
  "cpus": [
    {
      "model": "string",
      "speed": 0,
      "times": {}
    }
  ],
  "diskUsage": {
    "bytesAvailable": 0,
    "bytesUsed": 0,
    "diskPath": "string",
    "totalDiskSize": 0
  },
  "distMode": "edge",
  "env": {},
  "guid": "string",
  "hasCloudWorkspace": true,
  "hostname": "string",
  "installPath": "string",
  "isCriblSandbox": true,
  "isFedRampEnabled": true,
  "isFipsEnabled": true,
  "license": {
    "email": "string",
    "isRegistered": true,
    "isSplunkApp": true,
    "limits": {
      "edge_groups": 0,
      "edge_procs": 0,
      "kms": 0,
      "lake_access_groups": 0,
      "lake_ddss": 0,
      "lake_metrics": 0,
      "lakehouse": 0,
      "leader_resiliency": 0,
      "max_executors_per_search": 0,
      "notifications": 0,
      "outpost": 0,
      "persistent_queue": 0,
      "projects": 0,
      "rbac": 0,
      "remote_auth": 0,
      "remote_git": 0,
      "s3_bundle": 0,
      "sds": 0,
      "search_acceleration": 0,
      "search_groups": 0,
      "system_email": 0,
      "worker_groups": 0,
      "worker_procs": 0
    },
    "type": "prod"
  },
  "limits": {
    "samples": {
      "maxSize": "string"
    }
  },
  "loadavg": [
    0
  ],
  "memory": {
    "free": 0,
    "total": 0
  },
  "messages": [
    {
      "id": "string",
      "severity": "info",
      "title": "string",
      "text": "string",
      "time": 0,
      "group": "string",
      "metadata": [
        {}
      ]
    }
  ],
  "net": {},
  "openssl": {
    "fipsProviderVersion": "string",
    "version": "string"
  },
  "os": {
    "arch": "string",
    "endianness": "string",
    "platform": "string",
    "release": "string",
    "type": "string"
  },
  "systemConf": {
    "installType": "string",
    "restart": "string",
    "upgrade": "string"
  },
  "uptime": 0,
  "version": "string",
  "workerProcesses": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|BUILD|object|true|none|none|
|apiPort|number|true|none|none|
|conf|object|true|none|none|
|» confVersion|string|false|none|none|
|» inputs|number|true|none|none|
|» name|string|false|none|none|
|» outputs|number|true|none|none|
|» pipelines|number|true|none|none|
|» routes|number|true|none|none|
|» rules|number|true|none|none|
|configPath|string|true|none|none|
|cpus|[object]|true|none|none|
|» model|string|true|none|none|
|» speed|number|true|none|none|
|» times|object|true|none|none|
|diskUsage|object|true|none|none|
|» bytesAvailable|number|true|none|none|
|» bytesUsed|number|true|none|none|
|» diskPath|string|true|none|none|
|» totalDiskSize|number|true|none|none|
|distMode|string|true|none|none|
|env|object|true|none|none|
|guid|string|true|none|none|
|hasCloudWorkspace|boolean|false|none|none|
|hostname|string|true|none|none|
|installPath|string|true|none|none|
|isCriblSandbox|boolean|false|none|none|
|isFedRampEnabled|boolean|false|none|none|
|isFipsEnabled|boolean|false|none|none|
|license|[LicenseInfo](#schemalicenseinfo)|true|none|none|
|limits|object|true|none|none|
|» samples|object|true|none|none|
|»» maxSize|string|true|none|none|
|loadavg|[number]|true|none|none|
|memory|object|true|none|none|
|» free|number|true|none|none|
|» total|number|true|none|none|
|messages|[[BulletinMessage](#schemabulletinmessage)]|true|none|none|
|net|object|true|none|none|
|openssl|[OpenSslInfo](#schemaopensslinfo)|false|none|none|
|os|object|true|none|none|
|» arch|string|true|none|none|
|» endianness|string|true|none|none|
|» platform|string|true|none|none|
|» release|string|true|none|none|
|» type|string|true|none|none|
|systemConf|[SystemConf](#schemasystemconf)|true|none|none|
|uptime|number|true|none|none|
|version|string|true|none|none|
|workerProcesses|number|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|distMode|edge|
|distMode|worker|
|distMode|single|
|distMode|master|
|distMode|managed-edge|
|distMode|outpost|
|distMode|search-supervisor|

<h2 id="tocS_MetricNameInfo">MetricNameInfo</h2>
<!-- backwards compatibility -->
<a id="schemametricnameinfo"></a>
<a id="schema_MetricNameInfo"></a>
<a id="tocSmetricnameinfo"></a>
<a id="tocsmetricnameinfo"></a>

```json
{
  "dims": [
    {
      "count": 0,
      "name": "string",
      "values": [
        "string"
      ]
    }
  ],
  "name": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dims|[object]|true|none|none|
|» count|number|true|none|none|
|» name|string|true|none|none|
|» values|[string]|true|none|none|
|name|string|true|none|none|

<h2 id="tocS_SystemMetricsEnum">SystemMetricsEnum</h2>
<!-- backwards compatibility -->
<a id="schemasystemmetricsenum"></a>
<a id="schema_SystemMetricsEnum"></a>
<a id="tocSsystemmetricsenum"></a>
<a id="tocssystemmetricsenum"></a>

```json
{
  "dimKeyFilter": "string",
  "dimValueFilter": "string",
  "earliest": 0,
  "filterExpr": "string",
  "maxValues": 0,
  "metricNameFilter": "string",
  "namespace": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dimKeyFilter|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dimValueFilter|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|earliest|number|false|none|none|
|filterExpr|string|false|none|none|
|maxValues|number|false|none|none|
|metricNameFilter|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|namespace|string|false|none|none|

<h2 id="tocS_MetricsAggOpts">MetricsAggOpts</h2>
<!-- backwards compatibility -->
<a id="schemametricsaggopts"></a>
<a id="schema_MetricsAggOpts"></a>
<a id="tocSmetricsaggopts"></a>
<a id="tocsmetricsaggopts"></a>

```json
{
  "aggs": {
    "aggregations": [
      "string"
    ],
    "cumulative": true,
    "dotsAsLiterals": true,
    "flushEventLimit": 0,
    "flushMemLimit": 0,
    "hostname": "string",
    "idleTimeLimitSeconds": 0,
    "lagToleranceSeconds": 0,
    "metricsMode": true,
    "prefix": "string",
    "preserveSplitByStructure": true,
    "searchAggMode": "Coordinated",
    "splitBys": [
      "string"
    ],
    "sufficientStatsOnly": true,
    "timeWindowSeconds": 0
  },
  "alwaysBounds": true,
  "earliest": "string",
  "latest": "string",
  "metrics": {},
  "where": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|aggs|[AggregationMgrOptions](#schemaaggregationmgroptions)|true|none|none|
|alwaysBounds|boolean|false|none|none|
|earliest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|latest|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|metrics|[MetricsStore](#schemametricsstore)|false|none|none|
|where|string|false|none|none|

<h2 id="tocS_LogFileInfo">LogFileInfo</h2>
<!-- backwards compatibility -->
<a id="schemalogfileinfo"></a>
<a id="schema_LogFileInfo"></a>
<a id="tocSlogfileinfo"></a>
<a id="tocslogfileinfo"></a>

```json
{
  "id": "string",
  "path": "string",
  "size": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|path|string|true|none|none|
|size|number|false|none|none|

<h2 id="tocS_InsightsQueryResponse">InsightsQueryResponse</h2>
<!-- backwards compatibility -->
<a id="schemainsightsqueryresponse"></a>
<a id="schema_InsightsQueryResponse"></a>
<a id="tocSinsightsqueryresponse"></a>
<a id="tocsinsightsqueryresponse"></a>

```json
{
  "timeSeries": [
    {
      "dimensions": {
        "property1": "string",
        "property2": "string"
      },
      "name": "string",
      "points": [
        {
          "endTime": 0,
          "startTime": 0,
          "value": 0
        }
      ]
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|timeSeries|[object]|true|none|none|
|» dimensions|object|true|none|none|
|»» **additionalProperties**|string|false|none|none|
|» name|string|true|none|none|
|» points|[object]|true|none|none|
|»» endTime|number|true|none|none|
|»» startTime|number|true|none|none|
|»» value|number|true|none|none|

<h2 id="tocS_InsightsQueryRequest">InsightsQueryRequest</h2>
<!-- backwards compatibility -->
<a id="schemainsightsqueryrequest"></a>
<a id="schema_InsightsQueryRequest"></a>
<a id="tocSinsightsqueryrequest"></a>
<a id="tocsinsightsqueryrequest"></a>

```json
{
  "bucketSize": 0,
  "dimensionFilters": [
    {
      "name": "string",
      "operator": "eq",
      "value": "string"
    }
  ],
  "dimensionSplits": [
    "string"
  ],
  "endTime": 0,
  "metricExpressions": [
    "string"
  ],
  "startTime": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bucketSize|number|false|none|none|
|dimensionFilters|[oneOf]|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» name|string|true|none|none|
|»» operator|string|true|none|none|
|»» value|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» name|string|true|none|none|
|»» operator|string|true|none|none|
|»» value|string|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» name|string|true|none|none|
|»» operator|string|true|none|none|
|»» value|boolean|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dimensionSplits|[string]|false|none|none|
|endTime|number|false|none|none|
|metricExpressions|[string]|true|none|none|
|startTime|number|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|operator|eq|
|operator|ne|
|operator|exists|

<h2 id="tocS_IoMetricsEntry">IoMetricsEntry</h2>
<!-- backwards compatibility -->
<a id="schemaiometricsentry"></a>
<a id="schema_IoMetricsEntry"></a>
<a id="tocSiometricsentry"></a>
<a id="tocsiometricsentry"></a>

```json
{
  "canDelete": true,
  "id": "string",
  "level": "minimal"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|canDelete|boolean|false|none|none|
|id|string|true|none|none|
|level|[IoMetricsEntryLevel](#schemaiometricsentrylevel)|true|none|none|

<h2 id="tocS_LicenseInfo">LicenseInfo</h2>
<!-- backwards compatibility -->
<a id="schemalicenseinfo"></a>
<a id="schema_LicenseInfo"></a>
<a id="tocSlicenseinfo"></a>
<a id="tocslicenseinfo"></a>

```json
{
  "email": "string",
  "isRegistered": true,
  "isSplunkApp": true,
  "limits": {
    "edge_groups": 0,
    "edge_procs": 0,
    "kms": 0,
    "lake_access_groups": 0,
    "lake_ddss": 0,
    "lake_metrics": 0,
    "lakehouse": 0,
    "leader_resiliency": 0,
    "max_executors_per_search": 0,
    "notifications": 0,
    "outpost": 0,
    "persistent_queue": 0,
    "projects": 0,
    "rbac": 0,
    "remote_auth": 0,
    "remote_git": 0,
    "s3_bundle": 0,
    "sds": 0,
    "search_acceleration": 0,
    "search_groups": 0,
    "system_email": 0,
    "worker_groups": 0,
    "worker_procs": 0
  },
  "type": "prod"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|email|string|false|none|none|
|isRegistered|boolean|true|none|none|
|isSplunkApp|boolean|false|none|none|
|limits|[LicenseLimits](#schemalicenselimits)|true|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|prod|
|type|trial|
|type|free|

<h2 id="tocS_BulletinMessage">BulletinMessage</h2>
<!-- backwards compatibility -->
<a id="schemabulletinmessage"></a>
<a id="schema_BulletinMessage"></a>
<a id="tocSbulletinmessage"></a>
<a id="tocsbulletinmessage"></a>

```json
{
  "id": "string",
  "severity": "info",
  "title": "string",
  "text": "string",
  "time": 0,
  "group": "string",
  "metadata": [
    {}
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|severity|string|false|none|none|
|title|string|false|none|none|
|text|string|true|none|none|
|time|number|false|none|none|
|group|string|false|none|none|
|metadata|[object]|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|severity|info|
|severity|warn|
|severity|error|
|severity|fatal|

<h2 id="tocS_OpenSslInfo">OpenSslInfo</h2>
<!-- backwards compatibility -->
<a id="schemaopensslinfo"></a>
<a id="schema_OpenSslInfo"></a>
<a id="tocSopensslinfo"></a>
<a id="tocsopensslinfo"></a>

```json
{
  "fipsProviderVersion": "string",
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fipsProviderVersion|string|true|none|none|
|version|string|true|none|none|

<h2 id="tocS_SystemConf">SystemConf</h2>
<!-- backwards compatibility -->
<a id="schemasystemconf"></a>
<a id="schema_SystemConf"></a>
<a id="tocSsystemconf"></a>
<a id="tocssystemconf"></a>

```json
{
  "installType": "string",
  "restart": "string",
  "upgrade": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|installType|string|true|none|none|
|restart|string|true|none|none|
|upgrade|string|true|none|none|

<h2 id="tocS_AggregationMgrOptions">AggregationMgrOptions</h2>
<!-- backwards compatibility -->
<a id="schemaaggregationmgroptions"></a>
<a id="schema_AggregationMgrOptions"></a>
<a id="tocSaggregationmgroptions"></a>
<a id="tocsaggregationmgroptions"></a>

```json
{
  "aggregations": [
    "string"
  ],
  "cumulative": true,
  "dotsAsLiterals": true,
  "flushEventLimit": 0,
  "flushMemLimit": 0,
  "hostname": "string",
  "idleTimeLimitSeconds": 0,
  "lagToleranceSeconds": 0,
  "metricsMode": true,
  "prefix": "string",
  "preserveSplitByStructure": true,
  "searchAggMode": "Coordinated",
  "splitBys": [
    "string"
  ],
  "sufficientStatsOnly": true,
  "timeWindowSeconds": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|aggregations|[string]|true|none|none|
|cumulative|boolean|true|none|none|
|dotsAsLiterals|boolean|false|none|none|
|flushEventLimit|number|true|none|none|
|flushMemLimit|number|true|none|none|
|hostname|string|true|none|none|
|idleTimeLimitSeconds|number|false|none|none|
|lagToleranceSeconds|number|false|none|none|
|metricsMode|boolean|true|none|none|
|prefix|string|false|none|none|
|preserveSplitByStructure|boolean|false|none|none|
|searchAggMode|[SearchAggMode](#schemasearchaggmode)|false|none|none|
|splitBys|[string]|false|none|none|
|sufficientStatsOnly|boolean|true|none|none|
|timeWindowSeconds|number|true|none|none|

<h2 id="tocS_MetricsStore">MetricsStore</h2>
<!-- backwards compatibility -->
<a id="schemametricsstore"></a>
<a id="schema_MetricsStore"></a>
<a id="tocSmetricsstore"></a>
<a id="tocsmetricsstore"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_IoMetricsEntryLevel">IoMetricsEntryLevel</h2>
<!-- backwards compatibility -->
<a id="schemaiometricsentrylevel"></a>
<a id="schema_IoMetricsEntryLevel"></a>
<a id="tocSiometricsentrylevel"></a>
<a id="tocsiometricsentrylevel"></a>

```json
"minimal"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|minimal|
|*anonymous*|basic|
|*anonymous*|detailed|
|*anonymous*|comprehensive|

<h2 id="tocS_LicenseLimits">LicenseLimits</h2>
<!-- backwards compatibility -->
<a id="schemalicenselimits"></a>
<a id="schema_LicenseLimits"></a>
<a id="tocSlicenselimits"></a>
<a id="tocslicenselimits"></a>

```json
{
  "edge_groups": 0,
  "edge_procs": 0,
  "kms": 0,
  "lake_access_groups": 0,
  "lake_ddss": 0,
  "lake_metrics": 0,
  "lakehouse": 0,
  "leader_resiliency": 0,
  "max_executors_per_search": 0,
  "notifications": 0,
  "outpost": 0,
  "persistent_queue": 0,
  "projects": 0,
  "rbac": 0,
  "remote_auth": 0,
  "remote_git": 0,
  "s3_bundle": 0,
  "sds": 0,
  "search_acceleration": 0,
  "search_groups": 0,
  "system_email": 0,
  "worker_groups": 0,
  "worker_procs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|edge_groups|number|false|none|none|
|edge_procs|number|false|none|none|
|kms|number|false|none|none|
|lake_access_groups|number|false|none|none|
|lake_ddss|number|false|none|none|
|lake_metrics|number|false|none|none|
|lakehouse|number|false|none|none|
|leader_resiliency|number|false|none|none|
|max_executors_per_search|number|false|none|none|
|notifications|number|false|none|none|
|outpost|number|false|none|none|
|persistent_queue|number|false|none|none|
|projects|number|false|none|none|
|rbac|number|true|none|none|
|remote_auth|number|true|none|none|
|remote_git|number|true|none|none|
|s3_bundle|number|false|none|none|
|sds|number|false|none|none|
|search_acceleration|number|false|none|none|
|search_groups|number|false|none|none|
|system_email|number|false|none|none|
|worker_groups|number|true|none|none|
|worker_procs|number|true|none|none|

<h2 id="tocS_SearchAggMode">SearchAggMode</h2>
<!-- backwards compatibility -->
<a id="schemasearchaggmode"></a>
<a id="schema_SearchAggMode"></a>
<a id="tocSsearchaggmode"></a>
<a id="tocssearchaggmode"></a>

```json
"Coordinated"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string¦null|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Coordinated|
|*anonymous*|CoordinatedSuppressPreview|
|*anonymous*|DistributedCoordinated|
|*anonymous*|DistributedCoordinatedSuppressPreview|
|*anonymous*|Federated|

