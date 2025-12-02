
<h1 id="cribl-core-api-packs-pipelines-">Cribl Core API - Packs (Pipelines) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-packs-pipelines--packs-pipelines-">Packs (Pipelines)</h1>

## List all Pipelines within a Pack

<a id="opIdgetPipelineByPack"></a>

> Code samples

`GET /p/{pack}/pipelines`

Get a list of all Pipelines within the specified Pack.

<h3 id="list-all-pipelines-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pack|path|string|true|pack ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
          }
        ],
        "groups": {
          "property1": {
            "name": "string",
            "description": "string",
            "disabled": true
          },
          "property2": {
            "name": "string",
            "description": "string",
            "disabled": true
          }
        }
      }
    }
  ]
}
```

<h3 id="list-all-pipelines-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Pipeline objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-pipelines-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Pipeline](#schemapipeline)]|false|none|none|
|»» Pipeline Settings|[Pipeline](#schemapipeline)|false|none|none|
|»»» id|string|true|none|none|
|»»» conf|object|true|none|none|
|»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»» description|string|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»» id|string|true|none|Function ID|
|»»»»» description|string|false|none|Simple description of this step|
|»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»» conf|object|true|none|none|
|»»»»» groupId|string|false|none|Group ID|
|»»»» groups|object|false|none|none|
|»»»»» **additionalProperties**|object|false|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» description|string|false|none|Short description of this group|
|»»»»»» disabled|boolean|false|none|Whether this group is disabled|

<aside class="success">
This operation does not require authentication
</aside>

## Create a Pipeline within a Pack

<a id="opIdcreatePipelineByPack"></a>

> Code samples

`POST /p/{pack}/pipelines`

Create a new Pipeline within the specified Pack.

> Body parameter

```json
{
  "id": "string",
  "conf": {
    "asyncFuncTimeout": 10000,
    "output": "default",
    "description": "string",
    "streamtags": [],
    "functions": [
      {
        "filter": "true",
        "id": "string",
        "description": "string",
        "disabled": true,
        "final": true,
        "conf": {},
        "groupId": "string"
      }
    ],
    "groups": {
      "property1": {
        "name": "string",
        "description": "string",
        "disabled": true
      },
      "property2": {
        "name": "string",
        "description": "string",
        "disabled": true
      }
    }
  }
}
```

<h3 id="create-a-pipeline-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pack|path|string|true|pack ID to POST|
|body|body|[Pipeline](#schemapipeline)|true|Pipeline object|
|» id|body|string|true|none|
|» conf|body|object|true|none|
|»» asyncFuncTimeout|body|integer|false|Time (in ms) to wait for an async function to complete processing of a data item|
|»» output|body|string|false|The output destination for events processed by this Pipeline|
|»» description|body|string|false|none|
|»» streamtags|body|[string]|false|Tags for filtering and grouping in @{product}|
|»» functions|body|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|List of Functions to pass data through|
|»»» filter|body|string|false|Filter that selects data to be fed through this Function|
|»»» id|body|string|true|Function ID|
|»»» description|body|string|false|Simple description of this step|
|»»» disabled|body|boolean|false|If true, data will not be pushed through this function|
|»»» final|body|boolean|false|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»» conf|body|object|true|none|
|»»» groupId|body|string|false|Group ID|
|»» groups|body|object|false|none|
|»»» **additionalProperties**|body|object|false|none|
|»»»» name|body|string|true|none|
|»»»» description|body|string|false|Short description of this group|
|»»»» disabled|body|boolean|false|Whether this group is disabled|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
          }
        ],
        "groups": {
          "property1": {
            "name": "string",
            "description": "string",
            "disabled": true
          },
          "property2": {
            "name": "string",
            "description": "string",
            "disabled": true
          }
        }
      }
    }
  ]
}
```

