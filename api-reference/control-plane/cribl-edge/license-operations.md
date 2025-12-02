
<h1 id="cribl-edge-api-license-operations">Cribl Edge API - License Operations v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-license-operations-license-operations">License Operations</h1>

## Get a list of License objects

<a id="opIdlistLicense"></a>

> Code samples

`GET /system/licenses`

Get a list of License objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cls": "prod",
      "email": "string",
      "exp": 0,
      "f_ph": 0,
      "f_phg": 0,
      "guid": "string",
      "iat": 0,
      "id": "string",
      "iss": "string",
      "license": "string",
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
      "quota": 0,
      "sub": "string",
      "title": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-license-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of License objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-license-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[License](#schemalicense)]|false|none|none|
|»» cls|string|true|none|none|
|»» email|string|true|none|none|
|»» exp|number|true|none|none|
|»» f_ph|number|true|none|none|
|»» f_phg|number|true|none|none|
|»» guid|string|true|none|none|
|»» iat|number|true|none|none|
|»» id|string|true|none|none|
|»» iss|string|true|none|none|
|»» license|string|true|none|none|
|»» limits|object|false|none|none|
|»»» edge_groups|number|false|none|none|
|»»» edge_procs|number|false|none|none|
|»»» kms|number|false|none|none|
|»»» lake_access_groups|number|false|none|none|
|»»» lake_ddss|number|false|none|none|
|»»» lake_metrics|number|false|none|none|
|»»» lakehouse|number|false|none|none|
|»»» leader_resiliency|number|false|none|none|
|»»» max_executors_per_search|number|false|none|none|
|»»» notifications|number|false|none|none|
|»»» outpost|number|false|none|none|
|»»» persistent_queue|number|false|none|none|
|»»» projects|number|false|none|none|
|»»» rbac|number|false|none|none|
|»»» remote_auth|number|false|none|none|
|»»» remote_git|number|false|none|none|
|»»» s3_bundle|number|false|none|none|
|»»» sds|number|false|none|none|
|»»» search_acceleration|number|false|none|none|
|»»» search_groups|number|false|none|none|
|»»» system_email|number|false|none|none|
|»»» worker_groups|number|false|none|none|
|»»» worker_procs|number|false|none|none|
|»» quota|number|true|none|none|
|»» sub|string|false|none|none|
|»» title|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|cls|prod|
|cls|trial|
|cls|free|

<aside class="success">
This operation does not require authentication
</aside>

## Add a license to your deployment

<a id="opIdcreateLicense"></a>

> Code samples

`POST /system/licenses`

Add a license to your deployment

> Body parameter

```json
{
  "license": "string"
}
```

<h3 id="add-a-license-to-your-deployment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[LicenseRequest](#schemalicenserequest)|true|LicenseRequest object|
|» license|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cls": "prod",
      "email": "string",
      "exp": 0,
      "f_ph": 0,
      "f_phg": 0,
      "guid": "string",
      "iat": 0,
      "id": "string",
      "iss": "string",
      "license": "string",
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
      "quota": 0,
      "sub": "string",
      "title": "string"
    }
  ]
}
```

<h3 id="add-a-license-to-your-deployment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of License objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="add-a-license-to-your-deployment-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[License](#schemalicense)]|false|none|none|
|»» cls|string|true|none|none|
|»» email|string|true|none|none|
|»» exp|number|true|none|none|
|»» f_ph|number|true|none|none|
|»» f_phg|number|true|none|none|
|»» guid|string|true|none|none|
|»» iat|number|true|none|none|
|»» id|string|true|none|none|
|»» iss|string|true|none|none|
|»» license|string|true|none|none|
|»» limits|object|false|none|none|
|»»» edge_groups|number|false|none|none|
|»»» edge_procs|number|false|none|none|
|»»» kms|number|false|none|none|
|»»» lake_access_groups|number|false|none|none|
|»»» lake_ddss|number|false|none|none|
|»»» lake_metrics|number|false|none|none|
|»»» lakehouse|number|false|none|none|
|»»» leader_resiliency|number|false|none|none|
|»»» max_executors_per_search|number|false|none|none|
|»»» notifications|number|false|none|none|
|»»» outpost|number|false|none|none|
|»»» persistent_queue|number|false|none|none|
|»»» projects|number|false|none|none|
|»»» rbac|number|false|none|none|
|»»» remote_auth|number|false|none|none|
|»»» remote_git|number|false|none|none|
|»»» s3_bundle|number|false|none|none|
|»»» sds|number|false|none|none|
|»»» search_acceleration|number|false|none|none|
|»»» search_groups|number|false|none|none|
|»»» system_email|number|false|none|none|
|»»» worker_groups|number|false|none|none|
|»»» worker_procs|number|false|none|none|
|»» quota|number|true|none|none|
|»» sub|string|false|none|none|
|»» title|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|cls|prod|
|cls|trial|
|cls|free|

<aside class="success">
This operation does not require authentication
</aside>

## Delete License

<a id="opIddeleteLicenseById"></a>

> Code samples

`DELETE /system/licenses/{id}`

Delete License

<h3 id="delete-license-parameters">Parameters</h3>

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
      "cls": "prod",
      "email": "string",
      "exp": 0,
      "f_ph": 0,
      "f_phg": 0,
      "guid": "string",
      "iat": 0,
      "id": "string",
      "iss": "string",
      "license": "string",
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
      "quota": 0,
      "sub": "string",
      "title": "string"
    }
  ]
}
```

