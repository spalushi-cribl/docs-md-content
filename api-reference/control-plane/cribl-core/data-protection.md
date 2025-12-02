
<h1 id="cribl-core-api-data-protection">Cribl Core API - Data Protection v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-data-protection-data-protection">Data Protection</h1>

## Get a list of KeyMetadataEntity objects

<a id="opIdlistKeyMetadataEntity"></a>

> Code samples

`GET /system/keys`

Get a list of KeyMetadataEntity objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "keyId": "string",
      "description": "string",
      "algorithm": "aes-256-cbc",
      "kms": "local",
      "keyclass": 0,
      "created": 0,
      "expires": 0,
      "plainKey": "string",
      "cipherKey": "string",
      "useIV": false,
      "ivSize": 12,
      "group": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-keymetadataentity-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KeyMetadataEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-keymetadataentity-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KeyMetadataEntity](#schemakeymetadataentity)]|false|none|none|
|»» keyId|string|true|none|none|
|»» description|string|false|none|none|
|»» algorithm|string|true|none|none|
|»» kms|string|true|none|none|
|»» keyclass|number|true|none|none|
|»» created|number|false|none|none|
|»» expires|number|false|none|none|
|»» plainKey|string|false|none|none|
|»» cipherKey|string|false|none|none|
|»» useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|»» ivSize|integer|false|none|Length of the initialization vector, in bytes|
|»» group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

<aside class="success">
This operation does not require authentication
</aside>

## Create KeyMetadataEntity

<a id="opIdcreateKeyMetadataEntity"></a>

> Code samples

`POST /system/keys`

Create KeyMetadataEntity

> Body parameter

```json
{
  "keyId": "string",
  "description": "string",
  "algorithm": "aes-256-cbc",
  "kms": "local",
  "keyclass": 0,
  "created": 0,
  "expires": 0,
  "plainKey": "string",
  "cipherKey": "string",
  "useIV": false,
  "ivSize": 12,
  "group": "string"
}
```

<h3 id="create-keymetadataentity-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[KeyMetadataEntity](#schemakeymetadataentity)|true|New KeyMetadataEntity object|
|» keyId|body|string|true|none|
|» description|body|string|false|none|
|» algorithm|body|string|true|none|
|» kms|body|string|true|none|
|» keyclass|body|number|true|none|
|» created|body|number|false|none|
|» expires|body|number|false|none|
|» plainKey|body|string|false|none|
|» cipherKey|body|string|false|none|
|» useIV|body|boolean|false|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|» ivSize|body|integer|false|Length of the initialization vector, in bytes|
|» group|body|string|false|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» algorithm|aes-256-cbc|
|» algorithm|aes-256-gcm|
|» kms|local|
|» ivSize|12|
|» ivSize|13|
|» ivSize|14|
|» ivSize|15|
|» ivSize|16|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "keyId": "string",
      "description": "string",
      "algorithm": "aes-256-cbc",
      "kms": "local",
      "keyclass": 0,
      "created": 0,
      "expires": 0,
      "plainKey": "string",
      "cipherKey": "string",
      "useIV": false,
      "ivSize": 12,
      "group": "string"
    }
  ]
}
```

<h3 id="create-keymetadataentity-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KeyMetadataEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-keymetadataentity-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KeyMetadataEntity](#schemakeymetadataentity)]|false|none|none|
|»» keyId|string|true|none|none|
|»» description|string|false|none|none|
|»» algorithm|string|true|none|none|
|»» kms|string|true|none|none|
|»» keyclass|number|true|none|none|
|»» created|number|false|none|none|
|»» expires|number|false|none|none|
|»» plainKey|string|false|none|none|
|»» cipherKey|string|false|none|none|
|»» useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|»» ivSize|integer|false|none|Length of the initialization vector, in bytes|
|»» group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

<aside class="success">
This operation does not require authentication
</aside>

## Delete KeyMetadataEntity

<a id="opIddeleteKeyMetadataEntityById"></a>

> Code samples

`DELETE /system/keys/{id}`

Delete KeyMetadataEntity

<h3 id="delete-keymetadataentity-parameters">Parameters</h3>

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
      "keyId": "string",
      "description": "string",
      "algorithm": "aes-256-cbc",
      "kms": "local",
      "keyclass": 0,
      "created": 0,
      "expires": 0,
      "plainKey": "string",
      "cipherKey": "string",
      "useIV": false,
      "ivSize": 12,
      "group": "string"
    }
  ]
}
```

