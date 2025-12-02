
<h1 id="management-plane-api">Management Plane API v1.0</h1>

> Scroll down for example requests and responses.

Public API for the Cribl.Cloud platform. Powers the Speakeasy SDK.

Base URLs:

* <a href="https://gateway.cribl.cloud">https://gateway.cribl.cloud</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- oAuth2 authentication. 

    - Flow: clientCredentials

    - Token URL = [https://login.cribl.cloud/oauth/token](https://login.cribl.cloud/oauth/token)

|Scope|Scope Description|
|---|---|

- HTTP Authentication, scheme: bearer 

<h1 id="management-plane-api-workspaces">workspaces</h1>

Operations related to Workspaces

## Create a Workspace in the specified Organization

<a id="opIdv1.workspaces.createWorkspace"></a>

> Code samples

`POST /v1/organizations/{organizationId}/workspaces`

Create a new Workspace in the specified Organization.

> Body parameter

```json
{
  "workspaceId": "main",
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}
```

<h3 id="create-a-workspace-in-the-specified-organization-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|organizationId|path|string|true|The <code>id</code> of the Organization where you want to create the Workspace.|
|body|body|[WorkspaceCreateRequestDTO](#schemaworkspacecreaterequestdto)|true|none|
|» workspaceId|body|string|true|Unique identifier for the workspace|
|» alias|body|string|false|User-friendly alias for the workspace|
|» description|body|string|false|Detailed description of the workspace|
|» tags|body|[string]|false|Tags associated with the workspace|

> Example responses

> 201 Response

```json
{
  "workspaceId": "main",
  "region": "us-west-2",
  "leaderFQDN": "workspace-leader.example.com",
  "state": "Active",
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}
```

<h3 id="create-a-workspace-in-the-specified-organization-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|The Workspace has been successfully created|[WorkspaceSchema](#schemaworkspaceschema)|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

## List all Workspaces for the specified Organization

<a id="opIdv1.workspaces.listWorkspaces"></a>

> Code samples

`GET /v1/organizations/{organizationId}/workspaces`

Get a list of all Workspaces for the specified Organization.

<h3 id="list-all-workspaces-for-the-specified-organization-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|organizationId|path|string|true|The <code>id</code> of the Organization that contains the Workspaces.|

> Example responses

> 200 Response

```json
{
  "items": [
    {
      "workspaceId": "main",
      "region": "us-west-2",
      "leaderFQDN": "workspace-leader.example.com",
      "state": "Active",
      "alias": "Production Environment",
      "description": "Main production workspace for customer data processing",
      "tags": [
        "production",
        "customer-data"
      ]
    }
  ],
  "count": 5
}
```

<h3 id="list-all-workspaces-for-the-specified-organization-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of Workspaces has been successfully retrieved|[WorkspacesListResponseDTO](#schemaworkspaceslistresponsedto)|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

## Update a Workspace

<a id="opIdv1.workspaces.updateWorkspace"></a>

> Code samples

`PATCH /v1/organizations/{organizationId}/workspaces/{workspaceId}`

Update the specified Workspace.

> Body parameter

```json
{
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}
```

<h3 id="update-a-workspace-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|organizationId|path|string|true|The <code>id</code> of the Organization that contains the Workspace.|
|workspaceId|path|string|true|The <code>id</code> of the Workspace to update.|
|body|body|[WorkspacePatchRequestDTO](#schemaworkspacepatchrequestdto)|true|none|
|» alias|body|string|false|User-friendly alias for the workspace|
|» description|body|string|false|Detailed description of the workspace|
|» tags|body|[string]|false|Tags associated with the workspace|

> Example responses

> default Response

```json
{
  "statusCode": 0,
  "message": "string"
}
```

<h3 id="update-a-workspace-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|The Workspace has been successfully updated|None|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

## Delete a Workspace

<a id="opIdv1.workspaces.deleteWorkspace"></a>

> Code samples

`DELETE /v1/organizations/{organizationId}/workspaces/{workspaceId}`

Delete the specified Workspace in the specified Organization.

<h3 id="delete-a-workspace-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|organizationId|path|string|true|The <code>id</code> of the Organization that contains the Workspace.|
|workspaceId|path|string|true|The <code>id</code> of the Workspace to delete.|

> Example responses

> default Response

```json
{
  "statusCode": 0,
  "message": "string"
}
```

<h3 id="delete-a-workspace-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|202|[Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)|The Workspace deletion has been accepted|None|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

## Get a Workspace

<a id="opIdv1.workspaces.getWorkspace"></a>

> Code samples

`GET /v1/organizations/{organizationId}/workspaces/{workspaceId}`

Get the specified Workspace.

<h3 id="get-a-workspace-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|organizationId|path|string|true|The <code>id</code> of the Organization that contains the Workspace.|
|workspaceId|path|string|true|The <code>id</code> of the Workspace to get.|

> Example responses

> 200 Response

```json
{
  "workspaceId": "main",
  "region": "us-west-2",
  "leaderFQDN": "workspace-leader.example.com",
  "state": "Active",
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}
```

<h3 id="get-a-workspace-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The Workspace details have been retrieved|[WorkspaceSchema](#schemaworkspaceschema)|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

<h1 id="management-plane-api-health">health</h1>

Operations related to application health status

## Get the health status of the application

<a id="opIdgetHealthStatus"></a>

> Code samples

`GET /`

> Example responses

> 200 Response

```json
{
  "status": "OK"
}
```

<h3 id="get-the-health-status-of-the-application-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Health status|Inline|
|default|Default|Default error response|[DefaultErrorDTO](#schemadefaulterrordto)|

<h3 id="get-the-health-status-of-the-application-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» status|string|false|none|none|

<aside class="warning">
To perform this operation, you must be authenticated by means of one of the following methods:
clientOauth, bearerAuth
</aside>

# Schemas

<h2 id="tocS_AWSRegions">AWSRegions</h2>
<!-- backwards compatibility -->
<a id="schemaawsregions"></a>
<a id="schema_AWSRegions"></a>
<a id="tocSawsregions"></a>
<a id="tocsawsregions"></a>

```json
"us-west-2"

```

AWS region where the workspace is deployed

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|AWS region where the workspace is deployed|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|us-west-2|
|*anonymous*|us-east-1|
|*anonymous*|us-east-2|
|*anonymous*|eu-central-1|
|*anonymous*|eu-central-2|
|*anonymous*|eu-west-2|
|*anonymous*|ap-southeast-1|
|*anonymous*|ap-southeast-2|
|*anonymous*|ca-central-1|

<h2 id="tocS_WorkspaceState">WorkspaceState</h2>
<!-- backwards compatibility -->
<a id="schemaworkspacestate"></a>
<a id="schema_WorkspaceState"></a>
<a id="tocSworkspacestate"></a>
<a id="tocsworkspacestate"></a>

```json
"Provisioning"

```

Current state of the workspace

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Current state of the workspace|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|Provisioning|
|*anonymous*|Active|
|*anonymous*|Inactive|
|*anonymous*|Failed|
|*anonymous*|Deprovisioning|

<h2 id="tocS_WorkspaceSchema">WorkspaceSchema</h2>
<!-- backwards compatibility -->
<a id="schemaworkspaceschema"></a>
<a id="schema_WorkspaceSchema"></a>
<a id="tocSworkspaceschema"></a>
<a id="tocsworkspaceschema"></a>

```json
{
  "workspaceId": "main",
  "region": "us-west-2",
  "leaderFQDN": "workspace-leader.example.com",
  "state": "Active",
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|workspaceId|string|true|none|Unique identifier for the workspace|
|region|[AWSRegions](#schemaawsregions)|true|none|AWS region where the workspace is deployed|
|leaderFQDN|string|true|none|Fully Qualified Domain Name of the workspace leader|
|state|[WorkspaceState](#schemaworkspacestate)|true|none|Current state of the workspace|
|alias|string|false|none|User-friendly alias for the workspace|
|description|string|false|none|Detailed description of the workspace|
|tags|[string]|false|none|Tags associated with the workspace|

<h2 id="tocS_WorkspaceCreateRequestDTO">WorkspaceCreateRequestDTO</h2>
<!-- backwards compatibility -->
<a id="schemaworkspacecreaterequestdto"></a>
<a id="schema_WorkspaceCreateRequestDTO"></a>
<a id="tocSworkspacecreaterequestdto"></a>
<a id="tocsworkspacecreaterequestdto"></a>

```json
{
  "workspaceId": "main",
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|workspaceId|string|true|none|Unique identifier for the workspace|
|alias|string|false|none|User-friendly alias for the workspace|
|description|string|false|none|Detailed description of the workspace|
|tags|[string]|false|none|Tags associated with the workspace|

<h2 id="tocS_WorkspacePatchRequestDTO">WorkspacePatchRequestDTO</h2>
<!-- backwards compatibility -->
<a id="schemaworkspacepatchrequestdto"></a>
<a id="schema_WorkspacePatchRequestDTO"></a>
<a id="tocSworkspacepatchrequestdto"></a>
<a id="tocsworkspacepatchrequestdto"></a>

```json
{
  "alias": "Production Environment",
  "description": "Main production workspace for customer data processing",
  "tags": [
    "production",
    "customer-data"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|alias|string|false|none|User-friendly alias for the workspace|
|description|string|false|none|Detailed description of the workspace|
|tags|[string]|false|none|Tags associated with the workspace|

<h2 id="tocS_WorkspacesListResponseDTO">WorkspacesListResponseDTO</h2>
<!-- backwards compatibility -->
<a id="schemaworkspaceslistresponsedto"></a>
<a id="schema_WorkspacesListResponseDTO"></a>
<a id="tocSworkspaceslistresponsedto"></a>
<a id="tocsworkspaceslistresponsedto"></a>

```json
{
  "items": [
    {
      "workspaceId": "main",
      "region": "us-west-2",
      "leaderFQDN": "workspace-leader.example.com",
      "state": "Active",
      "alias": "Production Environment",
      "description": "Main production workspace for customer data processing",
      "tags": [
        "production",
        "customer-data"
      ]
    }
  ],
  "count": 5
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|items|[[WorkspaceSchema](#schemaworkspaceschema)]|true|none|List of workspaces|
|count|number|true|none|Total number of workspaces|

<h2 id="tocS_DefaultErrorDTO">DefaultErrorDTO</h2>
<!-- backwards compatibility -->
<a id="schemadefaulterrordto"></a>
<a id="schema_DefaultErrorDTO"></a>
<a id="tocSdefaulterrordto"></a>
<a id="tocsdefaulterrordto"></a>

```json
{
  "statusCode": 0,
  "message": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|statusCode|number|true|none|none|
|message|string|true|none|none|

