
<h1 id="cribl-search-api-datasets">Cribl Search API - Datasets v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-datasets-datasets">Datasets</h1>

## Create Dataset

<a id="opIdcreateDataset"></a>

> Code samples

`POST /search/datasets`

Create Dataset

> Body parameter

```json
{}
```

<h3 id="create-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UnionOfValues](#schemaunionofvalues)|true|New Dataset object|

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

<h3 id="create-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Dataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of Dataset objects

<a id="opIdlistDataset"></a>

> Code samples

`GET /search/datasets`

Get a list of Dataset objects

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

<h3 id="get-a-list-of-dataset-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Dataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-dataset-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Dataset

<a id="opIddeleteDatasetById"></a>

> Code samples

`DELETE /search/datasets/{id}`

Delete Dataset

<h3 id="delete-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

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

<h3 id="delete-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Dataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update Dataset

<a id="opIdupdateDatasetById"></a>

> Code samples

`PATCH /search/datasets/{id}`

Update Dataset

> Body parameter

```json
{}
```

<h3 id="update-dataset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[UnionOfValues](#schemaunionofvalues)|true|Dataset object to be updated|

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

<h3 id="update-dataset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Dataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-dataset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Dataset by ID

<a id="opIdgetDatasetById"></a>

> Code samples

`GET /search/datasets/{id}`

Get Dataset by ID

<h3 id="get-dataset-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

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

<h3 id="get-dataset-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Dataset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-dataset-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UnionOfValues](#schemaunionofvalues)]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Dataset ACL

<a id="opIdgetDatasetAclById"></a>

> Code samples

`GET /search/datasets/{id}/acl`

List accesses to Dataset granted to users

<h3 id="get-dataset-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Get|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "perms": [
        {
          "gid": "string",
          "id": "string",
          "policy": "string",
          "type": "groups"
        }
      ],
      "user": "string"
    }
  ]
}
```

<h3 id="get-dataset-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-dataset-acl-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UserAccessControlList](#schemauseraccesscontrollist)]|false|none|none|
|»» perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|»»» gid|string|true|none|none|
|»»» id|string|false|none|none|
|»»» policy|string|true|none|none|
|»»» type|[RbacResource](#schemarbacresource)|true|none|none|
|»» user|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

<aside class="success">
This operation does not require authentication
</aside>

## Modify Dataset ACL

<a id="opIdcreateDatasetAclApplyById"></a>

> Code samples

`POST /search/datasets/{id}/acl/apply`

Add/remove accesses to Dataset for users

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-dataset-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Create|
|body|body|[AccessControlSchema](#schemaaccesscontrolschema)|true|AccessControlSchema object|
|» add|body|[AccessControl](#schemaaccesscontrol)|false|none|
|» rm|body|[AccessControl](#schemaaccesscontrol)|false|none|

> Example responses

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="modify-dataset-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get Dataset Teams

<a id="opIdgetDatasetAclTeamsById"></a>

> Code samples

`GET /search/datasets/{id}/acl/teams`

List accesses to Dataset granted to Teams

<h3 id="get-dataset-teams-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Teams Get|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "perms": [
        {
          "gid": "string",
          "id": "string",
          "policy": "string",
          "type": "groups"
        }
      ],
      "user": "string"
    }
  ]
}
```

<h3 id="get-dataset-teams-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-dataset-teams-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UserAccessControlList](#schemauseraccesscontrollist)]|false|none|none|
|»» perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|»»» gid|string|true|none|none|
|»»» id|string|false|none|none|
|»»» policy|string|true|none|none|
|»»» type|[RbacResource](#schemarbacresource)|true|none|none|
|»» user|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

<aside class="success">
This operation does not require authentication
</aside>

## Modify Dataset Teams ACL

<a id="opIdcreateDatasetAclTeamsApplyById"></a>

> Code samples

`POST /search/datasets/{id}/acl/teams/apply`

Add/remove accesses to Dataset for Teams

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-dataset-teams-acl-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID for ACL Teams Create|
|body|body|[AccessControlSchema](#schemaaccesscontrolschema)|true|AccessControlSchema object|
|» add|body|[AccessControl](#schemaaccesscontrol)|false|none|
|» rm|body|[AccessControl](#schemaaccesscontrol)|false|none|

> Example responses

> 500 Response

```json
{
  "message": "string"
}
```

<h3 id="modify-dataset-teams-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Dataset">Dataset</h2>
<!-- backwards compatibility -->
<a id="schemadataset"></a>
<a id="schema_Dataset"></a>
<a id="tocSdataset"></a>
<a id="tocsdataset"></a>

```json
{}

```

### Properties

*None*

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

<h2 id="tocS_UserAccessControlList">UserAccessControlList</h2>
<!-- backwards compatibility -->
<a id="schemauseraccesscontrollist"></a>
<a id="schema_UserAccessControlList"></a>
<a id="tocSuseraccesscontrollist"></a>
<a id="tocsuseraccesscontrollist"></a>

```json
{
  "perms": [
    {
      "gid": "string",
      "id": "string",
      "policy": "string",
      "type": "groups"
    }
  ],
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|user|string|true|none|none|

<h2 id="tocS_AccessControlSchema">AccessControlSchema</h2>
<!-- backwards compatibility -->
<a id="schemaaccesscontrolschema"></a>
<a id="schema_AccessControlSchema"></a>
<a id="tocSaccesscontrolschema"></a>
<a id="tocsaccesscontrolschema"></a>

```json
{
  "add": {},
  "rm": {}
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|add|[AccessControl](#schemaaccesscontrol)|false|none|none|
|rm|[AccessControl](#schemaaccesscontrol)|false|none|none|

<h2 id="tocS_UnionOfValues">UnionOfValues</h2>
<!-- backwards compatibility -->
<a id="schemaunionofvalues"></a>
<a id="schema_UnionOfValues"></a>
<a id="tocSunionofvalues"></a>
<a id="tocsunionofvalues"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_ResourcePolicy">ResourcePolicy</h2>
<!-- backwards compatibility -->
<a id="schemaresourcepolicy"></a>
<a id="schema_ResourcePolicy"></a>
<a id="tocSresourcepolicy"></a>
<a id="tocsresourcepolicy"></a>

```json
{
  "gid": "string",
  "id": "string",
  "policy": "string",
  "type": "groups"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|gid|string|true|none|none|
|id|string|false|none|none|
|policy|string|true|none|none|
|type|[RbacResource](#schemarbacresource)|true|none|none|

<h2 id="tocS_AccessControl">AccessControl</h2>
<!-- backwards compatibility -->
<a id="schemaaccesscontrol"></a>
<a id="schema_AccessControl"></a>
<a id="tocSaccesscontrol"></a>
<a id="tocsaccesscontrol"></a>

```json
{}

```

### Properties

*None*

<h2 id="tocS_RbacResource">RbacResource</h2>
<!-- backwards compatibility -->
<a id="schemarbacresource"></a>
<a id="schema_RbacResource"></a>
<a id="tocSrbacresource"></a>
<a id="tocsrbacresource"></a>

```json
"groups"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|groups|
|*anonymous*|datasets|
|*anonymous*|dataset-providers|
|*anonymous*|projects|
|*anonymous*|dashboards|
|*anonymous*|macros|
|*anonymous*|notebooks|