<h3 id="delete-keymetadataentity-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KeyMetadataEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-keymetadataentity-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KeyMetadataEntity](#schemakeymetadataentity)]|false|none|none|
|»» keyId|string|true|none|none|
|»» description|string|false|none|none|
|»» algorithm|string|true|none|none|
|»» kms|string|true|none|none|
|»» keyclass|number|true|none|none|
|»» created|number|false|none|none|
|»» expires|number|false|none|none|
|»» plainKey|string|false|none|none|
|»» cipherKey|string|false|none|none|
|»» useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|»» ivSize|integer|false|none|Length of the initialization vector, in bytes|
|»» group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

<aside class="success">
This operation does not require authentication
</aside>

## Get KeyMetadataEntity by ID

<a id="opIdgetKeyMetadataEntityById"></a>

> Code samples

`GET /system/keys/{id}`

Get KeyMetadataEntity by ID

<h3 id="get-keymetadataentity-by-id-parameters">Parameters</h3>

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
      "keyId": "string",
      "description": "string",
      "algorithm": "aes-256-cbc",
      "kms": "local",
      "keyclass": 0,
      "created": 0,
      "expires": 0,
      "plainKey": "string",
      "cipherKey": "string",
      "useIV": false,
      "ivSize": 12,
      "group": "string"
    }
  ]
}
```

<h3 id="get-keymetadataentity-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KeyMetadataEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-keymetadataentity-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KeyMetadataEntity](#schemakeymetadataentity)]|false|none|none|
|»» keyId|string|true|none|none|
|»» description|string|false|none|none|
|»» algorithm|string|true|none|none|
|»» kms|string|true|none|none|
|»» keyclass|number|true|none|none|
|»» created|number|false|none|none|
|»» expires|number|false|none|none|
|»» plainKey|string|false|none|none|
|»» cipherKey|string|false|none|none|
|»» useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|»» ivSize|integer|false|none|Length of the initialization vector, in bytes|
|»» group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

<aside class="success">
This operation does not require authentication
</aside>

## Update KeyMetadataEntity

<a id="opIdupdateKeyMetadataEntityById"></a>

> Code samples

`PATCH /system/keys/{id}`

Update KeyMetadataEntity

> Body parameter

```json
{
  "keyId": "string",
  "description": "string",
  "algorithm": "aes-256-cbc",
  "kms": "local",
  "keyclass": 0,
  "created": 0,
  "expires": 0,
  "plainKey": "string",
  "cipherKey": "string",
  "useIV": false,
  "ivSize": 12,
  "group": "string"
}
```

<h3 id="update-keymetadataentity-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[KeyMetadataEntity](#schemakeymetadataentity)|true|KeyMetadataEntity object to be updated|
|» keyId|body|string|true|none|
|» description|body|string|false|none|
|» algorithm|body|string|true|none|
|» kms|body|string|true|none|
|» keyclass|body|number|true|none|
|» created|body|number|false|none|
|» expires|body|number|false|none|
|» plainKey|body|string|false|none|
|» cipherKey|body|string|false|none|
|» useIV|body|boolean|false|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|» ivSize|body|integer|false|Length of the initialization vector, in bytes|
|» group|body|string|false|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» algorithm|aes-256-cbc|
|» algorithm|aes-256-gcm|
|» kms|local|
|» ivSize|12|
|» ivSize|13|
|» ivSize|14|
|» ivSize|15|
|» ivSize|16|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "keyId": "string",
      "description": "string",
      "algorithm": "aes-256-cbc",
      "kms": "local",
      "keyclass": 0,
      "created": 0,
      "expires": 0,
      "plainKey": "string",
      "cipherKey": "string",
      "useIV": false,
      "ivSize": 12,
      "group": "string"
    }
  ]
}
```

<h3 id="update-keymetadataentity-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KeyMetadataEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-keymetadataentity-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KeyMetadataEntity](#schemakeymetadataentity)]|false|none|none|
|»» keyId|string|true|none|none|
|»» description|string|false|none|none|
|»» algorithm|string|true|none|none|
|»» kms|string|true|none|none|
|»» keyclass|number|true|none|none|
|»» created|number|false|none|none|
|»» expires|number|false|none|none|
|»» plainKey|string|false|none|none|
|»» cipherKey|string|false|none|none|
|»» useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|»» ivSize|integer|false|none|Length of the initialization vector, in bytes|
|»» group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

