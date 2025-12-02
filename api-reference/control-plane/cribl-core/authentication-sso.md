
<h1 id="cribl-core-api-authentication-sso-">Cribl Core API - Authentication (SSO) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-authentication-sso--authentication-sso-">Authentication (SSO)</h1>

## Obtain redirect information

<a id="opIdgetAuthSso"></a>

> Code samples

`GET /auth/sso`

Obtain information needed to redirect users to the configured SSO IDP for authentication.

> Example responses

> 200 Response

```json
{
  "name": "string",
  "redirectUrl": "string",
  "token": "string"
}
```

<h3 id="obtain-redirect-information-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Redirect info|[RedirectInfo](#schemaredirectinfo)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

## Accepts an authentication request from an IDP and authenticates the user

<a id="opIdgetAuthSsoCallback"></a>

> Code samples

`GET /auth/sso/callback`

<h3 id="accepts-an-authentication-request-from-an-idp-and-authenticates-the-user-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|SAMLResponse|query|string|false|Authentication request object|
|RelayState|query|string|false|none|

<h3 id="accepts-an-authentication-request-from-an-idp-and-authenticates-the-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authentication success|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Rate limit exceeded|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|429|Retry-After|integer||Number of seconds to wait before retrying the request|

<aside class="success">
This operation does not require authentication
</aside>

## API call that the IDP should use for an authentication request

<a id="opIdcreateAuthSsoCallback"></a>

> Code samples

`POST /auth/sso/callback`

> Body parameter

```yaml
SAMLResponse: string
RelayState: string

```

<h3 id="api-call-that-the-idp-should-use-for-an-authentication-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|true|Authentication request object|
|» SAMLResponse|body|string|false|Authentication request object|
|» RelayState|body|string|false|none|

<h3 id="api-call-that-the-idp-should-use-for-an-authentication-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authentication success|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Rate limit exceeded|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|429|Retry-After|integer||Number of seconds to wait before retrying the request|

<aside class="success">
This operation does not require authentication
</aside>

## Redirect user to IDP with logout request

<a id="opIdgetAuthSlo"></a>

> Code samples

`GET /auth/slo`

<h3 id="redirect-user-to-idp-with-logout-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|SLO redirect info|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

## Accepts a logout request from an IDP and logs out the user

<a id="opIdgetAuthSloCallback"></a>

> Code samples

`GET /auth/slo/callback`

<h3 id="accepts-a-logout-request-from-an-idp-and-logs-out-the-user-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|SAMLResponse|query|string|false|Logout request object|
|RelayState|query|string|false|none|

<h3 id="accepts-a-logout-request-from-an-idp-and-logs-out-the-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Logout success|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Rate limit exceeded|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|429|Retry-After|integer||Number of seconds to wait before retrying the request|

<aside class="success">
This operation does not require authentication
</aside>

## API call that the IDP should use for a logout request

<a id="opIdcreateAuthSloCallback"></a>

> Code samples

`POST /auth/slo/callback`

> Body parameter

```yaml
SAMLResponse: string
RelayState: string

```

<h3 id="api-call-that-the-idp-should-use-for-a-logout-request-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|true|Logout request object|
|» SAMLResponse|body|string|false|Logout request object|
|» RelayState|body|string|false|none|

<h3 id="api-call-that-the-idp-should-use-for-a-logout-request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Logout success|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Bad Request|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|429|[Too Many Requests](https://tools.ietf.org/html/rfc6585#section-4)|Rate limit exceeded|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|429|Retry-After|integer||Number of seconds to wait before retrying the request|

<aside class="success">
This operation does not require authentication
</aside>

## API call that the IDP should use for an authorization code callback

<a id="opIdgetAuthAuthorizationCodeCallback"></a>

> Code samples

`GET /auth/authorization-code/callback`

<h3 id="api-call-that-the-idp-should-use-for-an-authorization-code-callback-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|code|query|string|false|Authorization Code|
|state|query|string|false|none|

<h3 id="api-call-that-the-idp-should-use-for-an-authorization-code-callback-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Authorization success|None|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

## List the external authentication system's groups

<a id="opIdgetAuthGroups"></a>

> Code samples

`GET /auth/groups`

List the external authentication system's groups

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string"
    }
  ]
}
```

<h3 id="list-the-external-authentication-system's-groups-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CrudEntityBase objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-the-external-authentication-system's-groups-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CrudEntityBase](#schemacrudentitybase)]|false|none|none|
|»» id|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Obtain metadata which Cribl Stream/Edge uses when acting as a Service Provider

<a id="opIdgetAuthMetadata"></a>

> Code samples

`GET /auth/metadata`

> Example responses

> 200 Response

<h3 id="obtain-metadata-which-cribl-stream/edge-uses-when-acting-as-a-service-provider-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Service Provider metadata|string|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_RedirectInfo">RedirectInfo</h2>
<!-- backwards compatibility -->
<a id="schemaredirectinfo"></a>
<a id="schema_RedirectInfo"></a>
<a id="tocSredirectinfo"></a>
<a id="tocsredirectinfo"></a>

```json
{
  "name": "string",
  "redirectUrl": "string",
  "token": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|redirectUrl|string|false|none|none|
|token|string|false|none|none|

<h2 id="tocS_CrudEntityBase">CrudEntityBase</h2>
<!-- backwards compatibility -->
<a id="schemacrudentitybase"></a>
<a id="schema_CrudEntityBase"></a>
<a id="tocScrudentitybase"></a>
<a id="tocscrudentitybase"></a>

```json
{
  "id": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|

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