<h3 id="delete-license-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of License objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-license-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[License](#schemalicense)]|false|none|none|
|»» cls|string|true|none|none|
|»» email|string|true|none|none|
|»» exp|number|true|none|none|
|»» f_ph|number|true|none|none|
|»» f_phg|number|true|none|none|
|»» guid|string|true|none|none|
|»» iat|number|true|none|none|
|»» id|string|true|none|none|
|»» iss|string|true|none|none|
|»» license|string|true|none|none|
|»» limits|object|false|none|none|
|»»» edge_groups|number|false|none|none|
|»»» edge_procs|number|false|none|none|
|»»» kms|number|false|none|none|
|»»» lake_access_groups|number|false|none|none|
|»»» lake_ddss|number|false|none|none|
|»»» lake_metrics|number|false|none|none|
|»»» lakehouse|number|false|none|none|
|»»» leader_resiliency|number|false|none|none|
|»»» max_executors_per_search|number|false|none|none|
|»»» notifications|number|false|none|none|
|»»» outpost|number|false|none|none|
|»»» persistent_queue|number|false|none|none|
|»»» projects|number|false|none|none|
|»»» rbac|number|false|none|none|
|»»» remote_auth|number|false|none|none|
|»»» remote_git|number|false|none|none|
|»»» s3_bundle|number|false|none|none|
|»»» sds|number|false|none|none|
|»»» search_acceleration|number|false|none|none|
|»»» search_groups|number|false|none|none|
|»»» system_email|number|false|none|none|
|»»» worker_groups|number|false|none|none|
|»»» worker_procs|number|false|none|none|
|»» quota|number|true|none|none|
|»» sub|string|false|none|none|
|»» title|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|cls|prod|
|cls|trial|
|cls|free|

<aside class="success">
This operation does not require authentication
</aside>

## Get License by ID

<a id="opIdgetLicenseById"></a>

> Code samples

`GET /system/licenses/{id}`

Get License by ID

