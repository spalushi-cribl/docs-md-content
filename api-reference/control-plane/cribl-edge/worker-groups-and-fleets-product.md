
<h1 id="cribl-edge-api-worker-groups-and-fleets-product-">Cribl Edge API - Worker Groups and Fleets (Product) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-worker-groups-and-fleets-product--worker-groups-and-fleets-product-">Worker Groups and Fleets (Product)</h1>

## List all Worker Groups or Edge Fleets for the specified Cribl product

<a id="opIdlistConfigGroupByProduct"></a>

> Code samples

`GET /products/{product}/groups`

Get a list of all Worker Groups or Edge Fleets for the specified Cribl product.

<h3 id="list-all-worker-groups-or-edge-fleets-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|fields|query|string|false|Comma-separated list of additional properties to include in the response. Available values are <code>git.commit</code>, <code>git.localChanges</code>, and <code>git.log</code>.|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product to get the Worker Groups or Edge Fleets for.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cloud": {
        "provider": "aws",
        "region": "string"
      },
      "configVersion": "string",
      "deployingWorkerCount": 0,
      "description": "string",
      "estimatedIngestRate": 1024,
      "git": {
        "commit": "string",
        "localChanges": 0,
        "log": [
          {
            "author_email": "string",
            "author_name": "string",
            "date": "string",
            "hash": "string",
            "message": "string",
            "short": "string"
          }
        ]
      },
      "id": "string",
      "incompatibleWorkerCount": 0,
      "inherits": "string",
      "isFleet": true,
      "isSearch": true,
      "lookupDeployments": [
        {
          "context": "string",
          "lookups": [
            {
              "deployedVersion": "string",
              "file": "string",
              "version": "string"
            }
          ]
        }
      ],
      "maxWorkerAge": "string",
      "name": "string",
      "onPrem": true,
      "provisioned": true,
      "streamtags": [
        "string"
      ],
      "tags": "string",
      "type": "lake_access",
      "upgradeVersion": "string",
      "workerCount": 0,
      "workerRemoteAccess": true
    }
  ]
}
```

<h3 id="list-all-worker-groups-or-edge-fleets-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ConfigGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-worker-groups-or-edge-fleets-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ConfigGroup](#schemaconfiggroup)]|false|none|none|
|»» cloud|[ConfigGroupCloud](#schemaconfiggroupcloud)|false|none|none|
|»»» provider|[CloudProvider](#schemacloudprovider)¦null|true|none|none|
|»»» region|string|true|none|none|
|»» configVersion|string|false|none|none|
|»» deployingWorkerCount|number|false|none|none|
|»» description|string|false|none|none|
|»» estimatedIngestRate|integer|false|none|Maximum expected volume of data ingested by the @{group}. (This setting is available only on @{group}s consisting of Cribl-managed Cribl.Cloud @{node}s.)|
|»» git|object|false|none|none|
|»»» commit|string|false|none|none|
|»»» localChanges|number|false|none|none|
|»»» log|[[Commit](#schemacommit)]|false|none|none|
|»»»» author_email|string|false|none|none|
|»»»» author_name|string|false|none|none|
|»»»» date|string|true|none|none|
|»»»» hash|string|true|none|none|
|»»»» message|string|true|none|none|
|»»»» short|string|true|none|none|
|»» id|string|true|none|none|
|»» incompatibleWorkerCount|number|false|none|none|
|»» inherits|string|false|none|none|
|»» isFleet|boolean|false|none|none|
|»» isSearch|boolean|false|none|none|
|»» lookupDeployments|[[ConfigGroupLookups](#schemaconfiggrouplookups)]|false|none|none|
|»»» context|string|true|none|none|
|»»» lookups|[object]|true|none|none|
|»»»» deployedVersion|string|false|none|none|
|»»»» file|string|true|none|none|
|»»»» version|string|false|none|none|
|»» maxWorkerAge|string|false|none|none|
|»» name|string|false|none|none|
|»» onPrem|boolean|false|none|none|
|»» provisioned|boolean|false|none|none|
|»» streamtags|[string]|false|none|none|
|»» tags|string|false|none|none|
|»» type|string|false|none|none|
|»» upgradeVersion|string|false|none|none|
|»» workerCount|number|false|none|none|
|»» workerRemoteAccess|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|provider|aws|
|provider|azure|
|estimatedIngestRate|1024|
|estimatedIngestRate|2048|
|estimatedIngestRate|3072|
|estimatedIngestRate|4096|
|estimatedIngestRate|5120|
|estimatedIngestRate|7168|
|estimatedIngestRate|10240|
|estimatedIngestRate|13312|
|estimatedIngestRate|15360|
|type|lake_access|

<aside class="success">
This operation does not require authentication
</aside>

## Create a Worker Group or Edge Fleet for the specified Cribl product

<a id="opIdcreateConfigGroupByProduct"></a>

> Code samples

`POST /products/{product}/groups`

Create a new Worker Group or Edge Fleet for the specified Cribl product.

> Body parameter

```json
undefined
```

<h3 id="create-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|required Name of the Cribl product to add the Worker Group or Edge Fleet to.|
|body|body|[GroupCreateRequest](#schemagroupcreaterequest)|true|GroupCreateRequest object|
|» cloud|body|[ConfigGroupCloud](#schemaconfiggroupcloud)|false|none|
|»» provider|body|[CloudProvider](#schemacloudprovider)¦null|true|none|
|»» region|body|string|true|none|
|» deployingWorkerCount|body|number|false|none|
|» description|body|string|false|none|
|» estimatedIngestRate|body|integer|false|Maximum expected volume of data ingested by the @{group}. (This setting is available only on @{group}s consisting of Cribl-managed Cribl.Cloud @{node}s.)|
|» git|body|object|false|none|
|»» commit|body|string|false|none|
|»» localChanges|body|number|false|none|
|»» log|body|[[Commit](#schemacommit)]|false|none|
|»»» author_email|body|string|false|none|
|»»» author_name|body|string|false|none|
|»»» date|body|string|true|none|
|»»» hash|body|string|true|none|
|»»» message|body|string|true|none|
|»»» short|body|string|true|none|
|» id|body|string|true|none|
|» incompatibleWorkerCount|body|number|false|none|
|» inherits|body|string|false|none|
|» isFleet|body|boolean|false|none|
|» isSearch|body|boolean|false|none|
|» lookupDeployments|body|[[ConfigGroupLookups](#schemaconfiggrouplookups)]|false|none|
|»» context|body|string|true|none|
|»» lookups|body|[object]|true|none|
|»»» deployedVersion|body|string|false|none|
|»»» file|body|string|true|none|
|»»» version|body|string|false|none|
|» maxWorkerAge|body|string|false|none|
|» name|body|string|false|none|
|» onPrem|body|boolean|false|none|
|» provisioned|body|boolean|false|none|
|» sourceGroupId|body|string|false|none|
|» streamtags|body|[string]|false|none|
|» tags|body|string|false|none|
|» type|body|string|false|none|
|» upgradeVersion|body|string|false|none|
|» workerCount|body|number|false|none|
|» workerRemoteAccess|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
|»» provider|aws|
|»» provider|azure|
|» estimatedIngestRate|1024|
|» estimatedIngestRate|2048|
|» estimatedIngestRate|3072|
|» estimatedIngestRate|4096|
|» estimatedIngestRate|5120|
|» estimatedIngestRate|7168|
|» estimatedIngestRate|10240|
|» estimatedIngestRate|13312|
|» estimatedIngestRate|15360|
|» type|lake_access|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cloud": {
        "provider": "aws",
        "region": "string"
      },
      "configVersion": "string",
      "deployingWorkerCount": 0,
      "description": "string",
      "estimatedIngestRate": 1024,
      "git": {
        "commit": "string",
        "localChanges": 0,
        "log": [
          {
            "author_email": "string",
            "author_name": "string",
            "date": "string",
            "hash": "string",
            "message": "string",
            "short": "string"
          }
        ]
      },
      "id": "string",
      "incompatibleWorkerCount": 0,
      "inherits": "string",
      "isFleet": true,
      "isSearch": true,
      "lookupDeployments": [
        {
          "context": "string",
          "lookups": [
            {
              "deployedVersion": "string",
              "file": "string",
              "version": "string"
            }
          ]
        }
      ],
      "maxWorkerAge": "string",
      "name": "string",
      "onPrem": true,
      "provisioned": true,
      "streamtags": [
        "string"
      ],
      "tags": "string",
      "type": "lake_access",
      "upgradeVersion": "string",
      "workerCount": 0,
      "workerRemoteAccess": true
    }
  ]
}
```

