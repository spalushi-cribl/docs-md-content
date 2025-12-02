
<h1 id="cribl-lake-api-storage-locations">Cribl Lake API - Storage Locations v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-lake-api-storage-locations-storage-locations">Storage Locations</h1>

## Create a Storage Location in the specified Lake

<a id="opIdcreateCriblLakeStorageLocationByLakeId"></a>

> Code samples

`POST /products/lake/lakes/{lakeId}/storage-locations`

Create a Storage Location in the specified Lake

> Body parameter

```json
{
  "config": {
    "bucketName": "string",
    "encryption": "SSE-S3",
    "inventoryConfig": {
      "destinationBucketName": "string",
      "destinationPrefix": "string",
      "region": "string",
      "type": "s3-inventory"
    },
    "region": "string"
  },
  "credentials": {
    "apiKey": "string",
    "method": "manual",
    "roleToAssume": "string",
    "roleToAssumeExternalId": "string",
    "roleToAssumeHybrid": "string",
    "secretKey": "string"
  },
  "description": "string",
  "id": "string",
  "lastProvisionedMs": 0,
  "metricsLastGenerated": 0,
  "provider": "cribl_lake",
  "status": "provisioning"
}
```

<h3 id="create-a-storage-location-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Storage Locations|
|body|body|[CriblLakeStorageLocation](#schemacribllakestoragelocation)|true|CriblLakeStorageLocation object|
|» config|body|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|
|»» bucketName|body|string|true|none|
|»» encryption|body|string|false|none|
|»» inventoryConfig|body|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|
|»»» destinationBucketName|body|string|true|none|
|»»» destinationPrefix|body|string|true|none|
|»»» region|body|string|true|none|
|»»» type|body|string|true|none|
|»» region|body|string|true|none|
|» credentials|body|[Credentials](#schemacredentials)|true|none|
|»» apiKey|body|string|false|none|
|»» method|body|string|true|none|
|»» roleToAssume|body|string|false|none|
|»» roleToAssumeExternalId|body|string|false|none|
|»» roleToAssumeHybrid|body|string|false|none|
|»» secretKey|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» lastProvisionedMs|body|number|false|none|
|» metricsLastGenerated|body|number|false|none|
|» provider|body|string|true|none|
|» status|body|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» encryption|SSE-S3|
|»» encryption|SSE-KMS|
|»»» type|s3-inventory|
|»» method|manual|
|»» method|auto|
|»» method|auto_rpc|
|» provider|cribl_lake|
|» provider|aws-s3|
|» status|provisioning|
|» status|ready|
|» status|failed|
|» status|terminated|
|» status|delayed|
|» status|blocked|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "bucketName": "string",
        "encryption": "SSE-S3",
        "inventoryConfig": {
          "destinationBucketName": "string",
          "destinationPrefix": "string",
          "region": "string",
          "type": "s3-inventory"
        },
        "region": "string"
      },
      "credentials": {
        "apiKey": "string",
        "method": "manual",
        "roleToAssume": "string",
        "roleToAssumeExternalId": "string",
        "roleToAssumeHybrid": "string",
        "secretKey": "string"
      },
      "description": "string",
      "id": "string",
      "lastProvisionedMs": 0,
      "metricsLastGenerated": 0,
      "provider": "cribl_lake",
      "status": "provisioning"
    }
  ]
}
```

<h3 id="create-a-storage-location-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeStorageLocation objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-storage-location-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeStorageLocation](#schemacribllakestoragelocation)]|false|none|none|
|»» config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|»»» bucketName|string|true|none|none|
|»»» encryption|string|false|none|none|
|»»» inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|»»»» destinationBucketName|string|true|none|none|
|»»»» destinationPrefix|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» type|string|true|none|none|
|»»» region|string|true|none|none|
|»» credentials|[Credentials](#schemacredentials)|true|none|none|
|»»» apiKey|string|false|none|none|
|»»» method|string|true|none|none|
|»»» roleToAssume|string|false|none|none|
|»»» roleToAssumeExternalId|string|false|none|none|
|»»» roleToAssumeHybrid|string|false|none|none|
|»»» secretKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lastProvisionedMs|number|false|none|none|
|»» metricsLastGenerated|number|false|none|none|
|»» provider|string|true|none|none|
|»» status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|
|type|s3-inventory|
|method|manual|
|method|auto|
|method|auto_rpc|
|provider|cribl_lake|
|provider|aws-s3|
|status|provisioning|
|status|ready|
|status|failed|
|status|terminated|
|status|delayed|
|status|blocked|

<aside class="success">
This operation does not require authentication
</aside>

## Get the list of Storage Locations contained in the specified Lake

<a id="opIdgetCriblLakeStorageLocationByLakeId"></a>

> Code samples

`GET /products/lake/lakes/{lakeId}/storage-locations`

Get the list of Storage Locations contained in the specified Lake

<h3 id="get-the-list-of-storage-locations-contained-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Storage Locations|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "bucketName": "string",
        "encryption": "SSE-S3",
        "inventoryConfig": {
          "destinationBucketName": "string",
          "destinationPrefix": "string",
          "region": "string",
          "type": "s3-inventory"
        },
        "region": "string"
      },
      "credentials": {
        "apiKey": "string",
        "method": "manual",
        "roleToAssume": "string",
        "roleToAssumeExternalId": "string",
        "roleToAssumeHybrid": "string",
        "secretKey": "string"
      },
      "description": "string",
      "id": "string",
      "lastProvisionedMs": 0,
      "metricsLastGenerated": 0,
      "provider": "cribl_lake",
      "status": "provisioning"
    }
  ]
}
```

