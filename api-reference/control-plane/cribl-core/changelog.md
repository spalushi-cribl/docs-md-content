
<h1 id="cribl-core-api-changelog">Cribl Core API - Changelog v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-changelog-changelog">Changelog</h1>

## Get changelog viewed state

<a id="opIdgetChangelogViewed"></a>

> Code samples

`GET /changelog/viewed`

Get changelog viewed state

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "lastViewedCurrent": "string",
      "lastViewedUpgrade": "string"
    }
  ]
}
```

<h3 id="get-changelog-viewed-state-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of ChangelogState objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-changelog-viewed-state-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[ChangelogState](#schemachangelogstate)]|false|none|none|
|»» lastViewedCurrent|string|false|none|none|
|»» lastViewedUpgrade|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_ChangelogState">ChangelogState</h2>
<!-- backwards compatibility -->
<a id="schemachangelogstate"></a>
<a id="schema_ChangelogState"></a>
<a id="tocSchangelogstate"></a>
<a id="tocschangelogstate"></a>

```json
{
  "lastViewedCurrent": "string",
  "lastViewedUpgrade": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|lastViewedCurrent|string|false|none|none|
|lastViewedUpgrade|string|false|none|none|

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

