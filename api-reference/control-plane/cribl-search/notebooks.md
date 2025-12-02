
<h1 id="cribl-search-api-notebooks">Cribl Search API - Notebooks v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-search-api-notebooks-notebooks">Notebooks</h1>

## Get notebook activity history

<a id="opIdgetNotebookActivityById"></a>

> Code samples

`GET /search/notebooks/{id}/activity`

Get notebook activity history

<h3 id="get-notebook-activity-history-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Notebook ID|
|limit|query|integer|false|Maximum number of activity events to retrieve (default: 50, max: 1000)|
|offset|query|string|false|Offset for pagination (format: "filePath:rotationIdx:offset:reverseRead")|
|et|query|integer|false|Epoch timestamp of the earliest event|
|lt|query|integer|false|Epoch timestamp of the latest event|
|filter|query|string|false|Filter expression|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "endOfResults": true,
      "events": [
        {}
      ],
      "offset": "string"
    }
  ]
}
```

<h3 id="get-notebook-activity-history-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of NotebookActivityResult objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-notebook-activity-history-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[NotebookActivityResult](#schemanotebookactivityresult)]|false|none|none|
|»» endOfResults|boolean|true|none|none|
|»» events|[object]|true|none|none|
|»» offset|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_NotebookActivityResult">NotebookActivityResult</h2>
<!-- backwards compatibility -->
<a id="schemanotebookactivityresult"></a>
<a id="schema_NotebookActivityResult"></a>
<a id="tocSnotebookactivityresult"></a>
<a id="tocsnotebookactivityresult"></a>

```json
{
  "endOfResults": true,
  "events": [
    {}
  ],
  "offset": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|endOfResults|boolean|true|none|none|
|events|[object]|true|none|none|
|offset|string|true|none|none|

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

