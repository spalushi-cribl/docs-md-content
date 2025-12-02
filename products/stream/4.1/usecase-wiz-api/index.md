# Wiz API Collection


This topic covers how to configure Cribl Stream [REST Collectors](collectors-rest) and [Event Breakers](event-breakers) to gather data from the [Wiz](https://www.wiz.io/) cloud security platform. To do this, the REST Collectors will communicate with APIs that your organization's Wiz portal exposes. You'll create and configure four REST Collectors, one for each of these Wiz APIs: Audit Logs, Configuration Findings (sometimes called Cloud Configuration), Issues, and Vulnerabilities.

Throughout this topic, instead of configuring setting-by-setting, you'll open a JSON editor and paste in configurations provided in the Appendices. When creating REST Collectors, you'll replace a few values in the configuration with values from your Wiz portal. This **Manage as JSON** approach is faster and less error-prone than clicking your way through the relevant UIs, especially since the configurations required to interact with the Wiz APIs are quite complicated.

## Prerequisites From Wiz {#pre-reqs}

This topic assumes that when you became a Wiz customer, Wiz sent the following items:

Item | Value
--- | ---
**Client ID** | 53-character string
**Client secret** | 64-character string
**Auth URL** | `https://auth.app.wiz.io/oauth/token` or similar
**API endpoint** | `https://api.us17.app.wiz.io/graphql` or similar

These items should be the same across all Wiz APIs. The Wiz platform differentiates requests to one API from requests to another by inspecting the request body. (The configurations supplied later in this topic include an appropriate request body for each API.)

Note the values that Wiz sent you, and keep them handy for when you [configure the REST Collectors](#config-collectors) later on.

## Configuring the Event Breaker

Although you will create four REST Collectors, you only need to add the single Event Breaker provided [below](#event-breaker), whose ruleset contains one rule per API. All four REST Collectors will use that Event Breaker. We'll configure each REST Collector to use the rule that corresponds with the appropriate API.

The Event Breaker strips headers from events, leaving only the records of interest; and, it captures timestamps in the proper way.

1. Navigate to **Manage** > **Processing** > **Knowledge** > **Event Breaker Rules**.  
1. Click **Add Ruleset**. 
1. Click **Manage as JSON** at lower left.
1. Copy the Event Breaker from the Appendix [below](#event-breaker).
1. Paste the Event Breaker into the editor (replacing the default object that the editor opens with).
1. Click **Save**.

![The Event Breaker UI before you add the ruleset](wiz-api-01.629b432d96.png)

When you have finished adding the Event Breaker, you should see that it contains four rules:

![The complete Event Breaker ruleset](wiz-api-02.333df859d2.png)

## Configuring the REST Collectors {#config-collectors}

> You'll need to complete the procedure in this section four times. Each time, you'll create one REST Collector to handle a particular Wiz API.
> 
> To learn more about REST Collectors, see our [REST/API Endpoint](collectors-rest) and 
> [Scheduling and Running](collectors-schedule-run) topics.
>
{.box .info}

From the top nav of a Cribl Stream instance or Group, select **Data** > **Sources**, then select **Collectors** > **REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST** > **New Collector** modal.

1. Click **Manage as JSON** to open the configuration editor.
1. Copy the config for the desired API, from the Appendix below: [Audit Logs](#config-audit-logs), [Configuration Findings](#config-configuration-findings), [Issues](#config-issues), or [Vulnerabilities](#config-vulnerabilities).
1. Paste the config into the editor (replacing the default object that the editor opens with).
1. Locate the `collector` element, and within that, the `conf` element.
1. In the `conf` element, replace the values of the following keys with the values you noted [earlier](#pre-reqs). To make this easier, the values to be replaced all have the same placeholder value of `<insert_value_here>`.
    * Replace the value of `loginUrl` with the value of **Auth URL**. Enclose this value in single quotation marks within the double-quotes required by JSON, for example: `"'https://auth.app.wiz.io/oauth/token'"`.
    * Replace the value of `clientSecretParamValue` with the value of **Client secret**.
    * In the `authRequestParams` array, find the object where `name` has the value `client_id`, and replace the value of `value` with the value of **Client ID**.
    * Replace the value of `collectUrl` with the value of **API endpoint**. Enclose this value in single quotation marks within the double-quotes required by JSON, for example: `"'https://api.us17.app.wiz.io/graphql'"`.
1. Click **OK**.

 Once you've created all four REST Collectors, you'll be ready to start collecting from the Wiz APIs.

## Discovering and Collecting

You can perform the following procedures with any of the four REST Collectors you've created.

We'll start with the Discovery run: 

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector. 
1. In the resulting **Run Collector** modal, select **Mode** > **Discovery**. 
1. Configure a **Time range**, if desired.
1. Click **Run** to retrieve Discovery results.

After inspecting these results, launch the Collector run:

1. Back on the **Manage REST Collectors** page, again click **Run** beside your new Collector.
1. In the resulting **Run Collector** modal, this time select **Mode** > **Full Run**. 
1. Configure a **Time range**, if desired.
1. Click **Run**.



## Beware of Wiz API Results Limits {#limits}

To avoid degrading the performance of your Wiz environment, follow the principles described in this section.

* Do not exceed three API requests per second. Cribl Stream is generally not used in a manner that would approach this limit but if you do, the Wiz API will likely respond with `HTTP 429 Too Many Request` errors.
* Use filters in the query string to ensure that your queries pull only the relevant information.
* If you choose to capture historical data during your initial integration, keep each API's results limits in mind when running ad-hoc API collections.
* For ongoing collection, use incremental collections. To do this, schedule the REST collector to run over a timeframe that suits the task. For example, the Wiz Vulnerabilities Report API is for incremental use only and is best scheduled to run daily, pulling results only from the previous day.

The table below shows the results limits for each Wiz API. 

API | Limit (results)
--- | ------
Audit Logs | 10,000
Cloud Configuration | 10,000
Issues | no limit
Vulnerability | no limit

> Once your query hits the limit of results for a given Wiz API, the API will send no further results. 
>
{.box .warning}

## Troubleshooting {#troubleshooting}

The REST Collector treats all non-`200` responses from configured URL endpoints as errors. On Discover, Preview, and most Collect jobs, it interprets these as fatal errors. On Collect jobs, a few exceptions are treated as non-fatal:
* Where a Collect job launches multiple tasks, and only a subset of those tasks fail, Cribl Stream places the job in failed status, but treats the error as non-fatal. (Note that Cribl Stream does not retry the failed tasks.)
* Where a Collect job receives a 3xx redirection error code, it follows the error's treatment by the underlying library, and does not necessarily treat the error as fatal.

Detailed logging is available for each Collector run to help troubleshoot issues or explore the results of each collector run.  To view the jobs, click Monitoring->System->Job Inspector as shown below.

![The Job Inspector](wiz-api-03.6df344eeb5.png)


## Appendix: Event Breaker {#event-breaker}

```json {title="Event Breaker for Wiz APIs"}
{
  "lib": "custom",
  "minRawLength": 256,
  "id": "wiz-api",
  "rules": [
    {
      "condition": "_raw.startsWith('{\"data\":{\"configurationFindings\":{\"nodes\":')",
      "type": "json_array",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "disabled": false,
      "jsonExtractAll": true,
      "eventBreakerRegex": "/[\\n\\r]+(?!\\s)/",
      "jsonArrayField": "data.configurationFindings.nodes",
      "name": "configurationFindings",
      "jsonTimeField": "createdAt"
    },
    {
      "condition": "_raw.startsWith('{\"data\":{\"issues\":{\"nodes\":')",
      "type": "json_array",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "disabled": false,
      "jsonExtractAll": true,
      "eventBreakerRegex": "/[\\n\\r]+(?!\\s)/",
      "jsonArrayField": "data.issues.nodes",
      "jsonTimeField": "createdAt",
      "name": "issues"
    },
    {
      "condition": "_raw.startsWith('{\"data\":{\"vulnerabilityFindings\":{\"nodes\":')",
      "type": "json_array",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "disabled": false,
      "jsonExtractAll": true,
      "eventBreakerRegex": "/[\\n\\r]+(?!\\s)/",
      "jsonArrayField": "data.vulnerabilityFindings.nodes",
      "jsonTimeField": "createdAt",
      "name": "vulnerabilities"
    },
    {
      "condition": "_raw.startsWith('{\"data\":{\"auditLogEntries\":{\"nodes\":')",
      "type": "json_array",
      "timestampAnchorRegex": "/^/",
      "timestamp": {
        "type": "auto",
        "length": 150
      },
      "timestampTimezone": "local",
      "timestampEarliest": "-420weeks",
      "timestampLatest": "+1week",
      "maxEventBytes": 51200,
      "disabled": false,
      "jsonExtractAll": true,
      "eventBreakerRegex": "/[\\n\\r]+(?!\\s)/",
      "jsonArrayField": "data.auditLogEntries.nodes",
      "jsonTimeField": "timestamp",
      "name": "auditLogEntries"
    }
  ],
  "description": "Collection of rules for the purpose of event breaking data retrieved from the Wiz API"
}
```

## Appendix: REST Collector Configs

Each JSON object below is a configuration that you can copy and paste into the JSON editor when configuring a REST Collector. There's one for each Wiz API covered in this topic.

#### Config for Wiz Audit Logs API {#config-audit-logs}

```json {title="Config for Wiz Audit Logs API"}

{
  "type": "collection",
  "ttl": "4h",
  "removeFields": [],
  "resumeOnBoot": false,
  "schedule": {},
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "conf": {
      "discovery": {
        "discoverType": "none"
      },
      "collectMethod": "post_with_body",
      "pagination": {
        "type": "response_body",
        "maxPages": 20,
        "attribute": [
          "endCursor"
        ]
      },
      "authentication": "oauth",
      "timeout": 0,
      "useRoundRobinDns": false,
      "disableTimeFilter": false,
      "safeHeaders": [],
      "loginUrl": "<insert_value_here>",
      "tokenRespAttribute": "access_token",
      "authHeaderKey": "Authorization",
      "authHeaderExpr": "`Bearer ${token}`",
      "clientSecretParamName": "client_secret",
      "authRequestParams": [
        {
          "name": "client_id",
          "value": "<insert_value_here>"
        },
        {
          "name": "audience",
          "value": "wiz-api"
        },
        {
          "name": "grant_type",
          "value": "client_credentials"
        }
      ],
      "clientSecretParamValue": "<insert_value_here>",
      "collectUrl": "<insert_value_here>",
      "collectRequestParams": [
        {
          "name": "body",
          "value": "'{\"query\":\"query CloudConfigurationFindingsPage($filterBy: ConfigurationFindingFilters $first: Int $after: String $orderBy: ConfigurationFindingOrder ) {     configurationFindings(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) {       nodes {         id         firstSeenAt         severity         result         remediation         resource {         id         name         type         subscription {           id           name           externalId           cloudProvider         }         projects {           id           name           riskProfile {             businessImpact           }         }       }       rule {         id         graphId         name         description         remediationInstructions       }     }     pageInfo {       hasNextPage       endCursor     }   } }\",\"variables\":{\"first\":5}}'"
        }
      ],
      "collectRequestHeaders": [
        {
          "value": "'application/json'",
          "name": " accept"
        },
        {
          "value": "'application/json'",
          "name": "content-type"
        }
      ],
      "collectBody": "'{\"query\": \"query AuditLogTable($first: Int $after: String $filterBy: AuditLogEntryFilters){ auditLogEntries(first: $first after: $after filterBy: $filterBy) { nodes { id action requestId status timestamp actionParameters userAgent sourceIP serviceAccount { id name } user { id name } } pageInfo { hasNextPage endCursor } } }\",\"variables\": {\"first\": 50}}'"
    },
    "destructive": false,
    "type": "rest"
  },
  "input": {
    "type": "collection",
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true
    },
    "throttleRatePerSec": "0",
    "metadata": [],
    "breakerRulesets": [
      "wiz-api"
    ]
  },
  "id": "wiz-auditLogEntries"
}
```

#### Config for Wiz Configuration Findings API {#config-configuration-findings}

```json {title="Config for Wiz Configuration Findings API"}

{
  "type": "collection",
  "ttl": "4h",
  "removeFields": [],
  "resumeOnBoot": false,
  "schedule": {},
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "conf": {
      "discovery": {
        "discoverType": "none"
      },
      "collectMethod": "post_with_body",
      "pagination": {
        "type": "response_body",
        "maxPages": 10,
        "attribute": [
          "endCursor"
        ]
      },
      "authentication": "oauth",
      "timeout": 0,
      "useRoundRobinDns": false,
      "disableTimeFilter": false,
      "safeHeaders": [],
      "loginUrl": "<insert_value_here>",
      "tokenRespAttribute": "access_token",
      "authHeaderKey": "Authorization",
      "authHeaderExpr": "`Bearer ${token}`",
      "clientSecretParamName": "client_secret",
      "authRequestParams": [
        {
          "name": "client_id",
          "value": "<insert_value_here>"
        },
        {
          "name": "audience",
          "value": "wiz-api"
        },
        {
          "name": "grant_type",
          "value": "client_credentials"
        }
      ],
      "clientSecretParamValue": "<insert_value_here> ",
      "collectUrl": "<insert_value_here>",
      "collectRequestParams": [
        {
          "name": "body",
          "value": "'{\"query\":\"query CloudConfigurationFindingsPage($filterBy: ConfigurationFindingFilters $first: Int $after: String $orderBy: ConfigurationFindingOrder ) {     configurationFindings(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) {       nodes {         id         firstSeenAt         severity         result         remediation         resource {         id         name         type         subscription {           id           name           externalId           cloudProvider         }         projects {           id           name           riskProfile {             businessImpact           }         }       }       rule {         id         graphId         name         description         remediationInstructions       }     }     pageInfo {       hasNextPage       endCursor     }   } }\",\"variables\":{\"first\":5}}'"
        }
      ],
      "collectRequestHeaders": [
        {
          "value": "'application/json'",
          "name": " accept"
        },
        {
          "value": "'application/json'",
          "name": "content-type"
        }
      ],
      "collectBody": "'{\"query\": \"query CloudConfigurationFindingsPage($filterBy: ConfigurationFindingFilters $first: Int $after: String $orderBy: ConfigurationFindingOrder ) { configurationFindings(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) { nodes { id firstSeenAt severity result remediation resource { id name type subscription { id name externalId cloudProvider } projects { id name riskProfile { businessImpact } } } rule { id graphId name description remediationInstructions } } pageInfo { hasNextPage endCursor } } }\",\"variables\": {\"first\": 5}}'"
    },
    "destructive": false,
    "type": "rest"
  },
  "input": {
    "type": "collection",
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true
    },
    "throttleRatePerSec": "0",
    "metadata": [],
    "breakerRulesets": [
      "wiz-api"
    ]
  },
  "id": "wiz-configurationFindings"
}
```

#### Config for Wiz Issues API {#config-issues}

```json {title="Config for Wiz Issues API"}

{
  "type": "collection",
  "ttl": "4h",
  "removeFields": [],
  "resumeOnBoot": false,
  "schedule": {},
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "conf": {
      "discovery": {
        "discoverType": "none"
      },
      "collectMethod": "post_with_body",
      "pagination": {
        "type": "response_body",
        "maxPages": 10,
        "attribute": [
          "endCursor"
        ]
      },
      "authentication": "oauth",
      "timeout": 0,
      "useRoundRobinDns": false,
      "disableTimeFilter": false,
      "safeHeaders": [],
      "loginUrl": "<insert_value_here>",
      "tokenRespAttribute": "access_token",
      "authHeaderKey": "Authorization",
      "authHeaderExpr": "`Bearer ${token}`",
      "clientSecretParamName": "client_secret",
      "authRequestParams": [
        {
          "name": "client_id",
          "value": "<insert_value_here>"
        },
        {
          "name": "audience",
          "value": "wiz-api"
        },
        {
          "name": "grant_type",
          "value": "client_credentials"
        }
      ],
      "clientSecretParamValue": "<insert_value_here>",
      "collectUrl": "<insert_value_here>",
      "collectRequestParams": [
        {
          "name": "body",
          "value": "'{\"query\":\"query CloudConfigurationFindingsPage($filterBy: ConfigurationFindingFilters $first: Int $after: String $orderBy: ConfigurationFindingOrder ) {     configurationFindings(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) {       nodes {         id         firstSeenAt         severity         result         remediation         resource {         id         name         type         subscription {           id           name           externalId           cloudProvider         }         projects {           id           name           riskProfile {             businessImpact           }         }       }       rule {         id         graphId         name         description         remediationInstructions       }     }     pageInfo {       hasNextPage       endCursor     }   } }\",\"variables\":{\"first\":5}}'"
        }
      ],
      "collectRequestHeaders": [
        {
          "value": "'application/json'",
          "name": " accept"
        },
        {
          "value": "'application/json'",
          "name": "content-type"
        }
      ],
      "collectBody": "'{\"query\": \"query IssuesTable($filterBy: IssueFilters $first: Int $after: String $orderBy: IssueOrder) { issues(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) { nodes { id control { id name description resolutionRecommendation } createdAt updatedAt dueAt project { id name slug businessUnit riskProfile { businessImpact } } status severity entitySnapshot { id type nativeType name status cloudPlatform cloudProviderURL providerId region resourceGroupExternalId subscriptionExternalId subscriptionName subscriptionTags tags externalId } note serviceTickets { externalId name url } } pageInfo { hasNextPage endCursor } } }\", \"variables\": {\"first\": 5}}'"
    },
    "destructive": false,
    "type": "rest"
  },
  "input": {
    "type": "collection",
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true
    },
    "throttleRatePerSec": "0",
    "metadata": [],
    "breakerRulesets": [
      "wiz-api"
    ]
  },
  "id": "wiz-issues"
}
```

#### Config for Wiz Vulnerabilities API {#config-vulnerabilities}

```json {title="Config for Wiz Vulnerabilities API"}

{
  "type": "collection",
  "ttl": "4h",
  "removeFields": [],
  "resumeOnBoot": false,
  "schedule": {},
  "streamtags": [],
  "workerAffinity": false,
  "collector": {
    "conf": {
      "discovery": {
        "discoverType": "none"
      },
      "collectMethod": "post_with_body",
      "pagination": {
        "type": "response_body",
        "maxPages": 10,
        "attribute": [
          "endCursor"
        ]
      },
      "authentication": "oauth",
      "timeout": 0,
      "useRoundRobinDns": false,
      "disableTimeFilter": false,
      "safeHeaders": [],
      "loginUrl": "<insert_value_here>",
      "tokenRespAttribute": "access_token",
      "authHeaderKey": "Authorization",
      "authHeaderExpr": "`Bearer ${token}`",
      "clientSecretParamName": "client_secret",
      "authRequestParams": [
        {
          "name": "client_id",
          "value": "<insert_value_here>"
        },
        {
          "name": "audience",
          "value": "wiz-api"
        },
        {
          "name": "grant_type",
          "value": "client_credentials"
        }
      ],
      "clientSecretParamValue": "<insert_value_here>",
      "collectUrl": "<insert_value_here>",
      "collectRequestParams": [
        {
          "name": "body",
          "value": "'{\"query\":\"query CloudConfigurationFindingsPage($filterBy: ConfigurationFindingFilters $first: Int $after: String $orderBy: ConfigurationFindingOrder ) {     configurationFindings(filterBy: $filterBy first: $first after: $after orderBy: $orderBy) {       nodes {         id         firstSeenAt         severity         result         remediation         resource {         id         name         type         subscription {           id           name           externalId           cloudProvider         }         projects {           id           name           riskProfile {             businessImpact           }         }       }       rule {         id         graphId         name         description         remediationInstructions       }     }     pageInfo {       hasNextPage       endCursor     }   } }\",\"variables\":{\"first\":5}}'"
        }
      ],
      "collectRequestHeaders": [
        {
          "value": "'application/json'",
          "name": " accept"
        },
        {
          "value": "'application/json'",
          "name": "content-type"
        }
      ],
      "collectBody": "'{\"query\": \"query VulnerabilityFindingsPage($filterBy: VulnerabilityFindingFilters, $first: Int, $after: String, $orderBy: VulnerabilityFindingOrder) { vulnerabilityFindings( filterBy: $filterBy first: $first after: $after orderBy: $orderBy ) { nodes { id portalUrl name CVEDescription CVSSSeverity score exploitabilityScore impactScore hasExploit hasCisaKevExploit status vendorSeverity firstDetectedAt lastDetectedAt resolvedAt description remediation detailedName version fixedVersion detectionMethod link locationPath resolutionReason vulnerableAsset { ... on VulnerableAssetBase { id type name region providerUniqueId cloudProviderURL cloudPlatform status subscriptionName subscriptionExternalId subscriptionId tags } ... on VulnerableAssetVirtualMachine { operatingSystem ipAddresses } ... on VulnerableAssetServerless { runtime } ... on VulnerableAssetContainerImage { imageId } ... on VulnerableAssetContainer { ImageExternalId VmExternalId ServerlessContainer PodNamespace PodName NodeName } } } pageInfo { hasNextPage endCursor }}}\",\"variables\": {\"first\": 5}}'"
    },
    "destructive": false,
    "type": "rest"
  },
  "input": {
    "type": "collection",
    "staleChannelFlushMs": 10000,
    "sendToRoutes": true,
    "preprocess": {
      "disabled": true
    },
    "throttleRatePerSec": "0",
    "metadata": [],
    "breakerRulesets": [
      "wiz-api"
    ]
  },
  "id": "wiz-vulnerabilities"
}
```
