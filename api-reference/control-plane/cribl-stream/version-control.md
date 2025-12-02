
<h1 id="cribl-stream-api-version-control">Cribl Stream API - Version Control v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-stream-api-version-control-version-control">Version Control</h1>

## List all branches in the Git repository used for Cribl configuration

<a id="opIdgetVersionBranch"></a>

> Code samples

`GET /version/branch`

Get a list of all branches in the Git repository used for Cribl configuration.

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

<h3 id="list-all-branches-in-the-git-repository-used-for-cribl-configuration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of BranchInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="list-all-branches-in-the-git-repository-used-for-cribl-configuration-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[BranchInfo](#schemabranchinfo)]|false|none|none|
|»» id|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Create a new commit for pending changes to the Cribl configuration

<a id="opIdcreateVersionCommit"></a>

> Code samples

`POST /version/commit`

Create a new commit for pending changes to the Cribl configuration. Any merge conflicts indicated in the response must be resolved using Git.</br></br>To commit only a subset of configuration changes, specify the files to include in the commit in the <code>files</code> array.

> Body parameter

```json
{
  "effective": true,
  "files": [
    "string"
  ],
  "group": "string",
  "message": "string"
}
```

<h3 id="create-a-new-commit-for-pending-changes-to-the-cribl-configuration-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to create a new commit for.|
|body|body|[GitCommitParams](#schemagitcommitparams)|true|GitCommitParams object|
|» effective|body|boolean|false|none|
|» files|body|[string]|false|none|
|» group|body|string|false|none|
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

<h3 id="create-a-new-commit-for-pending-changes-to-the-cribl-configuration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitCommitSummary objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-a-new-commit-for-pending-changes-to-the-cribl-configuration-responseschema">Response Schema</h3>

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

## Get a count of files that changed since a commit

<a id="opIdgetVersionCount"></a>

> Code samples

`GET /version/count`

Get a count of the files that changed since a commit. Default is the latest commit (HEAD).

<h3 id="get-a-count-of-files-that-changed-since-a-commit-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get the count for.|
|ID|query|string|false|The Git commit hash to use as the starting point for the count.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0
    }
  ]
}
```

<h3 id="get-a-count-of-files-that-changed-since-a-commit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitCountResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-count-of-files-that-changed-since-a-commit-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitCountResult](#schemagitcountresult)]|false|none|none|
|»» count|number|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the name of the Git branch that the Cribl configuration is checked out to

<a id="opIdgetVersionCurrentBranch"></a>

> Code samples

`GET /version/current-branch`

Get the name of the Git branch that the Cribl configuration is checked out to. Useful for verifying the active configuration branch.

> Example responses

> 200 Response

```json
{
  "branch": "string"
}
```

<h3 id="get-the-name-of-the-git-branch-that-the-cribl-configuration-is-checked-out-to-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|CurrentBranchResult object|[CurrentBranchResult](#schemacurrentbranchresult)|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<aside class="success">
This operation does not require authentication
</aside>

## Get the diff for a commit

<a id="opIdgetVersionDiff"></a>

> Code samples

`GET /version/diff`

Get the diff for a commit. Default is the latest commit (HEAD).

<h3 id="get-the-diff-for-a-commit-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|commit|query|string|false|The Git commit hash to get the diff for.|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get the diff for.|
|filename|query|string|false|The relative path of the file to get the diff for.|
|diffLineLimit|query|number|false|Number of lines of the diff to return. Default is 1000. Set to <code>0</code> to return the full diff, regardless of the number of lines.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "diffJson": [
        {
          "addedLines": 0,
          "blocks": [
            {
              "header": "string",
              "lines": [
                {
                  "content": "string",
                  "oldNumber": 0
                }
              ],
              "newStartLine": 0,
              "oldStartLine": 0,
              "oldStartLine2": 0
            }
          ],
          "changedPercentage": 0,
          "checksumAfter": "string",
          "checksumBefore": "string",
          "deletedFileMode": "string",
          "deletedLines": 0,
          "isBinary": true,
          "isCombined": true,
          "isCopy": true,
          "isDeleted": true,
          "isGitDiff": true,
          "isNew": true,
          "isRename": true,
          "isTooBig": true,
          "language": "string",
          "mode": "string",
          "newFileMode": "string",
          "newMode": "string",
          "newName": "string",
          "oldMode": "string",
          "oldName": "string",
          "unchangedPercentage": 0
        }
      ]
    }
  ]
}
```