<h3 id="get-the-list-of-storage-locations-contained-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeStorageLocation objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-list-of-storage-locations-contained-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeStorageLocation](#schemacribllakestoragelocation)]|false|none|none|
|»» config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|»»» bucketName|string|true|none|none|
|»»» encryption|string|false|none|none|
|»»» inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|»»»» destinationBucketName|string|true|none|none|
|»»»» destinationPrefix|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» type|string|true|none|none|
|»»» region|string|true|none|none|
|»» credentials|[Credentials](#schemacredentials)|true|none|none|
|»»» apiKey|string|false|none|none|
|»»» method|string|true|none|none|
|»»» roleToAssume|string|false|none|none|
|»»» roleToAssumeExternalId|string|false|none|none|
|»»» roleToAssumeHybrid|string|false|none|none|
|»»» secretKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lastProvisionedMs|number|false|none|none|
|»» metricsLastGenerated|number|false|none|none|
|»» provider|string|true|none|none|
|»» status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|
|type|s3-inventory|
|method|manual|
|method|auto|
|method|auto_rpc|
|provider|cribl_lake|
|provider|aws-s3|
|status|provisioning|
|status|ready|
|status|failed|
|status|terminated|
|status|delayed|
|status|blocked|

<aside class="success">
This operation does not require authentication
</aside>

## Delete a Storage Location in the specified Lake

<a id="opIddeleteCriblLakeStorageLocationByLakeIdAndId"></a>

> Code samples

`DELETE /products/lake/lakes/{lakeId}/storage-locations/{id}`

Delete a Storage Location in the specified Lake

<h3 id="delete-a-storage-location-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Storage Locations|
|id|path|string|true|storage location id to delete|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "bucketName": "string",
        "encryption": "SSE-S3",
        "inventoryConfig": {
          "destinationBucketName": "string",
          "destinationPrefix": "string",
          "region": "string",
          "type": "s3-inventory"
        },
        "region": "string"
      },
      "credentials": {
        "apiKey": "string",
        "method": "manual",
        "roleToAssume": "string",
        "roleToAssumeExternalId": "string",
        "roleToAssumeHybrid": "string",
        "secretKey": "string"
      },
      "description": "string",
      "id": "string",
      "lastProvisionedMs": 0,
      "metricsLastGenerated": 0,
      "provider": "cribl_lake",
      "status": "provisioning"
    }
  ]
}
```

<h3 id="delete-a-storage-location-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeStorageLocation objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-a-storage-location-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeStorageLocation](#schemacribllakestoragelocation)]|false|none|none|
|»» config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|»»» bucketName|string|true|none|none|
|»»» encryption|string|false|none|none|
|»»» inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|»»»» destinationBucketName|string|true|none|none|
|»»»» destinationPrefix|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» type|string|true|none|none|
|»»» region|string|true|none|none|
|»» credentials|[Credentials](#schemacredentials)|true|none|none|
|»»» apiKey|string|false|none|none|
|»»» method|string|true|none|none|
|»»» roleToAssume|string|false|none|none|
|»»» roleToAssumeExternalId|string|false|none|none|
|»»» roleToAssumeHybrid|string|false|none|none|
|»»» secretKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lastProvisionedMs|number|false|none|none|
|»» metricsLastGenerated|number|false|none|none|
|»» provider|string|true|none|none|
|»» status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|
|type|s3-inventory|
|method|manual|
|method|auto|
|method|auto_rpc|
|provider|cribl_lake|
|provider|aws-s3|
|status|provisioning|
|status|ready|
|status|failed|
|status|terminated|
|status|delayed|
|status|blocked|

<aside class="success">
This operation does not require authentication
</aside>

## Get a Storage Location in the specified Lake

<a id="opIdgetCriblLakeStorageLocationByLakeIdAndId"></a>

> Code samples

`GET /products/lake/lakes/{lakeId}/storage-locations/{id}`

Get a Storage Location in the specified Lake

<h3 id="get-a-storage-location-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Storage Locations|
|id|path|string|true|storage location id to get|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "bucketName": "string",
        "encryption": "SSE-S3",
        "inventoryConfig": {
          "destinationBucketName": "string",
          "destinationPrefix": "string",
          "region": "string",
          "type": "s3-inventory"
        },
        "region": "string"
      },
      "credentials": {
        "apiKey": "string",
        "method": "manual",
        "roleToAssume": "string",
        "roleToAssumeExternalId": "string",
        "roleToAssumeHybrid": "string",
        "secretKey": "string"
      },
      "description": "string",
      "id": "string",
      "lastProvisionedMs": 0,
      "metricsLastGenerated": 0,
      "provider": "cribl_lake",
      "status": "provisioning"
    }
  ]
}
```

