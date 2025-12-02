
<h1 id="cribl-core-api-identity-and-trust-management">Cribl Core API - Identity and Trust Management v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-identity-and-trust-management-identity-and-trust-management">Identity and Trust Management</h1>

## Get a list of Certificate objects

<a id="opIdlistCertificate"></a>

> Code samples

`GET /system/certificates`

Get a list of Certificate objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "cert": "string",
      "privKey": "string",
      "passphrase": "string",
      "ca": "string",
      "inUse": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-certificate-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Certificate objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-certificate-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Certificate](#schemacertificate)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|»» privKey|string|true|none|none|
|»» passphrase|string|false|none|none|
|»» ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|»» inUse|[string]|false|none|List of configurations that reference this certificate|

<aside class="success">
This operation does not require authentication
</aside>

## Create Certificate

<a id="opIdcreateCertificate"></a>

> Code samples

`POST /system/certificates`

Create Certificate

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "cert": "string",
  "privKey": "string",
  "passphrase": "string",
  "ca": "string",
  "inUse": [
    "string"
  ]
}
```

<h3 id="create-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[Certificate](#schemacertificate)|true|New Certificate object|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» cert|body|string|true|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|» privKey|body|string|true|none|
|» passphrase|body|string|false|none|
|» ca|body|string|false|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|» inUse|body|[string]|false|List of configurations that reference this certificate|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "cert": "string",
      "privKey": "string",
      "passphrase": "string",
      "ca": "string",
      "inUse": [
        "string"
      ]
    }
  ]
}
```

<h3 id="create-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Certificate objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-certificate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Certificate](#schemacertificate)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|»» privKey|string|true|none|none|
|»» passphrase|string|false|none|none|
|»» ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|»» inUse|[string]|false|none|List of configurations that reference this certificate|

<aside class="success">
This operation does not require authentication
</aside>

## Delete Certificate

<a id="opIddeleteCertificateById"></a>

> Code samples

`DELETE /system/certificates/{id}`

Delete Certificate

<h3 id="delete-certificate-parameters">Parameters</h3>

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
      "cert": "string",
      "privKey": "string",
      "passphrase": "string",
      "ca": "string",
      "inUse": [
        "string"
      ]
    }
  ]
}
```

<h3 id="delete-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Certificate objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-certificate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Certificate](#schemacertificate)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|»» privKey|string|true|none|none|
|»» passphrase|string|false|none|none|
|»» ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|»» inUse|[string]|false|none|List of configurations that reference this certificate|

<aside class="success">
This operation does not require authentication
</aside>

## Get Certificate by ID

<a id="opIdgetCertificateById"></a>

> Code samples

`GET /system/certificates/{id}`

Get Certificate by ID

<h3 id="get-certificate-by-id-parameters">Parameters</h3>

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
      "cert": "string",
      "privKey": "string",
      "passphrase": "string",
      "ca": "string",
      "inUse": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-certificate-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Certificate objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-certificate-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Certificate](#schemacertificate)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|»» privKey|string|true|none|none|
|»» passphrase|string|false|none|none|
|»» ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|»» inUse|[string]|false|none|List of configurations that reference this certificate|

<aside class="success">
This operation does not require authentication
</aside>

## Update Certificate

<a id="opIdupdateCertificateById"></a>

> Code samples

`PATCH /system/certificates/{id}`

Update Certificate

> Body parameter

```json
{
  "id": "string",
  "description": "string",
  "cert": "string",
  "privKey": "string",
  "passphrase": "string",
  "ca": "string",
  "inUse": [
    "string"
  ]
}
```

<h3 id="update-certificate-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[Certificate](#schemacertificate)|true|Certificate object to be updated|
|» id|body|string|true|none|
|» description|body|string|false|none|
|» cert|body|string|true|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|» privKey|body|string|true|none|
|» passphrase|body|string|false|none|
|» ca|body|string|false|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|» inUse|body|[string]|false|List of configurations that reference this certificate|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "description": "string",
      "cert": "string",
      "privKey": "string",
      "passphrase": "string",
      "ca": "string",
      "inUse": [
        "string"
      ]
    }
  ]
}
```

<h3 id="update-certificate-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of Certificate objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-certificate-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[Certificate](#schemacertificate)]|false|none|none|
|»» id|string|true|none|none|
|»» description|string|false|none|none|
|»» cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|»» privKey|string|true|none|none|
|»» passphrase|string|false|none|none|
|»» ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|»» inUse|[string]|false|none|List of configurations that reference this certificate|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_Certificate">Certificate</h2>
<!-- backwards compatibility -->
<a id="schemacertificate"></a>
<a id="schema_Certificate"></a>
<a id="tocScertificate"></a>
<a id="tocscertificate"></a>

```json
{
  "id": "string",
  "description": "string",
  "cert": "string",
  "privKey": "string",
  "passphrase": "string",
  "ca": "string",
  "inUse": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|description|string|false|none|none|
|cert|string|true|none|Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents here|
|privKey|string|true|none|none|
|passphrase|string|false|none|none|
|ca|string|false|none|Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste certificate contents here. Certificates can be used for client and/or server authentication.|
|inUse|[string]|false|none|List of configurations that reference this certificate|

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