<h3 id="get-the-diff-for-a-commit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitDiffResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-diff-for-a-commit-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitDiffResult](#schemagitdiffresult)]|false|none|none|
|»» diffJson|[object]|true|none|none|
|»»» addedLines|number|true|none|none|
|»»» blocks|[object]|true|none|none|
|»»»» header|string|true|none|none|
|»»»» lines|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» oldNumber|number|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» newNumber|number|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» newNumber|number|true|none|none|
|»»»»»» oldNumber|number|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» newStartLine|number|true|none|none|
|»»»» oldStartLine|number|true|none|none|
|»»»» oldStartLine2|number|false|none|none|
|»»» changedPercentage|number|false|none|none|
|»»» checksumAfter|string|false|none|none|
|»»» checksumBefore|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» deletedFileMode|string|false|none|none|
|»»» deletedLines|number|true|none|none|
|»»» isBinary|boolean|false|none|none|
|»»» isCombined|boolean|true|none|none|
|»»» isCopy|boolean|false|none|none|
|»»» isDeleted|boolean|false|none|none|
|»»» isGitDiff|boolean|true|none|none|
|»»» isNew|boolean|false|none|none|
|»»» isRename|boolean|false|none|none|
|»»» isTooBig|boolean|false|none|none|
|»»» language|string|true|none|none|
|»»» mode|string|false|none|none|
|»»» newFileMode|string|false|none|none|
|»»» newMode|string|false|none|none|
|»»» newName|string|true|none|none|
|»»» oldMode|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» oldName|string|true|none|none|
|»»» unchangedPercentage|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the names and statuses of files that changed since a commit

<a id="opIdgetVersionFiles"></a>

> Code samples

`GET /version/files`

Get the names and statuses of files that changed since a commit. Default is the latest commit (HEAD).

<h3 id="get-the-names-and-statuses-of-files-that-changed-since-a-commit-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get file names and status for.|
|ID|query|string|false|The Git commit hash to use as the starting point for the request.|

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

<h3 id="get-the-names-and-statuses-of-files-that-changed-since-a-commit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitFilesResponse objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-names-and-statuses-of-files-that-changed-since-a-commit-responseschema">Response Schema</h3>

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

## Get the configuration and status for the Git integration

<a id="opIdgetVersionInfo"></a>

> Code samples

`GET /version/info`

Get the configuration and versioning status for the Git integration for the Cribl configuration.

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "remote": "string",
      "versioning": true
    }
  ]
}
```

<h3 id="get-the-configuration-and-status-for-the-git-integration-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitInfo objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-configuration-and-status-for-the-git-integration-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitInfo](#schemagitinfo)]|false|none|none|
|»» remote|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|boolean|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» versioning|boolean|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|false|

<aside class="success">
This operation does not require authentication
</aside>

## Push local commits to the remote repository

<a id="opIdcreateVersionPush"></a>

> Code samples

`POST /version/push`

Push all local commits from the local repository to the remote repository.

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

<h3 id="push-local-commits-to-the-remote-repository-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of string objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="push-local-commits-to-the-remote-repository-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[string]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Revert a commit in the local repository

<a id="opIdcreateVersionRevert"></a>

> Code samples

`POST /version/revert`

Revert a commit in the local repository.

> Body parameter

```json
{
  "commit": "string",
  "force": true,
  "message": "string"
}
```

<h3 id="revert-a-commit-in-the-local-repository-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to revert the commit for. Required in Distributed deployments. Omit in Single-instance deployments.|
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

<h3 id="revert-a-commit-in-the-local-repository-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitRevertResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="revert-a-commit-in-the-local-repository-responseschema">Response Schema</h3>

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

## Get the diff and log message for a commit

<a id="opIdgetVersionShow"></a>

> Code samples

`GET /version/show`

Get the diff and log message for a commit. Default is the latest commit (HEAD).

<h3 id="get-the-diff-and-log-message-for-a-commit-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|commit|query|string|false|The Git commit hash to retrieve the diff and log message for.|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get the diff and log message for.|
|filename|query|string|false|The relative path of the file to get the diff and log message for.|
|diffLineLimit|query|number|false|Number of lines of the diff to return. Default is 1000. Set to <code>0</code> to return the full diff, regardless of the number of lines.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "commitMessage": "string",
      "diffJson": [
        {
          "addedLines": 0,
          "blocks": [
            {
              "header": "string",
              "lines": [
                {
                  "content": "string",
                  "oldNumber": 0
                }
              ],
              "newStartLine": 0,
              "oldStartLine": 0,
              "oldStartLine2": 0
            }
          ],
          "changedPercentage": 0,
          "checksumAfter": "string",
          "checksumBefore": "string",
          "deletedFileMode": "string",
          "deletedLines": 0,
          "isBinary": true,
          "isCombined": true,
          "isCopy": true,
          "isDeleted": true,
          "isGitDiff": true,
          "isNew": true,
          "isRename": true,
          "isTooBig": true,
          "language": "string",
          "mode": "string",
          "newFileMode": "string",
          "newMode": "string",
          "newName": "string",
          "oldMode": "string",
          "oldName": "string",
          "unchangedPercentage": 0
        }
      ]
    }
  ]
}
```

