
<h1 id="cribl-core-api-system-upgrade">Cribl Core API - System Upgrade v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-system-upgrade-system-upgrade">System Upgrade</h1>

## Get a list of Cribl versions available for upgrade

<a id="opIdgetSystemSettingsUpgrade"></a>

> Code samples

`GET /system/settings/upgrade`

Get a list of Cribl versions available for upgrade

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "availableVersions": [
        {
          "architecture": "string",
          "build": "string",
          "downloadUrl": "string",
          "fullVersion": "string",
          "major": 0,
          "minor": 0,
          "platform": "string",
          "point": 0,
          "preRelease": "string"
        }
      ],
      "canUpgrade": true,
      "installedVersion": {
        "architecture": "string",
        "build": "string",
        "downloadUrl": "string",
        "fullVersion": "string",
        "major": 0,
        "minor": 0,
        "platform": "string",
        "point": 0,
        "preRelease": "string"
      },
      "isSuccess": true,
      "message": "string",
      "upgradedToVersion": {
        "architecture": "string",
        "build": "string",
        "downloadUrl": "string",
        "fullVersion": "string",
        "major": 0,
        "minor": 0,
        "platform": "string",
        "point": 0,
        "preRelease": "string"
      }
    }
  ]
}
```

<h3 id="get-a-list-of-cribl-versions-available-for-upgrade-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of UpgradeResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-cribl-versions-available-for-upgrade-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[UpgradeResult](#schemaupgraderesult)]|false|none|none|
|»» availableVersions|[[VersionInfo](#schemaversioninfo)]|true|none|none|
|»»» architecture|string|false|none|none|
|»»» build|string|true|none|none|
|»»» downloadUrl|string|false|none|none|
|»»» fullVersion|string|true|none|none|
|»»» major|number|true|none|none|
|»»» minor|number|true|none|none|
|»»» platform|string|false|none|none|
|»»» point|number|false|none|none|
|»»» preRelease|string|false|none|none|
|»» canUpgrade|boolean|true|none|none|
|»» installedVersion|[VersionInfo](#schemaversioninfo)|true|none|none|
|»» isSuccess|boolean|true|none|none|
|»» message|string|true|none|none|
|»» upgradedToVersion|[VersionInfo](#schemaversioninfo)|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Upgrade master node with the provided package

<a id="opIdcreateSystemSettingsUpgradeFromPackage"></a>

> Code samples

`POST /system/settings/upgrade-from-package`

Upgrade master node with the provided package

> Body parameter

```json
{
  "packages": [
    {
      "packageUrl": "string",
      "packageHashUrl": "string"
    }
  ]
}
```

<h3 id="upgrade-master-node-with-the-provided-package-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[UpgradeMasterRequest](#schemaupgrademasterrequest)|true|upgradeMaster object|
|» packages|body|[object]|false|Provide your own URLs or local paths for platform-specific Cribl packages|
|»» packageUrl|body|string|true|Package HTTP URL or local path|
|»» packageHashUrl|body|string|false|Package's MD5 or SHA256 hash HTTP URL or local path|

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

<h3 id="upgrade-master-node-with-the-provided-package-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="upgrade-master-node-with-the-provided-package-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Upgrade Cribl to a given version

<a id="opIdcreateSystemSettingsUpgradeByVersion"></a>

> Code samples

`POST /system/settings/upgrade/{version}`

Upgrade Cribl to a given version

<h3 id="upgrade-cribl-to-a-given-version-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|version|path|string|true|Version number|

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

<h3 id="upgrade-cribl-to-a-given-version-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="upgrade-cribl-to-a-given-version-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Execute distributed group upgrade

<a id="opIdcreateSystemDistributedUpgradeByGroup"></a>

> Code samples

`POST /system/distributed/upgrade/{group}`

Execute distributed group upgrade

> Body parameter

```json
{
  "packageUrls": [
    {
      "packageUrl": "string",
      "packageHashUrl": "string"
    }
  ],
  "upgradePercentage": 1,
  "workerRetries": 5,
  "workerRetryDelay": 1000,
  "upgradeMode": "rolling",
  "versionTo": "string"
}
```

<h3 id="execute-distributed-group-upgrade-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group|path|string|true|Group to upgrade|
|body|body|[DistributedUpgradeRequest](#schemadistributedupgraderequest)|true|distributedUpgrade object|
|» packageUrls|body|[object]|false|Provide your own URLs or local paths for platform-specific Cribl packages|
|»» packageUrl|body|string|true|Package HTTP URL or local path|
|»» packageHashUrl|body|string|false|Package's MD5 or SHA256 hash HTTP URL or local path|
|» upgradePercentage|body|number|false|Percentage of the total worker nodes on the group to run the upgrade on|
|» workerRetries|body|number|false|Number of times to retry conncecting to a worker node before marking the upgrade as failed.|
|» workerRetryDelay|body|number|false|Delay between retries|
|» upgradeMode|body|string|false|none|
|» versionTo|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» upgradeMode|rolling|
|» upgradeMode|regular|
|» upgradeMode|batch|

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

<h3 id="execute-distributed-group-upgrade-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="execute-distributed-group-upgrade-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Cancel a running group upgrade

<a id="opIdcreateSystemDistributedUpgradeCancelByGroup"></a>

> Code samples

`POST /system/distributed/upgrade/cancel/{group}`

Cancel a running group upgrade

<h3 id="cancel-a-running-group-upgrade-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group|path|string|true|id of the group|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="cancel-a-running-group-upgrade-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="cancel-a-running-group-upgrade-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the previously downloaded Cribl package

<a id="opIdgetSystemDistributedUpgradeDownloadByFile"></a>

> Code samples

`GET /system/distributed/upgrade/download/{file}`

Get the previously downloaded Cribl package

<h3 id="get-the-previously-downloaded-cribl-package-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|file|path|string|true|Name of the file to be downloaded|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="get-the-previously-downloaded-cribl-package-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-previously-downloaded-cribl-package-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Stage distributed group upgrade

<a id="opIdcreateSystemDistributedUpgradeStageByGroup"></a>

> Code samples

`POST /system/distributed/upgrade/stage/{group}`

Stage distributed group upgrade

> Body parameter

```json
{
  "packageUrls": [
    {
      "packageUrl": "string",
      "packageHashUrl": "string"
    }
  ],
  "upgradePercentage": 1,
  "workerRetries": 5,
  "workerRetryDelay": 1000,
  "upgradeMode": "rolling",
  "versionTo": "string"
}
```

<h3 id="stage-distributed-group-upgrade-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|group|path|string|true|Group to upgrade|
|body|body|[DistributedUpgradeRequest](#schemadistributedupgraderequest)|true|distributedUpgrade object|
|» packageUrls|body|[object]|false|Provide your own URLs or local paths for platform-specific Cribl packages|
|»» packageUrl|body|string|true|Package HTTP URL or local path|
|»» packageHashUrl|body|string|false|Package's MD5 or SHA256 hash HTTP URL or local path|
|» upgradePercentage|body|number|false|Percentage of the total worker nodes on the group to run the upgrade on|
|» workerRetries|body|number|false|Number of times to retry conncecting to a worker node before marking the upgrade as failed.|
|» workerRetryDelay|body|number|false|Delay between retries|
|» upgradeMode|body|string|false|none|
|» versionTo|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|» upgradeMode|rolling|
|» upgradeMode|regular|
|» upgradeMode|batch|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {}
  ]
}
```