<h3 id="get-license-by-id-parameters">Parameters</h3>

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
      "cls": "prod",
      "email": "string",
      "exp": 0,
      "f_ph": 0,
      "f_phg": 0,
      "guid": "string",
      "iat": 0,
      "id": "string",
      "iss": "string",
      "license": "string",
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
      "quota": 0,
      "sub": "string",
      "title": "string"
    }
  ]
}
```

<h3 id="get-license-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of License objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-license-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[License](#schemalicense)]|false|none|none|
|»» cls|string|true|none|none|
|»» email|string|true|none|none|
|»» exp|number|true|none|none|
|»» f_ph|number|true|none|none|
|»» f_phg|number|true|none|none|
|»» guid|string|true|none|none|
|»» iat|number|true|none|none|
|»» id|string|true|none|none|
|»» iss|string|true|none|none|
|»» license|string|true|none|none|
|»» limits|object|false|none|none|
|»»» edge_groups|number|false|none|none|
|»»» edge_procs|number|false|none|none|
|»»» kms|number|false|none|none|
|»»» lake_access_groups|number|false|none|none|
|»»» lake_ddss|number|false|none|none|
|»»» lake_metrics|number|false|none|none|
|»»» lakehouse|number|false|none|none|
|»»» leader_resiliency|number|false|none|none|
|»»» max_executors_per_search|number|false|none|none|
|»»» notifications|number|false|none|none|
|»»» outpost|number|false|none|none|
|»»» persistent_queue|number|false|none|none|
|»»» projects|number|false|none|none|
|»»» rbac|number|false|none|none|
|»»» remote_auth|number|false|none|none|
|»»» remote_git|number|false|none|none|
|»»» s3_bundle|number|false|none|none|
|»»» sds|number|false|none|none|
|»»» search_acceleration|number|false|none|none|
|»»» search_groups|number|false|none|none|
|»»» system_email|number|false|none|none|
|»»» worker_groups|number|false|none|none|
|»»» worker_procs|number|false|none|none|
|»» quota|number|true|none|none|
|»» sub|string|false|none|none|
|»» title|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|cls|prod|
|cls|trial|
|cls|free|

<aside class="success">
This operation does not require authentication
</aside>

## Get license usage metrics, aggregated by day, up to last 90 days

<a id="opIdgetLicense"></a>

> Code samples

`GET /system/licenses/usage`

Get license usage metrics, aggregated by day, up to last 90 days

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "droppedBytes": 0,
      "edge": {
        "droppedBytes": 0,
        "exemptedLicenseInBytes": 0,
        "inBytes": 0,
        "inEvents": 0,
        "inMetricsEvents": 0,
        "outBytes": 0,
        "outEvents": 0
      },
      "endTime": 0,
      "exemptedLicenseInBytes": 0,
      "guard": {
        "inBytes": 0,
        "inEvents": 0
      },
      "inBytes": 0,
      "inEvents": 0,
      "inMetricsEvents": 0,
      "outBytes": 0,
      "outEvents": 0,
      "startTime": 0
    }
  ]
}
```