<h3 id="get-the-diff-and-log-message-for-a-commit-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitShowResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-diff-and-log-message-for-a-commit-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitShowResult](#schemagitshowresult)]|false|none|none|
|»» commitMessage|string|true|none|none|
|»» diffJson|[object]|true|none|none|
|»»» addedLines|number|true|none|none|
|»»» blocks|[object]|true|none|none|
|»»»» header|string|true|none|none|
|»»»» lines|[oneOf]|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» oldNumber|number|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» newNumber|number|true|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|object|false|none|none|
|»»»»»» content|string|true|none|none|
|»»»»»» newNumber|number|true|none|none|
|»»»»»» oldNumber|number|true|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» newStartLine|number|true|none|none|
|»»»» oldStartLine|number|true|none|none|
|»»»» oldStartLine2|number|false|none|none|
|»»» changedPercentage|number|false|none|none|
|»»» checksumAfter|string|false|none|none|
|»»» checksumBefore|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» deletedFileMode|string|false|none|none|
|»»» deletedLines|number|true|none|none|
|»»» isBinary|boolean|false|none|none|
|»»» isCombined|boolean|true|none|none|
|»»» isCopy|boolean|false|none|none|
|»»» isDeleted|boolean|false|none|none|
|»»» isGitDiff|boolean|true|none|none|
|»»» isNew|boolean|false|none|none|
|»»» isRename|boolean|false|none|none|
|»»» isTooBig|boolean|false|none|none|
|»»» language|string|true|none|none|
|»»» mode|string|false|none|none|
|»»» newFileMode|string|false|none|none|
|»»» newMode|string|false|none|none|
|»»» newName|string|true|none|none|
|»»» oldMode|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|[string]|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» oldName|string|true|none|none|
|»»» unchangedPercentage|number|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get the status of the current working tree

<a id="opIdgetVersionStatus"></a>

> Code samples

`GET /version/status`

Get the status of the current working tree of the Git repository used for Cribl configuration. The response includes details about modified, staged, untracked, and conflicted files, as well as branch and remote tracking information.

