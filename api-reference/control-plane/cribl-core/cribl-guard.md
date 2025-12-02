
<h1 id="cribl-core-api-cribl-guard">Cribl Core API - Cribl Guard v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-cribl-guard-cribl-guard">Cribl Guard</h1>

## Get a list of SensitiveDataRule objects

<a id="opIdlistSensitiveDataRule"></a>

> Code samples

`GET /lib/sds-rules`

Get a list of SensitiveDataRule objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "contextKeywords": [
        {
          "keyword": "string",
          "placement": "before"
        }
      ],
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "regex": "string",
      "rulesets": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-a-list-of-sensitivedatarule-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-sensitivedatarule-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRule](#schemasensitivedatarule)]|false|none|none|
|»» contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|»»» keyword|string|true|none|none|
|»»» placement|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» regex|string|true|none|none|
|»» rulesets|[string]|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Create SensitiveDataRule

<a id="opIdcreateSensitiveDataRule"></a>

> Code samples

`POST /lib/sds-rules`

Create SensitiveDataRule

> Body parameter

```json
{
  "contextKeywords": [
    {
      "keyword": "string",
      "placement": "before"
    }
  ],
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "regex": "string",
  "rulesets": [
    "string"
  ]
}
```

<h3 id="create-sensitivedatarule-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SensitiveDataRule](#schemasensitivedatarule)|true|New SensitiveDataRule object|
|» contextKeywords|body|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|
|»» keyword|body|string|true|none|
|»» placement|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|false|none|
|» regex|body|string|true|none|
|» rulesets|body|[string]|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» placement|before|
|»» placement|after|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "contextKeywords": [
        {
          "keyword": "string",
          "placement": "before"
        }
      ],
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "regex": "string",
      "rulesets": [
        "string"
      ]
    }
  ]
}
```

<h3 id="create-sensitivedatarule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-sensitivedatarule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRule](#schemasensitivedatarule)]|false|none|none|
|»» contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|»»» keyword|string|true|none|none|
|»»» placement|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» regex|string|true|none|none|
|»» rulesets|[string]|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Get SensitiveDataRule by ID

<a id="opIdgetSensitiveDataRuleById"></a>

> Code samples

`GET /lib/sds-rules/{id}`

Get SensitiveDataRule by ID

<h3 id="get-sensitivedatarule-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "contextKeywords": [
        {
          "keyword": "string",
          "placement": "before"
        }
      ],
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "regex": "string",
      "rulesets": [
        "string"
      ]
    }
  ]
}
```

<h3 id="get-sensitivedatarule-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-sensitivedatarule-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRule](#schemasensitivedatarule)]|false|none|none|
|»» contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|»»» keyword|string|true|none|none|
|»»» placement|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» regex|string|true|none|none|
|»» rulesets|[string]|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Update SensitiveDataRule

<a id="opIdupdateSensitiveDataRuleById"></a>

> Code samples

`PATCH /lib/sds-rules/{id}`

Update SensitiveDataRule

> Body parameter

```json
{
  "contextKeywords": [
    {
      "keyword": "string",
      "placement": "before"
    }
  ],
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "regex": "string",
  "rulesets": [
    "string"
  ]
}
```

<h3 id="update-sensitivedatarule-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[SensitiveDataRule](#schemasensitivedatarule)|true|SensitiveDataRule object to be updated|
|» contextKeywords|body|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|
|»» keyword|body|string|true|none|
|»» placement|body|string|false|none|
|» description|body|string|false|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|false|none|
|» regex|body|string|true|none|
|» rulesets|body|[string]|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» placement|before|
|»» placement|after|
|» lib|cribl|
|» lib|cribl-custom|
|» lib|custom|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "contextKeywords": [
        {
          "keyword": "string",
          "placement": "before"
        }
      ],
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "regex": "string",
      "rulesets": [
        "string"
      ]
    }
  ]
}
```

<h3 id="update-sensitivedatarule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-sensitivedatarule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRule](#schemasensitivedatarule)]|false|none|none|
|»» contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|»»» keyword|string|true|none|none|
|»»» placement|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» regex|string|true|none|none|
|»» rulesets|[string]|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SensitiveDataRule

<a id="opIddeleteSensitiveDataRuleById"></a>

> Code samples

`DELETE /lib/sds-rules/{id}`

Delete SensitiveDataRule

<h3 id="delete-sensitivedatarule-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "contextKeywords": [
        {
          "keyword": "string",
          "placement": "before"
        }
      ],
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "regex": "string",
      "rulesets": [
        "string"
      ]
    }
  ]
}
```