<h3 id="create-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ConfigGroup objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ConfigGroup](#schemaconfiggroup)]|false|none|none|
|»» cloud|[ConfigGroupCloud](#schemaconfiggroupcloud)|false|none|none|
|»»» provider|[CloudProvider](#schemacloudprovider)¦null|true|none|none|
|»»» region|string|true|none|none|
|»» configVersion|string|false|none|none|
|»» deployingWorkerCount|number|false|none|none|
|»» description|string|false|none|none|
|»» estimatedIngestRate|integer|false|none|Maximum expected volume of data ingested by the @{group}. (This setting is available only on @{group}s consisting of Cribl-managed Cribl.Cloud @{node}s.)|
|»» git|object|false|none|none|
|»»» commit|string|false|none|none|
|»»» localChanges|number|false|none|none|
|»»» log|[[Commit](#schemacommit)]|false|none|none|
|»»»» author_email|string|false|none|none|
|»»»» author_name|string|false|none|none|
|»»»» date|string|true|none|none|
|»»»» hash|string|true|none|none|
|»»»» message|string|true|none|none|
|»»»» short|string|true|none|none|
|»» id|string|true|none|none|
|»» incompatibleWorkerCount|number|false|none|none|
|»» inherits|string|false|none|none|
|»» isFleet|boolean|false|none|none|
|»» isSearch|boolean|false|none|none|
|»» lookupDeployments|[[ConfigGroupLookups](#schemaconfiggrouplookups)]|false|none|none|
|»»» context|string|true|none|none|
|»»» lookups|[object]|true|none|none|
|»»»» deployedVersion|string|false|none|none|
|»»»» file|string|true|none|none|
|»»»» version|string|false|none|none|
|»» maxWorkerAge|string|false|none|none|
|»» name|string|false|none|none|
|»» onPrem|boolean|false|none|none|
|»» provisioned|boolean|false|none|none|
|»» streamtags|[string]|false|none|none|
|»» tags|string|false|none|none|
|»» type|string|false|none|none|
|»» upgradeVersion|string|false|none|none|
|»» workerCount|number|false|none|none|
|»» workerRemoteAccess|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|provider|aws|
|provider|azure|
|estimatedIngestRate|1024|
|estimatedIngestRate|2048|
|estimatedIngestRate|3072|
|estimatedIngestRate|4096|
|estimatedIngestRate|5120|
|estimatedIngestRate|7168|
|estimatedIngestRate|10240|
|estimatedIngestRate|13312|
|estimatedIngestRate|15360|
|type|lake_access|

<aside class="success">
This operation does not require authentication
</aside>

## Get the Access Control List for teams with permissions on a Worker Group or Edge Fleet for the specified Cribl product

<a id="opIdgetConfigGroupAclTeamsByProductAndId"></a>

> Code samples

`GET /products/{product}/groups/{id}/acl/teams`

Get the Access Control List (ACL) for teams that have permissions on a Worker Group or Edge Fleet for the specified Cribl product.

<h3 id="get-the-access-control-list-for-teams-with-permissions-on-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|[ProductsCore](#schemaproductscore)|true|Name of the Cribl product that contains the Worker Group or Edge Fleet.|
|id|path|string|true|The <code>id</code> of the Worker Group or Edge Fleet to get the team ACL for.|
|type|query|[RbacResource](#schemarbacresource)|false|Filter for limiting the response to ACL entries for the specified RBAC resource type.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|product|stream|
|product|edge|
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
      "perms": [
        {
          "gid": "string",
          "id": "string",
          "policy": "string",
          "type": "groups"
        }
      ],
      "team": "string"
    }
  ]
}
```

<h3 id="get-the-access-control-list-for-teams-with-permissions-on-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of TeamAccessControlList objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-access-control-list-for-teams-with-permissions-on-a-worker-group-or-edge-fleet-for-the-specified-cribl-product-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[TeamAccessControlList](#schemateamaccesscontrollist)]|false|none|none|
|»» perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|»»» gid|string|true|none|none|
|»»» id|string|false|none|none|
|»»» policy|string|true|none|none|
|»»» type|[RbacResource](#schemarbacresource)|true|none|none|
|»» team|string|true|none|none|

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

# Schemas

<h2 id="tocS_ConfigGroup">ConfigGroup</h2>
<!-- backwards compatibility -->
<a id="schemaconfiggroup"></a>
<a id="schema_ConfigGroup"></a>
<a id="tocSconfiggroup"></a>
<a id="tocsconfiggroup"></a>

```json
{
  "cloud": {
    "provider": "aws",
    "region": "string"
  },
  "configVersion": "string",
  "deployingWorkerCount": 0,
  "description": "string",
  "estimatedIngestRate": 1024,
  "git": {
    "commit": "string",
    "localChanges": 0,
    "log": [
      {
        "author_email": "string",
        "author_name": "string",
        "date": "string",
        "hash": "string",
        "message": "string",
        "short": "string"
      }
    ]
  },
  "id": "string",
  "incompatibleWorkerCount": 0,
  "inherits": "string",
  "isFleet": true,
  "isSearch": true,
  "lookupDeployments": [
    {
      "context": "string",
      "lookups": [
        {
          "deployedVersion": "string",
          "file": "string",
          "version": "string"
        }
      ]
    }
  ],
  "maxWorkerAge": "string",
  "name": "string",
  "onPrem": true,
  "provisioned": true,
  "streamtags": [
    "string"
  ],
  "tags": "string",
  "type": "lake_access",
  "upgradeVersion": "string",
  "workerCount": 0,
  "workerRemoteAccess": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cloud|[ConfigGroupCloud](#schemaconfiggroupcloud)|false|none|none|
|configVersion|string|false|none|none|
|deployingWorkerCount|number|false|none|none|
|description|string|false|none|none|
|estimatedIngestRate|integer|false|none|Maximum expected volume of data ingested by the @{group}. (This setting is available only on @{group}s consisting of Cribl-managed Cribl.Cloud @{node}s.)|
|git|object|false|none|none|
|» commit|string|false|none|none|
|» localChanges|number|false|none|none|
|» log|[[Commit](#schemacommit)]|false|none|none|
|id|string|true|none|none|
|incompatibleWorkerCount|number|false|none|none|
|inherits|string|false|none|none|
|isFleet|boolean|false|none|none|
|isSearch|boolean|false|none|none|
|lookupDeployments|[[ConfigGroupLookups](#schemaconfiggrouplookups)]|false|none|none|
|maxWorkerAge|string|false|none|none|
|name|string|false|none|none|
|onPrem|boolean|false|none|none|
|provisioned|boolean|false|none|none|
|streamtags|[string]|false|none|none|
|tags|string|false|none|none|
|type|string|false|none|none|
|upgradeVersion|string|false|none|none|
|workerCount|number|false|none|none|
|workerRemoteAccess|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|estimatedIngestRate|1024|
|estimatedIngestRate|2048|
|estimatedIngestRate|3072|
|estimatedIngestRate|4096|
|estimatedIngestRate|5120|
|estimatedIngestRate|7168|
|estimatedIngestRate|10240|
|estimatedIngestRate|13312|
|estimatedIngestRate|15360|
|type|lake_access|

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

<h2 id="tocS_ProductsCore">ProductsCore</h2>
<!-- backwards compatibility -->
<a id="schemaproductscore"></a>
<a id="schema_ProductsCore"></a>
<a id="tocSproductscore"></a>
<a id="tocsproductscore"></a>

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

<h2 id="tocS_GroupCreateRequest">GroupCreateRequest</h2>
<!-- backwards compatibility -->
<a id="schemagroupcreaterequest"></a>
<a id="schema_GroupCreateRequest"></a>
<a id="tocSgroupcreaterequest"></a>
<a id="tocsgroupcreaterequest"></a>

```json
{
  "cloud": {
    "provider": "aws",
    "region": "string"
  },
  "deployingWorkerCount": 0,
  "description": "string",
  "estimatedIngestRate": 1024,
  "git": {
    "commit": "string",
    "localChanges": 0,
    "log": [
      {
        "author_email": "string",
        "author_name": "string",
        "date": "string",
        "hash": "string",
        "message": "string",
        "short": "string"
      }
    ]
  },
  "id": "string",
  "incompatibleWorkerCount": 0,
  "inherits": "string",
  "isFleet": true,
  "isSearch": true,
  "lookupDeployments": [
    {
      "context": "string",
      "lookups": [
        {
          "deployedVersion": "string",
          "file": "string",
          "version": "string"
        }
      ]
    }
  ],
  "maxWorkerAge": "string",
  "name": "string",
  "onPrem": true,
  "provisioned": true,
  "sourceGroupId": "string",
  "streamtags": [
    "string"
  ],
  "tags": "string",
  "type": "lake_access",
  "upgradeVersion": "string",
  "workerCount": 0,
  "workerRemoteAccess": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cloud|[ConfigGroupCloud](#schemaconfiggroupcloud)|false|none|none|
|deployingWorkerCount|number|false|none|none|
|description|string|false|none|none|
|estimatedIngestRate|integer|false|none|Maximum expected volume of data ingested by the @{group}. (This setting is available only on @{group}s consisting of Cribl-managed Cribl.Cloud @{node}s.)|
|git|object|false|none|none|
|» commit|string|false|none|none|
|» localChanges|number|false|none|none|
|» log|[[Commit](#schemacommit)]|false|none|none|
|id|string|true|none|none|
|incompatibleWorkerCount|number|false|none|none|
|inherits|string|false|none|none|
|isFleet|boolean|false|none|none|
|isSearch|boolean|false|none|none|
|lookupDeployments|[[ConfigGroupLookups](#schemaconfiggrouplookups)]|false|none|none|
|maxWorkerAge|string|false|none|none|
|name|string|false|none|none|
|onPrem|boolean|false|none|none|
|provisioned|boolean|false|none|none|
|sourceGroupId|string|false|none|none|
|streamtags|[string]|false|none|none|
|tags|string|false|none|none|
|type|string|false|none|none|
|upgradeVersion|string|false|none|none|
|workerCount|number|false|none|none|
|workerRemoteAccess|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|estimatedIngestRate|1024|
|estimatedIngestRate|2048|
|estimatedIngestRate|3072|
|estimatedIngestRate|4096|
|estimatedIngestRate|5120|
|estimatedIngestRate|7168|
|estimatedIngestRate|10240|
|estimatedIngestRate|13312|
|estimatedIngestRate|15360|
|type|lake_access|

<h2 id="tocS_TeamAccessControlList">TeamAccessControlList</h2>
<!-- backwards compatibility -->
<a id="schemateamaccesscontrollist"></a>
<a id="schema_TeamAccessControlList"></a>
<a id="tocSteamaccesscontrollist"></a>
<a id="tocsteamaccesscontrollist"></a>

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
  "team": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|perms|[[ResourcePolicy](#schemaresourcepolicy)]|true|none|none|
|team|string|true|none|none|

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

<h2 id="tocS_ConfigGroupCloud">ConfigGroupCloud</h2>
<!-- backwards compatibility -->
<a id="schemaconfiggroupcloud"></a>
<a id="schema_ConfigGroupCloud"></a>
<a id="tocSconfiggroupcloud"></a>
<a id="tocsconfiggroupcloud"></a>

```json
{
  "provider": "aws",
  "region": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|provider|[CloudProvider](#schemacloudprovider)|true|none|none|
|region|string|true|none|none|

<h2 id="tocS_Commit">Commit</h2>
<!-- backwards compatibility -->
<a id="schemacommit"></a>
<a id="schema_Commit"></a>
<a id="tocScommit"></a>
<a id="tocscommit"></a>

```json
{
  "author_email": "string",
  "author_name": "string",
  "date": "string",
  "hash": "string",
  "message": "string",
  "short": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|author_email|string|false|none|none|
|author_name|string|false|none|none|
|date|string|true|none|none|
|hash|string|true|none|none|
|message|string|true|none|none|
|short|string|true|none|none|

<h2 id="tocS_ConfigGroupLookups">ConfigGroupLookups</h2>
<!-- backwards compatibility -->
<a id="schemaconfiggrouplookups"></a>
<a id="schema_ConfigGroupLookups"></a>
<a id="tocSconfiggrouplookups"></a>
<a id="tocsconfiggrouplookups"></a>

```json
{
  "context": "string",
  "lookups": [
    {
      "deployedVersion": "string",
      "file": "string",
      "version": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|context|string|true|none|none|
|lookups|[object]|true|none|none|
|» deployedVersion|string|false|none|none|
|» file|string|true|none|none|
|» version|string|false|none|none|

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

<h2 id="tocS_CloudProvider">CloudProvider</h2>
<!-- backwards compatibility -->
<a id="schemacloudprovider"></a>
<a id="schema_CloudProvider"></a>
<a id="tocScloudprovider"></a>
<a id="tocscloudprovider"></a>

```json
"aws"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string¦null|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|aws|
|*anonymous*|azure|

