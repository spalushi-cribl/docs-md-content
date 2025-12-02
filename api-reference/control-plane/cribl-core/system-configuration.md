
<h1 id="cribl-core-api-system-configuration">Cribl Core API - System Configuration v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-system-configuration-system-configuration">System Configuration</h1>

## Get authentication settings

<a id="opIdgetSystemSettingsAuth"></a>

> Code samples

`GET /system/settings/auth`

Get authentication settings

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "fallback": true,
      "fallbackBadLogin": true,
      "filter_type": "string",
      "host": "string",
      "port": 0,
      "ssl": true,
      "type": "ldap"
    }
  ]
}
```

<h3 id="get-authentication-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AuthConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-authentication-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AuthConfig](#schemaauthconfig)]|false|none|none|
|»» fallback|boolean|true|none|none|
|»» fallbackBadLogin|boolean|true|none|none|
|»» filter_type|string|false|none|none|
|»» host|string|true|none|none|
|»» port|number|true|none|none|
|»» ssl|boolean|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ldap|
|type|splunk|
|type|local|
|type|openid|
|type|saas|
|type|saml|

<aside class="success">
This operation does not require authentication
</aside>

## Update authentication settings

<a id="opIdupdateSystemSettingsAuth"></a>

> Code samples

`PATCH /system/settings/auth`

Update authentication settings

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "fallback": true,
      "fallbackBadLogin": true,
      "filter_type": "string",
      "host": "string",
      "port": 0,
      "ssl": true,
      "type": "ldap"
    }
  ]
}
```

<h3 id="update-authentication-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of AuthConfig objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-authentication-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[AuthConfig](#schemaauthconfig)]|false|none|none|
|»» fallback|boolean|true|none|none|
|»» fallbackBadLogin|boolean|true|none|none|
|»» filter_type|string|false|none|none|
|»» host|string|true|none|none|
|»» port|number|true|none|none|
|»» ssl|boolean|true|none|none|
|»» type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ldap|
|type|splunk|
|type|local|
|type|openid|
|type|saas|
|type|saml|

<aside class="success">
This operation does not require authentication
</aside>

## Get Cribl system settings

<a id="opIdgetSystemSettingsConf"></a>

> Code samples

`GET /system/settings/conf`

