
<h1 id="cribl-stream-api-version-control-project-">Cribl Stream API - Version Control (Project) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-version-control-project--version-control-project-">Version Control (Project)</h1>

## Commit project changes.

<a id="opIdcreateSystemProjectsVersionCommitByProjectId"></a>

> Code samples

`POST /system/projects/{projectId}/version/commit`

Commit project changes.

> Body parameter

```json
{
  "effective": true,
  "message": "string"
}
```

<h3 id="commit-project-changes.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|
|body|body|[ProjectGitCommitParams](#schemaprojectgitcommitparams)|true|ProjectGitCommitParams object|
|» effective|body|boolean|false|none|
|» message|body|string|true|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "author": {
        "email": "string",
        "name": "string"
      },
      "branch": "string",
      "commit": "string",
      "files": {
        "created": [
          "string"
        ],
        "deleted": [
          "string"
        ],
        "modified": [
          "string"
        ],
        "renamed": [
          "string"
        ]
      },
      "summary": {
        "changes": 0,
        "deletions": 0,
        "insertions": 0
      }
    }
  ]
}
```

<h3 id="commit-project-changes.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitCommitSummary objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="commit-project-changes.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitCommitSummary](#schemagitcommitsummary)]|false|none|none|
|»» author|object|false|none|none|
|»»» email|string|true|none|none|
|»»» name|string|true|none|none|
|»» branch|string|true|none|none|
|»» commit|string|true|none|none|
|»» files|object|true|none|none|
|»»» created|[string]|false|none|none|
|»»» deleted|[string]|false|none|none|
|»»» modified|[string]|false|none|none|
|»»» renamed|[string]|false|none|none|
|»» summary|object|true|none|none|
|»»» changes|number|true|none|none|
|»»» deletions|number|true|none|none|
|»»» insertions|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## get the count of files of changed

<a id="opIdgetSystemProjectsVersionCountByProjectId"></a>

> Code samples

`GET /system/projects/{projectId}/version/count`

get the count of files of changed

<h3 id="get-the-count-of-files-of-changed-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project ID|
|ID|query|string|false|Commit ID|

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

<h3 id="get-the-count-of-files-of-changed-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-count-of-files-of-changed-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## get the textual diff for given commit

<a id="opIdgetSystemProjectsVersionDiffByProjectId"></a>

> Code samples

`GET /system/projects/{projectId}/version/diff`

get the textual diff for given commit

<h3 id="get-the-textual-diff-for-given-commit-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|
|commit|query|string|false|Commit hash (default is HEAD)|
|filename|query|string|false|Filename|
|diffLineLimit|query|number|false|Limit maximum lines in the diff|

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

<h3 id="get-the-textual-diff-for-given-commit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-textual-diff-for-given-commit-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## get the files changed

<a id="opIdgetSystemProjectsVersionFilesByProjectId"></a>

> Code samples

`GET /system/projects/{projectId}/version/files`

get the files changed

<h3 id="get-the-files-changed-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project ID|
|ID|query|string|false|Commit ID|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "commitMessage": {},
      "count": 0,
      "items": [
        {
          "children": [
            {}
          ],
          "name": "string",
          "state": "string"
        }
      ]
    }
  ]
}
```

