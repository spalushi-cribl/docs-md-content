
<h1 id="cribl-core-api-packs">Cribl Core API - Packs v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-packs-packs">Packs</h1>

## Install a Pack

<a id="opIdcreatePacks"></a>

> Code samples

`POST /packs`

Install a Pack.<br><br>To install an uploaded Pack, provide the <code>source</code> value from the <code>PUT /packs</code> response as the <code>source</code> parameter in the request body.<br><br>To install a Pack by importing from a URL, provide the direct URL location of the <code>.crbl</code> file for the Pack as the <code>source</code> parameter in the request body.<br><br>To install a Pack by importing from a Git repository, provide <code>git+<repo-url></code> as the <code>source</code> parameter in the request body.<br><br>If you do not include the <code>source</code> parameter in the request body, an empty Pack is created.

> Body parameter

```json
undefined
```

<h3 id="install-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PackRequestBody](#schemapackrequestbody)|true|packRequestBody object|
|» id|body|string|false|none|
|» spec|body|string|false|none|
|» version|body|string|false|none|
|» minLogStreamVersion|body|string|false|none|
|» displayName|body|string|false|none|
|» author|body|string|false|none|
|» description|body|string|false|none|
|» source|body|string|false|The source of the pack. If not present, an empty pack will be created|
|» tags|body|object|false|none|
|»» dataType|body|[string]|false|none|
|»» domain|body|[string]|false|none|
|»» technology|body|[string]|false|none|
|»» streamtags|body|[string]|false|none|
|» allowCustomFunctions|body|boolean|false|none|
|» force|body|boolean|false|none|
|» *anonymous*|body|object|false|none|
|» *anonymous*|body|object|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": "string",
      "dependencies": {
        "property1": "string",
        "property2": "string"
      },
      "description": "string",
      "displayName": "string",
      "exports": [
        "string"
      ],
      "id": "string",
      "inputs": 0,
      "isDisabled": true,
      "minLogStreamVersion": "string",
      "outputs": 0,
      "settings": {},
      "source": "string",
      "spec": "string",
      "tags": {
        "dataType": [
          "string"
        ],
        "domain": [
          "string"
        ],
        "streamtags": [
          "string"
        ],
        "technology": [
          "string"
        ]
      },
      "version": "string",
      "warnings": [
        "string"
      ]
    }
  ]
}
```

<h3 id="install-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PackInstallInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="install-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PackInstallInfo](#schemapackinstallinfo)]|false|none|none|
|»» author|string|false|none|none|
|»» dependencies|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» description|string|false|none|none|
|»» displayName|string|false|none|none|
|»» exports|[string]|false|none|none|
|»» id|string|true|none|none|
|»» inputs|number|false|none|none|
|»» isDisabled|boolean|false|none|none|
|»» minLogStreamVersion|string|false|none|none|
|»» outputs|number|false|none|none|
|»» settings|object|false|none|none|
|»» source|string|true|none|none|
|»» spec|string|false|none|none|
|»» tags|object|false|none|none|
|»»» dataType|[string]|false|none|none|
|»»» domain|[string]|false|none|none|
|»»» streamtags|[string]|false|none|none|
|»»» technology|[string]|false|none|none|
|»» version|string|false|none|none|
|»» warnings|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## List all Packs

<a id="opIdgetPacks"></a>

> Code samples

`GET /packs`

Get a list of all Packs.

<h3 id="list-all-packs-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|with|query|string|false|Comma-separated list of additional properties to include in the response. When set, the response includes a count of the specified properties in the Pack. Available values are <code>inputs</code> and <code>outputs</code>.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": "string",
      "dependencies": {
        "property1": "string",
        "property2": "string"
      },
      "description": "string",
      "displayName": "string",
      "exports": [
        "string"
      ],
      "id": "string",
      "inputs": 0,
      "isDisabled": true,
      "minLogStreamVersion": "string",
      "outputs": 0,
      "settings": {},
      "source": "string",
      "spec": "string",
      "tags": {
        "dataType": [
          "string"
        ],
        "domain": [
          "string"
        ],
        "streamtags": [
          "string"
        ],
        "technology": [
          "string"
        ]
      },
      "version": "string"
    }
  ]
}
```

