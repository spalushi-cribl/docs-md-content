
<h1 id="cribl-core-api-authentication-basic-">Cribl Core API - Authentication (Basic) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-authentication-basic--authentication-basic-">Authentication (Basic)</h1>

## Log in and fetch an authentication token

<a id="opIdcreateAuthLogin"></a>

> Code samples

`POST /auth/login`

This endpoint is unavailable on Cribl.Cloud.Instead, follow the instructions at https://docs.cribl.io/stream/api-tutorials/#criblcloud to get an Auth token for Cribl.Cloud.

> Body parameter

```json
{
  "password": "string",
  "username": "string"
}
```

<h3 id="log-in-and-fetch-an-authentication-token-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[LoginInfo](#schemalogininfo)|true|LoginInfo object|
|» password|body|string|true|none|
|» username|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "forcePasswordChange": true,
  "token": "string"
}
```

<h3 id="log-in-and-fetch-an-authentication-token-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authentication token|[AuthToken](#schemaauthtoken)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Invalid credentials or authentication failed|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Rate limit exceeded|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|429|retry-after|integer||Number of seconds the client should wait before retrying the request|

<aside class="success">
This operation does not require authentication
</aside>

## Log current user out

<a id="opIdlogout"></a>

> Code samples

`POST /auth/logout`

<h3 id="log-current-user-out-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Log out success|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_AuthToken">AuthToken</h2>
<!-- backwards compatibility -->
<a id="schemaauthtoken"></a>
<a id="schema_AuthToken"></a>
<a id="tocSauthtoken"></a>
<a id="tocsauthtoken"></a>

```json
{
  "forcePasswordChange": true,
  "token": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|forcePasswordChange|boolean|true|none|none|
|token|string|true|none|none|

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

<h2 id="tocS_LoginInfo">LoginInfo</h2>
<!-- backwards compatibility -->
<a id="schemalogininfo"></a>
<a id="schema_LoginInfo"></a>
<a id="tocSlogininfo"></a>
<a id="tocslogininfo"></a>

```json
{
  "password": "string",
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|password|string|true|none|none|
|username|string|true|none|none|

