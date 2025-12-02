
<h1 id="cribl-edge-api-appscope-configuration">Cribl Edge API - AppScope Configuration v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-appscope-configuration-appscope-configuration">AppScope Configuration</h1>

## Get a list of AppscopeLibEntry objects

<a id="opIdlistAppscopeLibEntry"></a>

> Code samples

`GET /lib/appscope-configs`

Get a list of AppscopeLibEntry objects

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "custom": [
          {
            "ancestor": "string",
            "arg": "string",
            "config": {
              "cribl": {
                "authtoken": "string",
                "enable": true,
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "useScopeSourceTransport": true
              },
              "event": {
                "enable": true,
                "format": {
                  "enhancefs": true,
                  "maxeventpersec": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "type": "ndjson",
                "watch": [
                  {
                    "allowbinary": true,
                    "enabled": true,
                    "field": "string",
                    "headers": "string",
                    "name": "string",
                    "type": "string",
                    "value": "string"
                  }
                ]
              },
              "libscope": {
                "commanddir": "string",
                "configevent": true,
                "log": {
                  "level": "error",
                  "transport": {
                    "buffer": "line",
                    "host": "string",
                    "path": "string",
                    "port": 0,
                    "tls": {
                      "cacertpath": "string",
                      "enable": true,
                      "validateserver": true
                    },
                    "type": "string"
                  }
                },
                "summaryperiod": 0
              },
              "metric": {
                "enable": true,
                "format": {
                  "statsdmaxlen": 0,
                  "statsdprefix": "string",
                  "type": "string",
                  "verbosity": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "watch": [
                  "string"
                ]
              },
              "payload": {
                "dir": "string",
                "enable": true
              },
              "protocol": [
                {
                  "binary": true,
                  "detect": true,
                  "len": 0,
                  "name": "string",
                  "payload": true,
                  "regex": "string"
                }
              ],
              "tags": [
                {
                  "key": "string",
                  "value": "string"
                }
              ]
            },
            "env": "string",
            "hostname": "string",
            "procname": "string",
            "username": "string"
          }
        ],
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-a-list-of-appscopelibentry-objects-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppscopeLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-list-of-appscopelibentry-objects-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppscopeLibEntry](#schemaappscopelibentry)]|false|none|none|
|»» config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|»»» cribl|object|false|none|none|
|»»»» authtoken|string|false|none|none|
|»»»» enable|boolean|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»» buffer|string|false|none|none|
|»»»»» host|string|false|none|none|
|»»»»» path|string|false|none|none|
|»»»»» port|number|false|none|none|
|»»»»» tls|object|false|none|none|
|»»»»»» cacertpath|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» validateserver|boolean|false|none|none|
|»»»»» type|string|false|none|none|
|»»»» useScopeSourceTransport|boolean|false|none|none|
|»»» custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|»»»» ancestor|string|false|none|none|
|»»»» arg|string|false|none|none|
|»»»» config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|»»»»» cribl|object|false|none|none|
|»»»»»» authtoken|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» useScopeSourceTransport|boolean|false|none|none|
|»»»»» event|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» enhancefs|boolean|true|none|none|
|»»»»»»» maxeventpersec|number|true|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» watch|[object]|true|none|none|
|»»»»»»» allowbinary|boolean|false|none|none|
|»»»»»»» enabled|boolean|false|none|none|
|»»»»»»» field|string|false|none|none|
|»»»»»»» headers|string|false|none|none|
|»»»»»»» name|string|false|none|none|
|»»»»»»» type|string|true|none|none|
|»»»»»»» value|string|false|none|none|
|»»»»» libscope|object|false|none|none|
|»»»»»» commanddir|string|false|none|none|
|»»»»»» configevent|boolean|false|none|none|
|»»»»»» log|object|false|none|none|
|»»»»»»» level|string|false|none|none|
|»»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» summaryperiod|number|false|none|none|
|»»»»» metric|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» statsdmaxlen|number|false|none|none|
|»»»»»»» statsdprefix|string|false|none|none|
|»»»»»»» type|string|false|none|none|
|»»»»»»» verbosity|number|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» watch|[string]|true|none|none|
|»»»»» payload|object|false|none|none|
|»»»»»» dir|string|true|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»» protocol|[object]|false|none|none|
|»»»»»» binary|boolean|true|none|none|
|»»»»»» detect|boolean|true|none|none|
|»»»»»» len|number|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» payload|boolean|true|none|none|
|»»»»»» regex|string|true|none|none|
|»»»»» tags|[object]|false|none|none|
|»»»»»» key|string|true|none|none|
|»»»»»» value|string|true|none|none|
|»»»» env|string|false|none|none|
|»»»» hostname|string|false|none|none|
|»»»» procname|string|false|none|none|
|»»»» username|string|false|none|none|
|»»» event|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» enhancefs|boolean|true|none|none|
|»»»»» maxeventpersec|number|true|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» type|string|true|none|none|
|»»»» watch|[object]|true|none|none|
|»»»»» allowbinary|boolean|false|none|none|
|»»»»» enabled|boolean|false|none|none|
|»»»»» field|string|false|none|none|
|»»»»» headers|string|false|none|none|
|»»»»» name|string|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» value|string|false|none|none|
|»»» libscope|object|false|none|none|
|»»»» commanddir|string|false|none|none|
|»»»» configevent|boolean|false|none|none|
|»»»» log|object|false|none|none|
|»»»»» level|string|false|none|none|
|»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»» summaryperiod|number|false|none|none|
|»»» metric|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» statsdmaxlen|number|false|none|none|
|»»»»» statsdprefix|string|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» verbosity|number|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» watch|[string]|true|none|none|
|»»» payload|object|false|none|none|
|»»»» dir|string|true|none|none|
|»»»» enable|boolean|true|none|none|
|»»» protocol|[object]|false|none|none|
|»»»» binary|boolean|true|none|none|
|»»»» detect|boolean|true|none|none|
|»»»» len|number|true|none|none|
|»»»» name|string|true|none|none|
|»»»» payload|boolean|true|none|none|
|»»»» regex|string|true|none|none|
|»»» tags|[object]|false|none|none|
|»»»» key|string|true|none|none|
|»»»» value|string|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Create AppscopeLibEntry

<a id="opIdcreateAppscopeLibEntry"></a>

> Code samples

`POST /lib/appscope-configs`

Create AppscopeLibEntry

> Body parameter

```json
{
  "config": {
    "cribl": {
      "authtoken": "string",
      "enable": true,
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "useScopeSourceTransport": true
    },
    "custom": [
      {
        "ancestor": "string",
        "arg": "string",
        "config": {
          "cribl": {
            "authtoken": "string",
            "enable": true,
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "useScopeSourceTransport": true
          },
          "event": {
            "enable": true,
            "format": {
              "enhancefs": true,
              "maxeventpersec": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "type": "ndjson",
            "watch": [
              {
                "allowbinary": true,
                "enabled": true,
                "field": "string",
                "headers": "string",
                "name": "string",
                "type": "string",
                "value": "string"
              }
            ]
          },
          "libscope": {
            "commanddir": "string",
            "configevent": true,
            "log": {
              "level": "error",
              "transport": {
                "buffer": "line",
                "host": "string",
                "path": "string",
                "port": 0,
                "tls": {
                  "cacertpath": "string",
                  "enable": true,
                  "validateserver": true
                },
                "type": "string"
              }
            },
            "summaryperiod": 0
          },
          "metric": {
            "enable": true,
            "format": {
              "statsdmaxlen": 0,
              "statsdprefix": "string",
              "type": "string",
              "verbosity": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "watch": [
              "string"
            ]
          },
          "payload": {
            "dir": "string",
            "enable": true
          },
          "protocol": [
            {
              "binary": true,
              "detect": true,
              "len": 0,
              "name": "string",
              "payload": true,
              "regex": "string"
            }
          ],
          "tags": [
            {
              "key": "string",
              "value": "string"
            }
          ]
        },
        "env": "string",
        "hostname": "string",
        "procname": "string",
        "username": "string"
      }
    ],
    "event": {
      "enable": true,
      "format": {
        "enhancefs": true,
        "maxeventpersec": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "type": "ndjson",
      "watch": [
        {
          "allowbinary": true,
          "enabled": true,
          "field": "string",
          "headers": "string",
          "name": "string",
          "type": "string",
          "value": "string"
        }
      ]
    },
    "libscope": {
      "commanddir": "string",
      "configevent": true,
      "log": {
        "level": "error",
        "transport": {
          "buffer": "line",
          "host": "string",
          "path": "string",
          "port": 0,
          "tls": {
            "cacertpath": "string",
            "enable": true,
            "validateserver": true
          },
          "type": "string"
        }
      },
      "summaryperiod": 0
    },
    "metric": {
      "enable": true,
      "format": {
        "statsdmaxlen": 0,
        "statsdprefix": "string",
        "type": "string",
        "verbosity": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "watch": [
        "string"
      ]
    },
    "payload": {
      "dir": "string",
      "enable": true
    },
    "protocol": [
      {
        "binary": true,
        "detect": true,
        "len": 0,
        "name": "string",
        "payload": true,
        "regex": "string"
      }
    ],
    "tags": [
      {
        "key": "string",
        "value": "string"
      }
    ]
  },
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "tags": "string"
}
```

<h3 id="create-appscopelibentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[AppscopeLibEntry](#schemaappscopelibentry)|true|New AppscopeLibEntry object|
|» config|body|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|
|»» cribl|body|object|false|none|
|»»» authtoken|body|string|false|none|
|»»» enable|body|boolean|false|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»» buffer|body|string|false|none|
|»»»» host|body|string|false|none|
|»»»» path|body|string|false|none|
|»»»» port|body|number|false|none|
|»»»» tls|body|object|false|none|
|»»»»» cacertpath|body|string|false|none|
|»»»»» enable|body|boolean|false|none|
|»»»»» validateserver|body|boolean|false|none|
|»»»» type|body|string|false|none|
|»»» useScopeSourceTransport|body|boolean|false|none|
|»» custom|body|[[AppscopeCustom](#schemaappscopecustom)]|false|none|
|»»» ancestor|body|string|false|none|
|»»» arg|body|string|false|none|
|»»» config|body|[AppscopeConfig](#schemaappscopeconfig)|true|none|
|»»»» cribl|body|object|false|none|
|»»»»» authtoken|body|string|false|none|
|»»»»» enable|body|boolean|false|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»»» useScopeSourceTransport|body|boolean|false|none|
|»»»» event|body|object|false|none|
|»»»»» enable|body|boolean|true|none|
|»»»»» format|body|object|true|none|
|»»»»»» enhancefs|body|boolean|true|none|
|»»»»»» maxeventpersec|body|number|true|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»»»» type|body|string|true|none|
|»»»»» watch|body|[object]|true|none|
|»»»»»» allowbinary|body|boolean|false|none|
|»»»»»» enabled|body|boolean|false|none|
|»»»»»» field|body|string|false|none|
|»»»»»» headers|body|string|false|none|
|»»»»»» name|body|string|false|none|
|»»»»»» type|body|string|true|none|
|»»»»»» value|body|string|false|none|
|»»»» libscope|body|object|false|none|
|»»»»» commanddir|body|string|false|none|
|»»»»» configevent|body|boolean|false|none|
|»»»»» log|body|object|false|none|
|»»»»»» level|body|string|false|none|
|»»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»»» summaryperiod|body|number|false|none|
|»»»» metric|body|object|false|none|
|»»»»» enable|body|boolean|true|none|
|»»»»» format|body|object|true|none|
|»»»»»» statsdmaxlen|body|number|false|none|
|»»»»»» statsdprefix|body|string|false|none|
|»»»»»» type|body|string|false|none|
|»»»»»» verbosity|body|number|false|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»»»» watch|body|[string]|true|none|
|»»»» payload|body|object|false|none|
|»»»»» dir|body|string|true|none|
|»»»»» enable|body|boolean|true|none|
|»»»» protocol|body|[object]|false|none|
|»»»»» binary|body|boolean|true|none|
|»»»»» detect|body|boolean|true|none|
|»»»»» len|body|number|true|none|
|»»»»» name|body|string|true|none|
|»»»»» payload|body|boolean|true|none|
|»»»»» regex|body|string|true|none|
|»»»» tags|body|[object]|false|none|
|»»»»» key|body|string|true|none|
|»»»»» value|body|string|true|none|
|»»» env|body|string|false|none|
|»»» hostname|body|string|false|none|
|»»» procname|body|string|false|none|
|»»» username|body|string|false|none|
|»» event|body|object|false|none|
|»»» enable|body|boolean|true|none|
|»»» format|body|object|true|none|
|»»»» enhancefs|body|boolean|true|none|
|»»»» maxeventpersec|body|number|true|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»» type|body|string|true|none|
|»»» watch|body|[object]|true|none|
|»»»» allowbinary|body|boolean|false|none|
|»»»» enabled|body|boolean|false|none|
|»»»» field|body|string|false|none|
|»»»» headers|body|string|false|none|
|»»»» name|body|string|false|none|
|»»»» type|body|string|true|none|
|»»»» value|body|string|false|none|
|»» libscope|body|object|false|none|
|»»» commanddir|body|string|false|none|
|»»» configevent|body|boolean|false|none|
|»»» log|body|object|false|none|
|»»»» level|body|string|false|none|
|»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»» summaryperiod|body|number|false|none|
|»» metric|body|object|false|none|
|»»» enable|body|boolean|true|none|
|»»» format|body|object|true|none|
|»»»» statsdmaxlen|body|number|false|none|
|»»»» statsdprefix|body|string|false|none|
|»»»» type|body|string|false|none|
|»»»» verbosity|body|number|false|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»» watch|body|[string]|true|none|
|»» payload|body|object|false|none|
|»»» dir|body|string|true|none|
|»»» enable|body|boolean|true|none|
|»» protocol|body|[object]|false|none|
|»»» binary|body|boolean|true|none|
|»»» detect|body|boolean|true|none|
|»»» len|body|number|true|none|
|»»» name|body|string|true|none|
|»»» payload|body|boolean|true|none|
|»»» regex|body|string|true|none|
|»» tags|body|[object]|false|none|
|»»» key|body|string|true|none|
|»»» value|body|string|true|none|
|» description|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» tags|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»»» buffer|line|
|»»»» buffer|full|
|»»»»» type|ndjson|
|»»»»»» level|error|
|»»»»»» level|debug|
|»»»»»» level|info|
|»»»»»» level|warning|
|»»»»»» level|none|
|»»» type|ndjson|
|»»»» level|error|
|»»»» level|debug|
|»»»» level|info|
|»»»» level|warning|
|»»»» level|none|
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
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "custom": [
          {
            "ancestor": "string",
            "arg": "string",
            "config": {
              "cribl": {
                "authtoken": "string",
                "enable": true,
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "useScopeSourceTransport": true
              },
              "event": {
                "enable": true,
                "format": {
                  "enhancefs": true,
                  "maxeventpersec": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "type": "ndjson",
                "watch": [
                  {
                    "allowbinary": true,
                    "enabled": true,
                    "field": "string",
                    "headers": "string",
                    "name": "string",
                    "type": "string",
                    "value": "string"
                  }
                ]
              },
              "libscope": {
                "commanddir": "string",
                "configevent": true,
                "log": {
                  "level": "error",
                  "transport": {
                    "buffer": "line",
                    "host": "string",
                    "path": "string",
                    "port": 0,
                    "tls": {
                      "cacertpath": "string",
                      "enable": true,
                      "validateserver": true
                    },
                    "type": "string"
                  }
                },
                "summaryperiod": 0
              },
              "metric": {
                "enable": true,
                "format": {
                  "statsdmaxlen": 0,
                  "statsdprefix": "string",
                  "type": "string",
                  "verbosity": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "watch": [
                  "string"
                ]
              },
              "payload": {
                "dir": "string",
                "enable": true
              },
              "protocol": [
                {
                  "binary": true,
                  "detect": true,
                  "len": 0,
                  "name": "string",
                  "payload": true,
                  "regex": "string"
                }
              ],
              "tags": [
                {
                  "key": "string",
                  "value": "string"
                }
              ]
            },
            "env": "string",
            "hostname": "string",
            "procname": "string",
            "username": "string"
          }
        ],
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "tags": "string"
    }
  ]
}
```

<h3 id="create-appscopelibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppscopeLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="create-appscopelibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppscopeLibEntry](#schemaappscopelibentry)]|false|none|none|
|»» config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|»»» cribl|object|false|none|none|
|»»»» authtoken|string|false|none|none|
|»»»» enable|boolean|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»» buffer|string|false|none|none|
|»»»»» host|string|false|none|none|
|»»»»» path|string|false|none|none|
|»»»»» port|number|false|none|none|
|»»»»» tls|object|false|none|none|
|»»»»»» cacertpath|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» validateserver|boolean|false|none|none|
|»»»»» type|string|false|none|none|
|»»»» useScopeSourceTransport|boolean|false|none|none|
|»»» custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|»»»» ancestor|string|false|none|none|
|»»»» arg|string|false|none|none|
|»»»» config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|»»»»» cribl|object|false|none|none|
|»»»»»» authtoken|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» useScopeSourceTransport|boolean|false|none|none|
|»»»»» event|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» enhancefs|boolean|true|none|none|
|»»»»»»» maxeventpersec|number|true|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» watch|[object]|true|none|none|
|»»»»»»» allowbinary|boolean|false|none|none|
|»»»»»»» enabled|boolean|false|none|none|
|»»»»»»» field|string|false|none|none|
|»»»»»»» headers|string|false|none|none|
|»»»»»»» name|string|false|none|none|
|»»»»»»» type|string|true|none|none|
|»»»»»»» value|string|false|none|none|
|»»»»» libscope|object|false|none|none|
|»»»»»» commanddir|string|false|none|none|
|»»»»»» configevent|boolean|false|none|none|
|»»»»»» log|object|false|none|none|
|»»»»»»» level|string|false|none|none|
|»»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» summaryperiod|number|false|none|none|
|»»»»» metric|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» statsdmaxlen|number|false|none|none|
|»»»»»»» statsdprefix|string|false|none|none|
|»»»»»»» type|string|false|none|none|
|»»»»»»» verbosity|number|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» watch|[string]|true|none|none|
|»»»»» payload|object|false|none|none|
|»»»»»» dir|string|true|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»» protocol|[object]|false|none|none|
|»»»»»» binary|boolean|true|none|none|
|»»»»»» detect|boolean|true|none|none|
|»»»»»» len|number|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» payload|boolean|true|none|none|
|»»»»»» regex|string|true|none|none|
|»»»»» tags|[object]|false|none|none|
|»»»»»» key|string|true|none|none|
|»»»»»» value|string|true|none|none|
|»»»» env|string|false|none|none|
|»»»» hostname|string|false|none|none|
|»»»» procname|string|false|none|none|
|»»»» username|string|false|none|none|
|»»» event|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» enhancefs|boolean|true|none|none|
|»»»»» maxeventpersec|number|true|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» type|string|true|none|none|
|»»»» watch|[object]|true|none|none|
|»»»»» allowbinary|boolean|false|none|none|
|»»»»» enabled|boolean|false|none|none|
|»»»»» field|string|false|none|none|
|»»»»» headers|string|false|none|none|
|»»»»» name|string|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» value|string|false|none|none|
|»»» libscope|object|false|none|none|
|»»»» commanddir|string|false|none|none|
|»»»» configevent|boolean|false|none|none|
|»»»» log|object|false|none|none|
|»»»»» level|string|false|none|none|
|»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»» summaryperiod|number|false|none|none|
|»»» metric|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» statsdmaxlen|number|false|none|none|
|»»»»» statsdprefix|string|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» verbosity|number|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» watch|[string]|true|none|none|
|»»» payload|object|false|none|none|
|»»»» dir|string|true|none|none|
|»»»» enable|boolean|true|none|none|
|»»» protocol|[object]|false|none|none|
|»»»» binary|boolean|true|none|none|
|»»»» detect|boolean|true|none|none|
|»»»» len|number|true|none|none|
|»»»» name|string|true|none|none|
|»»»» payload|boolean|true|none|none|
|»»»» regex|string|true|none|none|
|»»» tags|[object]|false|none|none|
|»»»» key|string|true|none|none|
|»»»» value|string|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Delete AppscopeLibEntry

<a id="opIddeleteAppscopeLibEntryById"></a>

> Code samples

`DELETE /lib/appscope-configs/{id}`

Delete AppscopeLibEntry

<h3 id="delete-appscopelibentry-parameters">Parameters</h3>

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
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "custom": [
          {
            "ancestor": "string",
            "arg": "string",
            "config": {
              "cribl": {
                "authtoken": "string",
                "enable": true,
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "useScopeSourceTransport": true
              },
              "event": {
                "enable": true,
                "format": {
                  "enhancefs": true,
                  "maxeventpersec": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "type": "ndjson",
                "watch": [
                  {
                    "allowbinary": true,
                    "enabled": true,
                    "field": "string",
                    "headers": "string",
                    "name": "string",
                    "type": "string",
                    "value": "string"
                  }
                ]
              },
              "libscope": {
                "commanddir": "string",
                "configevent": true,
                "log": {
                  "level": "error",
                  "transport": {
                    "buffer": "line",
                    "host": "string",
                    "path": "string",
                    "port": 0,
                    "tls": {
                      "cacertpath": "string",
                      "enable": true,
                      "validateserver": true
                    },
                    "type": "string"
                  }
                },
                "summaryperiod": 0
              },
              "metric": {
                "enable": true,
                "format": {
                  "statsdmaxlen": 0,
                  "statsdprefix": "string",
                  "type": "string",
                  "verbosity": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "watch": [
                  "string"
                ]
              },
              "payload": {
                "dir": "string",
                "enable": true
              },
              "protocol": [
                {
                  "binary": true,
                  "detect": true,
                  "len": 0,
                  "name": "string",
                  "payload": true,
                  "regex": "string"
                }
              ],
              "tags": [
                {
                  "key": "string",
                  "value": "string"
                }
              ]
            },
            "env": "string",
            "hostname": "string",
            "procname": "string",
            "username": "string"
          }
        ],
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "tags": "string"
    }
  ]
}
```

<h3 id="delete-appscopelibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppscopeLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="delete-appscopelibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppscopeLibEntry](#schemaappscopelibentry)]|false|none|none|
|»» config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|»»» cribl|object|false|none|none|
|»»»» authtoken|string|false|none|none|
|»»»» enable|boolean|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»» buffer|string|false|none|none|
|»»»»» host|string|false|none|none|
|»»»»» path|string|false|none|none|
|»»»»» port|number|false|none|none|
|»»»»» tls|object|false|none|none|
|»»»»»» cacertpath|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» validateserver|boolean|false|none|none|
|»»»»» type|string|false|none|none|
|»»»» useScopeSourceTransport|boolean|false|none|none|
|»»» custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|»»»» ancestor|string|false|none|none|
|»»»» arg|string|false|none|none|
|»»»» config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|»»»»» cribl|object|false|none|none|
|»»»»»» authtoken|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» useScopeSourceTransport|boolean|false|none|none|
|»»»»» event|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» enhancefs|boolean|true|none|none|
|»»»»»»» maxeventpersec|number|true|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» watch|[object]|true|none|none|
|»»»»»»» allowbinary|boolean|false|none|none|
|»»»»»»» enabled|boolean|false|none|none|
|»»»»»»» field|string|false|none|none|
|»»»»»»» headers|string|false|none|none|
|»»»»»»» name|string|false|none|none|
|»»»»»»» type|string|true|none|none|
|»»»»»»» value|string|false|none|none|
|»»»»» libscope|object|false|none|none|
|»»»»»» commanddir|string|false|none|none|
|»»»»»» configevent|boolean|false|none|none|
|»»»»»» log|object|false|none|none|
|»»»»»»» level|string|false|none|none|
|»»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» summaryperiod|number|false|none|none|
|»»»»» metric|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» statsdmaxlen|number|false|none|none|
|»»»»»»» statsdprefix|string|false|none|none|
|»»»»»»» type|string|false|none|none|
|»»»»»»» verbosity|number|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» watch|[string]|true|none|none|
|»»»»» payload|object|false|none|none|
|»»»»»» dir|string|true|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»» protocol|[object]|false|none|none|
|»»»»»» binary|boolean|true|none|none|
|»»»»»» detect|boolean|true|none|none|
|»»»»»» len|number|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» payload|boolean|true|none|none|
|»»»»»» regex|string|true|none|none|
|»»»»» tags|[object]|false|none|none|
|»»»»»» key|string|true|none|none|
|»»»»»» value|string|true|none|none|
|»»»» env|string|false|none|none|
|»»»» hostname|string|false|none|none|
|»»»» procname|string|false|none|none|
|»»»» username|string|false|none|none|
|»»» event|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» enhancefs|boolean|true|none|none|
|»»»»» maxeventpersec|number|true|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» type|string|true|none|none|
|»»»» watch|[object]|true|none|none|
|»»»»» allowbinary|boolean|false|none|none|
|»»»»» enabled|boolean|false|none|none|
|»»»»» field|string|false|none|none|
|»»»»» headers|string|false|none|none|
|»»»»» name|string|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» value|string|false|none|none|
|»»» libscope|object|false|none|none|
|»»»» commanddir|string|false|none|none|
|»»»» configevent|boolean|false|none|none|
|»»»» log|object|false|none|none|
|»»»»» level|string|false|none|none|
|»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»» summaryperiod|number|false|none|none|
|»»» metric|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» statsdmaxlen|number|false|none|none|
|»»»»» statsdprefix|string|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» verbosity|number|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» watch|[string]|true|none|none|
|»»» payload|object|false|none|none|
|»»»» dir|string|true|none|none|
|»»»» enable|boolean|true|none|none|
|»»» protocol|[object]|false|none|none|
|»»»» binary|boolean|true|none|none|
|»»»» detect|boolean|true|none|none|
|»»»» len|number|true|none|none|
|»»»» name|string|true|none|none|
|»»»» payload|boolean|true|none|none|
|»»»» regex|string|true|none|none|
|»»» tags|[object]|false|none|none|
|»»»» key|string|true|none|none|
|»»»» value|string|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Get AppscopeLibEntry by ID

<a id="opIdgetAppscopeLibEntryById"></a>

> Code samples

`GET /lib/appscope-configs/{id}`

Get AppscopeLibEntry by ID

<h3 id="get-appscopelibentry-by-id-parameters">Parameters</h3>

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
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "custom": [
          {
            "ancestor": "string",
            "arg": "string",
            "config": {
              "cribl": {
                "authtoken": "string",
                "enable": true,
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "useScopeSourceTransport": true
              },
              "event": {
                "enable": true,
                "format": {
                  "enhancefs": true,
                  "maxeventpersec": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "type": "ndjson",
                "watch": [
                  {
                    "allowbinary": true,
                    "enabled": true,
                    "field": "string",
                    "headers": "string",
                    "name": "string",
                    "type": "string",
                    "value": "string"
                  }
                ]
              },
              "libscope": {
                "commanddir": "string",
                "configevent": true,
                "log": {
                  "level": "error",
                  "transport": {
                    "buffer": "line",
                    "host": "string",
                    "path": "string",
                    "port": 0,
                    "tls": {
                      "cacertpath": "string",
                      "enable": true,
                      "validateserver": true
                    },
                    "type": "string"
                  }
                },
                "summaryperiod": 0
              },
              "metric": {
                "enable": true,
                "format": {
                  "statsdmaxlen": 0,
                  "statsdprefix": "string",
                  "type": "string",
                  "verbosity": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "watch": [
                  "string"
                ]
              },
              "payload": {
                "dir": "string",
                "enable": true
              },
              "protocol": [
                {
                  "binary": true,
                  "detect": true,
                  "len": 0,
                  "name": "string",
                  "payload": true,
                  "regex": "string"
                }
              ],
              "tags": [
                {
                  "key": "string",
                  "value": "string"
                }
              ]
            },
            "env": "string",
            "hostname": "string",
            "procname": "string",
            "username": "string"
          }
        ],
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "tags": "string"
    }
  ]
}
```

<h3 id="get-appscopelibentry-by-id-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppscopeLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-appscopelibentry-by-id-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppscopeLibEntry](#schemaappscopelibentry)]|false|none|none|
|»» config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|»»» cribl|object|false|none|none|
|»»»» authtoken|string|false|none|none|
|»»»» enable|boolean|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»» buffer|string|false|none|none|
|»»»»» host|string|false|none|none|
|»»»»» path|string|false|none|none|
|»»»»» port|number|false|none|none|
|»»»»» tls|object|false|none|none|
|»»»»»» cacertpath|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» validateserver|boolean|false|none|none|
|»»»»» type|string|false|none|none|
|»»»» useScopeSourceTransport|boolean|false|none|none|
|»»» custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|»»»» ancestor|string|false|none|none|
|»»»» arg|string|false|none|none|
|»»»» config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|»»»»» cribl|object|false|none|none|
|»»»»»» authtoken|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» useScopeSourceTransport|boolean|false|none|none|
|»»»»» event|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» enhancefs|boolean|true|none|none|
|»»»»»»» maxeventpersec|number|true|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» watch|[object]|true|none|none|
|»»»»»»» allowbinary|boolean|false|none|none|
|»»»»»»» enabled|boolean|false|none|none|
|»»»»»»» field|string|false|none|none|
|»»»»»»» headers|string|false|none|none|
|»»»»»»» name|string|false|none|none|
|»»»»»»» type|string|true|none|none|
|»»»»»»» value|string|false|none|none|
|»»»»» libscope|object|false|none|none|
|»»»»»» commanddir|string|false|none|none|
|»»»»»» configevent|boolean|false|none|none|
|»»»»»» log|object|false|none|none|
|»»»»»»» level|string|false|none|none|
|»»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» summaryperiod|number|false|none|none|
|»»»»» metric|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» statsdmaxlen|number|false|none|none|
|»»»»»»» statsdprefix|string|false|none|none|
|»»»»»»» type|string|false|none|none|
|»»»»»»» verbosity|number|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» watch|[string]|true|none|none|
|»»»»» payload|object|false|none|none|
|»»»»»» dir|string|true|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»» protocol|[object]|false|none|none|
|»»»»»» binary|boolean|true|none|none|
|»»»»»» detect|boolean|true|none|none|
|»»»»»» len|number|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» payload|boolean|true|none|none|
|»»»»»» regex|string|true|none|none|
|»»»»» tags|[object]|false|none|none|
|»»»»»» key|string|true|none|none|
|»»»»»» value|string|true|none|none|
|»»»» env|string|false|none|none|
|»»»» hostname|string|false|none|none|
|»»»» procname|string|false|none|none|
|»»»» username|string|false|none|none|
|»»» event|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» enhancefs|boolean|true|none|none|
|»»»»» maxeventpersec|number|true|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» type|string|true|none|none|
|»»»» watch|[object]|true|none|none|
|»»»»» allowbinary|boolean|false|none|none|
|»»»»» enabled|boolean|false|none|none|
|»»»»» field|string|false|none|none|
|»»»»» headers|string|false|none|none|
|»»»»» name|string|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» value|string|false|none|none|
|»»» libscope|object|false|none|none|
|»»»» commanddir|string|false|none|none|
|»»»» configevent|boolean|false|none|none|
|»»»» log|object|false|none|none|
|»»»»» level|string|false|none|none|
|»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»» summaryperiod|number|false|none|none|
|»»» metric|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» statsdmaxlen|number|false|none|none|
|»»»»» statsdprefix|string|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» verbosity|number|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» watch|[string]|true|none|none|
|»»» payload|object|false|none|none|
|»»»» dir|string|true|none|none|
|»»»» enable|boolean|true|none|none|
|»»» protocol|[object]|false|none|none|
|»»»» binary|boolean|true|none|none|
|»»»» detect|boolean|true|none|none|
|»»»» len|number|true|none|none|
|»»»» name|string|true|none|none|
|»»»» payload|boolean|true|none|none|
|»»»» regex|string|true|none|none|
|»»» tags|[object]|false|none|none|
|»»»» key|string|true|none|none|
|»»»» value|string|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

## Update AppscopeLibEntry

<a id="opIdupdateAppscopeLibEntryById"></a>

> Code samples

`PATCH /lib/appscope-configs/{id}`

Update AppscopeLibEntry

> Body parameter

```json
{
  "config": {
    "cribl": {
      "authtoken": "string",
      "enable": true,
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "useScopeSourceTransport": true
    },
    "custom": [
      {
        "ancestor": "string",
        "arg": "string",
        "config": {
          "cribl": {
            "authtoken": "string",
            "enable": true,
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "useScopeSourceTransport": true
          },
          "event": {
            "enable": true,
            "format": {
              "enhancefs": true,
              "maxeventpersec": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "type": "ndjson",
            "watch": [
              {
                "allowbinary": true,
                "enabled": true,
                "field": "string",
                "headers": "string",
                "name": "string",
                "type": "string",
                "value": "string"
              }
            ]
          },
          "libscope": {
            "commanddir": "string",
            "configevent": true,
            "log": {
              "level": "error",
              "transport": {
                "buffer": "line",
                "host": "string",
                "path": "string",
                "port": 0,
                "tls": {
                  "cacertpath": "string",
                  "enable": true,
                  "validateserver": true
                },
                "type": "string"
              }
            },
            "summaryperiod": 0
          },
          "metric": {
            "enable": true,
            "format": {
              "statsdmaxlen": 0,
              "statsdprefix": "string",
              "type": "string",
              "verbosity": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "watch": [
              "string"
            ]
          },
          "payload": {
            "dir": "string",
            "enable": true
          },
          "protocol": [
            {
              "binary": true,
              "detect": true,
              "len": 0,
              "name": "string",
              "payload": true,
              "regex": "string"
            }
          ],
          "tags": [
            {
              "key": "string",
              "value": "string"
            }
          ]
        },
        "env": "string",
        "hostname": "string",
        "procname": "string",
        "username": "string"
      }
    ],
    "event": {
      "enable": true,
      "format": {
        "enhancefs": true,
        "maxeventpersec": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "type": "ndjson",
      "watch": [
        {
          "allowbinary": true,
          "enabled": true,
          "field": "string",
          "headers": "string",
          "name": "string",
          "type": "string",
          "value": "string"
        }
      ]
    },
    "libscope": {
      "commanddir": "string",
      "configevent": true,
      "log": {
        "level": "error",
        "transport": {
          "buffer": "line",
          "host": "string",
          "path": "string",
          "port": 0,
          "tls": {
            "cacertpath": "string",
            "enable": true,
            "validateserver": true
          },
          "type": "string"
        }
      },
      "summaryperiod": 0
    },
    "metric": {
      "enable": true,
      "format": {
        "statsdmaxlen": 0,
        "statsdprefix": "string",
        "type": "string",
        "verbosity": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "watch": [
        "string"
      ]
    },
    "payload": {
      "dir": "string",
      "enable": true
    },
    "protocol": [
      {
        "binary": true,
        "detect": true,
        "len": 0,
        "name": "string",
        "payload": true,
        "regex": "string"
      }
    ],
    "tags": [
      {
        "key": "string",
        "value": "string"
      }
    ]
  },
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "tags": "string"
}
```

<h3 id="update-appscopelibentry-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Unique ID to PATCH|
|body|body|[AppscopeLibEntry](#schemaappscopelibentry)|true|AppscopeLibEntry object to be updated|
|» config|body|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|
|»» cribl|body|object|false|none|
|»»» authtoken|body|string|false|none|
|»»» enable|body|boolean|false|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»» buffer|body|string|false|none|
|»»»» host|body|string|false|none|
|»»»» path|body|string|false|none|
|»»»» port|body|number|false|none|
|»»»» tls|body|object|false|none|
|»»»»» cacertpath|body|string|false|none|
|»»»»» enable|body|boolean|false|none|
|»»»»» validateserver|body|boolean|false|none|
|»»»» type|body|string|false|none|
|»»» useScopeSourceTransport|body|boolean|false|none|
|»» custom|body|[[AppscopeCustom](#schemaappscopecustom)]|false|none|
|»»» ancestor|body|string|false|none|
|»»» arg|body|string|false|none|
|»»» config|body|[AppscopeConfig](#schemaappscopeconfig)|true|none|
|»»»» cribl|body|object|false|none|
|»»»»» authtoken|body|string|false|none|
|»»»»» enable|body|boolean|false|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»»» useScopeSourceTransport|body|boolean|false|none|
|»»»» event|body|object|false|none|
|»»»»» enable|body|boolean|true|none|
|»»»»» format|body|object|true|none|
|»»»»»» enhancefs|body|boolean|true|none|
|»»»»»» maxeventpersec|body|number|true|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»»»» type|body|string|true|none|
|»»»»» watch|body|[object]|true|none|
|»»»»»» allowbinary|body|boolean|false|none|
|»»»»»» enabled|body|boolean|false|none|
|»»»»»» field|body|string|false|none|
|»»»»»» headers|body|string|false|none|
|»»»»»» name|body|string|false|none|
|»»»»»» type|body|string|true|none|
|»»»»»» value|body|string|false|none|
|»»»» libscope|body|object|false|none|
|»»»»» commanddir|body|string|false|none|
|»»»»» configevent|body|boolean|false|none|
|»»»»» log|body|object|false|none|
|»»»»»» level|body|string|false|none|
|»»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»»»» summaryperiod|body|number|false|none|
|»»»» metric|body|object|false|none|
|»»»»» enable|body|boolean|true|none|
|»»»»» format|body|object|true|none|
|»»»»»» statsdmaxlen|body|number|false|none|
|»»»»»» statsdprefix|body|string|false|none|
|»»»»»» type|body|string|false|none|
|»»»»»» verbosity|body|number|false|none|
|»»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»»»» watch|body|[string]|true|none|
|»»»» payload|body|object|false|none|
|»»»»» dir|body|string|true|none|
|»»»»» enable|body|boolean|true|none|
|»»»» protocol|body|[object]|false|none|
|»»»»» binary|body|boolean|true|none|
|»»»»» detect|body|boolean|true|none|
|»»»»» len|body|number|true|none|
|»»»»» name|body|string|true|none|
|»»»»» payload|body|boolean|true|none|
|»»»»» regex|body|string|true|none|
|»»»» tags|body|[object]|false|none|
|»»»»» key|body|string|true|none|
|»»»»» value|body|string|true|none|
|»»» env|body|string|false|none|
|»»» hostname|body|string|false|none|
|»»» procname|body|string|false|none|
|»»» username|body|string|false|none|
|»» event|body|object|false|none|
|»»» enable|body|boolean|true|none|
|»»» format|body|object|true|none|
|»»»» enhancefs|body|boolean|true|none|
|»»»» maxeventpersec|body|number|true|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»» type|body|string|true|none|
|»»» watch|body|[object]|true|none|
|»»»» allowbinary|body|boolean|false|none|
|»»»» enabled|body|boolean|false|none|
|»»»» field|body|string|false|none|
|»»»» headers|body|string|false|none|
|»»»» name|body|string|false|none|
|»»»» type|body|string|true|none|
|»»»» value|body|string|false|none|
|»» libscope|body|object|false|none|
|»»» commanddir|body|string|false|none|
|»»» configevent|body|boolean|false|none|
|»»» log|body|object|false|none|
|»»»» level|body|string|false|none|
|»»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|false|none|
|»»» summaryperiod|body|number|false|none|
|»» metric|body|object|false|none|
|»»» enable|body|boolean|true|none|
|»»» format|body|object|true|none|
|»»»» statsdmaxlen|body|number|false|none|
|»»»» statsdprefix|body|string|false|none|
|»»»» type|body|string|false|none|
|»»»» verbosity|body|number|false|none|
|»»» transport|body|[AppscopeTransport](#schemaappscopetransport)|true|none|
|»»» watch|body|[string]|true|none|
|»» payload|body|object|false|none|
|»»» dir|body|string|true|none|
|»»» enable|body|boolean|true|none|
|»» protocol|body|[object]|false|none|
|»»» binary|body|boolean|true|none|
|»»» detect|body|boolean|true|none|
|»»» len|body|number|true|none|
|»»» name|body|string|true|none|
|»»» payload|body|boolean|true|none|
|»»» regex|body|string|true|none|
|»» tags|body|[object]|false|none|
|»»» key|body|string|true|none|
|»»» value|body|string|true|none|
|» description|body|string|true|none|
|» id|body|string|true|none|
|» lib|body|[CriblLib](#schemacribllib)|true|none|
|» tags|body|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»»»» buffer|line|
|»»»» buffer|full|
|»»»»» type|ndjson|
|»»»»»» level|error|
|»»»»»» level|debug|
|»»»»»» level|info|
|»»»»»» level|warning|
|»»»»»» level|none|
|»»» type|ndjson|
|»»»» level|error|
|»»»» level|debug|
|»»»» level|info|
|»»»» level|warning|
|»»»» level|none|
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
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "custom": [
          {
            "ancestor": "string",
            "arg": "string",
            "config": {
              "cribl": {
                "authtoken": "string",
                "enable": true,
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "useScopeSourceTransport": true
              },
              "event": {
                "enable": true,
                "format": {
                  "enhancefs": true,
                  "maxeventpersec": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "type": "ndjson",
                "watch": [
                  {
                    "allowbinary": true,
                    "enabled": true,
                    "field": "string",
                    "headers": "string",
                    "name": "string",
                    "type": "string",
                    "value": "string"
                  }
                ]
              },
              "libscope": {
                "commanddir": "string",
                "configevent": true,
                "log": {
                  "level": "error",
                  "transport": {
                    "buffer": "line",
                    "host": "string",
                    "path": "string",
                    "port": 0,
                    "tls": {
                      "cacertpath": "string",
                      "enable": true,
                      "validateserver": true
                    },
                    "type": "string"
                  }
                },
                "summaryperiod": 0
              },
              "metric": {
                "enable": true,
                "format": {
                  "statsdmaxlen": 0,
                  "statsdprefix": "string",
                  "type": "string",
                  "verbosity": 0
                },
                "transport": {
                  "buffer": "line",
                  "host": "string",
                  "path": "string",
                  "port": 0,
                  "tls": {
                    "cacertpath": "string",
                    "enable": true,
                    "validateserver": true
                  },
                  "type": "string"
                },
                "watch": [
                  "string"
                ]
              },
              "payload": {
                "dir": "string",
                "enable": true
              },
              "protocol": [
                {
                  "binary": true,
                  "detect": true,
                  "len": 0,
                  "name": "string",
                  "payload": true,
                  "regex": "string"
                }
              ],
              "tags": [
                {
                  "key": "string",
                  "value": "string"
                }
              ]
            },
            "env": "string",
            "hostname": "string",
            "procname": "string",
            "username": "string"
          }
        ],
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "description": "string",
      "id": "string",
      "lib": "cribl",
      "tags": "string"
    }
  ]
}
```

<h3 id="update-appscopelibentry-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppscopeLibEntry objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-appscopelibentry-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppscopeLibEntry](#schemaappscopelibentry)]|false|none|none|
|»» config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|»»» cribl|object|false|none|none|
|»»»» authtoken|string|false|none|none|
|»»»» enable|boolean|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»» buffer|string|false|none|none|
|»»»»» host|string|false|none|none|
|»»»»» path|string|false|none|none|
|»»»»» port|number|false|none|none|
|»»»»» tls|object|false|none|none|
|»»»»»» cacertpath|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» validateserver|boolean|false|none|none|
|»»»»» type|string|false|none|none|
|»»»» useScopeSourceTransport|boolean|false|none|none|
|»»» custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|»»»» ancestor|string|false|none|none|
|»»»» arg|string|false|none|none|
|»»»» config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|»»»»» cribl|object|false|none|none|
|»»»»»» authtoken|string|false|none|none|
|»»»»»» enable|boolean|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» useScopeSourceTransport|boolean|false|none|none|
|»»»»» event|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» enhancefs|boolean|true|none|none|
|»»»»»»» maxeventpersec|number|true|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» type|string|true|none|none|
|»»»»»» watch|[object]|true|none|none|
|»»»»»»» allowbinary|boolean|false|none|none|
|»»»»»»» enabled|boolean|false|none|none|
|»»»»»»» field|string|false|none|none|
|»»»»»»» headers|string|false|none|none|
|»»»»»»» name|string|false|none|none|
|»»»»»»» type|string|true|none|none|
|»»»»»»» value|string|false|none|none|
|»»»»» libscope|object|false|none|none|
|»»»»»» commanddir|string|false|none|none|
|»»»»»» configevent|boolean|false|none|none|
|»»»»»» log|object|false|none|none|
|»»»»»»» level|string|false|none|none|
|»»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»»»» summaryperiod|number|false|none|none|
|»»»»» metric|object|false|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»»» format|object|true|none|none|
|»»»»»»» statsdmaxlen|number|false|none|none|
|»»»»»»» statsdprefix|string|false|none|none|
|»»»»»»» type|string|false|none|none|
|»»»»»»» verbosity|number|false|none|none|
|»»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»»»» watch|[string]|true|none|none|
|»»»»» payload|object|false|none|none|
|»»»»»» dir|string|true|none|none|
|»»»»»» enable|boolean|true|none|none|
|»»»»» protocol|[object]|false|none|none|
|»»»»»» binary|boolean|true|none|none|
|»»»»»» detect|boolean|true|none|none|
|»»»»»» len|number|true|none|none|
|»»»»»» name|string|true|none|none|
|»»»»»» payload|boolean|true|none|none|
|»»»»»» regex|string|true|none|none|
|»»»»» tags|[object]|false|none|none|
|»»»»»» key|string|true|none|none|
|»»»»»» value|string|true|none|none|
|»»»» env|string|false|none|none|
|»»»» hostname|string|false|none|none|
|»»»» procname|string|false|none|none|
|»»»» username|string|false|none|none|
|»»» event|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» enhancefs|boolean|true|none|none|
|»»»»» maxeventpersec|number|true|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» type|string|true|none|none|
|»»»» watch|[object]|true|none|none|
|»»»»» allowbinary|boolean|false|none|none|
|»»»»» enabled|boolean|false|none|none|
|»»»»» field|string|false|none|none|
|»»»»» headers|string|false|none|none|
|»»»»» name|string|false|none|none|
|»»»»» type|string|true|none|none|
|»»»»» value|string|false|none|none|
|»»» libscope|object|false|none|none|
|»»»» commanddir|string|false|none|none|
|»»»» configevent|boolean|false|none|none|
|»»»» log|object|false|none|none|
|»»»»» level|string|false|none|none|
|»»»»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|»»»» summaryperiod|number|false|none|none|
|»»» metric|object|false|none|none|
|»»»» enable|boolean|true|none|none|
|»»»» format|object|true|none|none|
|»»»»» statsdmaxlen|number|false|none|none|
|»»»»» statsdprefix|string|false|none|none|
|»»»»» type|string|false|none|none|
|»»»»» verbosity|number|false|none|none|
|»»»» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|»»»» watch|[string]|true|none|none|
|»»» payload|object|false|none|none|
|»»»» dir|string|true|none|none|
|»»»» enable|boolean|true|none|none|
|»»» protocol|[object]|false|none|none|
|»»»» binary|boolean|true|none|none|
|»»»» detect|boolean|true|none|none|
|»»»» len|number|true|none|none|
|»»»» name|string|true|none|none|
|»»»» payload|boolean|true|none|none|
|»»»» regex|string|true|none|none|
|»»» tags|[object]|false|none|none|
|»»»» key|string|true|none|none|
|»»»» value|string|true|none|none|
|»» description|string|true|none|none|
|»» id|string|true|none|none|
|»» lib|[CriblLib](#schemacribllib)|true|none|none|
|»» tags|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|
|lib|cribl|
|lib|cribl-custom|
|lib|custom|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_AppscopeLibEntry">AppscopeLibEntry</h2>
<!-- backwards compatibility -->
<a id="schemaappscopelibentry"></a>
<a id="schema_AppscopeLibEntry"></a>
<a id="tocSappscopelibentry"></a>
<a id="tocsappscopelibentry"></a>

```json
{
  "config": {
    "cribl": {
      "authtoken": "string",
      "enable": true,
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "useScopeSourceTransport": true
    },
    "custom": [
      {
        "ancestor": "string",
        "arg": "string",
        "config": {
          "cribl": {
            "authtoken": "string",
            "enable": true,
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "useScopeSourceTransport": true
          },
          "event": {
            "enable": true,
            "format": {
              "enhancefs": true,
              "maxeventpersec": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "type": "ndjson",
            "watch": [
              {
                "allowbinary": true,
                "enabled": true,
                "field": "string",
                "headers": "string",
                "name": "string",
                "type": "string",
                "value": "string"
              }
            ]
          },
          "libscope": {
            "commanddir": "string",
            "configevent": true,
            "log": {
              "level": "error",
              "transport": {
                "buffer": "line",
                "host": "string",
                "path": "string",
                "port": 0,
                "tls": {
                  "cacertpath": "string",
                  "enable": true,
                  "validateserver": true
                },
                "type": "string"
              }
            },
            "summaryperiod": 0
          },
          "metric": {
            "enable": true,
            "format": {
              "statsdmaxlen": 0,
              "statsdprefix": "string",
              "type": "string",
              "verbosity": 0
            },
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            },
            "watch": [
              "string"
            ]
          },
          "payload": {
            "dir": "string",
            "enable": true
          },
          "protocol": [
            {
              "binary": true,
              "detect": true,
              "len": 0,
              "name": "string",
              "payload": true,
              "regex": "string"
            }
          ],
          "tags": [
            {
              "key": "string",
              "value": "string"
            }
          ]
        },
        "env": "string",
        "hostname": "string",
        "procname": "string",
        "username": "string"
      }
    ],
    "event": {
      "enable": true,
      "format": {
        "enhancefs": true,
        "maxeventpersec": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "type": "ndjson",
      "watch": [
        {
          "allowbinary": true,
          "enabled": true,
          "field": "string",
          "headers": "string",
          "name": "string",
          "type": "string",
          "value": "string"
        }
      ]
    },
    "libscope": {
      "commanddir": "string",
      "configevent": true,
      "log": {
        "level": "error",
        "transport": {
          "buffer": "line",
          "host": "string",
          "path": "string",
          "port": 0,
          "tls": {
            "cacertpath": "string",
            "enable": true,
            "validateserver": true
          },
          "type": "string"
        }
      },
      "summaryperiod": 0
    },
    "metric": {
      "enable": true,
      "format": {
        "statsdmaxlen": 0,
        "statsdprefix": "string",
        "type": "string",
        "verbosity": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "watch": [
        "string"
      ]
    },
    "payload": {
      "dir": "string",
      "enable": true
    },
    "protocol": [
      {
        "binary": true,
        "detect": true,
        "len": 0,
        "name": "string",
        "payload": true,
        "regex": "string"
      }
    ],
    "tags": [
      {
        "key": "string",
        "value": "string"
      }
    ]
  },
  "description": "string",
  "id": "string",
  "lib": "cribl",
  "tags": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|config|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|true|none|none|
|description|string|true|none|none|
|id|string|true|none|none|
|lib|[CriblLib](#schemacribllib)|true|none|none|
|tags|string|false|none|none|

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

<h2 id="tocS_AppscopeConfigWithCustom">AppscopeConfigWithCustom</h2>
<!-- backwards compatibility -->
<a id="schemaappscopeconfigwithcustom"></a>
<a id="schema_AppscopeConfigWithCustom"></a>
<a id="tocSappscopeconfigwithcustom"></a>
<a id="tocsappscopeconfigwithcustom"></a>

```json
{
  "cribl": {
    "authtoken": "string",
    "enable": true,
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "useScopeSourceTransport": true
  },
  "custom": [
    {
      "ancestor": "string",
      "arg": "string",
      "config": {
        "cribl": {
          "authtoken": "string",
          "enable": true,
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "useScopeSourceTransport": true
        },
        "event": {
          "enable": true,
          "format": {
            "enhancefs": true,
            "maxeventpersec": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "type": "ndjson",
          "watch": [
            {
              "allowbinary": true,
              "enabled": true,
              "field": "string",
              "headers": "string",
              "name": "string",
              "type": "string",
              "value": "string"
            }
          ]
        },
        "libscope": {
          "commanddir": "string",
          "configevent": true,
          "log": {
            "level": "error",
            "transport": {
              "buffer": "line",
              "host": "string",
              "path": "string",
              "port": 0,
              "tls": {
                "cacertpath": "string",
                "enable": true,
                "validateserver": true
              },
              "type": "string"
            }
          },
          "summaryperiod": 0
        },
        "metric": {
          "enable": true,
          "format": {
            "statsdmaxlen": 0,
            "statsdprefix": "string",
            "type": "string",
            "verbosity": 0
          },
          "transport": {
            "buffer": "line",
            "host": "string",
            "path": "string",
            "port": 0,
            "tls": {
              "cacertpath": "string",
              "enable": true,
              "validateserver": true
            },
            "type": "string"
          },
          "watch": [
            "string"
          ]
        },
        "payload": {
          "dir": "string",
          "enable": true
        },
        "protocol": [
          {
            "binary": true,
            "detect": true,
            "len": 0,
            "name": "string",
            "payload": true,
            "regex": "string"
          }
        ],
        "tags": [
          {
            "key": "string",
            "value": "string"
          }
        ]
      },
      "env": "string",
      "hostname": "string",
      "procname": "string",
      "username": "string"
    }
  ],
  "event": {
    "enable": true,
    "format": {
      "enhancefs": true,
      "maxeventpersec": 0
    },
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "type": "ndjson",
    "watch": [
      {
        "allowbinary": true,
        "enabled": true,
        "field": "string",
        "headers": "string",
        "name": "string",
        "type": "string",
        "value": "string"
      }
    ]
  },
  "libscope": {
    "commanddir": "string",
    "configevent": true,
    "log": {
      "level": "error",
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      }
    },
    "summaryperiod": 0
  },
  "metric": {
    "enable": true,
    "format": {
      "statsdmaxlen": 0,
      "statsdprefix": "string",
      "type": "string",
      "verbosity": 0
    },
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "watch": [
      "string"
    ]
  },
  "payload": {
    "dir": "string",
    "enable": true
  },
  "protocol": [
    {
      "binary": true,
      "detect": true,
      "len": 0,
      "name": "string",
      "payload": true,
      "regex": "string"
    }
  ],
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cribl|object|false|none|none|
|» authtoken|string|false|none|none|
|» enable|boolean|false|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|» useScopeSourceTransport|boolean|false|none|none|
|custom|[[AppscopeCustom](#schemaappscopecustom)]|false|none|none|
|event|object|false|none|none|
|» enable|boolean|true|none|none|
|» format|object|true|none|none|
|»» enhancefs|boolean|true|none|none|
|»» maxeventpersec|number|true|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|» type|string|true|none|none|
|» watch|[object]|true|none|none|
|»» allowbinary|boolean|false|none|none|
|»» enabled|boolean|false|none|none|
|»» field|string|false|none|none|
|»» headers|string|false|none|none|
|»» name|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|libscope|object|false|none|none|
|» commanddir|string|false|none|none|
|» configevent|boolean|false|none|none|
|» log|object|false|none|none|
|»» level|string|false|none|none|
|»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|» summaryperiod|number|false|none|none|
|metric|object|false|none|none|
|» enable|boolean|true|none|none|
|» format|object|true|none|none|
|»» statsdmaxlen|number|false|none|none|
|»» statsdprefix|string|false|none|none|
|»» type|string|false|none|none|
|»» verbosity|number|false|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|» watch|[string]|true|none|none|
|payload|object|false|none|none|
|» dir|string|true|none|none|
|» enable|boolean|true|none|none|
|protocol|[object]|false|none|none|
|» binary|boolean|true|none|none|
|» detect|boolean|true|none|none|
|» len|number|true|none|none|
|» name|string|true|none|none|
|» payload|boolean|true|none|none|
|» regex|string|true|none|none|
|tags|[object]|false|none|none|
|» key|string|true|none|none|
|» value|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|

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

<h2 id="tocS_AppscopeTransport">AppscopeTransport</h2>
<!-- backwards compatibility -->
<a id="schemaappscopetransport"></a>
<a id="schema_AppscopeTransport"></a>
<a id="tocSappscopetransport"></a>
<a id="tocsappscopetransport"></a>

```json
{
  "buffer": "line",
  "host": "string",
  "path": "string",
  "port": 0,
  "tls": {
    "cacertpath": "string",
    "enable": true,
    "validateserver": true
  },
  "type": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|buffer|string|false|none|none|
|host|string|false|none|none|
|path|string|false|none|none|
|port|number|false|none|none|
|tls|object|false|none|none|
|» cacertpath|string|false|none|none|
|» enable|boolean|false|none|none|
|» validateserver|boolean|false|none|none|
|type|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|buffer|line|
|buffer|full|

<h2 id="tocS_AppscopeCustom">AppscopeCustom</h2>
<!-- backwards compatibility -->
<a id="schemaappscopecustom"></a>
<a id="schema_AppscopeCustom"></a>
<a id="tocSappscopecustom"></a>
<a id="tocsappscopecustom"></a>

```json
{
  "ancestor": "string",
  "arg": "string",
  "config": {
    "cribl": {
      "authtoken": "string",
      "enable": true,
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "useScopeSourceTransport": true
    },
    "event": {
      "enable": true,
      "format": {
        "enhancefs": true,
        "maxeventpersec": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "type": "ndjson",
      "watch": [
        {
          "allowbinary": true,
          "enabled": true,
          "field": "string",
          "headers": "string",
          "name": "string",
          "type": "string",
          "value": "string"
        }
      ]
    },
    "libscope": {
      "commanddir": "string",
      "configevent": true,
      "log": {
        "level": "error",
        "transport": {
          "buffer": "line",
          "host": "string",
          "path": "string",
          "port": 0,
          "tls": {
            "cacertpath": "string",
            "enable": true,
            "validateserver": true
          },
          "type": "string"
        }
      },
      "summaryperiod": 0
    },
    "metric": {
      "enable": true,
      "format": {
        "statsdmaxlen": 0,
        "statsdprefix": "string",
        "type": "string",
        "verbosity": 0
      },
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      },
      "watch": [
        "string"
      ]
    },
    "payload": {
      "dir": "string",
      "enable": true
    },
    "protocol": [
      {
        "binary": true,
        "detect": true,
        "len": 0,
        "name": "string",
        "payload": true,
        "regex": "string"
      }
    ],
    "tags": [
      {
        "key": "string",
        "value": "string"
      }
    ]
  },
  "env": "string",
  "hostname": "string",
  "procname": "string",
  "username": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ancestor|string|false|none|none|
|arg|string|false|none|none|
|config|[AppscopeConfig](#schemaappscopeconfig)|true|none|none|
|env|string|false|none|none|
|hostname|string|false|none|none|
|procname|string|false|none|none|
|username|string|false|none|none|

<h2 id="tocS_AppscopeConfig">AppscopeConfig</h2>
<!-- backwards compatibility -->
<a id="schemaappscopeconfig"></a>
<a id="schema_AppscopeConfig"></a>
<a id="tocSappscopeconfig"></a>
<a id="tocsappscopeconfig"></a>

```json
{
  "cribl": {
    "authtoken": "string",
    "enable": true,
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "useScopeSourceTransport": true
  },
  "event": {
    "enable": true,
    "format": {
      "enhancefs": true,
      "maxeventpersec": 0
    },
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "type": "ndjson",
    "watch": [
      {
        "allowbinary": true,
        "enabled": true,
        "field": "string",
        "headers": "string",
        "name": "string",
        "type": "string",
        "value": "string"
      }
    ]
  },
  "libscope": {
    "commanddir": "string",
    "configevent": true,
    "log": {
      "level": "error",
      "transport": {
        "buffer": "line",
        "host": "string",
        "path": "string",
        "port": 0,
        "tls": {
          "cacertpath": "string",
          "enable": true,
          "validateserver": true
        },
        "type": "string"
      }
    },
    "summaryperiod": 0
  },
  "metric": {
    "enable": true,
    "format": {
      "statsdmaxlen": 0,
      "statsdprefix": "string",
      "type": "string",
      "verbosity": 0
    },
    "transport": {
      "buffer": "line",
      "host": "string",
      "path": "string",
      "port": 0,
      "tls": {
        "cacertpath": "string",
        "enable": true,
        "validateserver": true
      },
      "type": "string"
    },
    "watch": [
      "string"
    ]
  },
  "payload": {
    "dir": "string",
    "enable": true
  },
  "protocol": [
    {
      "binary": true,
      "detect": true,
      "len": 0,
      "name": "string",
      "payload": true,
      "regex": "string"
    }
  ],
  "tags": [
    {
      "key": "string",
      "value": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cribl|object|false|none|none|
|» authtoken|string|false|none|none|
|» enable|boolean|false|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|» useScopeSourceTransport|boolean|false|none|none|
|event|object|false|none|none|
|» enable|boolean|true|none|none|
|» format|object|true|none|none|
|»» enhancefs|boolean|true|none|none|
|»» maxeventpersec|number|true|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|» type|string|true|none|none|
|» watch|[object]|true|none|none|
|»» allowbinary|boolean|false|none|none|
|»» enabled|boolean|false|none|none|
|»» field|string|false|none|none|
|»» headers|string|false|none|none|
|»» name|string|false|none|none|
|»» type|string|true|none|none|
|»» value|string|false|none|none|
|libscope|object|false|none|none|
|» commanddir|string|false|none|none|
|» configevent|boolean|false|none|none|
|» log|object|false|none|none|
|»» level|string|false|none|none|
|»» transport|[AppscopeTransport](#schemaappscopetransport)|false|none|none|
|» summaryperiod|number|false|none|none|
|metric|object|false|none|none|
|» enable|boolean|true|none|none|
|» format|object|true|none|none|
|»» statsdmaxlen|number|false|none|none|
|»» statsdprefix|string|false|none|none|
|»» type|string|false|none|none|
|»» verbosity|number|false|none|none|
|» transport|[AppscopeTransport](#schemaappscopetransport)|true|none|none|
|» watch|[string]|true|none|none|
|payload|object|false|none|none|
|» dir|string|true|none|none|
|» enable|boolean|true|none|none|
|protocol|[object]|false|none|none|
|» binary|boolean|true|none|none|
|» detect|boolean|true|none|none|
|» len|number|true|none|none|
|» name|string|true|none|none|
|» payload|boolean|true|none|none|
|» regex|string|true|none|none|
|tags|[object]|false|none|none|
|» key|string|true|none|none|
|» value|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ndjson|
|level|error|
|level|debug|
|level|info|
|level|warning|
|level|none|

