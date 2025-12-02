
<h1 id="cribl-stream-api-database-connections">Cribl Stream API - Database Connections v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-database-connections-database-connections">Database Connections</h1>

## Get a list of DatabaseConnection objects

<a id="opIdlistDatabaseConnectionConfig"></a>

> Code samples

`GET /lib/database-connections`

Get a list of DatabaseConnection objects

<h3 id="get-a-list-of-databaseconnection-objects-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|databaseType|query|string|false|type of database connection|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "authType": "string",
      "configObj": "string",
      "connectionString": "string",
      "connectionTimeout": 0,
      "credsSecrets": "string",
      "databaseType": "mysql",
      "description": "string",
      "id": "string",
      "password": "string",
      "requestTimeout": 0,
      "tags": "string",
      "user": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-databaseconnection-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatabaseConnectionConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-databaseconnection-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatabaseConnectionConfig](#schemadatabaseconnectionconfig)]|false|none|none|
|»» authType|string|true|none|none|
|»» configObj|string|false|none|none|
|»» connectionString|string|false|none|none|
|»» connectionTimeout|number|false|none|none|
|»» credsSecrets|string|false|none|none|
|»» databaseType|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» requestTimeout|number|false|none|none|
|»» tags|string|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|databaseType|mysql|
|databaseType|oracle|
|databaseType|postgres|
|databaseType|sqlserver|

<aside class="success">
This operation does not require authentication
</aside>

## Create DatabaseConnectionConfig

<a id="opIdcreateDatabaseConnectionConfig"></a>

> Code samples

`POST /lib/database-connections`

Create DatabaseConnectionConfig

> Body parameter

```json
{
  "authType": "string",
  "configObj": "string",
  "connectionString": "string",
  "connectionTimeout": 0,
  "credsSecrets": "string",
  "databaseType": "mysql",
  "description": "string",
  "id": "string",
  "password": "string",
  "requestTimeout": 0,
  "tags": "string",
  "user": "string"
}
```

<h3 id="create-databaseconnectionconfig-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DatabaseConnectionConfig](#schemadatabaseconnectionconfig)|true|New DatabaseConnectionConfig object|
|» authType|body|string|true|none|
|» configObj|body|string|false|none|
|» connectionString|body|string|false|none|
|» connectionTimeout|body|number|false|none|
|» credsSecrets|body|string|false|none|
|» databaseType|body|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|
|» description|body|string|true|none|
|» id|body|string|true|none|
|» password|body|string|false|none|
|» requestTimeout|body|number|false|none|
|» tags|body|string|false|none|
|» user|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» databaseType|mysql|
|» databaseType|oracle|
|» databaseType|postgres|
|» databaseType|sqlserver|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "authType": "string",
      "configObj": "string",
      "connectionString": "string",
      "connectionTimeout": 0,
      "credsSecrets": "string",
      "databaseType": "mysql",
      "description": "string",
      "id": "string",
      "password": "string",
      "requestTimeout": 0,
      "tags": "string",
      "user": "string"
    }
  ]
}
```

<h3 id="create-databaseconnectionconfig-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatabaseConnectionConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-databaseconnectionconfig-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatabaseConnectionConfig](#schemadatabaseconnectionconfig)]|false|none|none|
|»» authType|string|true|none|none|
|»» configObj|string|false|none|none|
|»» connectionString|string|false|none|none|
|»» connectionTimeout|number|false|none|none|
|»» credsSecrets|string|false|none|none|
|»» databaseType|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» requestTimeout|number|false|none|none|
|»» tags|string|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|databaseType|mysql|
|databaseType|oracle|
|databaseType|postgres|
|databaseType|sqlserver|

<aside class="success">
This operation does not require authentication
</aside>

## Delete DatabaseConnectionConfig

<a id="opIddeleteDatabaseConnectionConfigById"></a>

> Code samples

`DELETE /lib/database-connections/{id}`

Delete DatabaseConnectionConfig

<h3 id="delete-databaseconnectionconfig-parameters">Parameters</h3>

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
      "authType": "string",
      "configObj": "string",
      "connectionString": "string",
      "connectionTimeout": 0,
      "credsSecrets": "string",
      "databaseType": "mysql",
      "description": "string",
      "id": "string",
      "password": "string",
      "requestTimeout": 0,
      "tags": "string",
      "user": "string"
    }
  ]
}
```

<h3 id="delete-databaseconnectionconfig-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatabaseConnectionConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-databaseconnectionconfig-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatabaseConnectionConfig](#schemadatabaseconnectionconfig)]|false|none|none|
|»» authType|string|true|none|none|
|»» configObj|string|false|none|none|
|»» connectionString|string|false|none|none|
|»» connectionTimeout|number|false|none|none|
|»» credsSecrets|string|false|none|none|
|»» databaseType|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» requestTimeout|number|false|none|none|
|»» tags|string|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|databaseType|mysql|
|databaseType|oracle|
|databaseType|postgres|
|databaseType|sqlserver|

<aside class="success">
This operation does not require authentication
</aside>

## Get DatabaseConnectionConfig by ID

<a id="opIdgetDatabaseConnectionConfigById"></a>

> Code samples

`GET /lib/database-connections/{id}`

Get DatabaseConnectionConfig by ID