Get Cribl system settings

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "api": {
        "baseUrl": "string",
        "disableApiCache": true,
        "disabled": true,
        "headers": {},
        "host": "string",
        "idleSessionTTL": 0,
        "listenOnPort": true,
        "loginRateLimit": "string",
        "port": 0,
        "protocol": "string",
        "scripts": true,
        "sensitiveFields": [
          "string"
        ],
        "ssl": {
          "caPath": "string",
          "certPath": "string",
          "disabled": true,
          "passphrase": "string",
          "privKeyPath": "string"
        },
        "ssoRateLimit": "string",
        "workerRemoteAccess": true
      },
      "backups": {
        "backupPersistence": "string",
        "backupsDirectory": "string"
      },
      "customLogo": {
        "enabled": true,
        "logoDescription": "string",
        "logoImage": "string"
      },
      "pii": {
        "enablePiiDetection": true
      },
      "proxy": {
        "useEnvVars": true
      },
      "rollback": {
        "rollbackEnabled": true,
        "rollbackRetries": 0,
        "rollbackTimeout": 0
      },
      "shutdown": {
        "drainTimeout": 0
      },
      "sni": {
        "disableSNIRouting": true
      },
      "sockets": {
        "directory": "string"
      },
      "support": {
        "featureFlagOverrides": [
          {
            "disabled": true,
            "flagId": "string"
          }
        ]
      },
      "system": {
        "intercom": true,
        "upgrade": false
      },
      "tls": {
        "defaultCipherList": "string",
        "defaultEcdhCurve": "string",
        "maxVersion": "string",
        "minVersion": "string",
        "rejectUnauthorized": true
      },
      "upgradeGroupSettings": {
        "isRolling": true,
        "quantity": 0,
        "retryCount": 0,
        "retryDelay": 0
      },
      "upgradeSettings": {
        "automaticUpgradeCheckPeriod": "string",
        "disableAutomaticUpgrade": true,
        "enableLegacyEdgeUpgrade": true,
        "packageUrls": [
          {
            "packageHashUrl": "string",
            "packageUrl": "string"
          }
        ],
        "upgradeSource": "string"
      },
      "workers": {
        "count": 0,
        "enableHeapSnapshots": true,
        "loadThrottlePerc": 0,
        "memory": 0,
        "minimum": 0,
        "startupMaxConns": 0,
        "startupThrottleTimeout": 0,
        "v8SingleThread": true
      }
    }
  ]
}
```

<h3 id="get-cribl-system-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SystemSettingsConf objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-cribl-system-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SystemSettingsConf](#schemasystemsettingsconf)]|false|none|none|
|»» api|object|true|none|none|
|»»» baseUrl|string|false|none|none|
|»»» disableApiCache|boolean|false|none|none|
|»»» disabled|boolean|true|none|none|
|»»» headers|object|false|none|none|
|»»» host|string|true|none|none|
|»»» idleSessionTTL|number|false|none|none|
|»»» listenOnPort|boolean|false|none|none|
|»»» loginRateLimit|string|false|none|none|
|»»» port|number|true|none|none|
|»»» protocol|string|true|none|none|
|»»» scripts|boolean|false|none|none|
|»»» sensitiveFields|[string]|false|none|none|
|»»» ssl|object|true|none|none|
|»»»» caPath|string|false|none|none|
|»»»» certPath|string|true|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» passphrase|string|true|none|none|
|»»»» privKeyPath|string|true|none|none|
|»»» ssoRateLimit|string|false|none|none|
|»»» workerRemoteAccess|boolean|true|none|none|
|»» backups|object|true|none|none|
|»»» backupPersistence|string|true|none|none|
|»»» backupsDirectory|string|true|none|none|
|»» customLogo|object|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» logoDescription|string|true|none|none|
|»»» logoImage|string|true|none|none|
|»» pii|object|true|none|none|
|»»» enablePiiDetection|boolean|true|none|none|
|»» proxy|object|true|none|none|
|»»» useEnvVars|boolean|true|none|none|
|»» rollback|object|true|none|none|
|»»» rollbackEnabled|boolean|true|none|none|
|»»» rollbackRetries|number|false|none|none|
|»»» rollbackTimeout|number|false|none|none|
|»» shutdown|object|true|none|none|
|»»» drainTimeout|number|true|none|none|
|»» sni|object|true|none|none|
|»»» disableSNIRouting|boolean|true|none|none|
|»» sockets|object|false|none|none|
|»»» directory|string|false|none|none|
|»» support|object|false|none|none|
|»»» featureFlagOverrides|[object]|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» flagId|string|true|none|none|
|»» system|object|true|none|none|
|»»» intercom|boolean|true|none|none|
|»»» upgrade|any|true|none|none|
|»» tls|object|true|none|none|
|»»» defaultCipherList|string|true|none|none|
|»»» defaultEcdhCurve|string|true|none|none|
|»»» maxVersion|string|true|none|none|
|»»» minVersion|string|true|none|none|
|»»» rejectUnauthorized|boolean|true|none|none|
|»» upgradeGroupSettings|[UpgradeGroupSettings](#schemaupgradegroupsettings)|true|none|none|
|»»» isRolling|boolean|false|none|none|
|»»» quantity|number|false|none|none|
|»»» retryCount|number|false|none|none|
|»»» retryDelay|number|false|none|none|
|»» upgradeSettings|[UpgradeSettings](#schemaupgradesettings)|true|none|none|
|»»» automaticUpgradeCheckPeriod|string|false|none|none|
|»»» disableAutomaticUpgrade|boolean|true|none|none|
|»»» enableLegacyEdgeUpgrade|boolean|true|none|none|
|»»» packageUrls|[[UpgradePackageUrls](#schemaupgradepackageurls)]|false|none|none|
|»»»» packageHashUrl|string|false|none|none|
|»»»» packageUrl|string|true|none|none|
|»»» upgradeSource|string|true|none|none|
|»» workers|object|true|none|none|
|»»» count|number|true|none|none|
|»»» enableHeapSnapshots|boolean|false|none|none|
|»»» loadThrottlePerc|number|false|none|none|
|»»» memory|number|true|none|none|
|»»» minimum|number|true|none|none|
|»»» startupMaxConns|number|false|none|none|
|»»» startupThrottleTimeout|number|false|none|none|
|»»» v8SingleThread|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|upgrade|false|
|upgrade|api|

<aside class="success">
This operation does not require authentication
</aside>

## Update Cribl system settings

<a id="opIdupdateSystemSettingsConf"></a>

> Code samples

`PATCH /system/settings/conf`

Update Cribl system settings

> Body parameter

```json
{
  "api": {
    "baseUrl": "string",
    "disableApiCache": true,
    "disabled": true,
    "headers": {},
    "host": "string",
    "idleSessionTTL": 0,
    "listenOnPort": true,
    "loginRateLimit": "string",
    "port": 0,
    "protocol": "string",
    "scripts": true,
    "sensitiveFields": [
      "string"
    ],
    "ssl": {
      "caPath": "string",
      "certPath": "string",
      "disabled": true,
      "passphrase": "string",
      "privKeyPath": "string"
    },
    "ssoRateLimit": "string",
    "workerRemoteAccess": true
  },
  "backups": {
    "backupPersistence": "string",
    "backupsDirectory": "string"
  },
  "customLogo": {
    "enabled": true,
    "logoDescription": "string",
    "logoImage": "string"
  },
  "pii": {
    "enablePiiDetection": true
  },
  "proxy": {
    "useEnvVars": true
  },
  "rollback": {
    "rollbackEnabled": true,
    "rollbackRetries": 0,
    "rollbackTimeout": 0
  },
  "shutdown": {
    "drainTimeout": 0
  },
  "sni": {
    "disableSNIRouting": true
  },
  "sockets": {
    "directory": "string"
  },
  "support": {
    "featureFlagOverrides": [
      {
        "disabled": true,
        "flagId": "string"
      }
    ]
  },
  "system": {
    "intercom": true,
    "upgrade": false
  },
  "tls": {
    "defaultCipherList": "string",
    "defaultEcdhCurve": "string",
    "maxVersion": "string",
    "minVersion": "string",
    "rejectUnauthorized": true
  },
  "upgradeGroupSettings": {
    "isRolling": true,
    "quantity": 0,
    "retryCount": 0,
    "retryDelay": 0
  },
  "upgradeSettings": {
    "automaticUpgradeCheckPeriod": "string",
    "disableAutomaticUpgrade": true,
    "enableLegacyEdgeUpgrade": true,
    "packageUrls": [
      {
        "packageHashUrl": "string",
        "packageUrl": "string"
      }
    ],
    "upgradeSource": "string"
  },
  "workers": {
    "count": 0,
    "enableHeapSnapshots": true,
    "loadThrottlePerc": 0,
    "memory": 0,
    "minimum": 0,
    "startupMaxConns": 0,
    "startupThrottleTimeout": 0,
    "v8SingleThread": true
  }
}
```

<h3 id="update-cribl-system-settings-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[SystemSettingsConf](#schemasystemsettingsconf)|true|SystemSettingsConf object|
|» api|body|object|true|none|
|»» baseUrl|body|string|false|none|
|»» disableApiCache|body|boolean|false|none|
|»» disabled|body|boolean|true|none|
|»» headers|body|object|false|none|
|»» host|body|string|true|none|
|»» idleSessionTTL|body|number|false|none|
|»» listenOnPort|body|boolean|false|none|
|»» loginRateLimit|body|string|false|none|
|»» port|body|number|true|none|
|»» protocol|body|string|true|none|
|»» scripts|body|boolean|false|none|
|»» sensitiveFields|body|[string]|false|none|
|»» ssl|body|object|true|none|
|»»» caPath|body|string|false|none|
|»»» certPath|body|string|true|none|
|»»» disabled|body|boolean|true|none|
|»»» passphrase|body|string|true|none|
|»»» privKeyPath|body|string|true|none|
|»» ssoRateLimit|body|string|false|none|
|»» workerRemoteAccess|body|boolean|true|none|
|» backups|body|object|true|none|
|»» backupPersistence|body|string|true|none|
|»» backupsDirectory|body|string|true|none|
|» customLogo|body|object|true|none|
|»» enabled|body|boolean|true|none|
|»» logoDescription|body|string|true|none|
|»» logoImage|body|string|true|none|
|» pii|body|object|true|none|
|»» enablePiiDetection|body|boolean|true|none|
|» proxy|body|object|true|none|
|»» useEnvVars|body|boolean|true|none|
|» rollback|body|object|true|none|
|»» rollbackEnabled|body|boolean|true|none|
|»» rollbackRetries|body|number|false|none|
|»» rollbackTimeout|body|number|false|none|
|» shutdown|body|object|true|none|
|»» drainTimeout|body|number|true|none|
|» sni|body|object|true|none|
|»» disableSNIRouting|body|boolean|true|none|
|» sockets|body|object|false|none|
|»» directory|body|string|false|none|
|» support|body|object|false|none|
|»» featureFlagOverrides|body|[object]|false|none|
|»»» disabled|body|boolean|true|none|
|»»» flagId|body|string|true|none|
|» system|body|object|true|none|
|»» intercom|body|boolean|true|none|
|»» upgrade|body|any|true|none|
|» tls|body|object|true|none|
|»» defaultCipherList|body|string|true|none|
|»» defaultEcdhCurve|body|string|true|none|
|»» maxVersion|body|string|true|none|
|»» minVersion|body|string|true|none|
|»» rejectUnauthorized|body|boolean|true|none|
|» upgradeGroupSettings|body|[UpgradeGroupSettings](#schemaupgradegroupsettings)|true|none|
|»» isRolling|body|boolean|false|none|
|»» quantity|body|number|false|none|
|»» retryCount|body|number|false|none|
|»» retryDelay|body|number|false|none|
|» upgradeSettings|body|[UpgradeSettings](#schemaupgradesettings)|true|none|
|»» automaticUpgradeCheckPeriod|body|string|false|none|
|»» disableAutomaticUpgrade|body|boolean|true|none|
|»» enableLegacyEdgeUpgrade|body|boolean|true|none|
|»» packageUrls|body|[[UpgradePackageUrls](#schemaupgradepackageurls)]|false|none|
|»»» packageHashUrl|body|string|false|none|
|»»» packageUrl|body|string|true|none|
|»» upgradeSource|body|string|true|none|
|» workers|body|object|true|none|
|»» count|body|number|true|none|
|»» enableHeapSnapshots|body|boolean|false|none|
|»» loadThrottlePerc|body|number|false|none|
|»» memory|body|number|true|none|
|»» minimum|body|number|true|none|
|»» startupMaxConns|body|number|false|none|
|»» startupThrottleTimeout|body|number|false|none|
|»» v8SingleThread|body|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» upgrade|false|
|»» upgrade|api|

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "api": {
        "baseUrl": "string",
        "disableApiCache": true,
        "disabled": true,
        "headers": {},
        "host": "string",
        "idleSessionTTL": 0,
        "listenOnPort": true,
        "loginRateLimit": "string",
        "port": 0,
        "protocol": "string",
        "scripts": true,
        "sensitiveFields": [
          "string"
        ],
        "ssl": {
          "caPath": "string",
          "certPath": "string",
          "disabled": true,
          "passphrase": "string",
          "privKeyPath": "string"
        },
        "ssoRateLimit": "string",
        "workerRemoteAccess": true
      },
      "auth": {
        "fallback": true,
        "fallbackBadLogin": true,
        "filter_type": "string",
        "host": "string",
        "port": 0,
        "ssl": true,
        "type": "ldap"
      },
      "backups": {
        "backupPersistence": "string",
        "backupsDirectory": "string"
      },
      "customLogo": {
        "enabled": true,
        "logoDescription": "string",
        "logoImage": "string"
      },
      "distributed": {
        "mode": "edge"
      },
      "fips": true,
      "git": {
        "authType": "string",
        "autoAction": "string",
        "autoActionMessage": "string",
        "autoActionSchedule": "string",
        "branch": "string",
        "commitDeploySingleAction": true,
        "copilotAutoGitCommitMessages": true,
        "defaultCommitMessage": "string",
        "gitOps": "none",
        "password": "string",
        "remote": "string",
        "sshKey": "string",
        "strictHostKeyChecking": true,
        "timeout": 0,
        "user": "string"
      },
      "jobLimits": {
        "concurrentJobLimit": 0,
        "concurrentScheduledJobLimit": 0,
        "concurrentSystemJobLimit": 0,
        "concurrentSystemTaskLimit": 0,
        "concurrentTaskLimit": 0,
        "disableTasks": true,
        "finishedJobArtifactsLimit": 0,
        "finishedTaskArtifactsLimit": 0,
        "jobArtifactsReaperPeriod": "string",
        "jobTimeout": "string",
        "maxTaskPerc": 0,
        "schedulingPolicy": "string",
        "taskHeartbeatPeriod": 0,
        "taskManifestFlushPeriodMs": 0,
        "taskManifestMaxBufferSize": 0,
        "taskManifestReadBufferSize": "string",
        "taskPollTimeoutMs": 0
      },
      "limits": {
        "cpuProfileTTL": "string",
        "disableMetricsAccessorCache": true,
        "edgeMetricsCustomExpression": "string",
        "edgeMetricsMode": "minimal",
        "edgeNodesCount": 0,
        "enableMetricsPersistence": true,
        "enableWorkerPersistence": true,
        "eventsMetadataSources": [
          "string"
        ],
        "largeEventsThreshold": "string",
        "lookupMaxSize": "string",
        "lookupMaxTotalSize": "string",
        "maxDimensionValueSize": 0,
        "maxMetrics": 0,
        "maxPQSize": "string",
        "maxReconnectInterval": "string",
        "metricsDirectory": "string",
        "metricsDropList": [
          "string"
        ],
        "metricsFieldsBlacklist": [
          "string"
        ],
        "metricsGCPeriod": "string",
        "metricsMaxCardinality": 0,
        "metricsMaxDiskSpace": "string",
        "metricsNeverDropList": [
          "string"
        ],
        "metricsWorkerIdBlacklist": [
          "string"
        ],
        "minFreeSpace": "string",
        "minReconnectInterval": "string",
        "netFlowTemplateFlushInterval": 0,
        "randomReconnectInterval": "string",
        "samples": {
          "maxSize": "string"
        },
        "workerMaxMetrics": 0
      },
      "pii": {
        "enablePiiDetection": true
      },
      "proxy": {
        "useEnvVars": true
      },
      "redisCacheLimits": {
        "clientTrackingMechanism": "string",
        "enableServerAssist": true,
        "keyTTLSecs": 0,
        "maxCacheSize": 0,
        "maxNumKeys": 0,
        "servicePeriodSecs": 0
      },
      "redisLimits": {
        "connections": {
          "disabled": true,
          "maxConnections": 0
        }
      },
      "rollback": {
        "rollbackEnabled": true,
        "rollbackRetries": 0,
        "rollbackTimeout": 0
      },
      "searchLimits": {
        "compressObjectCacheArtifacts": true,
        "fieldSummaryMaxFields": 0,
        "fieldSummaryMaxNestedDepth": 0,
        "maxConcurrentSearches": 0,
        "maxExecutorsPerSearch": 0,
        "maxResultsPerSearch": 0,
        "maxSearchDuration": "string",
        "searchHistoryMaxJobs": 0,
        "searchHistoryTTL": "string",
        "searchQueueLength": 0,
        "warmPoolSize": 0,
        "writeOnlyProviderSecrets": true
      },
      "servicesLimits": {
        "connections": {
          "memoryLimit": "string",
          "procs": 0
        },
        "metrics": {
          "memoryLimit": "string",
          "procs": 0
        },
        "notifications": {
          "memoryLimit": "string",
          "procs": 0
        }
      },
      "shutdown": {
        "drainTimeout": 0
      },
      "sni": {
        "disableSNIRouting": true
      },
      "sockets": {
        "directory": "string"
      },
      "support": {
        "featureFlagOverrides": [
          {
            "disabled": true,
            "flagId": "string"
          }
        ]
      },
      "system": {
        "intercom": true,
        "upgrade": false
      },
      "tls": {
        "defaultCipherList": "string",
        "defaultEcdhCurve": "string",
        "maxVersion": "string",
        "minVersion": "string",
        "rejectUnauthorized": true
      },
      "upgradeGroupSettings": {
        "isRolling": true,
        "quantity": 0,
        "retryCount": 0,
        "retryDelay": 0
      },
      "upgradeSettings": {
        "automaticUpgradeCheckPeriod": "string",
        "disableAutomaticUpgrade": true,
        "enableLegacyEdgeUpgrade": true,
        "packageUrls": [
          {
            "packageHashUrl": "string",
            "packageUrl": "string"
          }
        ],
        "upgradeSource": "string"
      },
      "workers": {
        "count": 0,
        "enableHeapSnapshots": true,
        "loadThrottlePerc": 0,
        "memory": 0,
        "minimum": 0,
        "startupMaxConns": 0,
        "startupThrottleTimeout": 0,
        "v8SingleThread": true
      }
    }
  ]
}
```

