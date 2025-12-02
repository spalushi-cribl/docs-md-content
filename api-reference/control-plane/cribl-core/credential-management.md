
<h1 id="cribl-core-api-credential-management">Cribl Core API - Credential Management v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-credential-management-credential-management">Credential Management</h1>

## Get a list of RestSecret objects

<a id="opIdlistRestSecret"></a>

> Code samples

`GET /system/secrets`

Get a list of RestSecret objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "apiKey": "string",
      "description": "string",
      "id": "string",
      "password": "string",
      "secretKey": "string",
      "secretType": "text",
      "tags": "string",
      "username": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-restsecret-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestSecret objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-restsecret-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestSecret](#schemarestsecret)]|false|none|none|
|»» apiKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» secretKey|string|false|none|none|
|»» secretType|[SecretType](#schemasecrettype)|true|none|none|
|»» tags|string|false|none|none|
|»» username|string|false|none|none|
|»» value|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|secretType|text|
|secretType|keypair|
|secretType|credentials|

<aside class="success">
This operation does not require authentication
</aside>

## Create RestSecret

<a id="opIdcreateRestSecret"></a>

> Code samples

`POST /system/secrets`

Create RestSecret

> Body parameter

```json
{
  "apiKey": "string",
  "description": "string",
  "id": "string",
  "password": "string",
  "secretKey": "string",
  "secretType": "text",
  "tags": "string",
  "username": "string",
  "value": "string"
}
```

<h3 id="create-restsecret-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[RestSecret](#schemarestsecret)|true|New RestSecret object|
|» apiKey|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» password|body|string|false|none|
|» secretKey|body|string|false|none|
|» secretType|body|[SecretType](#schemasecrettype)|true|none|
|» tags|body|string|false|none|
|» username|body|string|false|none|
|» value|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» secretType|text|
|» secretType|keypair|
|» secretType|credentials|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "apiKey": "string",
      "description": "string",
      "id": "string",
      "password": "string",
      "secretKey": "string",
      "secretType": "text",
      "tags": "string",
      "username": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="create-restsecret-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestSecret objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-restsecret-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestSecret](#schemarestsecret)]|false|none|none|
|»» apiKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» secretKey|string|false|none|none|
|»» secretType|[SecretType](#schemasecrettype)|true|none|none|
|»» tags|string|false|none|none|
|»» username|string|false|none|none|
|»» value|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|secretType|text|
|secretType|keypair|
|secretType|credentials|

<aside class="success">
This operation does not require authentication
</aside>

## Delete RestSecret

<a id="opIddeleteRestSecretById"></a>

> Code samples

`DELETE /system/secrets/{id}`

Delete RestSecret

<h3 id="delete-restsecret-parameters">Parameters</h3>

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
      "apiKey": "string",
      "description": "string",
      "id": "string",
      "password": "string",
      "secretKey": "string",
      "secretType": "text",
      "tags": "string",
      "username": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="delete-restsecret-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestSecret objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-restsecret-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestSecret](#schemarestsecret)]|false|none|none|
|»» apiKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» secretKey|string|false|none|none|
|»» secretType|[SecretType](#schemasecrettype)|true|none|none|
|»» tags|string|false|none|none|
|»» username|string|false|none|none|
|»» value|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|secretType|text|
|secretType|keypair|
|secretType|credentials|

<aside class="success">
This operation does not require authentication
</aside>

## Get RestSecret by ID

<a id="opIdgetRestSecretById"></a>

> Code samples

`GET /system/secrets/{id}`

Get RestSecret by ID

<h3 id="get-restsecret-by-id-parameters">Parameters</h3>

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
      "apiKey": "string",
      "description": "string",
      "id": "string",
      "password": "string",
      "secretKey": "string",
      "secretType": "text",
      "tags": "string",
      "username": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="get-restsecret-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestSecret objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-restsecret-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestSecret](#schemarestsecret)]|false|none|none|
|»» apiKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» secretKey|string|false|none|none|
|»» secretType|[SecretType](#schemasecrettype)|true|none|none|
|»» tags|string|false|none|none|
|»» username|string|false|none|none|
|»» value|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|secretType|text|
|secretType|keypair|
|secretType|credentials|

<aside class="success">
This operation does not require authentication
</aside>

## Update RestSecret

<a id="opIdupdateRestSecretById"></a>

> Code samples

`PATCH /system/secrets/{id}`

Update RestSecret

> Body parameter

```json
{
  "apiKey": "string",
  "description": "string",
  "id": "string",
  "password": "string",
  "secretKey": "string",
  "secretType": "text",
  "tags": "string",
  "username": "string",
  "value": "string"
}
```

<h3 id="update-restsecret-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[RestSecret](#schemarestsecret)|true|RestSecret object to be updated|
|» apiKey|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» password|body|string|false|none|
|» secretKey|body|string|false|none|
|» secretType|body|[SecretType](#schemasecrettype)|true|none|
|» tags|body|string|false|none|
|» username|body|string|false|none|
|» value|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» secretType|text|
|» secretType|keypair|
|» secretType|credentials|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "apiKey": "string",
      "description": "string",
      "id": "string",
      "password": "string",
      "secretKey": "string",
      "secretType": "text",
      "tags": "string",
      "username": "string",
      "value": "string"
    }
  ]
}
```

<h3 id="update-restsecret-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of RestSecret objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-restsecret-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[RestSecret](#schemarestsecret)]|false|none|none|
|»» apiKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» password|string|false|none|none|
|»» secretKey|string|false|none|none|
|»» secretType|[SecretType](#schemasecrettype)|true|none|none|
|»» tags|string|false|none|none|
|»» username|string|false|none|none|
|»» value|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|secretType|text|
|secretType|keypair|
|secretType|credentials|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_RestSecret">RestSecret</h2>
<!-- backwards compatibility -->
<a id="schemarestsecret"></a>
<a id="schema_RestSecret"></a>
<a id="tocSrestsecret"></a>
<a id="tocsrestsecret"></a>

```json
{
  "apiKey": "string",
  "description": "string",
  "id": "string",
  "password": "string",
  "secretKey": "string",
  "secretType": "text",
  "tags": "string",
  "username": "string",
  "value": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|apiKey|string|false|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|password|string|false|none|none|
|secretKey|string|false|none|none|
|secretType|[SecretType](#schemasecrettype)|true|none|none|
|tags|string|false|none|none|
|username|string|false|none|none|
|value|string|false|none|none|

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

<h2 id="tocS_SecretType">SecretType</h2>
<!-- backwards compatibility -->
<a id="schemasecrettype"></a>
<a id="schema_SecretType"></a>
<a id="tocSsecrettype"></a>
<a id="tocssecrettype"></a>

```json
"text"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|text|
|*anonymous*|keypair|
|*anonymous*|credentials|

