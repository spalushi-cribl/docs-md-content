
<h1 id="cribl-edge-api-file-system-operations">Cribl Edge API - File System Operations v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-file-system-operations-file-system-operations">File System Operations</h1>

## Ingest a specified file through a specified pipeline to a specified destination or send to routes.

<a id="opIdcreateEdgeFileIngest"></a>

> Code samples

`POST /edge/file/ingest`

Ingest a specified file through a specified pipeline to a specified destination or send to routes.

<h3 id="ingest-a-specified-file-through-a-specified-pipeline-to-a-specified-destination-or-send-to-routes.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filePath|query|string|false|Absolute path to file to ingest.|
|pipelineId|query|string|false|Id of the pipeline to use.|
|outputId|query|string|false|Destination to send events to.|
|preProcessingPipelineId|query|string|false|Id to the pre-processing pipeline to use for routes.|
|sendToRoutes|query|string|false|boolean condition required on whether to send events to routes.|
|breakerRuleSet|query|string|false|Breaker rules to use on the file.|

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

<h3 id="ingest-a-specified-file-through-a-specified-pipeline-to-a-specified-destination-or-send-to-routes.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="ingest-a-specified-file-through-a-specified-pipeline-to-a-specified-destination-or-send-to-routes.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get some number of bytes from the file at the given path

<a id="opIdgetEdgeFileSample"></a>

> Code samples

`GET /edge/file/sample`

Get some number of bytes from the file at the given path

<h3 id="get-some-number-of-bytes-from-the-file-at-the-given-path-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|path|query|string|true|The path to the file to sample|
|bytesRequested|query|number|false|The number of bytes to return;   this value could be constrained by system limits.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "bytes": "string",
      "bytesRead": 0,
      "length": 0
    }
  ]
}
```

<h3 id="get-some-number-of-bytes-from-the-file-at-the-given-path-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SampleFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-some-number-of-bytes-from-the-file-at-the-given-path-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SampleFile](#schemasamplefile)]|false|none|none|
|»» bytes|string|true|none|none|
|»» bytesRead|number|true|none|none|
|»» length|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get details about a file on the edge host.

<a id="opIdgetEdgeFileinspect"></a>

> Code samples

`GET /edge/fileinspect`

Get details about a file on the edge host.

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

<h3 id="get-details-about-a-file-on-the-edge-host.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of EdgeFileInspectResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-details-about-a-file-on-the-edge-host.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[EdgeFileInspectResponse](#schemaedgefileinspectresponse)]|false|none|none|

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

<h2 id="tocS_SampleFile">SampleFile</h2>
<!-- backwards compatibility -->
<a id="schemasamplefile"></a>
<a id="schema_SampleFile"></a>
<a id="tocSsamplefile"></a>
<a id="tocssamplefile"></a>

```json
{
  "bytes": "string",
  "bytesRead": 0,
  "length": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bytes|string|true|none|none|
|bytesRead|number|true|none|none|
|length|number|true|none|none|

<h2 id="tocS_EdgeFileInspectResponse">EdgeFileInspectResponse</h2>
<!-- backwards compatibility -->
<a id="schemaedgefileinspectresponse"></a>
<a id="schema_EdgeFileInspectResponse"></a>
<a id="tocSedgefileinspectresponse"></a>
<a id="tocsedgefileinspectresponse"></a>

```json
{}

```

### Properties

*None*