<h3 id="get-a-storage-location-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeStorageLocation objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-storage-location-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeStorageLocation](#schemacribllakestoragelocation)]|false|none|none|
|»» config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|»»» bucketName|string|true|none|none|
|»»» encryption|string|false|none|none|
|»»» inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|»»»» destinationBucketName|string|true|none|none|
|»»»» destinationPrefix|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» type|string|true|none|none|
|»»» region|string|true|none|none|
|»» credentials|[Credentials](#schemacredentials)|true|none|none|
|»»» apiKey|string|false|none|none|
|»»» method|string|true|none|none|
|»»» roleToAssume|string|false|none|none|
|»»» roleToAssumeExternalId|string|false|none|none|
|»»» roleToAssumeHybrid|string|false|none|none|
|»»» secretKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lastProvisionedMs|number|false|none|none|
|»» metricsLastGenerated|number|false|none|none|
|»» provider|string|true|none|none|
|»» status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|
|type|s3-inventory|
|method|manual|
|method|auto|
|method|auto_rpc|
|provider|cribl_lake|
|provider|aws-s3|
|status|provisioning|
|status|ready|
|status|failed|
|status|terminated|
|status|delayed|
|status|blocked|

<aside class="success">
This operation does not require authentication
</aside>

## Update a Storage Location in the specified Lake

<a id="opIdupdateCriblLakeStorageLocationByLakeIdAndId"></a>

> Code samples

`PATCH /products/lake/lakes/{lakeId}/storage-locations/{id}`

Update a Storage Location in the specified Lake

> Body parameter

```json
{
  "config": {
    "bucketName": "string",
    "encryption": "SSE-S3",
    "inventoryConfig": {
      "destinationBucketName": "string",
      "destinationPrefix": "string",
      "region": "string",
      "type": "s3-inventory"
    },
    "region": "string"
  },
  "credentials": {
    "apiKey": "string",
    "method": "manual",
    "roleToAssume": "string",
    "roleToAssumeExternalId": "string",
    "roleToAssumeHybrid": "string",
    "secretKey": "string"
  },
  "description": "string",
  "id": "string",
  "lastProvisionedMs": 0,
  "metricsLastGenerated": 0,
  "provider": "cribl_lake",
  "status": "provisioning"
}
```

<h3 id="update-a-storage-location-in-the-specified-lake-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|lakeId|path|string|true|lake id that contains the Storage Locations|
|id|path|string|true|storage location id to update|
|body|body|[CriblLakeStorageLocation](#schemacribllakestoragelocation)|true|CriblLakeStorageLocation object|
|» config|body|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|
|»» bucketName|body|string|true|none|
|»» encryption|body|string|false|none|
|»» inventoryConfig|body|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|
|»»» destinationBucketName|body|string|true|none|
|»»» destinationPrefix|body|string|true|none|
|»»» region|body|string|true|none|
|»»» type|body|string|true|none|
|»» region|body|string|true|none|
|» credentials|body|[Credentials](#schemacredentials)|true|none|
|»» apiKey|body|string|false|none|
|»» method|body|string|true|none|
|»» roleToAssume|body|string|false|none|
|»» roleToAssumeExternalId|body|string|false|none|
|»» roleToAssumeHybrid|body|string|false|none|
|»» secretKey|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» lastProvisionedMs|body|number|false|none|
|» metricsLastGenerated|body|number|false|none|
|» provider|body|string|true|none|
|» status|body|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» encryption|SSE-S3|
|»» encryption|SSE-KMS|
|»»» type|s3-inventory|
|»» method|manual|
|»» method|auto|
|»» method|auto_rpc|
|» provider|cribl_lake|
|» provider|aws-s3|
|» status|provisioning|
|» status|ready|
|» status|failed|
|» status|terminated|
|» status|delayed|
|» status|blocked|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "bucketName": "string",
        "encryption": "SSE-S3",
        "inventoryConfig": {
          "destinationBucketName": "string",
          "destinationPrefix": "string",
          "region": "string",
          "type": "s3-inventory"
        },
        "region": "string"
      },
      "credentials": {
        "apiKey": "string",
        "method": "manual",
        "roleToAssume": "string",
        "roleToAssumeExternalId": "string",
        "roleToAssumeHybrid": "string",
        "secretKey": "string"
      },
      "description": "string",
      "id": "string",
      "lastProvisionedMs": 0,
      "metricsLastGenerated": 0,
      "provider": "cribl_lake",
      "status": "provisioning"
    }
  ]
}
```

<h3 id="update-a-storage-location-in-the-specified-lake-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CriblLakeStorageLocation objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-a-storage-location-in-the-specified-lake-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CriblLakeStorageLocation](#schemacribllakestoragelocation)]|false|none|none|
|»» config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|»»» bucketName|string|true|none|none|
|»»» encryption|string|false|none|none|
|»»» inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|»»»» destinationBucketName|string|true|none|none|
|»»»» destinationPrefix|string|true|none|none|
|»»»» region|string|true|none|none|
|»»»» type|string|true|none|none|
|»»» region|string|true|none|none|
|»» credentials|[Credentials](#schemacredentials)|true|none|none|
|»»» apiKey|string|false|none|none|
|»»» method|string|true|none|none|
|»»» roleToAssume|string|false|none|none|
|»»» roleToAssumeExternalId|string|false|none|none|
|»»» roleToAssumeHybrid|string|false|none|none|
|»»» secretKey|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lastProvisionedMs|number|false|none|none|
|»» metricsLastGenerated|number|false|none|none|
|»» provider|string|true|none|none|
|»» status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|
|type|s3-inventory|
|method|manual|
|method|auto|
|method|auto_rpc|
|provider|cribl_lake|
|provider|aws-s3|
|status|provisioning|
|status|ready|
|status|failed|
|status|terminated|
|status|delayed|
|status|blocked|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_CriblLakeStorageLocation">CriblLakeStorageLocation</h2>
<!-- backwards compatibility -->
<a id="schemacribllakestoragelocation"></a>
<a id="schema_CriblLakeStorageLocation"></a>
<a id="tocScribllakestoragelocation"></a>
<a id="tocscribllakestoragelocation"></a>

```json
{
  "config": {
    "bucketName": "string",
    "encryption": "SSE-S3",
    "inventoryConfig": {
      "destinationBucketName": "string",
      "destinationPrefix": "string",
      "region": "string",
      "type": "s3-inventory"
    },
    "region": "string"
  },
  "credentials": {
    "apiKey": "string",
    "method": "manual",
    "roleToAssume": "string",
    "roleToAssumeExternalId": "string",
    "roleToAssumeHybrid": "string",
    "secretKey": "string"
  },
  "description": "string",
  "id": "string",
  "lastProvisionedMs": 0,
  "metricsLastGenerated": 0,
  "provider": "cribl_lake",
  "status": "provisioning"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config|[CriblLakeStorageLocationConfig](#schemacribllakestoragelocationconfig)|true|none|none|
|credentials|[Credentials](#schemacredentials)|true|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|lastProvisionedMs|number|false|none|none|
|metricsLastGenerated|number|false|none|none|
|provider|string|true|none|none|
|status|[CriblLakeLifecycleItemStatus](#schemacribllakelifecycleitemstatus)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|provider|cribl_lake|
|provider|aws-s3|

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

<h2 id="tocS_CriblLakeStorageLocationConfig">CriblLakeStorageLocationConfig</h2>
<!-- backwards compatibility -->
<a id="schemacribllakestoragelocationconfig"></a>
<a id="schema_CriblLakeStorageLocationConfig"></a>
<a id="tocScribllakestoragelocationconfig"></a>
<a id="tocscribllakestoragelocationconfig"></a>

```json
{
  "bucketName": "string",
  "encryption": "SSE-S3",
  "inventoryConfig": {
    "destinationBucketName": "string",
    "destinationPrefix": "string",
    "region": "string",
    "type": "s3-inventory"
  },
  "region": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bucketName|string|true|none|none|
|encryption|string|false|none|none|
|inventoryConfig|[CriblLakeStorageLocationInventoryConfig](#schemacribllakestoragelocationinventoryconfig)|false|none|none|
|region|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|encryption|SSE-S3|
|encryption|SSE-KMS|

<h2 id="tocS_Credentials">Credentials</h2>
<!-- backwards compatibility -->
<a id="schemacredentials"></a>
<a id="schema_Credentials"></a>
<a id="tocScredentials"></a>
<a id="tocscredentials"></a>

```json
{
  "apiKey": "string",
  "method": "manual",
  "roleToAssume": "string",
  "roleToAssumeExternalId": "string",
  "roleToAssumeHybrid": "string",
  "secretKey": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|apiKey|string|false|none|none|
|method|string|true|none|none|
|roleToAssume|string|false|none|none|
|roleToAssumeExternalId|string|false|none|none|
|roleToAssumeHybrid|string|false|none|none|
|secretKey|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|method|manual|
|method|auto|
|method|auto_rpc|

<h2 id="tocS_CriblLakeLifecycleItemStatus">CriblLakeLifecycleItemStatus</h2>
<!-- backwards compatibility -->
<a id="schemacribllakelifecycleitemstatus"></a>
<a id="schema_CriblLakeLifecycleItemStatus"></a>
<a id="tocScribllakelifecycleitemstatus"></a>
<a id="tocscribllakelifecycleitemstatus"></a>

```json
"provisioning"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|provisioning|
|*anonymous*|ready|
|*anonymous*|failed|
|*anonymous*|terminated|
|*anonymous*|delayed|
|*anonymous*|blocked|

<h2 id="tocS_CriblLakeStorageLocationInventoryConfig">CriblLakeStorageLocationInventoryConfig</h2>
<!-- backwards compatibility -->
<a id="schemacribllakestoragelocationinventoryconfig"></a>
<a id="schema_CriblLakeStorageLocationInventoryConfig"></a>
<a id="tocScribllakestoragelocationinventoryconfig"></a>
<a id="tocscribllakestoragelocationinventoryconfig"></a>

```json
{
  "destinationBucketName": "string",
  "destinationPrefix": "string",
  "region": "string",
  "type": "s3-inventory"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|destinationBucketName|string|true|none|none|
|destinationPrefix|string|true|none|none|
|region|string|true|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|s3-inventory|

