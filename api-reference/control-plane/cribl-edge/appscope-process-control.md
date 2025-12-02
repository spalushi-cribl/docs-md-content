
<h1 id="cribl-edge-api-appscope-process-control">Cribl Edge API - AppScope Process Control v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-edge-api-appscope-process-control-appscope-process-control">AppScope Process Control</h1>

## Get a detailed list of scoped processes running on the edge host

<a id="opIdgetEdgeAppscopeProcesses"></a>

> Code samples

`GET /edge/appscope/processes`

Get a detailed list of scoped processes running on the edge host

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cfg": {
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
      "config_id": "string",
      "id": "string",
      "interfaces": [
        {
          "config": "string",
          "connected": true,
          "name": "string"
        }
      ],
      "lastError": "string",
      "process": {
        "hostPid": 0,
        "id": "string",
        "machine_id": "string",
        "pid": 0,
        "uuid": "string"
      },
      "processingStatus": "normal",
      "source_id": "string",
      "status": "green"
    }
  ]
}
```

<h3 id="get-a-detailed-list-of-scoped-processes-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppScopeProcess objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-a-detailed-list-of-scoped-processes-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppScopeProcess](#schemaappscopeprocess)]|false|none|none|
|»» cfg|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|false|none|none|
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
|»» config_id|string|false|none|none|
|»» id|string|true|none|none|
|»» interfaces|[object]|false|none|none|
|»»» config|string|false|none|none|
|»»» connected|boolean|false|none|none|
|»»» name|string|false|none|none|
|»» lastError|string|false|none|none|
|»» process|object|false|none|none|
|»»» hostPid|number|false|none|none|
|»»» id|string|false|none|none|
|»»» machine_id|string|false|none|none|
|»»» pid|number|true|none|none|
|»»» uuid|string|false|none|none|
|»» processingStatus|[AppScopeProcessingStatus](#schemaappscopeprocessingstatus)¦null|false|none|none|
|»» source_id|string|false|none|none|
|»» status|[AppScopeProcessStatus](#schemaappscopeprocessstatus)¦null|false|none|none|

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
|processingStatus|normal|
|processingStatus|attaching|
|processingStatus|attach_failed|
|processingStatus|updating|
|processingStatus|update_failed|
|processingStatus|detaching|
|processingStatus|detached|
|processingStatus|detach_failed|
|status|green|
|status|yellow|
|status|red|

<aside class="success">
This operation does not require authentication
</aside>

## Detach AppScope from a process running on the edge host

<a id="opIddeleteEdgeAppscopeProcessesByPid"></a>

> Code samples

`DELETE /edge/appscope/processes/{pid}`

Detach AppScope from a process running on the edge host

<h3 id="detach-appscope-from-a-process-running-on-the-edge-host-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pid|path|string|true|config string required|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cfg": {
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
      "config_id": "string",
      "id": "string",
      "interfaces": [
        {
          "config": "string",
          "connected": true,
          "name": "string"
        }
      ],
      "lastError": "string",
      "process": {
        "hostPid": 0,
        "id": "string",
        "machine_id": "string",
        "pid": 0,
        "uuid": "string"
      },
      "processingStatus": "normal",
      "source_id": "string",
      "status": "green"
    }
  ]
}
```