<h3 id="create-a-pipeline-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Pipelines objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-pipeline-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Pipeline](#schemapipeline)]|false|none|none|
|»» Pipeline Settings|[Pipeline](#schemapipeline)|false|none|none|
|»»» id|string|true|none|none|
|»»» conf|object|true|none|none|
|»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»» description|string|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»» id|string|true|none|Function ID|
|»»»»» description|string|false|none|Simple description of this step|
|»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»» conf|object|true|none|none|
|»»»»» groupId|string|false|none|Group ID|
|»»»» groups|object|false|none|none|
|»»»»» **additionalProperties**|object|false|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» description|string|false|none|Short description of this group|
|»»»»»» disabled|boolean|false|none|Whether this group is disabled|

<aside class="success">
This operation does not require authentication
</aside>

## Delete a Pipeline within a Pack

<a id="opIddeletePipelineByPackAndId"></a>

> Code samples

`DELETE /p/{pack}/pipelines/{id}`

Delete the specified Pipeline within the specified Pack.

<h3 id="delete-a-pipeline-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pipeline to delete.|
|pack|path|string|true|pack ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
          }
        ],
        "groups": {
          "property1": {
            "name": "string",
            "description": "string",
            "disabled": true
          },
          "property2": {
            "name": "string",
            "description": "string",
            "disabled": true
          }
        }
      }
    }
  ]
}
```

<h3 id="delete-a-pipeline-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Pipeline objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-a-pipeline-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Pipeline](#schemapipeline)]|false|none|none|
|»» Pipeline Settings|[Pipeline](#schemapipeline)|false|none|none|
|»»» id|string|true|none|none|
|»»» conf|object|true|none|none|
|»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»» description|string|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»» id|string|true|none|Function ID|
|»»»»» description|string|false|none|Simple description of this step|
|»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»» conf|object|true|none|none|
|»»»»» groupId|string|false|none|Group ID|
|»»»» groups|object|false|none|none|
|»»»»» **additionalProperties**|object|false|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» description|string|false|none|Short description of this group|
|»»»»»» disabled|boolean|false|none|Whether this group is disabled|

<aside class="success">
This operation does not require authentication
</aside>

## Get a Pipeline within a Pack

<a id="opIdgetPipelineByPackAndId"></a>

> Code samples

`GET /p/{pack}/pipelines/{id}`

Get the specified Pipeline within the specified Pack.

<h3 id="get-a-pipeline-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pipeline to get.|
|pack|path|string|true|pack ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
          }
        ],
        "groups": {
          "property1": {
            "name": "string",
            "description": "string",
            "disabled": true
          },
          "property2": {
            "name": "string",
            "description": "string",
            "disabled": true
          }
        }
      }
    }
  ]
}
```

