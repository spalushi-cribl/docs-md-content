
<h1 id="cribl-core-api-sample-data">Cribl Core API - Sample Data v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-sample-data-sample-data">Sample Data</h1>

## Get a list of DataSample objects

<a id="opIdlistDataSample"></a>

> Code samples

`GET /system/samples`

Get a list of DataSample objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "sampleName": "string",
      "pipelineId": "string",
      "description": "string",
      "ttl": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-datasample-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataSample objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-datasample-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataSample](#schemadatasample)]|false|none|none|
|»» id|string|true|none|none|
|»» sampleName|string|true|none|none|
|»» pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|»» description|string|false|none|Brief description of this sample file. Optional.|
|»» ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|»» tags|string|false|none|One or more tags related to this sample file. Optional.|

<aside class="success">
This operation does not require authentication
</aside>

## Create DataSample

<a id="opIdcreateDataSample"></a>

> Code samples

`POST /system/samples`

Create DataSample

> Body parameter

```json
{
  "id": "string",
  "sampleName": "string",
  "pipelineId": "string",
  "description": "string",
  "ttl": 0,
  "tags": "string"
}
```

<h3 id="create-datasample-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DataSample](#schemadatasample)|true|New DataSample object|
|» id|body|string|true|none|
|» sampleName|body|string|true|none|
|» pipelineId|body|string|false|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|» description|body|string|false|Brief description of this sample file. Optional.|
|» ttl|body|number|false|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|» tags|body|string|false|One or more tags related to this sample file. Optional.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "sampleName": "string",
      "pipelineId": "string",
      "description": "string",
      "ttl": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="create-datasample-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataSample objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-datasample-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataSample](#schemadatasample)]|false|none|none|
|»» id|string|true|none|none|
|»» sampleName|string|true|none|none|
|»» pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|»» description|string|false|none|Brief description of this sample file. Optional.|
|»» ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|»» tags|string|false|none|One or more tags related to this sample file. Optional.|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DataSample

<a id="opIddeleteDataSampleById"></a>

> Code samples

`DELETE /system/samples/{id}`

Delete DataSample

<h3 id="delete-datasample-parameters">Parameters</h3>

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
      "sampleName": "string",
      "pipelineId": "string",
      "description": "string",
      "ttl": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-datasample-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataSample objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-datasample-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataSample](#schemadatasample)]|false|none|none|
|»» id|string|true|none|none|
|»» sampleName|string|true|none|none|
|»» pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|»» description|string|false|none|Brief description of this sample file. Optional.|
|»» ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|»» tags|string|false|none|One or more tags related to this sample file. Optional.|

<aside class="success">
This operation does not require authentication
</aside>

## Get DataSample by ID

<a id="opIdgetDataSampleById"></a>

> Code samples

`GET /system/samples/{id}`

Get DataSample by ID

<h3 id="get-datasample-by-id-parameters">Parameters</h3>

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
      "sampleName": "string",
      "pipelineId": "string",
      "description": "string",
      "ttl": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="get-datasample-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataSample objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-datasample-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataSample](#schemadatasample)]|false|none|none|
|»» id|string|true|none|none|
|»» sampleName|string|true|none|none|
|»» pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|»» description|string|false|none|Brief description of this sample file. Optional.|
|»» ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|»» tags|string|false|none|One or more tags related to this sample file. Optional.|

<aside class="success">
This operation does not require authentication
</aside>

## Update DataSample

<a id="opIdupdateDataSampleById"></a>

> Code samples

`PATCH /system/samples/{id}`

Update DataSample

> Body parameter

```json
{
  "id": "string",
  "sampleName": "string",
  "pipelineId": "string",
  "description": "string",
  "ttl": 0,
  "tags": "string"
}
```

<h3 id="update-datasample-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[DataSample](#schemadatasample)|true|DataSample object to be updated|
|» id|body|string|true|none|
|» sampleName|body|string|true|none|
|» pipelineId|body|string|false|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|» description|body|string|false|Brief description of this sample file. Optional.|
|» ttl|body|number|false|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|» tags|body|string|false|One or more tags related to this sample file. Optional.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "sampleName": "string",
      "pipelineId": "string",
      "description": "string",
      "ttl": 0,
      "tags": "string"
    }
  ]
}
```

<h3 id="update-datasample-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DataSample objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-datasample-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DataSample](#schemadatasample)]|false|none|none|
|»» id|string|true|none|none|
|»» sampleName|string|true|none|none|
|»» pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|»» description|string|false|none|Brief description of this sample file. Optional.|
|»» ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|»» tags|string|false|none|One or more tags related to this sample file. Optional.|

<aside class="success">
This operation does not require authentication
</aside>

## Get sample content by ID

<a id="opIdgetDataSampleContentById"></a>

> Code samples

`GET /system/samples/{id}/content`

Get sample content by ID

<h3 id="get-sample-content-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Sample ID|
|alwaysReset|query|string|false|boolean on whether Sampler should always reset _time attribute|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    [
      {}
    ]
  ]
}
```

<h3 id="get-sample-content-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SampleContent objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-sample-content-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SampleContent](#schemasamplecontent)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DataSample">DataSample</h2>
<!-- backwards compatibility -->
<a id="schemadatasample"></a>
<a id="schema_DataSample"></a>
<a id="tocSdatasample"></a>
<a id="tocsdatasample"></a>

```json
{
  "id": "string",
  "sampleName": "string",
  "pipelineId": "string",
  "description": "string",
  "ttl": 0,
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|sampleName|string|true|none|none|
|pipelineId|string|false|none|Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.|
|description|string|false|none|Brief description of this sample file. Optional.|
|ttl|number|false|none|Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.|
|tags|string|false|none|One or more tags related to this sample file. Optional.|

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

<h2 id="tocS_SampleContent">SampleContent</h2>
<!-- backwards compatibility -->
<a id="schemasamplecontent"></a>
<a id="schema_SampleContent"></a>
<a id="tocSsamplecontent"></a>
<a id="tocssamplecontent"></a>

```json
[
  {}
]

```

### Properties

*None*