<h3 id="list-all-packs-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PackInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-packs-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PackInfo](#schemapackinfo)]|false|none|none|
|»» author|string|false|none|none|
|»» dependencies|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» description|string|false|none|none|
|»» displayName|string|false|none|none|
|»» exports|[string]|false|none|none|
|»» id|string|true|none|none|
|»» inputs|number|false|none|none|
|»» isDisabled|boolean|false|none|none|
|»» minLogStreamVersion|string|false|none|none|
|»» outputs|number|false|none|none|
|»» settings|object|false|none|none|
|»» source|string|true|none|none|
|»» spec|string|false|none|none|
|»» tags|object|false|none|none|
|»»» dataType|[string]|false|none|none|
|»»» domain|[string]|false|none|none|
|»»» streamtags|[string]|false|none|none|
|»»» technology|[string]|false|none|none|
|»» version|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Upload a Pack file

<a id="opIdupdatePacks"></a>

> Code samples

`PUT /packs`

Upload a Pack file. Returns the <code>source</code> ID needed to install the Pack with <code>POST /packs source</code>, which you must call separately.

> Body parameter

```yaml
string

```

<h3 id="upload-a-pack-file-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|filename|query|string|true|Filename of the Pack file to upload.|
|body|body|string(binary)|true|none|

> Example responses

> 200 Response

```json
{
  "source": "string"
}
```

<h3 id="upload-a-pack-file-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Pack file uploaded successfully|[UploadPackResponse](#schemauploadpackresponse)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get a Pack

<a id="opIdgetPacksById"></a>

> Code samples

`GET /packs/{id}`

Get the specified Pack.

<h3 id="get-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pack to get.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": "string",
      "dependencies": {
        "property1": "string",
        "property2": "string"
      },
      "description": "string",
      "displayName": "string",
      "exports": [
        "string"
      ],
      "id": "string",
      "inputs": 0,
      "isDisabled": true,
      "minLogStreamVersion": "string",
      "outputs": 0,
      "settings": {},
      "source": "string",
      "spec": "string",
      "tags": {
        "dataType": [
          "string"
        ],
        "domain": [
          "string"
        ],
        "streamtags": [
          "string"
        ],
        "technology": [
          "string"
        ]
      },
      "version": "string"
    }
  ]
}
```

<h3 id="get-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PackInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PackInfo](#schemapackinfo)]|false|none|none|
|»» author|string|false|none|none|
|»» dependencies|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» description|string|false|none|none|
|»» displayName|string|false|none|none|
|»» exports|[string]|false|none|none|
|»» id|string|true|none|none|
|»» inputs|number|false|none|none|
|»» isDisabled|boolean|false|none|none|
|»» minLogStreamVersion|string|false|none|none|
|»» outputs|number|false|none|none|
|»» settings|object|false|none|none|
|»» source|string|true|none|none|
|»» spec|string|false|none|none|
|»» tags|object|false|none|none|
|»»» dataType|[string]|false|none|none|
|»»» domain|[string]|false|none|none|
|»»» streamtags|[string]|false|none|none|
|»»» technology|[string]|false|none|none|
|»» version|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Uninstall a Pack

<a id="opIddeletePacksById"></a>

> Code samples

`DELETE /packs/{id}`

Uninstall the specified Pack.

<h3 id="uninstall-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pack to uninstall.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": "string",
      "dependencies": {
        "property1": "string",
        "property2": "string"
      },
      "description": "string",
      "displayName": "string",
      "exports": [
        "string"
      ],
      "id": "string",
      "inputs": 0,
      "isDisabled": true,
      "minLogStreamVersion": "string",
      "outputs": 0,
      "settings": {},
      "source": "string",
      "spec": "string",
      "tags": {
        "dataType": [
          "string"
        ],
        "domain": [
          "string"
        ],
        "streamtags": [
          "string"
        ],
        "technology": [
          "string"
        ]
      },
      "version": "string",
      "warnings": [
        "string"
      ]
    }
  ]
}
```