<h3 id="update-cribl-system-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SystemSettings objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-cribl-system-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SystemSettings](#schemasystemsettings)]|false|none|none|
|»» api|object|true|none|none|
|»»» baseUrl|string|false|none|none|
|»»» disableApiCache|boolean|false|none|none|
|»»» disabled|boolean|true|none|none|
|»»» headers|object|false|none|none|
|»»» host|string|true|none|none|
|»»» idleSessionTTL|number|false|none|none|
|»»» listenOnPort|boolean|false|none|none|
|»»» loginRateLimit|string|false|none|none|
|»»» port|number|true|none|none|
|»»» protocol|string|true|none|none|
|»»» scripts|boolean|false|none|none|
|»»» sensitiveFields|[string]|false|none|none|
|»»» ssl|object|true|none|none|
|»»»» caPath|string|false|none|none|
|»»»» certPath|string|true|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» passphrase|string|true|none|none|
|»»»» privKeyPath|string|true|none|none|
|»»» ssoRateLimit|string|false|none|none|
|»»» workerRemoteAccess|boolean|true|none|none|
|»» auth|[AuthConfig](#schemaauthconfig)|true|none|none|
|»»» fallback|boolean|true|none|none|
|»»» fallbackBadLogin|boolean|true|none|none|
|»»» filter_type|string|false|none|none|
|»»» host|string|true|none|none|
|»»» port|number|true|none|none|
|»»» ssl|boolean|true|none|none|
|»»» type|string|true|none|none|
|»» backups|object|true|none|none|
|»»» backupPersistence|string|true|none|none|
|»»» backupsDirectory|string|true|none|none|
|»» customLogo|object|true|none|none|
|»»» enabled|boolean|true|none|none|
|»»» logoDescription|string|true|none|none|
|»»» logoImage|string|true|none|none|
|»» distributed|object|true|none|none|
|»»» mode|string|true|none|none|
|»» fips|boolean|true|none|none|
|»» git|[GitSettings](#schemagitsettings)|true|none|none|
|»»» authType|string|false|none|none|
|»»» autoAction|string|false|none|none|
|»»» autoActionMessage|string|false|none|none|
|»»» autoActionSchedule|string|false|none|none|
|»»» branch|string|false|none|none|
|»»» commitDeploySingleAction|boolean|false|none|none|
|»»» copilotAutoGitCommitMessages|boolean|false|none|none|
|»»» defaultCommitMessage|string|false|none|none|
|»»» gitOps|[GitOpsType](#schemagitopstype)|false|none|none|
|»»» password|string|false|none|none|
|»»» remote|string|false|none|none|
|»»» sshKey|string|false|none|none|
|»»» strictHostKeyChecking|boolean|false|none|none|
|»»» timeout|number|false|none|none|
|»»» user|string|false|none|none|
|»» jobLimits|[JobSettings](#schemajobsettings)|true|none|none|
|»»» concurrentJobLimit|number|true|none|none|
|»»» concurrentScheduledJobLimit|number|true|none|none|
|»»» concurrentSystemJobLimit|number|true|none|none|
|»»» concurrentSystemTaskLimit|number|true|none|none|
|»»» concurrentTaskLimit|number|true|none|none|
|»»» disableTasks|boolean|false|none|none|
|»»» finishedJobArtifactsLimit|number|true|none|none|
|»»» finishedTaskArtifactsLimit|number|true|none|none|
|»»» jobArtifactsReaperPeriod|string|true|none|none|
|»»» jobTimeout|string|true|none|none|
|»»» maxTaskPerc|number|true|none|none|
|»»» schedulingPolicy|string|true|none|none|
|»»» taskHeartbeatPeriod|number|true|none|none|
|»»» taskManifestFlushPeriodMs|number|true|none|none|
|»»» taskManifestMaxBufferSize|number|true|none|none|
|»»» taskManifestReadBufferSize|string|true|none|none|
|»»» taskPollTimeoutMs|number|true|none|none|
|»» limits|[Limits](#schemalimits)|true|none|none|
|»»» cpuProfileTTL|string|true|none|none|
|»»» disableMetricsAccessorCache|boolean|false|none|none|
|»»» edgeMetricsCustomExpression|string|false|none|none|
|»»» edgeMetricsMode|[EdgeHeartbeatMetricsMode](#schemaedgeheartbeatmetricsmode)|false|none|none|
|»»» edgeNodesCount|number|false|none|none|
|»»» enableMetricsPersistence|boolean|true|none|none|
|»»» enableWorkerPersistence|boolean|false|none|none|
|»»» eventsMetadataSources|[string]|false|none|none|
|»»» largeEventsThreshold|string|false|none|none|
|»»» lookupMaxSize|string|false|none|none|
|»»» lookupMaxTotalSize|string|false|none|none|
|»»» maxDimensionValueSize|number|false|none|none|
|»»» maxMetrics|number|false|none|none|
|»»» maxPQSize|string|false|none|none|
|»»» maxReconnectInterval|string|false|none|none|
|»»» metricsDirectory|string|true|none|none|
|»»» metricsDropList|[string]|false|none|none|
|»»» metricsFieldsBlacklist|[string]|true|none|none|
|»»» metricsGCPeriod|string|true|none|none|
|»»» metricsMaxCardinality|number|false|none|none|
|»»» metricsMaxDiskSpace|string|false|none|none|
|»»» metricsNeverDropList|[string]|true|none|none|
|»»» metricsWorkerIdBlacklist|[string]|true|none|none|
|»»» minFreeSpace|string|true|none|none|
|»»» minReconnectInterval|string|false|none|none|
|»»» netFlowTemplateFlushInterval|number|false|none|none|
|»»» randomReconnectInterval|string|false|none|none|
|»»» samples|object|true|none|none|
|»»»» maxSize|string|true|none|none|
|»»» workerMaxMetrics|number|false|none|none|
|»» pii|object|true|none|none|
|»»» enablePiiDetection|boolean|true|none|none|
|»» proxy|object|true|none|none|
|»»» useEnvVars|boolean|true|none|none|
|»» redisCacheLimits|[RedisCacheLimits](#schemarediscachelimits)|true|none|none|
|»»» clientTrackingMechanism|string|false|none|none|
|»»» enableServerAssist|boolean|false|none|none|
|»»» keyTTLSecs|number|false|none|none|
|»»» maxCacheSize|number|false|none|none|
|»»» maxNumKeys|number|false|none|none|
|»»» servicePeriodSecs|number|false|none|none|
|»» redisLimits|[RedisLimits](#schemaredislimits)|true|none|none|
|»»» connections|[RedisConnectionLimits](#schemaredisconnectionlimits)|true|none|none|
|»»»» disabled|boolean|false|none|none|
|»»»» maxConnections|number|false|none|none|
|»» rollback|object|true|none|none|
|»»» rollbackEnabled|boolean|true|none|none|
|»»» rollbackRetries|number|false|none|none|
|»»» rollbackTimeout|number|false|none|none|
|»» searchLimits|[SearchSettings](#schemasearchsettings)|true|none|none|
|»»» compressObjectCacheArtifacts|boolean|true|none|none|
|»»» fieldSummaryMaxFields|number|true|none|none|
|»»» fieldSummaryMaxNestedDepth|number|true|none|none|
|»»» maxConcurrentSearches|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» maxExecutorsPerSearch|number|true|none|none|
|»»» maxResultsPerSearch|number|true|none|none|
|»»» maxSearchDuration|string|true|none|none|
|»»» searchHistoryMaxJobs|number|true|none|none|
|»»» searchHistoryTTL|string|true|none|none|
|»»» searchQueueLength|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» warmPoolSize|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» writeOnlyProviderSecrets|boolean|true|none|none|
|»» servicesLimits|[ServicesLimits](#schemaserviceslimits)|true|none|none|
|»»» connections|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|
|»»»» memoryLimit|string|true|none|none|
|»»»» procs|any|false|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» metrics|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|
|»»» notifications|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|
|»» shutdown|object|true|none|none|
|»»» drainTimeout|number|true|none|none|
|»» sni|object|true|none|none|
|»»» disableSNIRouting|boolean|true|none|none|
|»» sockets|object|false|none|none|
|»»» directory|string|false|none|none|
|»» support|object|false|none|none|
|»»» featureFlagOverrides|[object]|false|none|none|
|»»»» disabled|boolean|true|none|none|
|»»»» flagId|string|true|none|none|
|»» system|object|true|none|none|
|»»» intercom|boolean|true|none|none|
|»»» upgrade|any|true|none|none|
|»» tls|object|true|none|none|
|»»» defaultCipherList|string|true|none|none|
|»»» defaultEcdhCurve|string|true|none|none|
|»»» maxVersion|string|true|none|none|
|»»» minVersion|string|true|none|none|
|»»» rejectUnauthorized|boolean|true|none|none|
|»» upgradeGroupSettings|[UpgradeGroupSettings](#schemaupgradegroupsettings)|true|none|none|
|»»» isRolling|boolean|false|none|none|
|»»» quantity|number|false|none|none|
|»»» retryCount|number|false|none|none|
|»»» retryDelay|number|false|none|none|
|»» upgradeSettings|[UpgradeSettings](#schemaupgradesettings)|true|none|none|
|»»» automaticUpgradeCheckPeriod|string|false|none|none|
|»»» disableAutomaticUpgrade|boolean|true|none|none|
|»»» enableLegacyEdgeUpgrade|boolean|true|none|none|
|»»» packageUrls|[[UpgradePackageUrls](#schemaupgradepackageurls)]|false|none|none|
|»»»» packageHashUrl|string|false|none|none|
|»»»» packageUrl|string|true|none|none|
|»»» upgradeSource|string|true|none|none|
|»» workers|object|true|none|none|
|»»» count|number|true|none|none|
|»»» enableHeapSnapshots|boolean|false|none|none|
|»»» loadThrottlePerc|number|false|none|none|
|»»» memory|number|true|none|none|
|»»» minimum|number|true|none|none|
|»»» startupMaxConns|number|false|none|none|
|»»» startupThrottleTimeout|number|false|none|none|
|»»» v8SingleThread|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ldap|
|type|splunk|
|type|local|
|type|openid|
|type|saas|
|type|saml|
|mode|edge|
|mode|worker|
|mode|single|
|mode|master|
|mode|managed-edge|
|mode|outpost|
|mode|search-supervisor|
|gitOps|none|
|gitOps|pull|
|gitOps|push|
|edgeMetricsMode|minimal|
|edgeMetricsMode|basic|
|edgeMetricsMode|all|
|edgeMetricsMode|custom|
|*anonymous*|auto|
|*anonymous*|auto|
|upgrade|false|
|upgrade|api|

<aside class="success">
This operation does not require authentication
</aside>

## Get public settings visible to any logged user

<a id="opIdgetSystemSettingsCribl"></a>

> Code samples

`GET /system/settings/cribl`

Get public settings visible to any logged user

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "apiProtocol": "string",
      "intercom": true,
      "isRegistered": true
    }
  ]
}
```

<h3 id="get-public-settings-visible-to-any-logged-user-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of PublicSettings objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-public-settings-visible-to-any-logged-user-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[PublicSettings](#schemapublicsettings)]|false|none|none|
|»» apiProtocol|string|true|none|none|
|»» intercom|boolean|true|none|none|
|»» isRegistered|boolean|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

## Get git settings

<a id="opIdgetSystemSettingsGitSettings"></a>

> Code samples

`GET /system/settings/git-settings`

Get git settings

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "authType": "string",
      "autoAction": "string",
      "autoActionMessage": "string",
      "autoActionSchedule": "string",
      "branch": "string",
      "commitDeploySingleAction": true,
      "copilotAutoGitCommitMessages": true,
      "defaultCommitMessage": "string",
      "gitOps": "none",
      "password": "string",
      "remote": "string",
      "sshKey": "string",
      "strictHostKeyChecking": true,
      "timeout": 0,
      "user": "string"
    }
  ]
}
```

<h3 id="get-git-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitSettings objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-git-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitSettings](#schemagitsettings)]|false|none|none|
|»» authType|string|false|none|none|
|»» autoAction|string|false|none|none|
|»» autoActionMessage|string|false|none|none|
|»» autoActionSchedule|string|false|none|none|
|»» branch|string|false|none|none|
|»» commitDeploySingleAction|boolean|false|none|none|
|»» copilotAutoGitCommitMessages|boolean|false|none|none|
|»» defaultCommitMessage|string|false|none|none|
|»» gitOps|[GitOpsType](#schemagitopstype)|false|none|none|
|»» password|string|false|none|none|
|»» remote|string|false|none|none|
|»» sshKey|string|false|none|none|
|»» strictHostKeyChecking|boolean|false|none|none|
|»» timeout|number|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|gitOps|none|
|gitOps|pull|
|gitOps|push|

<aside class="success">
This operation does not require authentication
</aside>

## Update git settings

<a id="opIdupdateSystemSettingsGitSettings"></a>

> Code samples

`PATCH /system/settings/git-settings`

Update git settings

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "authType": "string",
      "autoAction": "string",
      "autoActionMessage": "string",
      "autoActionSchedule": "string",
      "branch": "string",
      "commitDeploySingleAction": true,
      "copilotAutoGitCommitMessages": true,
      "defaultCommitMessage": "string",
      "gitOps": "none",
      "password": "string",
      "remote": "string",
      "sshKey": "string",
      "strictHostKeyChecking": true,
      "timeout": 0,
      "user": "string"
    }
  ]
}
```

<h3 id="update-git-settings-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of GitSettings objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="update-git-settings-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[GitSettings](#schemagitsettings)]|false|none|none|
|»» authType|string|false|none|none|
|»» autoAction|string|false|none|none|
|»» autoActionMessage|string|false|none|none|
|»» autoActionSchedule|string|false|none|none|
|»» branch|string|false|none|none|
|»» commitDeploySingleAction|boolean|false|none|none|
|»» copilotAutoGitCommitMessages|boolean|false|none|none|
|»» defaultCommitMessage|string|false|none|none|
|»» gitOps|[GitOpsType](#schemagitopstype)|false|none|none|
|»» password|string|false|none|none|
|»» remote|string|false|none|none|
|»» sshKey|string|false|none|none|
|»» strictHostKeyChecking|boolean|false|none|none|
|»» timeout|number|false|none|none|
|»» user|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|gitOps|none|
|gitOps|pull|
|gitOps|push|

<aside class="success">
This operation does not require authentication
</aside>

## Get search limits

<a id="opIdgetSystemSettingsSearchLimits"></a>

> Code samples

`GET /system/settings/search-limits`

Get search limits

> Example responses

> 200 Response

```json
{
  "count": 0,
  "items": [
    {
      "compressObjectCacheArtifacts": true,
      "fieldSummaryMaxFields": 0,
      "fieldSummaryMaxNestedDepth": 0,
      "maxConcurrentSearches": 0,
      "maxExecutorsPerSearch": 0,
      "maxResultsPerSearch": 0,
      "maxSearchDuration": "string",
      "searchHistoryMaxJobs": 0,
      "searchHistoryTTL": "string",
      "searchQueueLength": 0,
      "warmPoolSize": 0,
      "writeOnlyProviderSecrets": true
    }
  ]
}
```

<h3 id="get-search-limits-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|a list of SearchSettings objects|Inline|
|401|[Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)|Unauthorized|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Unexpected error|[Error](#schemaerror)|

<h3 id="get-search-limits-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» count|integer|false|none|number of items present in the items array|
|» items|[[SearchSettings](#schemasearchsettings)]|false|none|none|
|»» compressObjectCacheArtifacts|boolean|true|none|none|
|»» fieldSummaryMaxFields|number|true|none|none|
|»» fieldSummaryMaxNestedDepth|number|true|none|none|
|»» maxConcurrentSearches|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» maxExecutorsPerSearch|number|true|none|none|
|»» maxResultsPerSearch|number|true|none|none|
|»» maxSearchDuration|string|true|none|none|
|»» searchHistoryMaxJobs|number|true|none|none|
|»» searchHistoryTTL|string|true|none|none|
|»» searchQueueLength|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» warmPoolSize|any|true|none|none|

*oneOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|number|false|none|none|

*xor*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»»» *anonymous*|string|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» writeOnlyProviderSecrets|boolean|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|auto|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_AuthConfig">AuthConfig</h2>
<!-- backwards compatibility -->
<a id="schemaauthconfig"></a>
<a id="schema_AuthConfig"></a>
<a id="tocSauthconfig"></a>
<a id="tocsauthconfig"></a>

```json
{
  "fallback": true,
  "fallbackBadLogin": true,
  "filter_type": "string",
  "host": "string",
  "port": 0,
  "ssl": true,
  "type": "ldap"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fallback|boolean|true|none|none|
|fallbackBadLogin|boolean|true|none|none|
|filter_type|string|false|none|none|
|host|string|true|none|none|
|port|number|true|none|none|
|ssl|boolean|true|none|none|
|type|string|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|type|ldap|
|type|splunk|
|type|local|
|type|openid|
|type|saas|
|type|saml|

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

<h2 id="tocS_SystemSettingsConf">SystemSettingsConf</h2>
<!-- backwards compatibility -->
<a id="schemasystemsettingsconf"></a>
<a id="schema_SystemSettingsConf"></a>
<a id="tocSsystemsettingsconf"></a>
<a id="tocssystemsettingsconf"></a>

```json
{
  "api": {
    "baseUrl": "string",
    "disableApiCache": true,
    "disabled": true,
    "headers": {},
    "host": "string",
    "idleSessionTTL": 0,
    "listenOnPort": true,
    "loginRateLimit": "string",
    "port": 0,
    "protocol": "string",
    "scripts": true,
    "sensitiveFields": [
      "string"
    ],
    "ssl": {
      "caPath": "string",
      "certPath": "string",
      "disabled": true,
      "passphrase": "string",
      "privKeyPath": "string"
    },
    "ssoRateLimit": "string",
    "workerRemoteAccess": true
  },
  "backups": {
    "backupPersistence": "string",
    "backupsDirectory": "string"
  },
  "customLogo": {
    "enabled": true,
    "logoDescription": "string",
    "logoImage": "string"
  },
  "pii": {
    "enablePiiDetection": true
  },
  "proxy": {
    "useEnvVars": true
  },
  "rollback": {
    "rollbackEnabled": true,
    "rollbackRetries": 0,
    "rollbackTimeout": 0
  },
  "shutdown": {
    "drainTimeout": 0
  },
  "sni": {
    "disableSNIRouting": true
  },
  "sockets": {
    "directory": "string"
  },
  "support": {
    "featureFlagOverrides": [
      {
        "disabled": true,
        "flagId": "string"
      }
    ]
  },
  "system": {
    "intercom": true,
    "upgrade": false
  },
  "tls": {
    "defaultCipherList": "string",
    "defaultEcdhCurve": "string",
    "maxVersion": "string",
    "minVersion": "string",
    "rejectUnauthorized": true
  },
  "upgradeGroupSettings": {
    "isRolling": true,
    "quantity": 0,
    "retryCount": 0,
    "retryDelay": 0
  },
  "upgradeSettings": {
    "automaticUpgradeCheckPeriod": "string",
    "disableAutomaticUpgrade": true,
    "enableLegacyEdgeUpgrade": true,
    "packageUrls": [
      {
        "packageHashUrl": "string",
        "packageUrl": "string"
      }
    ],
    "upgradeSource": "string"
  },
  "workers": {
    "count": 0,
    "enableHeapSnapshots": true,
    "loadThrottlePerc": 0,
    "memory": 0,
    "minimum": 0,
    "startupMaxConns": 0,
    "startupThrottleTimeout": 0,
    "v8SingleThread": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|api|object|true|none|none|
|» baseUrl|string|false|none|none|
|» disableApiCache|boolean|false|none|none|
|» disabled|boolean|true|none|none|
|» headers|object|false|none|none|
|» host|string|true|none|none|
|» idleSessionTTL|number|false|none|none|
|» listenOnPort|boolean|false|none|none|
|» loginRateLimit|string|false|none|none|
|» port|number|true|none|none|
|» protocol|string|true|none|none|
|» scripts|boolean|false|none|none|
|» sensitiveFields|[string]|false|none|none|
|» ssl|object|true|none|none|
|»» caPath|string|false|none|none|
|»» certPath|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» passphrase|string|true|none|none|
|»» privKeyPath|string|true|none|none|
|» ssoRateLimit|string|false|none|none|
|» workerRemoteAccess|boolean|true|none|none|
|backups|object|true|none|none|
|» backupPersistence|string|true|none|none|
|» backupsDirectory|string|true|none|none|
|customLogo|object|true|none|none|
|» enabled|boolean|true|none|none|
|» logoDescription|string|true|none|none|
|» logoImage|string|true|none|none|
|pii|object|true|none|none|
|» enablePiiDetection|boolean|true|none|none|
|proxy|object|true|none|none|
|» useEnvVars|boolean|true|none|none|
|rollback|object|true|none|none|
|» rollbackEnabled|boolean|true|none|none|
|» rollbackRetries|number|false|none|none|
|» rollbackTimeout|number|false|none|none|
|shutdown|object|true|none|none|
|» drainTimeout|number|true|none|none|
|sni|object|true|none|none|
|» disableSNIRouting|boolean|true|none|none|
|sockets|object|false|none|none|
|» directory|string|false|none|none|
|support|object|false|none|none|
|» featureFlagOverrides|[object]|false|none|none|
|»» disabled|boolean|true|none|none|
|»» flagId|string|true|none|none|
|system|object|true|none|none|
|» intercom|boolean|true|none|none|
|» upgrade|any|true|none|none|
|tls|object|true|none|none|
|» defaultCipherList|string|true|none|none|
|» defaultEcdhCurve|string|true|none|none|
|» maxVersion|string|true|none|none|
|» minVersion|string|true|none|none|
|» rejectUnauthorized|boolean|true|none|none|
|upgradeGroupSettings|[UpgradeGroupSettings](#schemaupgradegroupsettings)|true|none|none|
|upgradeSettings|[UpgradeSettings](#schemaupgradesettings)|true|none|none|
|workers|object|true|none|none|
|» count|number|true|none|none|
|» enableHeapSnapshots|boolean|false|none|none|
|» loadThrottlePerc|number|false|none|none|
|» memory|number|true|none|none|
|» minimum|number|true|none|none|
|» startupMaxConns|number|false|none|none|
|» startupThrottleTimeout|number|false|none|none|
|» v8SingleThread|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|upgrade|false|
|upgrade|api|

<h2 id="tocS_SystemSettings">SystemSettings</h2>
<!-- backwards compatibility -->
<a id="schemasystemsettings"></a>
<a id="schema_SystemSettings"></a>
<a id="tocSsystemsettings"></a>
<a id="tocssystemsettings"></a>

```json
{
  "api": {
    "baseUrl": "string",
    "disableApiCache": true,
    "disabled": true,
    "headers": {},
    "host": "string",
    "idleSessionTTL": 0,
    "listenOnPort": true,
    "loginRateLimit": "string",
    "port": 0,
    "protocol": "string",
    "scripts": true,
    "sensitiveFields": [
      "string"
    ],
    "ssl": {
      "caPath": "string",
      "certPath": "string",
      "disabled": true,
      "passphrase": "string",
      "privKeyPath": "string"
    },
    "ssoRateLimit": "string",
    "workerRemoteAccess": true
  },
  "auth": {
    "fallback": true,
    "fallbackBadLogin": true,
    "filter_type": "string",
    "host": "string",
    "port": 0,
    "ssl": true,
    "type": "ldap"
  },
  "backups": {
    "backupPersistence": "string",
    "backupsDirectory": "string"
  },
  "customLogo": {
    "enabled": true,
    "logoDescription": "string",
    "logoImage": "string"
  },
  "distributed": {
    "mode": "edge"
  },
  "fips": true,
  "git": {
    "authType": "string",
    "autoAction": "string",
    "autoActionMessage": "string",
    "autoActionSchedule": "string",
    "branch": "string",
    "commitDeploySingleAction": true,
    "copilotAutoGitCommitMessages": true,
    "defaultCommitMessage": "string",
    "gitOps": "none",
    "password": "string",
    "remote": "string",
    "sshKey": "string",
    "strictHostKeyChecking": true,
    "timeout": 0,
    "user": "string"
  },
  "jobLimits": {
    "concurrentJobLimit": 0,
    "concurrentScheduledJobLimit": 0,
    "concurrentSystemJobLimit": 0,
    "concurrentSystemTaskLimit": 0,
    "concurrentTaskLimit": 0,
    "disableTasks": true,
    "finishedJobArtifactsLimit": 0,
    "finishedTaskArtifactsLimit": 0,
    "jobArtifactsReaperPeriod": "string",
    "jobTimeout": "string",
    "maxTaskPerc": 0,
    "schedulingPolicy": "string",
    "taskHeartbeatPeriod": 0,
    "taskManifestFlushPeriodMs": 0,
    "taskManifestMaxBufferSize": 0,
    "taskManifestReadBufferSize": "string",
    "taskPollTimeoutMs": 0
  },
  "limits": {
    "cpuProfileTTL": "string",
    "disableMetricsAccessorCache": true,
    "edgeMetricsCustomExpression": "string",
    "edgeMetricsMode": "minimal",
    "edgeNodesCount": 0,
    "enableMetricsPersistence": true,
    "enableWorkerPersistence": true,
    "eventsMetadataSources": [
      "string"
    ],
    "largeEventsThreshold": "string",
    "lookupMaxSize": "string",
    "lookupMaxTotalSize": "string",
    "maxDimensionValueSize": 0,
    "maxMetrics": 0,
    "maxPQSize": "string",
    "maxReconnectInterval": "string",
    "metricsDirectory": "string",
    "metricsDropList": [
      "string"
    ],
    "metricsFieldsBlacklist": [
      "string"
    ],
    "metricsGCPeriod": "string",
    "metricsMaxCardinality": 0,
    "metricsMaxDiskSpace": "string",
    "metricsNeverDropList": [
      "string"
    ],
    "metricsWorkerIdBlacklist": [
      "string"
    ],
    "minFreeSpace": "string",
    "minReconnectInterval": "string",
    "netFlowTemplateFlushInterval": 0,
    "randomReconnectInterval": "string",
    "samples": {
      "maxSize": "string"
    },
    "workerMaxMetrics": 0
  },
  "pii": {
    "enablePiiDetection": true
  },
  "proxy": {
    "useEnvVars": true
  },
  "redisCacheLimits": {
    "clientTrackingMechanism": "string",
    "enableServerAssist": true,
    "keyTTLSecs": 0,
    "maxCacheSize": 0,
    "maxNumKeys": 0,
    "servicePeriodSecs": 0
  },
  "redisLimits": {
    "connections": {
      "disabled": true,
      "maxConnections": 0
    }
  },
  "rollback": {
    "rollbackEnabled": true,
    "rollbackRetries": 0,
    "rollbackTimeout": 0
  },
  "searchLimits": {
    "compressObjectCacheArtifacts": true,
    "fieldSummaryMaxFields": 0,
    "fieldSummaryMaxNestedDepth": 0,
    "maxConcurrentSearches": 0,
    "maxExecutorsPerSearch": 0,
    "maxResultsPerSearch": 0,
    "maxSearchDuration": "string",
    "searchHistoryMaxJobs": 0,
    "searchHistoryTTL": "string",
    "searchQueueLength": 0,
    "warmPoolSize": 0,
    "writeOnlyProviderSecrets": true
  },
  "servicesLimits": {
    "connections": {
      "memoryLimit": "string",
      "procs": 0
    },
    "metrics": {
      "memoryLimit": "string",
      "procs": 0
    },
    "notifications": {
      "memoryLimit": "string",
      "procs": 0
    }
  },
  "shutdown": {
    "drainTimeout": 0
  },
  "sni": {
    "disableSNIRouting": true
  },
  "sockets": {
    "directory": "string"
  },
  "support": {
    "featureFlagOverrides": [
      {
        "disabled": true,
        "flagId": "string"
      }
    ]
  },
  "system": {
    "intercom": true,
    "upgrade": false
  },
  "tls": {
    "defaultCipherList": "string",
    "defaultEcdhCurve": "string",
    "maxVersion": "string",
    "minVersion": "string",
    "rejectUnauthorized": true
  },
  "upgradeGroupSettings": {
    "isRolling": true,
    "quantity": 0,
    "retryCount": 0,
    "retryDelay": 0
  },
  "upgradeSettings": {
    "automaticUpgradeCheckPeriod": "string",
    "disableAutomaticUpgrade": true,
    "enableLegacyEdgeUpgrade": true,
    "packageUrls": [
      {
        "packageHashUrl": "string",
        "packageUrl": "string"
      }
    ],
    "upgradeSource": "string"
  },
  "workers": {
    "count": 0,
    "enableHeapSnapshots": true,
    "loadThrottlePerc": 0,
    "memory": 0,
    "minimum": 0,
    "startupMaxConns": 0,
    "startupThrottleTimeout": 0,
    "v8SingleThread": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|api|object|true|none|none|
|» baseUrl|string|false|none|none|
|» disableApiCache|boolean|false|none|none|
|» disabled|boolean|true|none|none|
|» headers|object|false|none|none|
|» host|string|true|none|none|
|» idleSessionTTL|number|false|none|none|
|» listenOnPort|boolean|false|none|none|
|» loginRateLimit|string|false|none|none|
|» port|number|true|none|none|
|» protocol|string|true|none|none|
|» scripts|boolean|false|none|none|
|» sensitiveFields|[string]|false|none|none|
|» ssl|object|true|none|none|
|»» caPath|string|false|none|none|
|»» certPath|string|true|none|none|
|»» disabled|boolean|true|none|none|
|»» passphrase|string|true|none|none|
|»» privKeyPath|string|true|none|none|
|» ssoRateLimit|string|false|none|none|
|» workerRemoteAccess|boolean|true|none|none|
|auth|[AuthConfig](#schemaauthconfig)|true|none|none|
|backups|object|true|none|none|
|» backupPersistence|string|true|none|none|
|» backupsDirectory|string|true|none|none|
|customLogo|object|true|none|none|
|» enabled|boolean|true|none|none|
|» logoDescription|string|true|none|none|
|» logoImage|string|true|none|none|
|distributed|object|true|none|none|
|» mode|string|true|none|none|
|fips|boolean|true|none|none|
|git|[GitSettings](#schemagitsettings)|true|none|none|
|jobLimits|[JobSettings](#schemajobsettings)|true|none|none|
|limits|[Limits](#schemalimits)|true|none|none|
|pii|object|true|none|none|
|» enablePiiDetection|boolean|true|none|none|
|proxy|object|true|none|none|
|» useEnvVars|boolean|true|none|none|
|redisCacheLimits|[RedisCacheLimits](#schemarediscachelimits)|true|none|none|
|redisLimits|[RedisLimits](#schemaredislimits)|true|none|none|
|rollback|object|true|none|none|
|» rollbackEnabled|boolean|true|none|none|
|» rollbackRetries|number|false|none|none|
|» rollbackTimeout|number|false|none|none|
|searchLimits|[SearchSettings](#schemasearchsettings)|true|none|none|
|servicesLimits|[ServicesLimits](#schemaserviceslimits)|true|none|none|
|shutdown|object|true|none|none|
|» drainTimeout|number|true|none|none|
|sni|object|true|none|none|
|» disableSNIRouting|boolean|true|none|none|
|sockets|object|false|none|none|
|» directory|string|false|none|none|
|support|object|false|none|none|
|» featureFlagOverrides|[object]|false|none|none|
|»» disabled|boolean|true|none|none|
|»» flagId|string|true|none|none|
|system|object|true|none|none|
|» intercom|boolean|true|none|none|
|» upgrade|any|true|none|none|
|tls|object|true|none|none|
|» defaultCipherList|string|true|none|none|
|» defaultEcdhCurve|string|true|none|none|
|» maxVersion|string|true|none|none|
|» minVersion|string|true|none|none|
|» rejectUnauthorized|boolean|true|none|none|
|upgradeGroupSettings|[UpgradeGroupSettings](#schemaupgradegroupsettings)|true|none|none|
|upgradeSettings|[UpgradeSettings](#schemaupgradesettings)|true|none|none|
|workers|object|true|none|none|
|» count|number|true|none|none|
|» enableHeapSnapshots|boolean|false|none|none|
|» loadThrottlePerc|number|false|none|none|
|» memory|number|true|none|none|
|» minimum|number|true|none|none|
|» startupMaxConns|number|false|none|none|
|» startupThrottleTimeout|number|false|none|none|
|» v8SingleThread|boolean|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|mode|edge|
|mode|worker|
|mode|single|
|mode|master|
|mode|managed-edge|
|mode|outpost|
|mode|search-supervisor|
|upgrade|false|
|upgrade|api|

<h2 id="tocS_PublicSettings">PublicSettings</h2>
<!-- backwards compatibility -->
<a id="schemapublicsettings"></a>
<a id="schema_PublicSettings"></a>
<a id="tocSpublicsettings"></a>
<a id="tocspublicsettings"></a>

```json
{
  "apiProtocol": "string",
  "intercom": true,
  "isRegistered": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|apiProtocol|string|true|none|none|
|intercom|boolean|true|none|none|
|isRegistered|boolean|true|none|none|

<h2 id="tocS_GitSettings">GitSettings</h2>
<!-- backwards compatibility -->
<a id="schemagitsettings"></a>
<a id="schema_GitSettings"></a>
<a id="tocSgitsettings"></a>
<a id="tocsgitsettings"></a>

```json
{
  "authType": "string",
  "autoAction": "string",
  "autoActionMessage": "string",
  "autoActionSchedule": "string",
  "branch": "string",
  "commitDeploySingleAction": true,
  "copilotAutoGitCommitMessages": true,
  "defaultCommitMessage": "string",
  "gitOps": "none",
  "password": "string",
  "remote": "string",
  "sshKey": "string",
  "strictHostKeyChecking": true,
  "timeout": 0,
  "user": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|authType|string|false|none|none|
|autoAction|string|false|none|none|
|autoActionMessage|string|false|none|none|
|autoActionSchedule|string|false|none|none|
|branch|string|false|none|none|
|commitDeploySingleAction|boolean|false|none|none|
|copilotAutoGitCommitMessages|boolean|false|none|none|
|defaultCommitMessage|string|false|none|none|
|gitOps|[GitOpsType](#schemagitopstype)|false|none|none|
|password|string|false|none|none|
|remote|string|false|none|none|
|sshKey|string|false|none|none|
|strictHostKeyChecking|boolean|false|none|none|
|timeout|number|false|none|none|
|user|string|false|none|none|

<h2 id="tocS_SearchSettings">SearchSettings</h2>
<!-- backwards compatibility -->
<a id="schemasearchsettings"></a>
<a id="schema_SearchSettings"></a>
<a id="tocSsearchsettings"></a>
<a id="tocssearchsettings"></a>

```json
{
  "compressObjectCacheArtifacts": true,
  "fieldSummaryMaxFields": 0,
  "fieldSummaryMaxNestedDepth": 0,
  "maxConcurrentSearches": 0,
  "maxExecutorsPerSearch": 0,
  "maxResultsPerSearch": 0,
  "maxSearchDuration": "string",
  "searchHistoryMaxJobs": 0,
  "searchHistoryTTL": "string",
  "searchQueueLength": 0,
  "warmPoolSize": 0,
  "writeOnlyProviderSecrets": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|compressObjectCacheArtifacts|boolean|true|none|none|
|fieldSummaryMaxFields|number|true|none|none|
|fieldSummaryMaxNestedDepth|number|true|none|none|
|maxConcurrentSearches|[MaxConcurrentSearchesType](#schemamaxconcurrentsearchestype)|true|none|none|
|maxExecutorsPerSearch|number|true|none|none|
|maxResultsPerSearch|number|true|none|none|
|maxSearchDuration|string|true|none|none|
|searchHistoryMaxJobs|number|true|none|none|
|searchHistoryTTL|string|true|none|none|
|searchQueueLength|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|warmPoolSize|any|true|none|none|

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|string|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|writeOnlyProviderSecrets|boolean|true|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|auto|

<h2 id="tocS_UpgradeGroupSettings">UpgradeGroupSettings</h2>
<!-- backwards compatibility -->
<a id="schemaupgradegroupsettings"></a>
<a id="schema_UpgradeGroupSettings"></a>
<a id="tocSupgradegroupsettings"></a>
<a id="tocsupgradegroupsettings"></a>

```json
{
  "isRolling": true,
  "quantity": 0,
  "retryCount": 0,
  "retryDelay": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|isRolling|boolean|false|none|none|
|quantity|number|false|none|none|
|retryCount|number|false|none|none|
|retryDelay|number|false|none|none|

<h2 id="tocS_UpgradeSettings">UpgradeSettings</h2>
<!-- backwards compatibility -->
<a id="schemaupgradesettings"></a>
<a id="schema_UpgradeSettings"></a>
<a id="tocSupgradesettings"></a>
<a id="tocsupgradesettings"></a>

```json
{
  "automaticUpgradeCheckPeriod": "string",
  "disableAutomaticUpgrade": true,
  "enableLegacyEdgeUpgrade": true,
  "packageUrls": [
    {
      "packageHashUrl": "string",
      "packageUrl": "string"
    }
  ],
  "upgradeSource": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|automaticUpgradeCheckPeriod|string|false|none|none|
|disableAutomaticUpgrade|boolean|true|none|none|
|enableLegacyEdgeUpgrade|boolean|true|none|none|
|packageUrls|[[UpgradePackageUrls](#schemaupgradepackageurls)]|false|none|none|
|upgradeSource|string|true|none|none|

<h2 id="tocS_JobSettings">JobSettings</h2>
<!-- backwards compatibility -->
<a id="schemajobsettings"></a>
<a id="schema_JobSettings"></a>
<a id="tocSjobsettings"></a>
<a id="tocsjobsettings"></a>

```json
{
  "concurrentJobLimit": 0,
  "concurrentScheduledJobLimit": 0,
  "concurrentSystemJobLimit": 0,
  "concurrentSystemTaskLimit": 0,
  "concurrentTaskLimit": 0,
  "disableTasks": true,
  "finishedJobArtifactsLimit": 0,
  "finishedTaskArtifactsLimit": 0,
  "jobArtifactsReaperPeriod": "string",
  "jobTimeout": "string",
  "maxTaskPerc": 0,
  "schedulingPolicy": "string",
  "taskHeartbeatPeriod": 0,
  "taskManifestFlushPeriodMs": 0,
  "taskManifestMaxBufferSize": 0,
  "taskManifestReadBufferSize": "string",
  "taskPollTimeoutMs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|concurrentJobLimit|number|true|none|none|
|concurrentScheduledJobLimit|number|true|none|none|
|concurrentSystemJobLimit|number|true|none|none|
|concurrentSystemTaskLimit|number|true|none|none|
|concurrentTaskLimit|number|true|none|none|
|disableTasks|boolean|false|none|none|
|finishedJobArtifactsLimit|number|true|none|none|
|finishedTaskArtifactsLimit|number|true|none|none|
|jobArtifactsReaperPeriod|string|true|none|none|
|jobTimeout|string|true|none|none|
|maxTaskPerc|number|true|none|none|
|schedulingPolicy|string|true|none|none|
|taskHeartbeatPeriod|number|true|none|none|
|taskManifestFlushPeriodMs|number|true|none|none|
|taskManifestMaxBufferSize|number|true|none|none|
|taskManifestReadBufferSize|string|true|none|none|
|taskPollTimeoutMs|number|true|none|none|

<h2 id="tocS_Limits">Limits</h2>
<!-- backwards compatibility -->
<a id="schemalimits"></a>
<a id="schema_Limits"></a>
<a id="tocSlimits"></a>
<a id="tocslimits"></a>

```json
{
  "cpuProfileTTL": "string",
  "disableMetricsAccessorCache": true,
  "edgeMetricsCustomExpression": "string",
  "edgeMetricsMode": "minimal",
  "edgeNodesCount": 0,
  "enableMetricsPersistence": true,
  "enableWorkerPersistence": true,
  "eventsMetadataSources": [
    "string"
  ],
  "largeEventsThreshold": "string",
  "lookupMaxSize": "string",
  "lookupMaxTotalSize": "string",
  "maxDimensionValueSize": 0,
  "maxMetrics": 0,
  "maxPQSize": "string",
  "maxReconnectInterval": "string",
  "metricsDirectory": "string",
  "metricsDropList": [
    "string"
  ],
  "metricsFieldsBlacklist": [
    "string"
  ],
  "metricsGCPeriod": "string",
  "metricsMaxCardinality": 0,
  "metricsMaxDiskSpace": "string",
  "metricsNeverDropList": [
    "string"
  ],
  "metricsWorkerIdBlacklist": [
    "string"
  ],
  "minFreeSpace": "string",
  "minReconnectInterval": "string",
  "netFlowTemplateFlushInterval": 0,
  "randomReconnectInterval": "string",
  "samples": {
    "maxSize": "string"
  },
  "workerMaxMetrics": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|cpuProfileTTL|string|true|none|none|
|disableMetricsAccessorCache|boolean|false|none|none|
|edgeMetricsCustomExpression|string|false|none|none|
|edgeMetricsMode|[EdgeHeartbeatMetricsMode](#schemaedgeheartbeatmetricsmode)|false|none|none|
|edgeNodesCount|number|false|none|none|
|enableMetricsPersistence|boolean|true|none|none|
|enableWorkerPersistence|boolean|false|none|none|
|eventsMetadataSources|[string]|false|none|none|
|largeEventsThreshold|string|false|none|none|
|lookupMaxSize|string|false|none|none|
|lookupMaxTotalSize|string|false|none|none|
|maxDimensionValueSize|number|false|none|none|
|maxMetrics|number|false|none|none|
|maxPQSize|string|false|none|none|
|maxReconnectInterval|string|false|none|none|
|metricsDirectory|string|true|none|none|
|metricsDropList|[string]|false|none|none|
|metricsFieldsBlacklist|[string]|true|none|none|
|metricsGCPeriod|string|true|none|none|
|metricsMaxCardinality|number|false|none|none|
|metricsMaxDiskSpace|string|false|none|none|
|metricsNeverDropList|[string]|true|none|none|
|metricsWorkerIdBlacklist|[string]|true|none|none|
|minFreeSpace|string|true|none|none|
|minReconnectInterval|string|false|none|none|
|netFlowTemplateFlushInterval|number|false|none|none|
|randomReconnectInterval|string|false|none|none|
|samples|object|true|none|none|
|» maxSize|string|true|none|none|
|workerMaxMetrics|number|false|none|none|

<h2 id="tocS_RedisCacheLimits">RedisCacheLimits</h2>
<!-- backwards compatibility -->
<a id="schemarediscachelimits"></a>
<a id="schema_RedisCacheLimits"></a>
<a id="tocSrediscachelimits"></a>
<a id="tocsrediscachelimits"></a>

```json
{
  "clientTrackingMechanism": "string",
  "enableServerAssist": true,
  "keyTTLSecs": 0,
  "maxCacheSize": 0,
  "maxNumKeys": 0,
  "servicePeriodSecs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|clientTrackingMechanism|string|false|none|none|
|enableServerAssist|boolean|false|none|none|
|keyTTLSecs|number|false|none|none|
|maxCacheSize|number|false|none|none|
|maxNumKeys|number|false|none|none|
|servicePeriodSecs|number|false|none|none|

<h2 id="tocS_RedisLimits">RedisLimits</h2>
<!-- backwards compatibility -->
<a id="schemaredislimits"></a>
<a id="schema_RedisLimits"></a>
<a id="tocSredislimits"></a>
<a id="tocsredislimits"></a>

```json
{
  "connections": {
    "disabled": true,
    "maxConnections": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|connections|[RedisConnectionLimits](#schemaredisconnectionlimits)|true|none|none|

<h2 id="tocS_ServicesLimits">ServicesLimits</h2>
<!-- backwards compatibility -->
<a id="schemaserviceslimits"></a>
<a id="schema_ServicesLimits"></a>
<a id="tocSserviceslimits"></a>
<a id="tocsserviceslimits"></a>

```json
{
  "connections": {
    "memoryLimit": "string",
    "procs": 0
  },
  "metrics": {
    "memoryLimit": "string",
    "procs": 0
  },
  "notifications": {
    "memoryLimit": "string",
    "procs": 0
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|connections|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|
|metrics|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|
|notifications|[CommonServiceLimitConfigs](#schemacommonservicelimitconfigs)|true|none|none|

<h2 id="tocS_GitOpsType">GitOpsType</h2>
<!-- backwards compatibility -->
<a id="schemagitopstype"></a>
<a id="schema_GitOpsType"></a>
<a id="tocSgitopstype"></a>
<a id="tocsgitopstype"></a>

```json
"none"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|none|
|*anonymous*|pull|
|*anonymous*|push|

<h2 id="tocS_MaxConcurrentSearchesType">MaxConcurrentSearchesType</h2>
<!-- backwards compatibility -->
<a id="schemamaxconcurrentsearchestype"></a>
<a id="schema_MaxConcurrentSearchesType"></a>
<a id="tocSmaxconcurrentsearchestype"></a>
<a id="tocsmaxconcurrentsearchestype"></a>

```json
0

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

<h2 id="tocS_UpgradePackageUrls">UpgradePackageUrls</h2>
<!-- backwards compatibility -->
<a id="schemaupgradepackageurls"></a>
<a id="schema_UpgradePackageUrls"></a>
<a id="tocSupgradepackageurls"></a>
<a id="tocsupgradepackageurls"></a>

```json
{
  "packageHashUrl": "string",
  "packageUrl": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|packageHashUrl|string|false|none|none|
|packageUrl|string|true|none|none|

<h2 id="tocS_EdgeHeartbeatMetricsMode">EdgeHeartbeatMetricsMode</h2>
<!-- backwards compatibility -->
<a id="schemaedgeheartbeatmetricsmode"></a>
<a id="schema_EdgeHeartbeatMetricsMode"></a>
<a id="tocSedgeheartbeatmetricsmode"></a>
<a id="tocsedgeheartbeatmetricsmode"></a>

```json
"minimal"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|minimal|
|*anonymous*|basic|
|*anonymous*|all|
|*anonymous*|custom|

<h2 id="tocS_RedisConnectionLimits">RedisConnectionLimits</h2>
<!-- backwards compatibility -->
<a id="schemaredisconnectionlimits"></a>
<a id="schema_RedisConnectionLimits"></a>
<a id="tocSredisconnectionlimits"></a>
<a id="tocsredisconnectionlimits"></a>

```json
{
  "disabled": true,
  "maxConnections": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|disabled|boolean|false|none|none|
|maxConnections|number|false|none|none|

<h2 id="tocS_CommonServiceLimitConfigs">CommonServiceLimitConfigs</h2>
<!-- backwards compatibility -->
<a id="schemacommonservicelimitconfigs"></a>
<a id="schema_CommonServiceLimitConfigs"></a>
<a id="tocScommonservicelimitconfigs"></a>
<a id="tocscommonservicelimitconfigs"></a>

```json
{
  "memoryLimit": "string",
  "procs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|memoryLimit|string|true|none|none|
|procs|[ServiceProcsType](#schemaserviceprocstype)|false|none|none|

<h2 id="tocS_ServiceProcsType">ServiceProcsType</h2>
<!-- backwards compatibility -->
<a id="schemaserviceprocstype"></a>
<a id="schema_ServiceProcsType"></a>
<a id="tocSserviceprocstype"></a>
<a id="tocsserviceprocstype"></a>

```json
0

```

### Properties

oneOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|number|false|none|none|

xor

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|auto|