<h3 id="detach-appscope-from-a-process-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppScopeProcess objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="detach-appscope-from-a-process-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppScopeProcess](#schemaappscopeprocess)]|false|none|none|
|»» cfg|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|false|none|none|
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
|»» config_id|string|false|none|none|
|»» id|string|true|none|none|
|»» interfaces|[object]|false|none|none|
|»»» config|string|false|none|none|
|»»» connected|boolean|false|none|none|
|»»» name|string|false|none|none|
|»» lastError|string|false|none|none|
|»» process|object|false|none|none|
|»»» hostPid|number|false|none|none|
|»»» id|string|false|none|none|
|»»» machine_id|string|false|none|none|
|»»» pid|number|true|none|none|
|»»» uuid|string|false|none|none|
|»» processingStatus|[AppScopeProcessingStatus](#schemaappscopeprocessingstatus)¦null|false|none|none|
|»» source_id|string|false|none|none|
|»» status|[AppScopeProcessStatus](#schemaappscopeprocessstatus)¦null|false|none|none|

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
|processingStatus|normal|
|processingStatus|attaching|
|processingStatus|attach_failed|
|processingStatus|updating|
|processingStatus|update_failed|
|processingStatus|detaching|
|processingStatus|detached|
|processingStatus|detach_failed|
|status|green|
|status|yellow|
|status|red|

<aside class="success">
This operation does not require authentication
</aside>

## Get details of a scoped process running on the edge host

<a id="opIdgetEdgeAppscopeProcessesByPid"></a>

> Code samples

`GET /edge/appscope/processes/{pid}`

Get details of a scoped process running on the edge host

<h3 id="get-details-of-a-scoped-process-running-on-the-edge-host-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pid|path|string|true|pid|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cfg": {
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
      "config_id": "string",
      "id": "string",
      "interfaces": [
        {
          "config": "string",
          "connected": true,
          "name": "string"
        }
      ],
      "lastError": "string",
      "process": {
        "hostPid": 0,
        "id": "string",
        "machine_id": "string",
        "pid": 0,
        "uuid": "string"
      },
      "processingStatus": "normal",
      "source_id": "string",
      "status": "green"
    }
  ]
}
```

<h3 id="get-details-of-a-scoped-process-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppScopeProcess objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-details-of-a-scoped-process-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppScopeProcess](#schemaappscopeprocess)]|false|none|none|
|»» cfg|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|false|none|none|
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
|»» config_id|string|false|none|none|
|»» id|string|true|none|none|
|»» interfaces|[object]|false|none|none|
|»»» config|string|false|none|none|
|»»» connected|boolean|false|none|none|
|»»» name|string|false|none|none|
|»» lastError|string|false|none|none|
|»» process|object|false|none|none|
|»»» hostPid|number|false|none|none|
|»»» id|string|false|none|none|
|»»» machine_id|string|false|none|none|
|»»» pid|number|true|none|none|
|»»» uuid|string|false|none|none|
|»» processingStatus|[AppScopeProcessingStatus](#schemaappscopeprocessingstatus)¦null|false|none|none|
|»» source_id|string|false|none|none|
|»» status|[AppScopeProcessStatus](#schemaappscopeprocessstatus)¦null|false|none|none|

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
|processingStatus|normal|
|processingStatus|attaching|
|processingStatus|attach_failed|
|processingStatus|updating|
|processingStatus|update_failed|
|processingStatus|detaching|
|processingStatus|detached|
|processingStatus|detach_failed|
|status|green|
|status|yellow|
|status|red|

<aside class="success">
This operation does not require authentication
</aside>

## Update AppScope configuration for a process running on the edge host

<a id="opIdupdateEdgeAppscopeProcessesByPid"></a>

> Code samples

`PUT /edge/appscope/processes/{pid}`

Update AppScope configuration for a process running on the edge host

<h3 id="update-appscope-configuration-for-a-process-running-on-the-edge-host-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|pid|path|string|true|config string required|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "cfg": {
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
      "config_id": "string",
      "id": "string",
      "interfaces": [
        {
          "config": "string",
          "connected": true,
          "name": "string"
        }
      ],
      "lastError": "string",
      "process": {
        "hostPid": 0,
        "id": "string",
        "machine_id": "string",
        "pid": 0,
        "uuid": "string"
      },
      "processingStatus": "normal",
      "source_id": "string",
      "status": "green"
    }
  ]
}
```

<h3 id="update-appscope-configuration-for-a-process-running-on-the-edge-host-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AppScopeProcess objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-appscope-configuration-for-a-process-running-on-the-edge-host-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AppScopeProcess](#schemaappscopeprocess)]|false|none|none|
|»» cfg|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|false|none|none|
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
|»» config_id|string|false|none|none|
|»» id|string|true|none|none|
|»» interfaces|[object]|false|none|none|
|»»» config|string|false|none|none|
|»»» connected|boolean|false|none|none|
|»»» name|string|false|none|none|
|»» lastError|string|false|none|none|
|»» process|object|false|none|none|
|»»» hostPid|number|false|none|none|
|»»» id|string|false|none|none|
|»»» machine_id|string|false|none|none|
|»»» pid|number|true|none|none|
|»»» uuid|string|false|none|none|
|»» processingStatus|[AppScopeProcessingStatus](#schemaappscopeprocessingstatus)¦null|false|none|none|
|»» source_id|string|false|none|none|
|»» status|[AppScopeProcessStatus](#schemaappscopeprocessstatus)¦null|false|none|none|

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
|processingStatus|normal|
|processingStatus|attaching|
|processingStatus|attach_failed|
|processingStatus|updating|
|processingStatus|update_failed|
|processingStatus|detaching|
|processingStatus|detached|
|processingStatus|detach_failed|
|status|green|
|status|yellow|
|status|red|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_AppScopeProcess">AppScopeProcess</h2>
<!-- backwards compatibility -->
<a id="schemaappscopeprocess"></a>
<a id="schema_AppScopeProcess"></a>
<a id="tocSappscopeprocess"></a>
<a id="tocsappscopeprocess"></a>

```json
{
  "cfg": {
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
  "config_id": "string",
  "id": "string",
  "interfaces": [
    {
      "config": "string",
      "connected": true,
      "name": "string"
    }
  ],
  "lastError": "string",
  "process": {
    "hostPid": 0,
    "id": "string",
    "machine_id": "string",
    "pid": 0,
    "uuid": "string"
  },
  "processingStatus": "normal",
  "source_id": "string",
  "status": "green"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cfg|[AppscopeConfigWithCustom](#schemaappscopeconfigwithcustom)|false|none|none|
|config_id|string|false|none|none|
|id|string|true|none|none|
|interfaces|[object]|false|none|none|
|» config|string|false|none|none|
|» connected|boolean|false|none|none|
|» name|string|false|none|none|
|lastError|string|false|none|none|
|process|object|false|none|none|
|» hostPid|number|false|none|none|
|» id|string|false|none|none|
|» machine_id|string|false|none|none|
|» pid|number|true|none|none|
|» uuid|string|false|none|none|
|processingStatus|[AppScopeProcessingStatus](#schemaappscopeprocessingstatus)|false|none|none|
|source_id|string|false|none|none|
|status|[AppScopeProcessStatus](#schemaappscopeprocessstatus)|false|none|none|

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

<h2 id="tocS_AppScopeProcessingStatus">AppScopeProcessingStatus</h2>
<!-- backwards compatibility -->
<a id="schemaappscopeprocessingstatus"></a>
<a id="schema_AppScopeProcessingStatus"></a>
<a id="tocSappscopeprocessingstatus"></a>
<a id="tocsappscopeprocessingstatus"></a>

```json
"normal"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string¦null|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|normal|
|*anonymous*|attaching|
|*anonymous*|attach_failed|
|*anonymous*|updating|
|*anonymous*|update_failed|
|*anonymous*|detaching|
|*anonymous*|detached|
|*anonymous*|detach_failed|

<h2 id="tocS_AppScopeProcessStatus">AppScopeProcessStatus</h2>
<!-- backwards compatibility -->
<a id="schemaappscopeprocessstatus"></a>
<a id="schema_AppScopeProcessStatus"></a>
<a id="tocSappscopeprocessstatus"></a>
<a id="tocsappscopeprocessstatus"></a>

```json
"green"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string¦null|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|green|
|*anonymous*|yellow|
|*anonymous*|red|

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