<h3 id="get-databaseconnectionconfig-by-id-parameters">Parameters</h3>

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
      "authType": "string",
      "configObj": "string",
      "connectionString": "string",
      "connectionTimeout": 0,
      "credsSecrets": "string",
      "databaseType": "mysql",
      "description": "string",
      "id": "string",
      "password": "string",
      "requestTimeout": 0,
      "tags": "string",
      "user": "string"
    }
  ]
}
```

<h3 id="get-databaseconnectionconfig-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatabaseConnectionConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-databaseconnectionconfig-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatabaseConnectionConfig](#schemadatabaseconnectionconfig)]|false|none|none|
|»» authType|string|true|none|none|
|»» configObj|string|false|none|none|
|»» connectionString|string|false|none|none|
|»» connectionTimeout|number|false|none|none|
|»» credsSecrets|string|false|none|none|
|»» databaseType|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» requestTimeout|number|false|none|none|
|»» tags|string|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|databaseType|mysql|
|databaseType|oracle|
|databaseType|postgres|
|databaseType|sqlserver|

<aside class="success">
This operation does not require authentication
</aside>

## Test a database connection given a type and connectionString

<a id="opIdcreateLibDatabaseConnectionsTest"></a>

> Code samples

`POST /lib/database-connections/test`

Test a database connection given a type and connectionString

> Body parameter

```json
{
  "authType": "string",
  "configObj": "string",
  "connectionString": "string",
  "connectionTimeout": 0,
  "credsSecrets": "string",
  "databaseType": "string",
  "password": "string",
  "textSecret": "string",
  "user": "string"
}
```

<h3 id="test-a-database-connection-given-a-type-and-connectionstring-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[DatabaseConnectionTest](#schemadatabaseconnectiontest)|true|DatabaseConnectionTest object|
|» authType|body|string|true|none|
|» configObj|body|string|false|none|
|» connectionString|body|string|false|none|
|» connectionTimeout|body|number|false|none|
|» credsSecrets|body|string|false|none|
|» databaseType|body|string|true|none|
|» password|body|string|false|none|
|» textSecret|body|string|false|none|
|» user|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "errorMsg": "string",
      "success": true
    }
  ]
}
```

<h3 id="test-a-database-connection-given-a-type-and-connectionstring-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of DatabaseConnectionTestResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="test-a-database-connection-given-a-type-and-connectionstring-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[DatabaseConnectionTestResult](#schemadatabaseconnectiontestresult)]|false|none|none|
|»» errorMsg|string|false|none|none|
|»» success|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_DatabaseConnectionConfig">DatabaseConnectionConfig</h2>
<!-- backwards compatibility -->
<a id="schemadatabaseconnectionconfig"></a>
<a id="schema_DatabaseConnectionConfig"></a>
<a id="tocSdatabaseconnectionconfig"></a>
<a id="tocsdatabaseconnectionconfig"></a>

```json
{
  "authType": "string",
  "configObj": "string",
  "connectionString": "string",
  "connectionTimeout": 0,
  "credsSecrets": "string",
  "databaseType": "mysql",
  "description": "string",
  "id": "string",
  "password": "string",
  "requestTimeout": 0,
  "tags": "string",
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|authType|string|true|none|none|
|configObj|string|false|none|none|
|connectionString|string|false|none|none|
|connectionTimeout|number|false|none|none|
|credsSecrets|string|false|none|none|
|databaseType|[DatabaseConnectionType](#schemadatabaseconnectiontype)|true|none|none|
|description|string|true|none|none|
|id|string|true|none|none|
|password|string|false|none|none|
|requestTimeout|number|false|none|none|
|tags|string|false|none|none|
|user|string|false|none|none|

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

<h2 id="tocS_DatabaseConnectionTestResult">DatabaseConnectionTestResult</h2>
<!-- backwards compatibility -->
<a id="schemadatabaseconnectiontestresult"></a>
<a id="schema_DatabaseConnectionTestResult"></a>
<a id="tocSdatabaseconnectiontestresult"></a>
<a id="tocsdatabaseconnectiontestresult"></a>

```json
{
  "errorMsg": "string",
  "success": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|errorMsg|string|false|none|none|
|success|boolean|true|none|none|

<h2 id="tocS_DatabaseConnectionTest">DatabaseConnectionTest</h2>
<!-- backwards compatibility -->
<a id="schemadatabaseconnectiontest"></a>
<a id="schema_DatabaseConnectionTest"></a>
<a id="tocSdatabaseconnectiontest"></a>
<a id="tocsdatabaseconnectiontest"></a>

```json
{
  "authType": "string",
  "configObj": "string",
  "connectionString": "string",
  "connectionTimeout": 0,
  "credsSecrets": "string",
  "databaseType": "string",
  "password": "string",
  "textSecret": "string",
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|authType|string|true|none|none|
|configObj|string|false|none|none|
|connectionString|string|false|none|none|
|connectionTimeout|number|false|none|none|
|credsSecrets|string|false|none|none|
|databaseType|string|true|none|none|
|password|string|false|none|none|
|textSecret|string|false|none|none|
|user|string|false|none|none|

<h2 id="tocS_DatabaseConnectionType">DatabaseConnectionType</h2>
<!-- backwards compatibility -->
<a id="schemadatabaseconnectiontype"></a>
<a id="schema_DatabaseConnectionType"></a>
<a id="tocSdatabaseconnectiontype"></a>
<a id="tocsdatabaseconnectiontype"></a>

```json
"mysql"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|mysql|
|*anonymous*|oracle|
|*anonymous*|postgres|
|*anonymous*|sqlserver|

