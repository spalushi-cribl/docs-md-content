
<h1 id="cribl-stream-api-lookups">Cribl Stream API - Lookups v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-lookups-lookups">Lookups</h1>

## Upload LookupFile

<a id="opIdupdateLookupFile"></a>

> Code samples

`PUT /system/lookups`

Upload LookupFile

> Body parameter

```json
"string"
```

<h3 id="upload-lookupfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filename|query|string|true|Name of the lookup file to upload|
|cancel|query|boolean|false|Cancel the file upload operation|
|body|body|[LookupFileInfo](#schemalookupfileinfo)|true|LookupFileInfo object|

> Example responses

> 200 Response

```json
{
  "filename": "string",
  "rows": 0,
  "size": 0
}
```

<h3 id="upload-lookupfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|LookupFileInfoResponse object|[LookupFileInfoResponse](#schemalookupfileinforesponse)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of LookupFile objects

<a id="opIdlistLookupFile"></a>

> Code samples

`GET /system/lookups`

Get a list of LookupFile objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-lookupfile-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-lookupfile-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

## Create LookupFile

<a id="opIdcreateLookupFile"></a>

> Code samples

`POST /system/lookups`

Create LookupFile

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "tags": "string",
  "size": 0,
  "mode": "memory",
  "fileInfo": {
    "filename": "string"
  }
}
```

<h3 id="create-lookupfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[LookupFile](#schemalookupfile)|true|New LookupFile object|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» tags|body|string|false|none|
|» size|body|number|false|File size. Optional.|
|» version|body|string|false|Unique string generated for each modification of this lookup|
|» mode|body|string|false|none|
|» pendingTask|body|object|false|none|
|»» id|body|string|false|Task ID (generated).|
|»» type|body|string|false|Task type|
|»» error|body|string|false|Error message if task has failed|
|» *anonymous*|body|object|false|none|
|»» fileInfo|body|object|false|none|
|»»» filename|body|string|true|none|
|» *anonymous*|body|object|false|none|
|»» content|body|string|false|File content.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» mode|memory|
|» mode|disk|
|»» type|IMPORT|
|»» type|INDEX|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="create-lookupfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-lookupfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

## Clone a lookup from one context to another

<a id="opIdupdateLookupFileCloneById"></a>

> Code samples

`PUT /system/lookups/{id}/clone`

Clone a lookup from one context to another

> Body parameter

```json
{
  "context": {
    "id": "string",
    "type": "project"
  },
  "newId": "string"
}
```

<h3 id="clone-a-lookup-from-one-context-to-another-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|id|
|body|body|[LookupCloneBody](#schemalookupclonebody)|true|LookupCloneBody object|
|» context|body|[Context](#schemacontext)|true|none|
|»» id|body|string|true|none|
|»» type|body|string|true|none|
|» newId|body|string|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» type|project|
|»» type|pack|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="clone-a-lookup-from-one-context-to-another-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="clone-a-lookup-from-one-context-to-another-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

## Delete LookupFile

<a id="opIddeleteLookupFileById"></a>

> Code samples

`DELETE /system/lookups/{id}`

Delete LookupFile

<h3 id="delete-lookupfile-parameters">Parameters</h3>

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
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="delete-lookupfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-lookupfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

## Get LookupFile by ID

<a id="opIdgetLookupFileById"></a>

> Code samples

`GET /system/lookups/{id}`

Get LookupFile by ID

<h3 id="get-lookupfile-by-id-parameters">Parameters</h3>

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
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="get-lookupfile-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-lookupfile-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

## Update LookupFile

<a id="opIdupdateLookupFileById"></a>

> Code samples

`PATCH /system/lookups/{id}`

Update LookupFile

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "tags": "string",
  "size": 0,
  "mode": "memory",
  "fileInfo": {
    "filename": "string"
  }
}
```