<h3 id="get-a-pipeline-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Pipeline objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-pipeline-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Pipeline](#schemapipeline)]|false|none|none|
|»» Pipeline Settings|[Pipeline](#schemapipeline)|false|none|none|
|»»» id|string|true|none|none|
|»»» conf|object|true|none|none|
|»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»» description|string|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»» id|string|true|none|Function ID|
|»»»»» description|string|false|none|Simple description of this step|
|»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»» conf|object|true|none|none|
|»»»»» groupId|string|false|none|Group ID|
|»»»» groups|object|false|none|none|
|»»»»» **additionalProperties**|object|false|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» description|string|false|none|Short description of this group|
|»»»»»» disabled|boolean|false|none|Whether this group is disabled|

<aside class="success">
This operation does not require authentication
</aside>

## Update a Pipeline within a Pack

<a id="opIdupdatePipelineByPackAndId"></a>

> Code samples

`PATCH /p/{pack}/pipelines/{id}`

Update the specified Pipeline within the specified Pack.</br></br>Provide a complete representation of the Pipeline that you want to update in the request body. This endpoint does not support partial updates. Cribl removes any omitted fields when updating the Pipeline.</br></br>Confirm that the configuration in your request body is correct before sending the request. If the configuration is incorrect, the updated Pipeline might not function as expected.

> Body parameter

```json
{
  "id": "string",
  "conf": {
    "asyncFuncTimeout": 10000,
    "output": "default",
    "description": "string",
    "streamtags": [],
    "functions": [
      {
        "filter": "true",
        "id": "string",
        "description": "string",
        "disabled": true,
        "final": true,
        "conf": {},
        "groupId": "string"
      }
    ],
    "groups": {
      "property1": {
        "name": "string",
        "description": "string",
        "disabled": true
      },
      "property2": {
        "name": "string",
        "description": "string",
        "disabled": true
      }
    }
  }
}
```

<h3 id="update-a-pipeline-within-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pipeline to update.|
|pack|path|string|true|pack ID to PATCH|
|body|body|[Pipeline](#schemapipeline)|true|Pipeline object|
|» id|body|string|true|none|
|» conf|body|object|true|none|
|»» asyncFuncTimeout|body|integer|false|Time (in ms) to wait for an async function to complete processing of a data item|
|»» output|body|string|false|The output destination for events processed by this Pipeline|
|»» description|body|string|false|none|
|»» streamtags|body|[string]|false|Tags for filtering and grouping in @{product}|
|»» functions|body|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|List of Functions to pass data through|
|»»» filter|body|string|false|Filter that selects data to be fed through this Function|
|»»» id|body|string|true|Function ID|
|»»» description|body|string|false|Simple description of this step|
|»»» disabled|body|boolean|false|If true, data will not be pushed through this function|
|»»» final|body|boolean|false|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»» conf|body|object|true|none|
|»»» groupId|body|string|false|Group ID|
|»» groups|body|object|false|none|
|»»» **additionalProperties**|body|object|false|none|
|»»»» name|body|string|true|none|
|»»»» description|body|string|false|Short description of this group|
|»»»» disabled|body|boolean|false|Whether this group is disabled|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "conf": {
        "asyncFuncTimeout": 10000,
        "output": "default",
        "description": "string",
        "streamtags": [],
        "functions": [
          {
            "filter": "true",
            "id": "string",
            "description": "string",
            "disabled": true,
            "final": true,
            "conf": {},
            "groupId": "string"
          }
        ],
        "groups": {
          "property1": {
            "name": "string",
            "description": "string",
            "disabled": true
          },
          "property2": {
            "name": "string",
            "description": "string",
            "disabled": true
          }
        }
      }
    }
  ]
}
```

<h3 id="update-a-pipeline-within-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Pipeline objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-a-pipeline-within-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Pipeline](#schemapipeline)]|false|none|none|
|»» Pipeline Settings|[Pipeline](#schemapipeline)|false|none|none|
|»»» id|string|true|none|none|
|»»» conf|object|true|none|none|
|»»»» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|»»»» output|string|false|none|The output destination for events processed by this Pipeline|
|»»»» description|string|false|none|none|
|»»»» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|»»»» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|»»»»» filter|string|false|none|Filter that selects data to be fed through this Function|
|»»»»» id|string|true|none|Function ID|
|»»»»» description|string|false|none|Simple description of this step|
|»»»»» disabled|boolean|false|none|If true, data will not be pushed through this function|
|»»»»» final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|»»»»» conf|object|true|none|none|
|»»»»» groupId|string|false|none|Group ID|
|»»»» groups|object|false|none|none|
|»»»»» **additionalProperties**|object|false|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» description|string|false|none|Short description of this group|
|»»»»»» disabled|boolean|false|none|Whether this group is disabled|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Pipeline">Pipeline</h2>
<!-- backwards compatibility -->
<a id="schemapipeline"></a>
<a id="schema_Pipeline"></a>
<a id="tocSpipeline"></a>
<a id="tocspipeline"></a>

```json
{
  "id": "string",
  "conf": {
    "asyncFuncTimeout": 10000,
    "output": "default",
    "description": "string",
    "streamtags": [],
    "functions": [
      {
        "filter": "true",
        "id": "string",
        "description": "string",
        "disabled": true,
        "final": true,
        "conf": {},
        "groupId": "string"
      }
    ],
    "groups": {
      "property1": {
        "name": "string",
        "description": "string",
        "disabled": true
      },
      "property2": {
        "name": "string",
        "description": "string",
        "disabled": true
      }
    }
  }
}

```

Pipeline Settings

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|conf|object|true|none|none|
|» asyncFuncTimeout|integer|false|none|Time (in ms) to wait for an async function to complete processing of a data item|
|» output|string|false|none|The output destination for events processed by this Pipeline|
|» description|string|false|none|none|
|» streamtags|[string]|false|none|Tags for filtering and grouping in @{product}|
|» functions|[[PipelineFunctionConf](#schemapipelinefunctionconf)]|false|none|List of Functions to pass data through|
|» groups|object|false|none|none|
|»» **additionalProperties**|object|false|none|none|
|»»» name|string|true|none|none|
|»»» description|string|false|none|Short description of this group|
|»»» disabled|boolean|false|none|Whether this group is disabled|

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

<h2 id="tocS_PipelineFunctionConf">PipelineFunctionConf</h2>
<!-- backwards compatibility -->
<a id="schemapipelinefunctionconf"></a>
<a id="schema_PipelineFunctionConf"></a>
<a id="tocSpipelinefunctionconf"></a>
<a id="tocspipelinefunctionconf"></a>

```json
{
  "filter": "true",
  "id": "string",
  "description": "string",
  "disabled": true,
  "final": true,
  "conf": {},
  "groupId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filter|string|false|none|Filter that selects data to be fed through this Function|
|id|string|true|none|Function ID|
|description|string|false|none|Simple description of this step|
|disabled|boolean|false|none|If true, data will not be pushed through this function|
|final|boolean|false|none|If enabled, stops the results of this Function from being passed to the downstream Functions|
|conf|object|true|none|none|
|groupId|string|false|none|Group ID|