<h3 id="get-license-usage-metrics,-aggregated-by-day,-up-to-last-90-days-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UsageMetricsExtended objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-license-usage-metrics,-aggregated-by-day,-up-to-last-90-days-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UsageMetricsExtended](#schemausagemetricsextended)]|false|none|none|
|»» droppedBytes|number|false|none|none|
|»» edge|[EdgeUsageMetrics](#schemaedgeusagemetrics)|false|none|none|
|»»» droppedBytes|number|false|none|none|
|»»» exemptedLicenseInBytes|number|true|none|none|
|»»» inBytes|number|true|none|none|
|»»» inEvents|number|true|none|none|
|»»» inMetricsEvents|number|false|none|none|
|»»» outBytes|number|true|none|none|
|»»» outEvents|number|true|none|none|
|»» endTime|number|true|none|none|
|»» exemptedLicenseInBytes|number|true|none|none|
|»» guard|[GuardUsageMetrics](#schemaguardusagemetrics)|false|none|none|
|»»» inBytes|number|true|none|none|
|»»» inEvents|number|true|none|none|
|»» inBytes|number|true|none|none|
|»» inEvents|number|true|none|none|
|»» inMetricsEvents|number|false|none|none|
|»» outBytes|number|true|none|none|
|»» outEvents|number|true|none|none|
|»» startTime|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_License">License</h2>
<!-- backwards compatibility -->
<a id="schemalicense"></a>
<a id="schema_License"></a>
<a id="tocSlicense"></a>
<a id="tocslicense"></a>

```json
{
  "cls": "prod",
  "email": "string",
  "exp": 0,
  "f_ph": 0,
  "f_phg": 0,
  "guid": "string",
  "iat": 0,
  "id": "string",
  "iss": "string",
  "license": "string",
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
  "quota": 0,
  "sub": "string",
  "title": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cls|string|true|none|none|
|email|string|true|none|none|
|exp|number|true|none|none|
|f_ph|number|true|none|none|
|f_phg|number|true|none|none|
|guid|string|true|none|none|
|iat|number|true|none|none|
|id|string|true|none|none|
|iss|string|true|none|none|
|license|string|true|none|none|
|limits|object|false|none|none|
|» edge_groups|number|false|none|none|
|» edge_procs|number|false|none|none|
|» kms|number|false|none|none|
|» lake_access_groups|number|false|none|none|
|» lake_ddss|number|false|none|none|
|» lake_metrics|number|false|none|none|
|» lakehouse|number|false|none|none|
|» leader_resiliency|number|false|none|none|
|» max_executors_per_search|number|false|none|none|
|» notifications|number|false|none|none|
|» outpost|number|false|none|none|
|» persistent_queue|number|false|none|none|
|» projects|number|false|none|none|
|» rbac|number|false|none|none|
|» remote_auth|number|false|none|none|
|» remote_git|number|false|none|none|
|» s3_bundle|number|false|none|none|
|» sds|number|false|none|none|
|» search_acceleration|number|false|none|none|
|» search_groups|number|false|none|none|
|» system_email|number|false|none|none|
|» worker_groups|number|false|none|none|
|» worker_procs|number|false|none|none|
|quota|number|true|none|none|
|sub|string|false|none|none|
|title|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|cls|prod|
|cls|trial|
|cls|free|

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

<h2 id="tocS_LicenseRequest">LicenseRequest</h2>
<!-- backwards compatibility -->
<a id="schemalicenserequest"></a>
<a id="schema_LicenseRequest"></a>
<a id="tocSlicenserequest"></a>
<a id="tocslicenserequest"></a>

```json
{
  "license": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|license|string|true|none|none|

<h2 id="tocS_UsageMetricsExtended">UsageMetricsExtended</h2>
<!-- backwards compatibility -->
<a id="schemausagemetricsextended"></a>
<a id="schema_UsageMetricsExtended"></a>
<a id="tocSusagemetricsextended"></a>
<a id="tocsusagemetricsextended"></a>

```json
{
  "droppedBytes": 0,
  "edge": {
    "droppedBytes": 0,
    "exemptedLicenseInBytes": 0,
    "inBytes": 0,
    "inEvents": 0,
    "inMetricsEvents": 0,
    "outBytes": 0,
    "outEvents": 0
  },
  "endTime": 0,
  "exemptedLicenseInBytes": 0,
  "guard": {
    "inBytes": 0,
    "inEvents": 0
  },
  "inBytes": 0,
  "inEvents": 0,
  "inMetricsEvents": 0,
  "outBytes": 0,
  "outEvents": 0,
  "startTime": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|droppedBytes|number|false|none|none|
|edge|[EdgeUsageMetrics](#schemaedgeusagemetrics)|false|none|none|
|endTime|number|true|none|none|
|exemptedLicenseInBytes|number|true|none|none|
|guard|[GuardUsageMetrics](#schemaguardusagemetrics)|false|none|none|
|inBytes|number|true|none|none|
|inEvents|number|true|none|none|
|inMetricsEvents|number|false|none|none|
|outBytes|number|true|none|none|
|outEvents|number|true|none|none|
|startTime|number|true|none|none|

<h2 id="tocS_EdgeUsageMetrics">EdgeUsageMetrics</h2>
<!-- backwards compatibility -->
<a id="schemaedgeusagemetrics"></a>
<a id="schema_EdgeUsageMetrics"></a>
<a id="tocSedgeusagemetrics"></a>
<a id="tocsedgeusagemetrics"></a>

```json
{
  "droppedBytes": 0,
  "exemptedLicenseInBytes": 0,
  "inBytes": 0,
  "inEvents": 0,
  "inMetricsEvents": 0,
  "outBytes": 0,
  "outEvents": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|droppedBytes|number|false|none|none|
|exemptedLicenseInBytes|number|true|none|none|
|inBytes|number|true|none|none|
|inEvents|number|true|none|none|
|inMetricsEvents|number|false|none|none|
|outBytes|number|true|none|none|
|outEvents|number|true|none|none|

<h2 id="tocS_GuardUsageMetrics">GuardUsageMetrics</h2>
<!-- backwards compatibility -->
<a id="schemaguardusagemetrics"></a>
<a id="schema_GuardUsageMetrics"></a>
<a id="tocSguardusagemetrics"></a>
<a id="tocsguardusagemetrics"></a>

```json
{
  "inBytes": 0,
  "inEvents": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|inBytes|number|true|none|none|
|inEvents|number|true|none|none|

