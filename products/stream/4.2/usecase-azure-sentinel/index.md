# Azure Sentinel SIEM Integration {#preamble}


You can send events through Cribl Stream [Azure Sentinel Destinations](destinations-sentinel) to the [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) SIEM in Azure.

> These docs use the terms "Azure Sentinel" and "Microsoft Sentinel" interchangeably.
> 
> For Azure Sentinel in `azure.us` GovCloud, the Sentinel Destination is not compatible due to its preconfigured OAuth **Login URL**. Attempting to use it would result in an `invalid audience` error. Instead, leverage the [Webhook Destination](destinations-webhook) and manually specify the OAuth **Login URL** as `https://monitor.azure.us` to integrate with your GovCloud deployment.
> 
{.box .success}

This topic explains the overall workflow for getting data from Cribl Stream into Microsoft Sentinel, with an emphasis on the "Azure-y" steps in the process. We'll also cover key points about the Cribl Stream Azure Sentinel Destination, but see also the [Sentinel Destination page](destinations-sentinel) for complete UI details.

## Before You Begin

Ensure that you have [prepared your Azure Workspace](usecase-azure-workspace). You must complete this step before you can send events through the Azure Sentinel Destination to the Microsoft Sentinel SIEM.

If you do not yet have Microsoft Sentinel up and running, please join us on Cribl's Community Slack at [https://cribl‑community.slack.com/](https://cribl-community.slack.com/) and let us know; very likely, a wise goat will offer time-saving suggestions.

### Event Types

Of the [many event types](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables-category) (also called "tables") present in the Microsoft ecosystem, this topic provides what you need to get started quickly with any or all of the following four:

* [CommonSecurityLog](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/commonsecuritylog)
* [SecurityEvent](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityevent)
* [Syslog](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/syslog)
* [WindowsEvent](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/windowsevent)

We'll also explain how you can bring additional event types into your deployment.

To plan your Cribl/Sentinel integration, start by deciding which event types (a.k.a. tables) you want to send to Sentinel.

If the answer is "more than one," you should use an [Output Router](destinations-output-router) and multiple Destinations. You'll configure one Destination per table, but a single URL will ingest all the data. The Output Router can then route a given event type to the correct Destination.

## Creating Data Collection Rules

The Sentinel SIEM can have multiple ingestion endpoints, called Data Collection Endpoints (DCEs). In turn, each DCE can have multiple Data Collection Rules (DCRs). Each DCR dictates the structure of the data flowing into one table. 

Consider the Syslog table, as [documented by Microsoft](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/syslog). In the DCR, the `outputStream` element specifies what table the data should populate (here, `Microsoft-Syslog`). The `transformKql` element is a query that translates incoming fields into values in the table's columns (e.g., the `TimeGenerated` column).

Use Cribl's [template](usecase-webhook-azure-sentinel-dcr-template) to create a custom DCR that includes each type of data you plan to send to the Sentinel SIEM. The template already includes the four event types cited above, and you can add onto it to support additional event types, as described in Microsoft's [documentation](https://learn.microsoft.com/en-us/azure/sentinel/data-transformation#data-transformation-support-for-custom-data-connectors).

Here's an example of how you might have one table that's populated by multiple DCRs, each sending a different kind of data:
* The table could be Syslog (i.e., `Microsoft-Syslog`).
* One DCR could handle Windows system logs; another could handle Linux system logs; and a third could handle Mac system logs.

It's also possible for a single DCR to send data to multiple tables.

> ##### Custom DCR Template Errors
>
> If you get an error such as `Table for output stream 'Microsoft-CommonSecurityLog' is not available for destination 'logAnalyticsWorkspace')`, it is probably because you don't have Sentinel assigned to your Log Analytics workspace. Please add Microsoft Sentinel to your Log Analytics Workspace to correct this error.
>
{.box .info}

## Obtaining Your URL {#obtaining-url}

To configure your Cribl Stream Azure Sentinel Destination, you must know the ingestion URL to send data to. This is in **General Settings** > **Endpoint configuration**, where you choose between the **URL** and **ID** options. The **ID** option is just a somewhat roundabout method for generating the URL – in this section, we'll ignore **ID** in favor of an easier way to obtain the URL.

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

    ![Azure Resource Graph Explorer query output](st-sentinel-integ.fee641c948.png)
    {border="true"}
    > ##### Problems with Resource Graph Explorer Queries
    >
    > If you get zero returns or the query returns unexpected data, be sure you have properly prepared your [Azure workspace](usecase-azure-workspace). This step is a preprequisite.
    {.box .info}
1. Click a DCE of interest to open its **Details** drawer.
1. Find the **Endpoint** field and click its **Copy to clipboard** icon. 
   * Microsoft [defines](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview) the Endpoint URI as follows:
    ```
    {Data Collection Endpoint URI}/dataCollectionRules/{DCR Immutable ID}/streams/{Stream Name}?api-version=2021-11-01-preview
    ```
1. In your Cribl Stream Azure Sentinel Destination, paste the copied Endpoint URI into the **URL** field.

## Configuring Your Destinations, Continued

In most respects, Cribl Stream's Azure Sentinel and Webhook Destinations are alike. This section notes the few differences, while highlighting key points about configuring Azure Sentinel Destinations. 

In Azure Sentinel Destination, **General Settings** > **Authentication** offers fewer options than the Webhook Destination. This is because OAuth is Cribl Stream's only supported option for Microsoft Sentinel, for now. Some fields that present in other Destinations always have the same values for Microsoft Sentinel, and Cribl Stream fills those out for you without exposing them in the UI.

The Azure Sentinel Destination does not support client-side TLS.

The value for **Advanced Settings** > **Max body size (KB)** value defaults to `1000`. The intent here is to keep request bodies from exceeding Microsoft Sentinel's limit of 1 MB, meaning 1024 KB including headers. If necessary, you can lower **Max body size (KB)** even further.
