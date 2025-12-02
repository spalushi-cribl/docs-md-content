# Microsoft Sentinel SIEM Integration {#preamble}


You can send events through Cribl Stream [Microsoft Sentinel Destinations](destinations-sentinel) to the [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) SIEM in Azure.

This use case demonstrates how to send data from Cribl Stream to Microsoft Sentinel in various Azure environments, including Azure Public Cloud, Azure US Government Cloud, and Microsoft Azure operated by 21Vianet. While we'll cover key points about the Cribl Stream Microsoft Sentinel Destination here, you should refer to the [Sentinel Destination topic](destinations-sentinel) for complete UI details.

## Before You Begin

Ensure that you have [prepared your Azure Workspace](usecase-azure-workspace). You must complete this step before you can send events through the Microsoft Sentinel Destination to the Microsoft Sentinel SIEM.

If you do not yet have Microsoft Sentinel up and running, please join us on Cribl's Community Slack at [https://cribl‑community.slack.com/](https://cribl-community.slack.com/) and let us know; very likely, a wise goat will offer time-saving suggestions.

### Event Types {#event-types}

Of the [many event types](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables-category) (also called "tables") present in the Microsoft ecosystem, this topic provides what you need to get started quickly with any or all of the following four:

* [CommonSecurityLog](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/commonsecuritylog)
* [SecurityEvent](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityevent)
* [Syslog](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/syslog)
* [WindowsEvent](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/windowsevent)

We'll also explain how you can bring additional event types into your deployment.

To plan your Cribl/Sentinel integration, start by deciding which event types (a.k.a. tables) you want to send to Sentinel.

If the answer is "more than one," you should use an [Output Router](destinations-output-router) and multiple Destinations. You'll configure one Destination per table, but a single URL will ingest all the data. The Output Router can then route a given event type to the correct Destination.