<aside class="success">
This operation does not require authentication
</aside>

## Get Cribl KMS config

<a id="opIdgetSecurityKmsConfig"></a>

> Code samples

`GET /security/kms/config`

Get Cribl KMS config

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "auth": {
        "assumeRoleArn": "string",
        "assumeRoleExternalId": "string",
        "awsApiKey": "string",
        "awsAuthenticationMethod": "string",
        "awsSecretKey": "string",
        "enableAssumeRole": true,
        "provider": "token",
        "token": "string",
        "vaultAWSIAMServerID": "string",
        "vaultRole": "string"
      },
      "enableHealthCheck": true,
      "healthCheckEndpoint": "string",
      "namespace": "string",
      "provider": "local",
      "secretDir": "string",
      "service": {
        "kmsKeyArn": "string",
        "region": "string"
      },
      "tls": {
        "caPath": "string",
        "certPath": "string",
        "certificateName": "string",
        "disabled": true,
        "maxVersion": "TLSv1.3",
        "minVersion": "TLSv1.3",
        "passphrase": "string",
        "privKeyPath": "string",
        "rejectUnauthorized": true,
        "servername": "string"
      },
      "url": "string"
    }
  ]
}
```

<h3 id="get-cribl-kms-config-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KMSProviderConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-cribl-kms-config-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KMSProviderConfig](#schemakmsproviderconfig)]|false|none|none|
|»» auth|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» assumeRoleArn|string|false|none|none|
|»»»» assumeRoleExternalId|string|false|none|none|
|»»»» awsApiKey|string|false|none|none|
|»»»» awsAuthenticationMethod|string|false|none|none|
|»»»» awsSecretKey|string|false|none|none|
|»»»» enableAssumeRole|boolean|false|none|none|
|»»»» provider|[AUTH_PROVIDER](#schemaauth_provider)|true|none|none|
|»»»» token|string|false|none|none|
|»»»» vaultAWSIAMServerID|string|true|none|none|
|»»»» vaultRole|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» assumeRoleArn|string|false|none|none|
|»»»» assumeRoleExternalId|string|false|none|none|
|»»»» awsApiKey|string|false|none|none|
|»»»» awsAuthenticationMethod|string|false|none|none|
|»»»» awsSecretKey|string|false|none|none|
|»»»» enableAssumeRole|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» enableHealthCheck|boolean|true|none|none|
|»» healthCheckEndpoint|string|false|none|none|
|»» namespace|string|false|none|none|
|»» provider|[SECRET_PROVIDER](#schemasecret_provider)|true|none|none|
|»» secretDir|string|false|none|none|
|»» service|[AWSKMSServiceConfig](#schemaawskmsserviceconfig)|false|none|none|
|»»» kmsKeyArn|string|true|none|none|
|»»» region|string|true|none|none|
|»» tls|[VaultKMSTlsClientConfig](#schemavaultkmstlsclientconfig)|false|none|none|
|»»» caPath|string|false|none|none|
|»»» certPath|string|false|none|none|
|»»» certificateName|string|false|none|none|
|»»» disabled|boolean|true|none|none|
|»»» maxVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|»»» minVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|»»» passphrase|string|false|none|none|
|»»» privKeyPath|string|false|none|none|
|»»» rejectUnauthorized|boolean|false|none|none|
|»»» servername|string|false|none|none|
|»» url|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|provider|token|
|provider|aws-iam|
|provider|aws-ec2|
|provider|local|
|provider|vault|
|provider|aws-kms|
|maxVersion|TLSv1.3|
|maxVersion|TLSv1.2|
|maxVersion|TLSv1.1|
|maxVersion|TLSv1|
|minVersion|TLSv1.3|
|minVersion|TLSv1.2|
|minVersion|TLSv1.1|
|minVersion|TLSv1|

<aside class="success">
This operation does not require authentication
</aside>

## Update Cribl KMS config

<a id="opIdupdateSecurityKmsConfig"></a>

> Code samples

`PATCH /security/kms/config`

Update Cribl KMS config

> Body parameter

```json
{
  "auth": {
    "assumeRoleArn": "string",
    "assumeRoleExternalId": "string",
    "awsApiKey": "string",
    "awsAuthenticationMethod": "string",
    "awsSecretKey": "string",
    "enableAssumeRole": true,
    "provider": "token",
    "token": "string",
    "vaultAWSIAMServerID": "string",
    "vaultRole": "string"
  },
  "enableHealthCheck": true,
  "healthCheckEndpoint": "string",
  "namespace": "string",
  "provider": "local",
  "secretDir": "string",
  "service": {
    "kmsKeyArn": "string",
    "region": "string"
  },
  "tls": {
    "caPath": "string",
    "certPath": "string",
    "certificateName": "string",
    "disabled": true,
    "maxVersion": "TLSv1.3",
    "minVersion": "TLSv1.3",
    "passphrase": "string",
    "privKeyPath": "string",
    "rejectUnauthorized": true,
    "servername": "string"
  },
  "url": "string"
}
```

<h3 id="update-cribl-kms-config-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[KMSProviderConfig](#schemakmsproviderconfig)|true|KMSProviderConfig object|
|» auth|body|any|false|none|
|»» *anonymous*|body|object|false|none|
|»»» assumeRoleArn|body|string|false|none|
|»»» assumeRoleExternalId|body|string|false|none|
|»»» awsApiKey|body|string|false|none|
|»»» awsAuthenticationMethod|body|string|false|none|
|»»» awsSecretKey|body|string|false|none|
|»»» enableAssumeRole|body|boolean|false|none|
|»»» provider|body|[AUTH_PROVIDER](#schemaauth_provider)|true|none|
|»»» token|body|string|false|none|
|»»» vaultAWSIAMServerID|body|string|true|none|
|»»» vaultRole|body|string|false|none|
|»» *anonymous*|body|object|false|none|
|»»» assumeRoleArn|body|string|false|none|
|»»» assumeRoleExternalId|body|string|false|none|
|»»» awsApiKey|body|string|false|none|
|»»» awsAuthenticationMethod|body|string|false|none|
|»»» awsSecretKey|body|string|false|none|
|»»» enableAssumeRole|body|boolean|false|none|
|» enableHealthCheck|body|boolean|true|none|
|» healthCheckEndpoint|body|string|false|none|
|» namespace|body|string|false|none|
|» provider|body|[SECRET_PROVIDER](#schemasecret_provider)|true|none|
|» secretDir|body|string|false|none|
|» service|body|[AWSKMSServiceConfig](#schemaawskmsserviceconfig)|false|none|
|»» kmsKeyArn|body|string|true|none|
|»» region|body|string|true|none|
|» tls|body|[VaultKMSTlsClientConfig](#schemavaultkmstlsclientconfig)|false|none|
|»» caPath|body|string|false|none|
|»» certPath|body|string|false|none|
|»» certificateName|body|string|false|none|
|»» disabled|body|boolean|true|none|
|»» maxVersion|body|[SecureVersion](#schemasecureversion)|false|none|
|»» minVersion|body|[SecureVersion](#schemasecureversion)|false|none|
|»» passphrase|body|string|false|none|
|»» privKeyPath|body|string|false|none|
|»» rejectUnauthorized|body|boolean|false|none|
|»» servername|body|string|false|none|
|» url|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»» provider|token|
|»»» provider|aws-iam|
|»»» provider|aws-ec2|
|» provider|local|
|» provider|vault|
|» provider|aws-kms|
|»» maxVersion|TLSv1.3|
|»» maxVersion|TLSv1.2|
|»» maxVersion|TLSv1.1|
|»» maxVersion|TLSv1|
|»» minVersion|TLSv1.3|
|»» minVersion|TLSv1.2|
|»» minVersion|TLSv1.1|
|»» minVersion|TLSv1|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "auth": {
        "assumeRoleArn": "string",
        "assumeRoleExternalId": "string",
        "awsApiKey": "string",
        "awsAuthenticationMethod": "string",
        "awsSecretKey": "string",
        "enableAssumeRole": true,
        "provider": "token",
        "token": "string",
        "vaultAWSIAMServerID": "string",
        "vaultRole": "string"
      },
      "enableHealthCheck": true,
      "healthCheckEndpoint": "string",
      "namespace": "string",
      "provider": "local",
      "secretDir": "string",
      "service": {
        "kmsKeyArn": "string",
        "region": "string"
      },
      "tls": {
        "caPath": "string",
        "certPath": "string",
        "certificateName": "string",
        "disabled": true,
        "maxVersion": "TLSv1.3",
        "minVersion": "TLSv1.3",
        "passphrase": "string",
        "privKeyPath": "string",
        "rejectUnauthorized": true,
        "servername": "string"
      },
      "url": "string"
    }
  ]
}
```