<h3 id="uninstall-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PackInstallInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="uninstall-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PackInstallInfo](#schemapackinstallinfo)]|false|none|none|
|»» author|string|false|none|none|
|»» dependencies|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» description|string|false|none|none|
|»» displayName|string|false|none|none|
|»» exports|[string]|false|none|none|
|»» id|string|true|none|none|
|»» inputs|number|false|none|none|
|»» isDisabled|boolean|false|none|none|
|»» minLogStreamVersion|string|false|none|none|
|»» outputs|number|false|none|none|
|»» settings|object|false|none|none|
|»» source|string|true|none|none|
|»» spec|string|false|none|none|
|»» tags|object|false|none|none|
|»»» dataType|[string]|false|none|none|
|»»» domain|[string]|false|none|none|
|»»» streamtags|[string]|false|none|none|
|»»» technology|[string]|false|none|none|
|»» version|string|false|none|none|
|»» warnings|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Upgrade a Pack

<a id="opIdupdatePacksById"></a>

> Code samples

`PATCH /packs/{id}`

Upgrade the specified Pack.</br></br>If the Pack includes any user–modified versions of default Cribl Knowledge resources such as lookups, copy the modified files locally for safekeeping before upgrading the Pack.Copy the modified files back to the upgraded Pack after you install it with <code>POST /packs</code> to overwrite the default versions in the Pack.</br></br>After you upgrade the Pack, update any Routes, Pipelines, Sources, and Destinations that use the previous Pack version so that they reference the upgraded Pack.

> Body parameter

```json
undefined
```

<h3 id="upgrade-a-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|The <code>id</code> of the Pack to upgrade.|
|body|body|[PackUpgradeRequest](#schemapackupgraderequest)|true|PackUpgradeRequest object|
|» allowCustomFunctions|body|boolean|false|none|
|» minor|body|string|false|none|
|» source|body|string|true|none|
|» spec|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": "string",
      "dependencies": {
        "property1": "string",
        "property2": "string"
      },
      "description": "string",
      "displayName": "string",
      "exports": [
        "string"
      ],
      "id": "string",
      "inputs": 0,
      "isDisabled": true,
      "minLogStreamVersion": "string",
      "outputs": 0,
      "settings": {},
      "source": "string",
      "spec": "string",
      "tags": {
        "dataType": [
          "string"
        ],
        "domain": [
          "string"
        ],
        "streamtags": [
          "string"
        ],
        "technology": [
          "string"
        ]
      },
      "version": "string"
    }
  ]
}
```

<h3 id="upgrade-a-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PackInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="upgrade-a-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PackInfo](#schemapackinfo)]|false|none|none|
|»» author|string|false|none|none|
|»» dependencies|object|false|none|none|
|»»» **additionalProperties**|string|false|none|none|
|»» description|string|false|none|none|
|»» displayName|string|false|none|none|
|»» exports|[string]|false|none|none|
|»» id|string|true|none|none|
|»» inputs|number|false|none|none|
|»» isDisabled|boolean|false|none|none|
|»» minLogStreamVersion|string|false|none|none|
|»» outputs|number|false|none|none|
|»» settings|object|false|none|none|
|»» source|string|true|none|none|
|»» spec|string|false|none|none|
|»» tags|object|false|none|none|
|»»» dataType|[string]|false|none|none|
|»»» domain|[string]|false|none|none|
|»»» streamtags|[string]|false|none|none|
|»»» technology|[string]|false|none|none|
|»» version|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Returns all entities that reference objects from outside of the pack

<a id="opIdgetPacksRefsById"></a>

> Code samples

`GET /packs/{id}/refs`

Returns all entities that reference objects from outside of the pack

<h3 id="returns-all-entities-that-reference-objects-from-outside-of-the-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Pack name|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "id": "string",
      "refs": [
        {
          "id": "string",
          "type": "string"
        }
      ],
      "type": "string"
    }
  ]
}
```

