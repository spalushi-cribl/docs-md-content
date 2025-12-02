
<h1 id="cribl-core-api-command-line-user-interface-clui-">Cribl Core API - Command Line User Interface (CLUI) v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-command-line-user-interface-clui--command-line-user-interface-clui-">Command Line User Interface (CLUI)</h1>

## Get CLUI search results

<a id="opIdgetClui"></a>

> Code samples

`GET /clui`

Get CLUI search results

<h3 id="get-clui-search-results-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|query|query|string|true|Search query|
|groupId|query|string|false|The <code>id</code> of the Worker Group or Edge Fleet to get results from. Required in Distributed deployments. Omit in Single-instance deployments.|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "category": "link",
      "groupId": "string",
      "id": "string",
      "name": "string",
      "packId": "string",
      "subType": "string",
      "type": "input"
    }
  ]
}
```

<h3 id="get-clui-search-results-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of CluiItem objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-clui-search-results-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[CluiItem](#schemacluiitem)]|false|none|none|
|»» category|string|true|none|none|
|»» groupId|string|false|none|none|
|»» id|string|false|none|none|
|»» name|string|false|none|none|
|»» packId|string|false|none|none|
|»» subType|string|false|none|none|
|»» type|[CluiType](#schemacluitype)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|category|link|
|type|input|
|type|output|
|type|route|
|type|pipeline|
|type|knowledge|
|type|collector|
|type|pack|
|type|monitoring|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_CluiItem">CluiItem</h2>
<!-- backwards compatibility -->
<a id="schemacluiitem"></a>
<a id="schema_CluiItem"></a>
<a id="tocScluiitem"></a>
<a id="tocscluiitem"></a>

```json
{
  "category": "link",
  "groupId": "string",
  "id": "string",
  "name": "string",
  "packId": "string",
  "subType": "string",
  "type": "input"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|category|string|true|none|none|
|groupId|string|false|none|none|
|id|string|false|none|none|
|name|string|false|none|none|
|packId|string|false|none|none|
|subType|string|false|none|none|
|type|[CluiType](#schemacluitype)|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|category|link|

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

<h2 id="tocS_CluiType">CluiType</h2>
<!-- backwards compatibility -->
<a id="schemacluitype"></a>
<a id="schema_CluiType"></a>
<a id="tocScluitype"></a>
<a id="tocscluitype"></a>

```json
"input"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|input|
|*anonymous*|output|
|*anonymous*|route|
|*anonymous*|pipeline|
|*anonymous*|knowledge|
|*anonymous*|collector|
|*anonymous*|pack|
|*anonymous*|monitoring|