<h3 id="get-the-status-of-the-current-working-tree-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get the status for.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "ahead": 0,
      "behind": 0,
      "conflicted": [
        "string"
      ],
      "created": [
        "string"
      ],
      "current": "string",
      "deleted": [
        "string"
      ],
      "files": [
        {
          "index": "string",
          "path": "string",
          "working_dir": "string"
        }
      ],
      "modified": [
        "string"
      ],
      "not_added": [
        "string"
      ],
      "renamed": [
        {
          "from": "string",
          "to": "string"
        }
      ],
      "staged": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-the-status-of-the-current-working-tree-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitStatusResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-the-status-of-the-current-working-tree-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitStatusResult](#schemagitstatusresult)]|false|none|none|
|»» ahead|number|true|none|none|
|»» behind|number|true|none|none|
|»» conflicted|[string]|true|none|none|
|»» created|[string]|true|none|none|
|»» current|string|true|none|none|
|»» deleted|[string]|true|none|none|
|»» files|[object]|true|none|none|
|»»» index|string|true|none|none|
|»»» path|string|true|none|none|
|»»» working_dir|string|true|none|none|
|»» modified|[string]|true|none|none|
|»» not_added|[string]|true|none|none|
|»» renamed|[object]|true|none|none|
|»»» from|string|true|none|none|
|»»» to|string|true|none|none|
|»» staged|[string]|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Synchronize the local branch with the remote repository

<a id="opIdcreateVersionSync"></a>

> Code samples

`POST /version/sync`

Synchronize the local branch with the remote repository. Any merge conflicts indicated in the response must be resolved using Git.

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

<h3 id="synchronize-the-local-branch-with-the-remote-repository-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of any objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="synchronize-the-local-branch-with-the-remote-repository-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Discard uncommitted (staged) changes

<a id="opIdcreateVersionUndo"></a>

> Code samples

`POST /version/undo`

Discard all uncommitted (staged) configuration changes, resetting the working directory to the last committed state. Use only if you are certain that you do not need to preserve your local changes.

<h3 id="discard-uncommitted-(staged)-changes-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to undo the uncommited changes for.|

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

<h3 id="discard-uncommitted-(staged)-changes-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of object objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="discard-uncommitted-(staged)-changes-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[object]|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_BranchInfo">BranchInfo</h2>
<!-- backwards compatibility -->
<a id="schemabranchinfo"></a>
<a id="schema_BranchInfo"></a>
<a id="tocSbranchinfo"></a>
<a id="tocsbranchinfo"></a>

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

<h2 id="tocS_GitCommitParams">GitCommitParams</h2>
<!-- backwards compatibility -->
<a id="schemagitcommitparams"></a>
<a id="schema_GitCommitParams"></a>
<a id="tocSgitcommitparams"></a>
<a id="tocsgitcommitparams"></a>

```json
{
  "effective": true,
  "files": [
    "string"
  ],
  "group": "string",
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|effective|boolean|false|none|none|
|files|[string]|false|none|none|
|group|string|false|none|none|
|message|string|true|none|none|

<h2 id="tocS_GitCountResult">GitCountResult</h2>
<!-- backwards compatibility -->
<a id="schemagitcountresult"></a>
<a id="schema_GitCountResult"></a>
<a id="tocSgitcountresult"></a>
<a id="tocsgitcountresult"></a>

```json
{
  "count": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|count|number|true|none|none|

<h2 id="tocS_CurrentBranchResult">CurrentBranchResult</h2>
<!-- backwards compatibility -->
<a id="schemacurrentbranchresult"></a>
<a id="schema_CurrentBranchResult"></a>
<a id="tocScurrentbranchresult"></a>
<a id="tocscurrentbranchresult"></a>

```json
{
  "branch": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|branch|string|true|none|none|

<h2 id="tocS_GitDiffResult">GitDiffResult</h2>
<!-- backwards compatibility -->
<a id="schemagitdiffresult"></a>
<a id="schema_GitDiffResult"></a>
<a id="tocSgitdiffresult"></a>
<a id="tocsgitdiffresult"></a>

```json
{
  "diffJson": [
    {
      "addedLines": 0,
      "blocks": [
        {
          "header": "string",
          "lines": [
            {
              "content": "string",
              "oldNumber": 0
            }
          ],
          "newStartLine": 0,
          "oldStartLine": 0,
          "oldStartLine2": 0
        }
      ],
      "changedPercentage": 0,
      "checksumAfter": "string",
      "checksumBefore": "string",
      "deletedFileMode": "string",
      "deletedLines": 0,
      "isBinary": true,
      "isCombined": true,
      "isCopy": true,
      "isDeleted": true,
      "isGitDiff": true,
      "isNew": true,
      "isRename": true,
      "isTooBig": true,
      "language": "string",
      "mode": "string",
      "newFileMode": "string",
      "newMode": "string",
      "newName": "string",
      "oldMode": "string",
      "oldName": "string",
      "unchangedPercentage": 0
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|diffJson|[DiffFiles](#schemadifffiles)|true|none|none|

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

<h2 id="tocS_GitInfo">GitInfo</h2>
<!-- backwards compatibility -->
<a id="schemagitinfo"></a>
<a id="schema_GitInfo"></a>
<a id="tocSgitinfo"></a>
<a id="tocsgitinfo"></a>

```json
{
  "remote": "string",
  "versioning": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|remote|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|boolean|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|versioning|boolean|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|false|

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

<h2 id="tocS_GitShowResult">GitShowResult</h2>
<!-- backwards compatibility -->
<a id="schemagitshowresult"></a>
<a id="schema_GitShowResult"></a>
<a id="tocSgitshowresult"></a>
<a id="tocsgitshowresult"></a>

```json
{
  "commitMessage": "string",
  "diffJson": [
    {
      "addedLines": 0,
      "blocks": [
        {
          "header": "string",
          "lines": [
            {
              "content": "string",
              "oldNumber": 0
            }
          ],
          "newStartLine": 0,
          "oldStartLine": 0,
          "oldStartLine2": 0
        }
      ],
      "changedPercentage": 0,
      "checksumAfter": "string",
      "checksumBefore": "string",
      "deletedFileMode": "string",
      "deletedLines": 0,
      "isBinary": true,
      "isCombined": true,
      "isCopy": true,
      "isDeleted": true,
      "isGitDiff": true,
      "isNew": true,
      "isRename": true,
      "isTooBig": true,
      "language": "string",
      "mode": "string",
      "newFileMode": "string",
      "newMode": "string",
      "newName": "string",
      "oldMode": "string",
      "oldName": "string",
      "unchangedPercentage": 0
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|commitMessage|string|true|none|none|
|diffJson|[DiffFiles](#schemadifffiles)|true|none|none|

<h2 id="tocS_GitStatusResult">GitStatusResult</h2>
<!-- backwards compatibility -->
<a id="schemagitstatusresult"></a>
<a id="schema_GitStatusResult"></a>
<a id="tocSgitstatusresult"></a>
<a id="tocsgitstatusresult"></a>

```json
{
  "ahead": 0,
  "behind": 0,
  "conflicted": [
    "string"
  ],
  "created": [
    "string"
  ],
  "current": "string",
  "deleted": [
    "string"
  ],
  "files": [
    {
      "index": "string",
      "path": "string",
      "working_dir": "string"
    }
  ],
  "modified": [
    "string"
  ],
  "not_added": [
    "string"
  ],
  "renamed": [
    {
      "from": "string",
      "to": "string"
    }
  ],
  "staged": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ahead|number|true|none|none|
|behind|number|true|none|none|
|conflicted|[string]|true|none|none|
|created|[string]|true|none|none|
|current|string|true|none|none|
|deleted|[string]|true|none|none|
|files|[object]|true|none|none|
|» index|string|true|none|none|
|» path|string|true|none|none|
|» working_dir|string|true|none|none|
|modified|[string]|true|none|none|
|not_added|[string]|true|none|none|
|renamed|[object]|true|none|none|
|» from|string|true|none|none|
|» to|string|true|none|none|
|staged|[string]|true|none|none|

<h2 id="tocS_DiffFiles">DiffFiles</h2>
<!-- backwards compatibility -->
<a id="schemadifffiles"></a>
<a id="schema_DiffFiles"></a>
<a id="tocSdifffiles"></a>
<a id="tocsdifffiles"></a>

```json
[
  {
    "addedLines": 0,
    "blocks": [
      {
        "header": "string",
        "lines": [
          {
            "content": "string",
            "oldNumber": 0
          }
        ],
        "newStartLine": 0,
        "oldStartLine": 0,
        "oldStartLine2": 0
      }
    ],
    "changedPercentage": 0,
    "checksumAfter": "string",
    "checksumBefore": "string",
    "deletedFileMode": "string",
    "deletedLines": 0,
    "isBinary": true,
    "isCombined": true,
    "isCopy": true,
    "isDeleted": true,
    "isGitDiff": true,
    "isNew": true,
    "isRename": true,
    "isTooBig": true,
    "language": "string",
    "mode": "string",
    "newFileMode": "string",
    "newMode": "string",
    "newName": "string",
    "oldMode": "string",
    "oldName": "string",
    "unchangedPercentage": 0
  }
]

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|addedLines|number|true|none|none|
|blocks|[object]|true|none|none|
|» header|string|true|none|none|
|» lines|[oneOf]|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|true|none|none|
|»»» oldNumber|number|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|true|none|none|
|»»» newNumber|number|true|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|object|false|none|none|
|»»» content|string|true|none|none|
|»»» newNumber|number|true|none|none|
|»»» oldNumber|number|true|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» newStartLine|number|true|none|none|
|» oldStartLine|number|true|none|none|
|» oldStartLine2|number|false|none|none|
|changedPercentage|number|false|none|none|
|checksumAfter|string|false|none|none|
|checksumBefore|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|deletedFileMode|string|false|none|none|
|deletedLines|number|true|none|none|
|isBinary|boolean|false|none|none|
|isCombined|boolean|true|none|none|
|isCopy|boolean|false|none|none|
|isDeleted|boolean|false|none|none|
|isGitDiff|boolean|true|none|none|
|isNew|boolean|false|none|none|
|isRename|boolean|false|none|none|
|isTooBig|boolean|false|none|none|
|language|string|true|none|none|
|mode|string|false|none|none|
|newFileMode|string|false|none|none|
|newMode|string|false|none|none|
|newName|string|true|none|none|
|oldMode|any|false|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[string]|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|oldName|string|true|none|none|
|unchangedPercentage|number|false|none|none|

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

