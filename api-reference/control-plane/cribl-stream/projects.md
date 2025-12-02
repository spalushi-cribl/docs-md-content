
<h1 id="cribl-stream-api-projects">Cribl Stream API - Projects v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-projects-projects">Projects</h1>

## Get a list of Project objects

<a id="opIdlistProject"></a>

> Code samples

`GET /system/projects`

Get a list of Project objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumers": {},
      "description": "string",
      "destinations": [
        "string"
      ],
      "id": "string",
      "subscriptions": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-project-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Project objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-project-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProjectConfig](#schemaprojectconfig)]|false|none|none|
|»» consumers|object|false|none|none|
|»» description|string|false|none|none|
|»» destinations|[string]|true|none|none|
|»» id|string|true|none|none|
|»» subscriptions|[string]|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create Project

<a id="opIdcreateProject"></a>

> Code samples

`POST /system/projects`

Create Project

> Body parameter

```json
{
  "consumers": {},
  "description": "string",
  "destinations": [
    "string"
  ],
  "id": "string",
  "subscriptions": [
    "string"
  ]
}
```

<h3 id="create-project-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ProjectConfig](#schemaprojectconfig)|true|New Project object|
|» consumers|body|object|false|none|
|» description|body|string|false|none|
|» destinations|body|[string]|true|none|
|» id|body|string|true|none|
|» subscriptions|body|[string]|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumers": {},
      "description": "string",
      "destinations": [
        "string"
      ],
      "id": "string",
      "subscriptions": [
        "string"
      ]
    }
  ]
}
```

<h3 id="create-project-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Project objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-project-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProjectConfig](#schemaprojectconfig)]|false|none|none|
|»» consumers|object|false|none|none|
|»» description|string|false|none|none|
|»» destinations|[string]|true|none|none|
|»» id|string|true|none|none|
|»» subscriptions|[string]|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Project

<a id="opIddeleteProjectById"></a>

> Code samples

`DELETE /system/projects/{id}`

Delete Project

<h3 id="delete-project-parameters">Parameters</h3>

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
      "consumers": {},
      "description": "string",
      "destinations": [
        "string"
      ],
      "id": "string",
      "subscriptions": [
        "string"
      ]
    }
  ]
}
```

<h3 id="delete-project-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Project objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-project-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProjectConfig](#schemaprojectconfig)]|false|none|none|
|»» consumers|object|false|none|none|
|»» description|string|false|none|none|
|»» destinations|[string]|true|none|none|
|»» id|string|true|none|none|
|»» subscriptions|[string]|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update Project

<a id="opIdupdateProjectById"></a>

> Code samples

`PATCH /system/projects/{id}`

Update Project

> Body parameter

```json
{
  "consumers": {},
  "description": "string",
  "destinations": [
    "string"
  ],
  "id": "string",
  "subscriptions": [
    "string"
  ]
}
```

<h3 id="update-project-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[ProjectConfig](#schemaprojectconfig)|true|Project object to be updated|
|» consumers|body|object|false|none|
|» description|body|string|false|none|
|» destinations|body|[string]|true|none|
|» id|body|string|true|none|
|» subscriptions|body|[string]|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "consumers": {},
      "description": "string",
      "destinations": [
        "string"
      ],
      "id": "string",
      "subscriptions": [
        "string"
      ]
    }
  ]
}
```

<h3 id="update-project-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Project objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-project-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ProjectConfig](#schemaprojectconfig)]|false|none|none|
|»» consumers|object|false|none|none|
|»» description|string|false|none|none|
|»» destinations|[string]|true|none|none|
|»» id|string|true|none|none|
|»» subscriptions|[string]|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Project ACL

<a id="opIdgetProjectAclById"></a>

> Code samples

`GET /system/projects/{id}/acl`

List accesses to Project granted to users

<h3 id="get-project-acl-parameters">Parameters</h3>

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

<h3 id="get-project-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-project-acl-responseschema">Response Schema</h3>

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

## Modify Project ACL

<a id="opIdcreateProjectAclApplyById"></a>

> Code samples

`POST /system/projects/{id}/acl/apply`

Add/remove accesses to Project for users

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-project-acl-parameters">Parameters</h3>

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

<h3 id="modify-project-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get Project Teams

<a id="opIdgetProjectAclTeamsById"></a>

> Code samples

`GET /system/projects/{id}/acl/teams`

List accesses to Project granted to Teams

<h3 id="get-project-teams-parameters">Parameters</h3>

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

<h3 id="get-project-teams-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-project-teams-responseschema">Response Schema</h3>

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

## Modify Project Teams ACL

<a id="opIdcreateProjectAclTeamsApplyById"></a>

> Code samples

`POST /system/projects/{id}/acl/teams/apply`

Add/remove accesses to Project for Teams

> Body parameter

```json
{
  "add": {},
  "rm": {}
}
```

<h3 id="modify-project-teams-acl-parameters">Parameters</h3>

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

<h3 id="modify-project-teams-acl-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|N/A|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ProjectConfig">ProjectConfig</h2>
<!-- backwards compatibility -->
<a id="schemaprojectconfig"></a>
<a id="schema_ProjectConfig"></a>
<a id="tocSprojectconfig"></a>
<a id="tocsprojectconfig"></a>

```json
{
  "consumers": {},
  "description": "string",
  "destinations": [
    "string"
  ],
  "id": "string",
  "subscriptions": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|consumers|object|false|none|none|
|description|string|false|none|none|
|destinations|[string]|true|none|none|
|id|string|true|none|none|
|subscriptions|[string]|true|none|none|

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