<h3 id="get-the-files-changed-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitFilesResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-files-changed-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitFilesResponse](#schemagitfilesresponse)]|false|none|none|
|»» commitMessage|object|false|none|none|
|»» count|number|true|none|none|
|»» items|[[GitFile](#schemagitfile)]|true|none|none|
|»»» children|[[GitFile](#schemagitfile)]|false|none|none|
|»»» name|string|true|none|none|
|»»» state|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Revert project changes.

<a id="opIdcreateSystemProjectsVersionRevertByProjectId"></a>

> Code samples

`POST /system/projects/{projectId}/version/revert`

Revert project changes.

> Body parameter

```json
{
  "commit": "string",
  "force": true,
  "message": "string"
}
```

<h3 id="revert-project-changes.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|
|body|body|[GitRevertParams](#schemagitrevertparams)|true|GitRevertParams object|
|» commit|body|string|true|none|
|» force|body|boolean|false|none|
|» message|body|string|false|none|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "audit": {
        "files": {
          "created": [
            "string"
          ],
          "deleted": [
            "string"
          ],
          "modified": [
            "string"
          ],
          "renamed": [
            "string"
          ]
        },
        "group": "string",
        "id": "string"
      },
      "reverted": true
    }
  ]
}
```

<h3 id="revert-project-changes.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitRevertResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="revert-project-changes.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitRevertResult](#schemagitrevertresult)]|false|none|none|
|»» audit|object|true|none|none|
|»»» files|object|false|none|none|
|»»»» created|[string]|false|none|none|
|»»»» deleted|[string]|false|none|none|
|»»»» modified|[string]|false|none|none|
|»»»» renamed|[string]|false|none|none|
|»»» group|string|false|none|none|
|»»» id|string|true|none|none|
|»» reverted|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Show project changes.

<a id="opIdgetSystemProjectsVersionShowByProjectId"></a>

> Code samples

`GET /system/projects/{projectId}/version/show`

Show project changes.

<h3 id="show-project-changes.-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|projectId|path|string|true|Project Id|
|ID|query|string|false|Commit ID|
|filename|query|string|false|Filename|
|diffLineLimit|query|number|false|Limit maximum lines in the diff|

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

<h3 id="show-project-changes.-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="show-project-changes.-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_GitCommitSummary">GitCommitSummary</h2>
<!-- backwards compatibility -->
<a id="schemagitcommitsummary"></a>
<a id="schema_GitCommitSummary"></a>
<a id="tocSgitcommitsummary"></a>
<a id="tocsgitcommitsummary"></a>

```json
{
  "author": {
    "email": "string",
    "name": "string"
  },
  "branch": "string",
  "commit": "string",
  "files": {
    "created": [
      "string"
    ],
    "deleted": [
      "string"
    ],
    "modified": [
      "string"
    ],
    "renamed": [
      "string"
    ]
  },
  "summary": {
    "changes": 0,
    "deletions": 0,
    "insertions": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|author|object|false|none|none|
|» email|string|true|none|none|
|» name|string|true|none|none|
|branch|string|true|none|none|
|commit|string|true|none|none|
|files|object|true|none|none|
|» created|[string]|false|none|none|
|» deleted|[string]|false|none|none|
|» modified|[string]|false|none|none|
|» renamed|[string]|false|none|none|
|summary|object|true|none|none|
|» changes|number|true|none|none|
|» deletions|number|true|none|none|
|» insertions|number|true|none|none|

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

<h2 id="tocS_ProjectGitCommitParams">ProjectGitCommitParams</h2>
<!-- backwards compatibility -->
<a id="schemaprojectgitcommitparams"></a>
<a id="schema_ProjectGitCommitParams"></a>
<a id="tocSprojectgitcommitparams"></a>
<a id="tocsprojectgitcommitparams"></a>

```json
{
  "effective": true,
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|effective|boolean|false|none|none|
|message|string|true|none|none|

<h2 id="tocS_GitFilesResponse">GitFilesResponse</h2>
<!-- backwards compatibility -->
<a id="schemagitfilesresponse"></a>
<a id="schema_GitFilesResponse"></a>
<a id="tocSgitfilesresponse"></a>
<a id="tocsgitfilesresponse"></a>

```json
{
  "commitMessage": {},
  "count": 0,
  "items": [
    {
      "children": [
        {}
      ],
      "name": "string",
      "state": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|commitMessage|object|false|none|none|
|count|number|true|none|none|
|items|[[GitFile](#schemagitfile)]|true|none|none|

<h2 id="tocS_GitRevertResult">GitRevertResult</h2>
<!-- backwards compatibility -->
<a id="schemagitrevertresult"></a>
<a id="schema_GitRevertResult"></a>
<a id="tocSgitrevertresult"></a>
<a id="tocsgitrevertresult"></a>

```json
{
  "audit": {
    "files": {
      "created": [
        "string"
      ],
      "deleted": [
        "string"
      ],
      "modified": [
        "string"
      ],
      "renamed": [
        "string"
      ]
    },
    "group": "string",
    "id": "string"
  },
  "reverted": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|audit|object|true|none|none|
|» files|object|false|none|none|
|»» created|[string]|false|none|none|
|»» deleted|[string]|false|none|none|
|»» modified|[string]|false|none|none|
|»» renamed|[string]|false|none|none|
|» group|string|false|none|none|
|» id|string|true|none|none|
|reverted|boolean|true|none|none|

<h2 id="tocS_GitRevertParams">GitRevertParams</h2>
<!-- backwards compatibility -->
<a id="schemagitrevertparams"></a>
<a id="schema_GitRevertParams"></a>
<a id="tocSgitrevertparams"></a>
<a id="tocsgitrevertparams"></a>

```json
{
  "commit": "string",
  "force": true,
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|commit|string|true|none|none|
|force|boolean|false|none|none|
|message|string|false|none|none|

<h2 id="tocS_GitFile">GitFile</h2>
<!-- backwards compatibility -->
<a id="schemagitfile"></a>
<a id="schema_GitFile"></a>
<a id="tocSgitfile"></a>
<a id="tocsgitfile"></a>

```json
{
  "children": [
    {
      "children": [],
      "name": "string",
      "state": "string"
    }
  ],
  "name": "string",
  "state": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|children|[[GitFile](#schemagitfile)]|false|none|none|
|name|string|true|none|none|
|state|string|false|none|none|