<h3 id="delete-sensitivedatarule-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRule objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-sensitivedatarule-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRule](#schemasensitivedatarule)]|false|none|none|
|»» contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|»»» keyword|string|true|none|none|
|»»» placement|string|false|none|none|
|»» description|string|false|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|false|none|none|
|»» regex|string|true|none|none|
|»» rulesets|[string]|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Get a list of SensitiveDataRuleset objects

<a id="opIdlistSensitiveDataRuleset"></a>

> Code samples

`GET /lib/sds-rulesets`

Get a list of SensitiveDataRuleset objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0,
      "id": "string",
      "lib": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-sensitivedataruleset-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRuleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-sensitivedataruleset-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRuleset](#schemasensitivedataruleset)]|false|none|none|
|»» count|number|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get SensitiveDataRuleset by ID

<a id="opIdgetSensitiveDataRulesetById"></a>

> Code samples

`GET /lib/sds-rulesets/{id}`

Get SensitiveDataRuleset by ID

<h3 id="get-sensitivedataruleset-by-id-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to GET|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0,
      "id": "string",
      "lib": "string"
    }
  ]
}
```

<h3 id="get-sensitivedataruleset-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRuleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-sensitivedataruleset-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRuleset](#schemasensitivedataruleset)]|false|none|none|
|»» count|number|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Delete SensitiveDataRuleset

<a id="opIddeleteSensitiveDataRulesetById"></a>

> Code samples

`DELETE /lib/sds-rulesets/{id}`

Delete SensitiveDataRuleset

<h3 id="delete-sensitivedataruleset-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to DELETE|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "count": 0,
      "id": "string",
      "lib": "string"
    }
  ]
}
```

<h3 id="delete-sensitivedataruleset-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SensitiveDataRuleset objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-sensitivedataruleset-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SensitiveDataRuleset](#schemasensitivedataruleset)]|false|none|none|
|»» count|number|false|none|none|
|»» id|string|true|none|none|
|»» lib|string|false|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_SensitiveDataRule">SensitiveDataRule</h2>
<!-- backwards compatibility -->
<a id="schemasensitivedatarule"></a>
<a id="schema_SensitiveDataRule"></a>
<a id="tocSsensitivedatarule"></a>
<a id="tocssensitivedatarule"></a>

```json
{
  "contextKeywords": [
    {
      "keyword": "string",
      "placement": "before"
    }
  ],
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "regex": "string",
  "rulesets": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|contextKeywords|[[SensitiveDataContextKeyword](#schemasensitivedatacontextkeyword)]|false|none|none|
|description|string|false|none|none|
|id|string|true|none|none|
|lib|[CriblLib](#schemacribllib)|false|none|none|
|regex|string|true|none|none|
|rulesets|[string]|true|none|none|

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

<h2 id="tocS_SensitiveDataRuleset">SensitiveDataRuleset</h2>
<!-- backwards compatibility -->
<a id="schemasensitivedataruleset"></a>
<a id="schema_SensitiveDataRuleset"></a>
<a id="tocSsensitivedataruleset"></a>
<a id="tocssensitivedataruleset"></a>

```json
{
  "count": 0,
  "id": "string",
  "lib": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|count|number|false|none|none|
|id|string|true|none|none|
|lib|string|false|none|none|

<h2 id="tocS_SensitiveDataContextKeyword">SensitiveDataContextKeyword</h2>
<!-- backwards compatibility -->
<a id="schemasensitivedatacontextkeyword"></a>
<a id="schema_SensitiveDataContextKeyword"></a>
<a id="tocSsensitivedatacontextkeyword"></a>
<a id="tocssensitivedatacontextkeyword"></a>

```json
{
  "keyword": "string",
  "placement": "before"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|keyword|string|true|none|none|
|placement|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|placement|before|
|placement|after|

<h2 id="tocS_CriblLib">CriblLib</h2>
<!-- backwards compatibility -->
<a id="schemacribllib"></a>
<a id="schema_CriblLib"></a>
<a id="tocScribllib"></a>
<a id="tocscribllib"></a>

```json
"cribl"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|cribl|
|*anonymous*|cribl-custom|
|*anonymous*|custom|