The four event types above belong to the much larger set of event types that the Azure Monitor provides in its [Logs Ingestion API](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview#supported-tables). You can work with any of these event types as you would the four types above. See the [Appendix](#all-event-types) below for the full list.

## Creating Data Collection Rules

The Sentinel SIEM can have multiple ingestion endpoints, called Data Collection Endpoints (DCEs). In turn, each DCE can have multiple Data Collection Rules (DCRs). A DCR can dictate the structure of the data flowing into one table, or multiple tables.

> Ensure that the field names in your data match the schema defined in the Data Collection Rule (DCR) for Microsoft Sentinel. The DCR specifies the expected structure, including field names and types. Mismatched field names can result in dropped events.
{.box .warning}

Cribl's [template](usecase-webhook-azure-sentinel-dcr-template) is an example of a custom DCR that sends data to four different tables, one for each of the four [event types](#event-types) enumerated above.

Alternatively, you could populate one table with different kinds of data. For example, you could define a table for the [Syslog](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/syslog) event type, and then create a **separate** DCR for Windows, Linux, and Mac system logs, respectively, all of which would populate that single table.

Besides the [Azure documentation](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection-rule-overview), you can study the Cribl template to understand how DCRs are structured. For example, each table's columns are defined in a sub-element of the `resources` > `properties` > `streamDeclarations`  element. The `dataFlows` element – at the same level as `streamDeclarations` – specifies the table that a given type of data should populate, via the `outputStream` sub-element. It also specifies the query that will translate incoming fields into values in the table's columns, via the `transformKql` sub-element.

Read more about DCRs in the [Microsoft Sentinel documentation](https://learn.microsoft.com/en-us/azure/sentinel/data-transformation#data-transformation-support-for-custom-data-connectors).

> ##### Custom DCR Template Errors
>
> If you get an error such as `Table for output stream 'Microsoft-CommonSecurityLog' is not available for destination 'logAnalyticsWorkspace')`, it is probably because you don't have Sentinel assigned to your Log Analytics workspace. Please add Microsoft Sentinel to your Log Analytics Workspace to correct this error.
>
{.box .info}

## Obtaining Your URL {#obtaining-url}

To configure your Cribl Stream Microsoft Sentinel Destination, you must know the ingestion URL to send data to. This is in **General Settings** > **Endpoint configuration**, where you choose between the **URL** and **ID** options. The **ID** option is just a somewhat roundabout method for generating the URL – in this section, we'll ignore **ID** in favor of an easier way to obtain the URL.

In your Azure portal:

1. Navigate to the [Azure Resource Graph Explorer](https://learn.microsoft.com/en-us/azure/governance/resource-graph/concepts/explore-resources). The Resource Graph Explorer queries across your whole Azure cloud.
1. Run the following query in the Explorer to list all your Data Collection Endpoints (DCEs) and Data Collection Rules (DCRs).
    ```kusto
    Resources
    | where type =~ 'microsoft.insights/datacollectionrules'
    | mv-expand Streams= properties['dataFlows']
    | project name, id, DCE = tostring(properties['dataCollectionEndpointId']), ImmutableId = properties['immutableId'], StreamName = Streams['streams'][0]
     | join kind=leftouter (Resources
    | where type =~ 'microsoft.insights/datacollectionendpoints'
    | project name,  DCE = tostring(id), endpoint = properties['logsIngestion']['endpoint']) on DCE
    | project name, StreamName, Endpoint = strcat(endpoint, '/dataCollectionRules/',ImmutableId,'/streams/',StreamName,'?api-version=2021-11-01-preview')
    ```
1. If you are using all four of the tables mentioned above, the output should look something like this:

    ![Azure Resource Graph Explorer query output](st-sentinel-integ.4981bc86df.png)
    {border="true"}
    > ##### Problems with Resource Graph Explorer Queries
    >
    > If you get zero returns or the query returns unexpected data, be sure you have properly prepared your [Azure workspace](usecase-azure-workspace). This step is a preprequisite.
    {.box .info}
1. Click a DCE of interest to open its **Details** drawer.
1. Find the **Endpoint** field and click its **Copy to clipboard** icon. 
   * Microsoft [defines](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview) the Endpoint URI as follows:
    ```http
    {Data Collection Endpoint URI}/dataCollectionRules/{DCR Immutable ID}/streams/{Stream Name}?api-version=2021-11-01-preview
    ```
1. In your Cribl Stream Microsoft Sentinel Destination, paste the copied Endpoint URI into the **URL** field.

## Configuring Your Destinations, Continued

In most respects, Cribl Stream's Microsoft Sentinel and Webhook Destinations are alike. This section notes the few differences, while highlighting key points about configuring Microsoft Sentinel Destinations. 

In Microsoft Sentinel Destination, **General Settings** > **Authentication** offers fewer options than the Webhook Destination. This is because OAuth is Cribl Stream's only supported option for Microsoft Sentinel, for now. Some fields that present in other Destinations always have the same values for Microsoft Sentinel, and Cribl Stream fills those out for you without exposing them in the UI.

The Microsoft Sentinel Destination does not support client-side TLS.

The value for **Advanced Settings** > **Max body size (KB)** value defaults to `1000`. The intent here is to keep request bodies from exceeding Microsoft Sentinel's limit of 1 MB, meaning 1024 KB including headers. If necessary, you can lower **Max body size (KB)** even further.

## Appendix: Azure Monitor Logs Ingestion API Event Types {#all-event-types}

Here's the whole list – note that this includes event types for AWS and for Google Cloud Platform (GCP).

| Category | Event Type |
| --- | --- |
| Active Directory |ADAssessmentRecommendation | 
| |ADSecurityAssessmentRecommendation | 
| Advanced Security Information Model (ASIM) |ASimAuditEventLogs | 
| |ASimAuthenticationEventLogs | 
| |ASimDhcpEventLogs | 
| |ASimDnsActivityLogs | 
| |ASimDnsAuditLogs | 
| |ASimFileEventLogs | 
| |ASimNetworkSessionLogs | 
| |ASimProcessEventLogs | 
| |ASimRegistryEventLogs | 
| |ASimUserManagementActivityLogs | 
| |ASimWebSessionLogs | 
| AWS |AWSCloudTrail | 
| |AWSCloudWatch | 
| |AWSGuardDuty | 
| |AWSVPCFlow | 
| Azure |AzureAssessmentRecommendation | 
| Common Event Format (CEF) |CommonSecurityLog | 
| Microsoft TVM (Defender for Endpoints) |DeviceTvmSecureConfigurationAssessmentKB | 
| |DeviceTvmSoftwareVulnerabilitiesKB | 
| Microsoft Exchange |ExchangeAssessmentRecommendation | 
| |ExchangeOnlineAssessmentRecommendation | 
| Google Cloud Platform |GCPAuditLogs | 
| |GoogleCloudSCC | 
| Microsoft SCCM (System Center Configuration Manager) |SCCMAssessmentRecommendation | 
| Microsoft System Center Operations Manager (SCOM) |SCOMAssessmentRecommendation | 
| Security events collected from windows machines by Azure Security Center or Microsoft Sentinel |SecurityEvent | 
| Skype for Business Online |SfBAssessmentRecommendation | 
| |SfBOnlineAssessmentRecommendation | 
| SharePoint |SharePointOnlineAssessmentRecommendation | 
| Workloads |SPAssessmentRecommendation | 
| |SQLAssessmentRecommendation | 
| StorageInsights |StorageInsightsAccountPropertiesDaily | 
| |StorageInsightsDailyMetrics | 
| |StorageInsightsHourlyMetrics | 
| |StorageInsightsMonthlyMetrics | 
| |StorageInsightsWeeklyMetrics | 
| Syslog |Syslog | 
|Update Compliance |UCClient | 
| |UCClientReadinessStatus | 
| |UCClientUpdateStatus | 
| |UCDeviceAlert | 
| |UCDOAggregatedStatus | 
| |UCDOStatus | 
| |UCServiceUpdateStatus | 
| |UCUpdateAlert | 
| Windows |WindowsClientAssessmentRecommendation | 
| |WindowsEvent | 
| |WindowsServerAssessmentRecommendation | 
