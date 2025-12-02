
<h1 id="cribl-core-api-team-management">Cribl Core API - Team Management v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-team-management-team-management">Team Management</h1>

## Get a list of Team objects

<a id="opIdgetTeam"></a>

> Code samples

`GET /system/teams`

Get a list of Team objects

<h3 id="get-a-list-of-team-objects-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|query|[ProductsExtended](#schemaproductsextended)|false|filter teams by product|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
|product|search|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "name": "string",
      "roles": [
        "string"
      ],
      "ssoGroupIds": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-team-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Team objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-team-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Team](#schemateam)]|false|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» roles|[string]|true|none|none|
|»» ssoGroupIds|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create Team

<a id="opIdcreateTeam"></a>

> Code samples

`POST /system/teams`

Create Team

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "name": "string",
  "roles": [
    "string"
  ],
  "ssoGroupIds": [
    "string"
  ]
}
```

<h3 id="create-team-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Team](#schemateam)|true|New Team object|
|» description|body|string|true|none|
|» id|body|string|true|none|
|» name|body|string|true|none|
|» roles|body|[string]|true|none|
|» ssoGroupIds|body|[string]|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "name": "string",
      "roles": [
        "string"
      ],
      "ssoGroupIds": [
        "string"
      ]
    }
  ]
}
```

<h3 id="create-team-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Team objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-team-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Team](#schemateam)]|false|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» roles|[string]|true|none|none|
|»» ssoGroupIds|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Team

<a id="opIddeleteTeamById"></a>

> Code samples

`DELETE /system/teams/{id}`

Delete Team

<h3 id="delete-team-parameters">Parameters</h3>

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
      "description": "string",
      "id": "string",
      "name": "string",
      "roles": [
        "string"
      ],
      "ssoGroupIds": [
        "string"
      ]
    }
  ]
}
```

<h3 id="delete-team-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Team objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-team-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Team](#schemateam)]|false|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» roles|[string]|true|none|none|
|»» ssoGroupIds|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Team by ID

<a id="opIdgetTeamById"></a>

> Code samples

`GET /system/teams/{id}`

Get Team by ID

<h3 id="get-team-by-id-parameters">Parameters</h3>

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
      "description": "string",
      "id": "string",
      "name": "string",
      "roles": [
        "string"
      ],
      "ssoGroupIds": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-team-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Team objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-team-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Team](#schemateam)]|false|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» roles|[string]|true|none|none|
|»» ssoGroupIds|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update Team

<a id="opIdupdateTeamById"></a>

> Code samples

`PATCH /system/teams/{id}`

Update Team

> Body parameter

```json
{
  "description": "string",
  "id": "string",
  "name": "string",
  "roles": [
    "string"
  ],
  "ssoGroupIds": [
    "string"
  ]
}
```

<h3 id="update-team-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[Team](#schemateam)|true|Team object to be updated|
|» description|body|string|true|none|
|» id|body|string|true|none|
|» name|body|string|true|none|
|» roles|body|[string]|true|none|
|» ssoGroupIds|body|[string]|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "description": "string",
      "id": "string",
      "name": "string",
      "roles": [
        "string"
      ],
      "ssoGroupIds": [
        "string"
      ]
    }
  ]
}
```

<h3 id="update-team-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Team objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-team-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Team](#schemateam)]|false|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» name|string|true|none|none|
|»» roles|[string]|true|none|none|
|»» ssoGroupIds|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get Teams's Access Control List

<a id="opIdgetTeamAclById"></a>

> Code samples

`GET /system/teams/{id}/acl`

Get Teams's Access Control List

<h3 id="get-teams's-access-control-list-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Team name|
|type|query|[RbacResource](#schemarbacresource)|false|resource type by which to filter access levels|

#### Enumerated Values

|Parameter|Value|
|---|---|
|type|groups|
|type|datasets|
|type|dataset-providers|
|type|projects|
|type|dashboards|
|type|macros|
|type|notebooks|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "gid": "string",
      "id": "string",
      "policy": "string",
      "type": "groups"
    }
  ]
}
```

<h3 id="get-teams's-access-control-list-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ResourcePolicy objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-teams's-access-control-list-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ResourcePolicy](#schemaresourcepolicy)]|false|none|none|
|»» gid|string|true|none|none|
|»» id|string|false|none|none|
|»» policy|string|true|none|none|
|»» type|[RbacResource](#schemarbacresource)|true|none|none|

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

## Get users on a team

<a id="opIdgetTeamUsersById"></a>

> Code samples

`GET /system/teams/{id}/users`

Get users on a team

<h3 id="get-users-on-a-team-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Team Name|

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

<h3 id="get-users-on-a-team-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-users-on-a-team-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update existing users on a team – admin use only

<a id="opIdcreateTeamUsersById"></a>

> Code samples

`POST /system/teams/{id}/users`

Update existing users on a team – admin use only

> Body parameter

```json
{
  "add": [
    "string"
  ],
  "rm": [
    "string"
  ]
}
```

<h3 id="update-existing-users-on-a-team-–-admin-use-only-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Team name|
|body|body|[MembershipSchema](#schemamembershipschema)|true|MembershipSchema object|
|» add|body|[string]|false|none|
|» rm|body|[string]|false|none|

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

<h3 id="update-existing-users-on-a-team-–-admin-use-only-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-existing-users-on-a-team-–-admin-use-only-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get user's product roles

<a id="opIdgetTeamRolesById"></a>

> Code samples

`GET /system/teams/users/{id}/roles`

Get user's product roles

<h3 id="get-user's-product-roles-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|user id|

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

<h3 id="get-user's-product-roles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-user's-product-roles-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Team">Team</h2>
<!-- backwards compatibility -->
<a id="schemateam"></a>
<a id="schema_Team"></a>
<a id="tocSteam"></a>
<a id="tocsteam"></a>

```json
{
  "description": "string",
  "id": "string",
  "name": "string",
  "roles": [
    "string"
  ],
  "ssoGroupIds": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|description|string|true|none|none|
|id|string|true|none|none|
|name|string|true|none|none|
|roles|[string]|true|none|none|
|ssoGroupIds|[string]|false|none|none|

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

<h2 id="tocS_ProductsExtended">ProductsExtended</h2>
<!-- backwards compatibility -->
<a id="schemaproductsextended"></a>
<a id="schema_ProductsExtended"></a>
<a id="tocSproductsextended"></a>
<a id="tocsproductsextended"></a>

```json
"stream"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|stream|
|*anonymous*|edge|
|*anonymous*|search|

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

<h2 id="tocS_MembershipSchema">MembershipSchema</h2>
<!-- backwards compatibility -->
<a id="schemamembershipschema"></a>
<a id="schema_MembershipSchema"></a>
<a id="tocSmembershipschema"></a>
<a id="tocsmembershipschema"></a>

```json
{
  "add": [
    "string"
  ],
  "rm": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|add|[string]|false|none|none|
|rm|[string]|false|none|none|