<h3 id="update-lookupfile-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[LookupFile](#schemalookupfile)|true|LookupFile object to be updated|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» tags|body|string|false|none|
|» size|body|number|false|File size. Optional.|
|» version|body|string|false|Unique string generated for each modification of this lookup|
|» mode|body|string|false|none|
|» pendingTask|body|object|false|none|
|»» id|body|string|false|Task ID (generated).|
|»» type|body|string|false|Task type|
|»» error|body|string|false|Error message if task has failed|
|» *anonymous*|body|object|false|none|
|»» fileInfo|body|object|false|none|
|»»» filename|body|string|true|none|
|» *anonymous*|body|object|false|none|
|»» content|body|string|false|File content.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» mode|memory|
|» mode|disk|
|»» type|IMPORT|
|»» type|INDEX|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "tags": "string",
      "size": 0,
      "version": "string",
      "mode": "memory",
      "pendingTask": {
        "id": "string",
        "type": "IMPORT",
        "error": "string"
      },
      "fileInfo": {
        "filename": "string"
      }
    }
  ]
}
```

<h3 id="update-lookupfile-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of LookupFile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-lookupfile-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[anyOf]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» tags|string|false|none|none|
|»» size|number|false|none|File size. Optional.|
|»» version|string|false|read-only|Unique string generated for each modification of this lookup|
|»» mode|string|false|none|none|
|»» pendingTask|object|false|read-only|none|
|»»» id|string|false|read-only|Task ID (generated).|
|»»» type|string|false|read-only|Task type|
|»»» error|string|false|read-only|Error message if task has failed|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» fileInfo|object|false|none|none|
|»»»» filename|string|true|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_LookupFileInfoResponse">LookupFileInfoResponse</h2>
<!-- backwards compatibility -->
<a id="schemalookupfileinforesponse"></a>
<a id="schema_LookupFileInfoResponse"></a>
<a id="tocSlookupfileinforesponse"></a>
<a id="tocslookupfileinforesponse"></a>

```json
{
  "filename": "string",
  "rows": 0,
  "size": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filename|string|true|none|none|
|rows|number|true|none|none|
|size|number|true|none|none|

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

<h2 id="tocS_LookupFileInfo">LookupFileInfo</h2>
<!-- backwards compatibility -->
<a id="schemalookupfileinfo"></a>
<a id="schema_LookupFileInfo"></a>
<a id="tocSlookupfileinfo"></a>
<a id="tocslookupfileinfo"></a>

```json
{
  "filename": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|filename|string|true|none|none|

<h2 id="tocS_LookupFile">LookupFile</h2>
<!-- backwards compatibility -->
<a id="schemalookupfile"></a>
<a id="schema_LookupFile"></a>
<a id="tocSlookupfile"></a>
<a id="tocslookupfile"></a>

```json
{
  "id": "string",
  "description": "string",
  "tags": "string",
  "size": 0,
  "version": "string",
  "mode": "memory",
  "pendingTask": {
    "id": "string",
    "type": "IMPORT",
    "error": "string"
  },
  "fileInfo": {
    "filename": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|description|string|false|none|none|
|tags|string|false|none|none|
|size|number|false|none|File size. Optional.|
|version|string|false|read-only|Unique string generated for each modification of this lookup|
|mode|string|false|none|none|
|pendingTask|object|false|read-only|none|
|» id|string|false|read-only|Task ID (generated).|
|» type|string|false|read-only|Task type|
|» error|string|false|read-only|Error message if task has failed|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» fileInfo|object|false|none|none|
|»» filename|string|true|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» content|string|false|none|File content.|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|memory|
|mode|disk|
|type|IMPORT|
|type|INDEX|

<h2 id="tocS_LookupCloneBody">LookupCloneBody</h2>
<!-- backwards compatibility -->
<a id="schemalookupclonebody"></a>
<a id="schema_LookupCloneBody"></a>
<a id="tocSlookupclonebody"></a>
<a id="tocslookupclonebody"></a>

```json
{
  "context": {
    "id": "string",
    "type": "project"
  },
  "newId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|context|[Context](#schemacontext)|true|none|none|
|newId|string|true|none|none|

<h2 id="tocS_Context">Context</h2>
<!-- backwards compatibility -->
<a id="schemacontext"></a>
<a id="schema_Context"></a>
<a id="tocScontext"></a>
<a id="tocscontext"></a>

```json
{
  "id": "string",
  "type": "project"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|project|
|type|pack|