<h3 id="update-cribl-kms-config-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KMSProviderConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-cribl-kms-config-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KMSProviderConfig](#schemakmsproviderconfig)]|false|none|none|
|»» auth|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» assumeRoleArn|string|false|none|none|
|»»»» assumeRoleExternalId|string|false|none|none|
|»»»» awsApiKey|string|false|none|none|
|»»»» awsAuthenticationMethod|string|false|none|none|
|»»»» awsSecretKey|string|false|none|none|
|»»»» enableAssumeRole|boolean|false|none|none|
|»»»» provider|[AUTH_PROVIDER](#schemaauth_provider)|true|none|none|
|»»»» token|string|false|none|none|
|»»»» vaultAWSIAMServerID|string|true|none|none|
|»»»» vaultRole|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|object|false|none|none|
|»»»» assumeRoleArn|string|false|none|none|
|»»»» assumeRoleExternalId|string|false|none|none|
|»»»» awsApiKey|string|false|none|none|
|»»»» awsAuthenticationMethod|string|false|none|none|
|»»»» awsSecretKey|string|false|none|none|
|»»»» enableAssumeRole|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» enableHealthCheck|boolean|true|none|none|
|»» healthCheckEndpoint|string|false|none|none|
|»» namespace|string|false|none|none|
|»» provider|[SECRET_PROVIDER](#schemasecret_provider)|true|none|none|
|»» secretDir|string|false|none|none|
|»» service|[AWSKMSServiceConfig](#schemaawskmsserviceconfig)|false|none|none|
|»»» kmsKeyArn|string|true|none|none|
|»»» region|string|true|none|none|
|»» tls|[VaultKMSTlsClientConfig](#schemavaultkmstlsclientconfig)|false|none|none|
|»»» caPath|string|false|none|none|
|»»» certPath|string|false|none|none|
|»»» certificateName|string|false|none|none|
|»»» disabled|boolean|true|none|none|
|»»» maxVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|»»» minVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|»»» passphrase|string|false|none|none|
|»»» privKeyPath|string|false|none|none|
|»»» rejectUnauthorized|boolean|false|none|none|
|»»» servername|string|false|none|none|
|»» url|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|provider|token|
|provider|aws-iam|
|provider|aws-ec2|
|provider|local|
|provider|vault|
|provider|aws-kms|
|maxVersion|TLSv1.3|
|maxVersion|TLSv1.2|
|maxVersion|TLSv1.1|
|maxVersion|TLSv1|
|minVersion|TLSv1.3|
|minVersion|TLSv1.2|
|minVersion|TLSv1.1|
|minVersion|TLSv1|

<aside class="success">
This operation does not require authentication
</aside>

## Get Cribl KMS health

<a id="opIdgetSecurityKmsHealth"></a>

> Code samples

`GET /security/kms/health`

Get Cribl KMS health

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "auth": {
        "details": {},
        "error": {
          "name": "string",
          "message": "string",
          "stack": "string"
        },
        "status": 0
      },
      "connection": {
        "details": {},
        "error": {
          "name": "string",
          "message": "string",
          "stack": "string"
        },
        "status": 0
      },
      "system": {
        "details": {},
        "error": {
          "name": "string",
          "message": "string",
          "stack": "string"
        },
        "status": 0
      }
    }
  ]
}
```

<h3 id="get-cribl-kms-health-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of KMSHealth objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-cribl-kms-health-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[KMSHealth](#schemakmshealth)]|false|none|none|
|»» auth|[KMSHealthTest](#schemakmshealthtest)|true|none|none|
|»»» details|object|false|none|none|
|»»» error|object|false|none|none|
|»»»» name|string|true|none|none|
|»»»» message|string|true|none|none|
|»»»» stack|string|false|none|none|
|»»» status|[KMSHealthStatus](#schemakmshealthstatus)|true|none|none|
|»» connection|[KMSHealthTest](#schemakmshealthtest)|true|none|none|
|»» system|[KMSHealthTest](#schemakmshealthtest)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|0|
|status|1|
|status|2|
|status|3|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_KeyMetadataEntity">KeyMetadataEntity</h2>
<!-- backwards compatibility -->
<a id="schemakeymetadataentity"></a>
<a id="schema_KeyMetadataEntity"></a>
<a id="tocSkeymetadataentity"></a>
<a id="tocskeymetadataentity"></a>

```json
{
  "keyId": "string",
  "description": "string",
  "algorithm": "aes-256-cbc",
  "kms": "local",
  "keyclass": 0,
  "created": 0,
  "expires": 0,
  "plainKey": "string",
  "cipherKey": "string",
  "useIV": false,
  "ivSize": 12,
  "group": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|keyId|string|true|none|none|
|description|string|false|none|none|
|algorithm|string|true|none|none|
|kms|string|true|none|none|
|keyclass|number|true|none|none|
|created|number|false|none|none|
|expires|number|false|none|none|
|plainKey|string|false|none|none|
|cipherKey|string|false|none|none|
|useIV|boolean|false|none|Seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Must be enabled with the aes-256-gcm algorithm.|
|ivSize|integer|false|none|Length of the initialization vector, in bytes|
|group|string|false|none|Name of the Worker Group/Fleet that created this key|

#### Enumerated Values

|Property|Value|
|---|---|
|algorithm|aes-256-cbc|
|algorithm|aes-256-gcm|
|kms|local|
|ivSize|12|
|ivSize|13|
|ivSize|14|
|ivSize|15|
|ivSize|16|

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

<h2 id="tocS_KMSProviderConfig">KMSProviderConfig</h2>
<!-- backwards compatibility -->
<a id="schemakmsproviderconfig"></a>
<a id="schema_KMSProviderConfig"></a>
<a id="tocSkmsproviderconfig"></a>
<a id="tocskmsproviderconfig"></a>

```json
{
  "auth": {
    "assumeRoleArn": "string",
    "assumeRoleExternalId": "string",
    "awsApiKey": "string",
    "awsAuthenticationMethod": "string",
    "awsSecretKey": "string",
    "enableAssumeRole": true,
    "provider": "token",
    "token": "string",
    "vaultAWSIAMServerID": "string",
    "vaultRole": "string"
  },
  "enableHealthCheck": true,
  "healthCheckEndpoint": "string",
  "namespace": "string",
  "provider": "local",
  "secretDir": "string",
  "service": {
    "kmsKeyArn": "string",
    "region": "string"
  },
  "tls": {
    "caPath": "string",
    "certPath": "string",
    "certificateName": "string",
    "disabled": true,
    "maxVersion": "TLSv1.3",
    "minVersion": "TLSv1.3",
    "passphrase": "string",
    "privKeyPath": "string",
    "rejectUnauthorized": true,
    "servername": "string"
  },
  "url": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|auth|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» assumeRoleArn|string|false|none|none|
|»» assumeRoleExternalId|string|false|none|none|
|»» awsApiKey|string|false|none|none|
|»» awsAuthenticationMethod|string|false|none|none|
|»» awsSecretKey|string|false|none|none|
|»» enableAssumeRole|boolean|false|none|none|
|»» provider|[AUTH_PROVIDER](#schemaauth_provider)|true|none|none|
|»» token|string|false|none|none|
|»» vaultAWSIAMServerID|string|true|none|none|
|»» vaultRole|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|object|false|none|none|
|»» assumeRoleArn|string|false|none|none|
|»» assumeRoleExternalId|string|false|none|none|
|»» awsApiKey|string|false|none|none|
|»» awsAuthenticationMethod|string|false|none|none|
|»» awsSecretKey|string|false|none|none|
|»» enableAssumeRole|boolean|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|enableHealthCheck|boolean|true|none|none|
|healthCheckEndpoint|string|false|none|none|
|namespace|string|false|none|none|
|provider|[SECRET_PROVIDER](#schemasecret_provider)|true|none|none|
|secretDir|string|false|none|none|
|service|[AWSKMSServiceConfig](#schemaawskmsserviceconfig)|false|none|none|
|tls|[VaultKMSTlsClientConfig](#schemavaultkmstlsclientconfig)|false|none|none|
|url|string|false|none|none|

<h2 id="tocS_KMSHealth">KMSHealth</h2>
<!-- backwards compatibility -->
<a id="schemakmshealth"></a>
<a id="schema_KMSHealth"></a>
<a id="tocSkmshealth"></a>
<a id="tocskmshealth"></a>

```json
{
  "auth": {
    "details": {},
    "error": {
      "name": "string",
      "message": "string",
      "stack": "string"
    },
    "status": 0
  },
  "connection": {
    "details": {},
    "error": {
      "name": "string",
      "message": "string",
      "stack": "string"
    },
    "status": 0
  },
  "system": {
    "details": {},
    "error": {
      "name": "string",
      "message": "string",
      "stack": "string"
    },
    "status": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|auth|[KMSHealthTest](#schemakmshealthtest)|true|none|none|
|connection|[KMSHealthTest](#schemakmshealthtest)|true|none|none|
|system|[KMSHealthTest](#schemakmshealthtest)|true|none|none|

<h2 id="tocS_AUTH_PROVIDER">AUTH_PROVIDER</h2>
<!-- backwards compatibility -->
<a id="schemaauth_provider"></a>
<a id="schema_AUTH_PROVIDER"></a>
<a id="tocSauth_provider"></a>
<a id="tocsauth_provider"></a>

```json
"token"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|token|
|*anonymous*|aws-iam|
|*anonymous*|aws-ec2|

<h2 id="tocS_SECRET_PROVIDER">SECRET_PROVIDER</h2>
<!-- backwards compatibility -->
<a id="schemasecret_provider"></a>
<a id="schema_SECRET_PROVIDER"></a>
<a id="tocSsecret_provider"></a>
<a id="tocssecret_provider"></a>

```json
"local"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|local|
|*anonymous*|vault|
|*anonymous*|aws-kms|

<h2 id="tocS_AWSKMSServiceConfig">AWSKMSServiceConfig</h2>
<!-- backwards compatibility -->
<a id="schemaawskmsserviceconfig"></a>
<a id="schema_AWSKMSServiceConfig"></a>
<a id="tocSawskmsserviceconfig"></a>
<a id="tocsawskmsserviceconfig"></a>

```json
{
  "kmsKeyArn": "string",
  "region": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|kmsKeyArn|string|true|none|none|
|region|string|true|none|none|

<h2 id="tocS_VaultKMSTlsClientConfig">VaultKMSTlsClientConfig</h2>
<!-- backwards compatibility -->
<a id="schemavaultkmstlsclientconfig"></a>
<a id="schema_VaultKMSTlsClientConfig"></a>
<a id="tocSvaultkmstlsclientconfig"></a>
<a id="tocsvaultkmstlsclientconfig"></a>

```json
{
  "caPath": "string",
  "certPath": "string",
  "certificateName": "string",
  "disabled": true,
  "maxVersion": "TLSv1.3",
  "minVersion": "TLSv1.3",
  "passphrase": "string",
  "privKeyPath": "string",
  "rejectUnauthorized": true,
  "servername": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|caPath|string|false|none|none|
|certPath|string|false|none|none|
|certificateName|string|false|none|none|
|disabled|boolean|true|none|none|
|maxVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|minVersion|[SecureVersion](#schemasecureversion)|false|none|none|
|passphrase|string|false|none|none|
|privKeyPath|string|false|none|none|
|rejectUnauthorized|boolean|false|none|none|
|servername|string|false|none|none|

<h2 id="tocS_KMSHealthTest">KMSHealthTest</h2>
<!-- backwards compatibility -->
<a id="schemakmshealthtest"></a>
<a id="schema_KMSHealthTest"></a>
<a id="tocSkmshealthtest"></a>
<a id="tocskmshealthtest"></a>

```json
{
  "details": {},
  "error": {
    "name": "string",
    "message": "string",
    "stack": "string"
  },
  "status": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|details|object|false|none|none|
|error|object|false|none|none|
|» name|string|true|none|none|
|» message|string|true|none|none|
|» stack|string|false|none|none|
|status|[KMSHealthStatus](#schemakmshealthstatus)|true|none|none|

<h2 id="tocS_SecureVersion">SecureVersion</h2>
<!-- backwards compatibility -->
<a id="schemasecureversion"></a>
<a id="schema_SecureVersion"></a>
<a id="tocSsecureversion"></a>
<a id="tocssecureversion"></a>

```json
"TLSv1.3"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|TLSv1.3|
|*anonymous*|TLSv1.2|
|*anonymous*|TLSv1.1|
|*anonymous*|TLSv1|

<h2 id="tocS_KMSHealthStatus">KMSHealthStatus</h2>
<!-- backwards compatibility -->
<a id="schemakmshealthstatus"></a>
<a id="schema_KMSHealthStatus"></a>
<a id="tocSkmshealthstatus"></a>
<a id="tocskmshealthstatus"></a>

```json
0

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|integer|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|0|
|*anonymous*|1|
|*anonymous*|2|
|*anonymous*|3|