<h3 id="returns-all-entities-that-reference-objects-from-outside-of-the-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ReferencingEntity objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="returns-all-entities-that-reference-objects-from-outside-of-the-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ReferencingEntity](#schemareferencingentity)]|false|none|none|
|»» id|string|true|none|none|
|»» refs|[[ReferencedEntity](#schemareferencedentity)]|true|none|none|
|»»» id|string|true|none|none|
|»»» type|string|true|none|none|
|»» type|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Publish Pack back to its repository

<a id="opIdcreatePacksPublishById"></a>

> Code samples

`POST /packs/{id}/publish`

Publish Pack back to its repository

<h3 id="publish-pack-back-to-its-repository-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Pack name|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "git": {
        "branch": "string",
        "commitHash": "string",
        "commitMessage": "string"
      },
      "id": "string",
      "message": "string",
      "source": "string",
      "status": "error"
    }
  ]
}
```

<h3 id="publish-pack-back-to-its-repository-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PublishPackResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="publish-pack-back-to-its-repository-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PublishPackResponse](#schemapublishpackresponse)]|false|none|none|
|»» git|object|false|none|none|
|»»» branch|string|true|none|none|
|»»» commitHash|string|true|none|none|
|»»» commitMessage|string|true|none|none|
|»» id|string|true|none|none|
|»» message|string|false|none|none|
|»» source|string|false|none|none|
|»» status|[PublishPackToGitStatusType](#schemapublishpacktogitstatustype)|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|status|error|
|status|success|
|status|noop|

<aside class="success">
This operation does not require authentication
</aside>

## Clone Pack

<a id="opIdcreatePacksClone"></a>

> Code samples

`POST /packs/__clone__`

Clone Pack

> Body parameter

```json
{
  "dest": "string",
  "dstGroups": [
    "string"
  ],
  "force": true,
  "packs": [
    "string"
  ],
  "srcGroup": "string"
}
```

<h3 id="clone-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[PackClone](#schemapackclone)|true|PackClone object|
|» dest|body|string|false|none|
|» dstGroups|body|[string]|true|none|
|» force|body|boolean|false|none|
|» packs|body|[string]|true|none|
|» srcGroup|body|string|true|none|

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

<h3 id="clone-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="clone-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Export Pack

<a id="opIdgetPacksExportById"></a>

> Code samples

`GET /packs/{id}/export`

Export Pack

<h3 id="export-pack-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Pack name|
|mode|query|[PackExportModeTypes](#schemapackexportmodetypes)|true|Export mode. Note: "merge_safe" is deprecated and will be removed in v5.0.0. Use "merge" instead.|
|filename|query|string|false|Filename of the exported Pack|

#### Enumerated Values

|Parameter|Value|
|---|---|
|mode|merge_safe|
|mode|merge|
|mode|default_only|

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

<h3 id="export-pack-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="export-pack-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_PackInstallInfo">PackInstallInfo</h2>
<!-- backwards compatibility -->
<a id="schemapackinstallinfo"></a>
<a id="schema_PackInstallInfo"></a>
<a id="tocSpackinstallinfo"></a>
<a id="tocspackinstallinfo"></a>

```json
{
  "author": "string",
  "dependencies": {
    "property1": "string",
    "property2": "string"
  },
  "description": "string",
  "displayName": "string",
  "exports": [
    "string"
  ],
  "id": "string",
  "inputs": 0,
  "isDisabled": true,
  "minLogStreamVersion": "string",
  "outputs": 0,
  "settings": {},
  "source": "string",
  "spec": "string",
  "tags": {
    "dataType": [
      "string"
    ],
    "domain": [
      "string"
    ],
    "streamtags": [
      "string"
    ],
    "technology": [
      "string"
    ]
  },
  "version": "string",
  "warnings": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|author|string|false|none|none|
|dependencies|object|false|none|none|
|» **additionalProperties**|string|false|none|none|
|description|string|false|none|none|
|displayName|string|false|none|none|
|exports|[string]|false|none|none|
|id|string|true|none|none|
|inputs|number|false|none|none|
|isDisabled|boolean|false|none|none|
|minLogStreamVersion|string|false|none|none|
|outputs|number|false|none|none|
|settings|object|false|none|none|
|source|string|true|none|none|
|spec|string|false|none|none|
|tags|object|false|none|none|
|» dataType|[string]|false|none|none|
|» domain|[string]|false|none|none|
|» streamtags|[string]|false|none|none|
|» technology|[string]|false|none|none|
|version|string|false|none|none|
|warnings|[InstallWarnings](#schemainstallwarnings)|false|none|none|

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

<h2 id="tocS_PackRequestBody">PackRequestBody</h2>
<!-- backwards compatibility -->
<a id="schemapackrequestbody"></a>
<a id="schema_PackRequestBody"></a>
<a id="tocSpackrequestbody"></a>
<a id="tocspackrequestbody"></a>

```json
{
  "id": "string",
  "spec": "string",
  "version": "string",
  "minLogStreamVersion": "string",
  "displayName": "string",
  "author": "string",
  "description": "string",
  "source": "string",
  "tags": {
    "dataType": [
      "string"
    ],
    "domain": [
      "string"
    ],
    "technology": [
      "string"
    ],
    "streamtags": [
      "string"
    ]
  },
  "allowCustomFunctions": true,
  "force": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|false|none|none|
|spec|string|false|none|none|
|version|string|false|none|none|
|minLogStreamVersion|string|false|none|none|
|displayName|string|false|none|none|
|author|string|false|none|none|
|description|string|false|none|none|
|source|string|false|none|The source of the pack. If not present, an empty pack will be created|
|tags|object|false|none|none|
|» dataType|[string]|false|none|none|
|» domain|[string]|false|none|none|
|» technology|[string]|false|none|none|
|» streamtags|[string]|false|none|none|
|allowCustomFunctions|boolean|false|none|none|
|force|boolean|false|none|none|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|

<h2 id="tocS_PackInfo">PackInfo</h2>
<!-- backwards compatibility -->
<a id="schemapackinfo"></a>
<a id="schema_PackInfo"></a>
<a id="tocSpackinfo"></a>
<a id="tocspackinfo"></a>

```json
{
  "author": "string",
  "dependencies": {
    "property1": "string",
    "property2": "string"
  },
  "description": "string",
  "displayName": "string",
  "exports": [
    "string"
  ],
  "id": "string",
  "inputs": 0,
  "isDisabled": true,
  "minLogStreamVersion": "string",
  "outputs": 0,
  "settings": {},
  "source": "string",
  "spec": "string",
  "tags": {
    "dataType": [
      "string"
    ],
    "domain": [
      "string"
    ],
    "streamtags": [
      "string"
    ],
    "technology": [
      "string"
    ]
  },
  "version": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|author|string|false|none|none|
|dependencies|object|false|none|none|
|» **additionalProperties**|string|false|none|none|
|description|string|false|none|none|
|displayName|string|false|none|none|
|exports|[string]|false|none|none|
|id|string|true|none|none|
|inputs|number|false|none|none|
|isDisabled|boolean|false|none|none|
|minLogStreamVersion|string|false|none|none|
|outputs|number|false|none|none|
|settings|object|false|none|none|
|source|string|true|none|none|
|spec|string|false|none|none|
|tags|object|false|none|none|
|» dataType|[string]|false|none|none|
|» domain|[string]|false|none|none|
|» streamtags|[string]|false|none|none|
|» technology|[string]|false|none|none|
|version|string|false|none|none|

<h2 id="tocS_ReferencingEntity">ReferencingEntity</h2>
<!-- backwards compatibility -->
<a id="schemareferencingentity"></a>
<a id="schema_ReferencingEntity"></a>
<a id="tocSreferencingentity"></a>
<a id="tocsreferencingentity"></a>

```json
{
  "id": "string",
  "refs": [
    {
      "id": "string",
      "type": "string"
    }
  ],
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|refs|[[ReferencedEntity](#schemareferencedentity)]|true|none|none|
|type|string|true|none|none|

<h2 id="tocS_PublishPackResponse">PublishPackResponse</h2>
<!-- backwards compatibility -->
<a id="schemapublishpackresponse"></a>
<a id="schema_PublishPackResponse"></a>
<a id="tocSpublishpackresponse"></a>
<a id="tocspublishpackresponse"></a>

```json
{
  "git": {
    "branch": "string",
    "commitHash": "string",
    "commitMessage": "string"
  },
  "id": "string",
  "message": "string",
  "source": "string",
  "status": "error"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|git|object|false|none|none|
|» branch|string|true|none|none|
|» commitHash|string|true|none|none|
|» commitMessage|string|true|none|none|
|id|string|true|none|none|
|message|string|false|none|none|
|source|string|false|none|none|
|status|[PublishPackToGitStatusType](#schemapublishpacktogitstatustype)|false|none|none|

<h2 id="tocS_UploadPackResponse">UploadPackResponse</h2>
<!-- backwards compatibility -->
<a id="schemauploadpackresponse"></a>
<a id="schema_UploadPackResponse"></a>
<a id="tocSuploadpackresponse"></a>
<a id="tocsuploadpackresponse"></a>

```json
{
  "source": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|source|string|true|none|none|

<h2 id="tocS_PackClone">PackClone</h2>
<!-- backwards compatibility -->
<a id="schemapackclone"></a>
<a id="schema_PackClone"></a>
<a id="tocSpackclone"></a>
<a id="tocspackclone"></a>

```json
{
  "dest": "string",
  "dstGroups": [
    "string"
  ],
  "force": true,
  "packs": [
    "string"
  ],
  "srcGroup": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|dest|string|false|none|none|
|dstGroups|[string]|true|none|none|
|force|boolean|false|none|none|
|packs|[string]|true|none|none|
|srcGroup|string|true|none|none|

<h2 id="tocS_PackUpgradeRequest">PackUpgradeRequest</h2>
<!-- backwards compatibility -->
<a id="schemapackupgraderequest"></a>
<a id="schema_PackUpgradeRequest"></a>
<a id="tocSpackupgraderequest"></a>
<a id="tocspackupgraderequest"></a>

```json
{
  "allowCustomFunctions": true,
  "minor": "string",
  "source": "string",
  "spec": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|allowCustomFunctions|boolean|false|none|none|
|minor|string|false|none|none|
|source|string|true|none|none|
|spec|string|false|none|none|

<h2 id="tocS_PackExportModeTypes">PackExportModeTypes</h2>
<!-- backwards compatibility -->
<a id="schemapackexportmodetypes"></a>
<a id="schema_PackExportModeTypes"></a>
<a id="tocSpackexportmodetypes"></a>
<a id="tocspackexportmodetypes"></a>

```json
"merge_safe"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|merge_safe|
|*anonymous*|merge|
|*anonymous*|default_only|

<h2 id="tocS_InstallWarnings">InstallWarnings</h2>
<!-- backwards compatibility -->
<a id="schemainstallwarnings"></a>
<a id="schema_InstallWarnings"></a>
<a id="tocSinstallwarnings"></a>
<a id="tocsinstallwarnings"></a>

```json
[
  "string"
]

```

### Properties

*None*

<h2 id="tocS_ReferencedEntity">ReferencedEntity</h2>
<!-- backwards compatibility -->
<a id="schemareferencedentity"></a>
<a id="schema_ReferencedEntity"></a>
<a id="tocSreferencedentity"></a>
<a id="tocsreferencedentity"></a>

```json
{
  "id": "string",
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|type|string|true|none|none|

<h2 id="tocS_PublishPackToGitStatusType">PublishPackToGitStatusType</h2>
<!-- backwards compatibility -->
<a id="schemapublishpacktogitstatustype"></a>
<a id="schema_PublishPackToGitStatusType"></a>
<a id="tocSpublishpacktogitstatustype"></a>
<a id="tocspublishpacktogitstatustype"></a>

```json
"error"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|error|
|*anonymous*|success|
|*anonymous*|noop|