<h3 id="stage-distributed-group-upgrade-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="stage-distributed-group-upgrade-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_UpgradeResult">UpgradeResult</h2>
<!-- backwards compatibility -->
<a id="schemaupgraderesult"></a>
<a id="schema_UpgradeResult"></a>
<a id="tocSupgraderesult"></a>
<a id="tocsupgraderesult"></a>

```json
{
  "availableVersions": [
    {
      "architecture": "string",
      "build": "string",
      "downloadUrl": "string",
      "fullVersion": "string",
      "major": 0,
      "minor": 0,
      "platform": "string",
      "point": 0,
      "preRelease": "string"
    }
  ],
  "canUpgrade": true,
  "installedVersion": {
    "architecture": "string",
    "build": "string",
    "downloadUrl": "string",
    "fullVersion": "string",
    "major": 0,
    "minor": 0,
    "platform": "string",
    "point": 0,
    "preRelease": "string"
  },
  "isSuccess": true,
  "message": "string",
  "upgradedToVersion": {
    "architecture": "string",
    "build": "string",
    "downloadUrl": "string",
    "fullVersion": "string",
    "major": 0,
    "minor": 0,
    "platform": "string",
    "point": 0,
    "preRelease": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|availableVersions|[[VersionInfo](#schemaversioninfo)]|true|none|none|
|canUpgrade|boolean|true|none|none|
|installedVersion|[VersionInfo](#schemaversioninfo)|true|none|none|
|isSuccess|boolean|true|none|none|
|message|string|true|none|none|
|upgradedToVersion|[VersionInfo](#schemaversioninfo)|true|none|none|

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

<h2 id="tocS_UpgradeMasterRequest">UpgradeMasterRequest</h2>
<!-- backwards compatibility -->
<a id="schemaupgrademasterrequest"></a>
<a id="schema_UpgradeMasterRequest"></a>
<a id="tocSupgrademasterrequest"></a>
<a id="tocsupgrademasterrequest"></a>

```json
{
  "packages": [
    {
      "packageUrl": "string",
      "packageHashUrl": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|packages|[object]|false|none|Provide your own URLs or local paths for platform-specific Cribl packages|
|» packageUrl|string|true|none|Package HTTP URL or local path|
|» packageHashUrl|string|false|none|Package's MD5 or SHA256 hash HTTP URL or local path|

<h2 id="tocS_DistributedUpgradeRequest">DistributedUpgradeRequest</h2>
<!-- backwards compatibility -->
<a id="schemadistributedupgraderequest"></a>
<a id="schema_DistributedUpgradeRequest"></a>
<a id="tocSdistributedupgraderequest"></a>
<a id="tocsdistributedupgraderequest"></a>

```json
{
  "packageUrls": [
    {
      "packageUrl": "string",
      "packageHashUrl": "string"
    }
  ],
  "upgradePercentage": 1,
  "workerRetries": 5,
  "workerRetryDelay": 1000,
  "upgradeMode": "rolling",
  "versionTo": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|packageUrls|[object]|false|none|Provide your own URLs or local paths for platform-specific Cribl packages|
|» packageUrl|string|true|none|Package HTTP URL or local path|
|» packageHashUrl|string|false|none|Package's MD5 or SHA256 hash HTTP URL or local path|
|upgradePercentage|number|false|none|Percentage of the total worker nodes on the group to run the upgrade on|
|workerRetries|number|false|none|Number of times to retry conncecting to a worker node before marking the upgrade as failed.|
|workerRetryDelay|number|false|none|Delay between retries|
|upgradeMode|string|false|none|none|
|versionTo|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|upgradeMode|rolling|
|upgradeMode|regular|
|upgradeMode|batch|

<h2 id="tocS_VersionInfo">VersionInfo</h2>
<!-- backwards compatibility -->
<a id="schemaversioninfo"></a>
<a id="schema_VersionInfo"></a>
<a id="tocSversioninfo"></a>
<a id="tocsversioninfo"></a>

```json
{
  "architecture": "string",
  "build": "string",
  "downloadUrl": "string",
  "fullVersion": "string",
  "major": 0,
  "minor": 0,
  "platform": "string",
  "point": 0,
  "preRelease": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|architecture|string|false|none|none|
|build|string|true|none|none|
|downloadUrl|string|false|none|none|
|fullVersion|string|true|none|none|
|major|number|true|none|none|
|minor|number|true|none|none|
|platform|string|false|none|none|
|point|number|false|none|none|
|preRelease|string|false|none|none|

