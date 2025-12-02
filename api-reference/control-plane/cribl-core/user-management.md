
<h1 id="cribl-core-api-user-management">Cribl Core API - User Management v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-user-management-user-management">User Management</h1>

## Get a list of User objects

<a id="opIdlistUser"></a>

> Code samples

`GET /system/users`

Get a list of User objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-user-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-user-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create User

<a id="opIdcreateUser"></a>

> Code samples

`POST /system/users`

Create User

> Body parameter

```json
{
  "currentPassword": "string",
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "teams": [
    "string"
  ],
  "username": "string"
}
```

<h3 id="create-user-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[User](#schemauser)|true|New User object|
|» currentPassword|body|string|false|none|
|» disabled|body|boolean|true|none|
|» email|body|string|true|none|
|» first|body|string|true|none|
|» id|body|string|true|none|
|» last|body|string|true|none|
|» password|body|string|false|none|
|» roles|body|[string]|false|none|
|» teams|body|[string]|false|none|
|» username|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="create-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-user-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete User

<a id="opIddeleteUserById"></a>

> Code samples

`DELETE /system/users/{id}`

Delete User

<h3 id="delete-user-parameters">Parameters</h3>

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
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="delete-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-user-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get User by ID

<a id="opIdgetUserById"></a>

> Code samples

`GET /system/users/{id}`

Get User by ID

<h3 id="get-user-by-id-parameters">Parameters</h3>

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
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="get-user-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-user-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update User properties – admin use only

<a id="opIdupdateUserById"></a>

> Code samples

`PATCH /system/users/{id}`

Update User properties – admin use only

> Body parameter

```json
{
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "username": "string"
}
```

<h3 id="update-user-properties-–-admin-use-only-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|User Id|
|body|body|[UserProfile](#schemauserprofile)|true|UserProfile object|
|» disabled|body|boolean|true|none|
|» email|body|string|true|none|
|» first|body|string|true|none|
|» id|body|string|true|none|
|» last|body|string|true|none|
|» password|body|string|false|none|
|» roles|body|[string]|false|none|
|» username|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="update-user-properties-–-admin-use-only-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UserProfile objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-user-properties-–-admin-use-only-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UserProfile](#schemauserprofile)]|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Update User except for their roles

<a id="opIdupdateUserInfoById"></a>

> Code samples

`PATCH /system/users/{id}/info`

Update User except for their roles

> Body parameter

```json
{
  "currentPassword": "string",
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "username": "string"
}
```

<h3 id="update-user-except-for-their-roles-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|User Id|
|body|body|[UserInfo](#schemauserinfo)|true|UserInfo object|
|» currentPassword|body|string|false|none|
|» disabled|body|boolean|true|none|
|» email|body|string|true|none|
|» first|body|string|true|none|
|» id|body|string|true|none|
|» last|body|string|true|none|
|» password|body|string|false|none|
|» roles|body|[string]|false|none|
|» username|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "currentPassword": "string",
      "disabled": true,
      "email": "string",
      "first": "string",
      "id": "string",
      "last": "string",
      "password": "string",
      "roles": [
        "string"
      ],
      "teams": [
        "string"
      ],
      "username": "string"
    }
  ]
}
```

<h3 id="update-user-except-for-their-roles-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of User objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-user-except-for-their-roles-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[User](#schemauser)]|false|none|none|
|»» currentPassword|string|false|none|none|
|»» disabled|boolean|true|none|none|
|»» email|string|true|none|none|
|»» first|string|true|none|none|
|»» id|string|true|none|none|
|»» last|string|true|none|none|
|»» password|string|false|none|none|
|»» roles|[string]|false|none|none|
|»» teams|[string]|false|none|none|
|»» username|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_User">User</h2>
<!-- backwards compatibility -->
<a id="schemauser"></a>
<a id="schema_User"></a>
<a id="tocSuser"></a>
<a id="tocsuser"></a>

```json
{
  "currentPassword": "string",
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "teams": [
    "string"
  ],
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|currentPassword|string|false|none|none|
|disabled|boolean|true|none|none|
|email|string|true|none|none|
|first|string|true|none|none|
|id|string|true|none|none|
|last|string|true|none|none|
|password|string|false|none|none|
|roles|[string]|false|none|none|
|teams|[string]|false|none|none|
|username|string|true|none|none|

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

<h2 id="tocS_UserProfile">UserProfile</h2>
<!-- backwards compatibility -->
<a id="schemauserprofile"></a>
<a id="schema_UserProfile"></a>
<a id="tocSuserprofile"></a>
<a id="tocsuserprofile"></a>

```json
{
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|disabled|boolean|true|none|none|
|email|string|true|none|none|
|first|string|true|none|none|
|id|string|true|none|none|
|last|string|true|none|none|
|password|string|false|none|none|
|roles|[string]|false|none|none|
|username|string|true|none|none|

<h2 id="tocS_UserInfo">UserInfo</h2>
<!-- backwards compatibility -->
<a id="schemauserinfo"></a>
<a id="schema_UserInfo"></a>
<a id="tocSuserinfo"></a>
<a id="tocsuserinfo"></a>

```json
{
  "currentPassword": "string",
  "disabled": true,
  "email": "string",
  "first": "string",
  "id": "string",
  "last": "string",
  "password": "string",
  "roles": [
    "string"
  ],
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|currentPassword|string|false|none|none|
|disabled|boolean|true|none|none|
|email|string|true|none|none|
|first|string|true|none|none|
|id|string|true|none|none|
|last|string|true|none|none|
|password|string|false|none|none|
|roles|[string]|false|none|none|
|username|string|true|none|none|

