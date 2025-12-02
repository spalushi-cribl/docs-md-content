# Microsoft Sentinel SIEM Integration {#preamble}


You can send events through Cribl Stream [Microsoft Sentinel Destinations](destinations-sentinel) to the [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview) SIEM in Azure.

This use case demonstrates how to send data from Cribl Stream to Microsoft Sentinel in various Azure environments, including Azure Public Cloud, Azure US Government Cloud, and Microsoft Azure operated by 21Vianet. While we'll cover key points about the Cribl Stream Microsoft Sentinel Destination here, you should refer to the [Sentinel Destination topic](destinations-sentinel) for complete UI details.

## Before You Begin

Ensure that you have [prepared your Azure Workspace](usecase-azure-workspace). You must complete this step before you can send events through the Microsoft Sentinel Destination to the Microsoft Sentinel SIEM.

If you do not yet have Microsoft Sentinel up and running, please join us on Cribl's Community Slack at [https://cribl‑community.slack.com/](https://cribl-community.slack.com/) and let us know; very likely, a wise goat will offer time-saving suggestions.

### Event Types {#event-types}

Of the many event types (also called "tables") present in the Microsoft ecosystem, this topic provides a starting point for integrating Cribl with Sentinel. For the complete list of supported tables and their schemas, refer to the [Microsoft documentation for supported tables](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview#supported-tables).

To plan your Cribl/Sentinel integration, start by deciding which event types (a.k.a. tables) you want to send to Sentinel. Common event types you might work with include:

- `CommonSecurityLog`
- `SecurityEvent`
- `Syslog`
- `WindowsEvent`

## Choosing a Data Collection Strategy

When sending data to Sentinel, you have a decision to make that balances ease of troubleshooting with the number of Cribl resources you'll need:

- **One Cribl Destination for a single Data Collection Rule (DCR)**: This approach minimizes the number of Destinations you need in Cribl. You can send all data to a single Sentinel Destination, which is configured to use a single DCR that handles all of your event types. While this reduces Cribl resource overhead, it can make troubleshooting more complex as all data flows through a single point.
- **A unique Cribl Destination for each DCR**: This approach simplifies troubleshooting and provides greater control over your data. By creating a unique Sentinel Destination for each DCR, you can easily isolate and debug issues related to a specific data type. This is the recommended approach for most deployments, as it makes managing your data flows easier.

## Creating Data Collection Rules

> In Cribl Stream 4.1.4 and later, DCRs (data collection rules) support direct log ingestion URLs. This update means that DCEs (data collection endpoints) are no longer needed unless your Log Analytics workspace has restricted public access, such as with private endpoints.
{.box .success}

The Sentinel SIEM can have multiple ingestion endpoints, called Data Collection Endpoints (DCEs). In turn, each DCE can have multiple Data Collection Rules (DCRs). A DCR can dictate the structure of the data flowing into one table, or multiple tables.

For ready-to-use DCR templates and automation options covering all supported tables, visit the [Cribl-Microsoft GitHub](https://github.com/criblio/Cribl-Microsoft) repository. Cribl provides DCR templates for various tables. You can use these templates to quickly set up your DCRs.

> Ensure that the field names in your data match the schema defined in the Data Collection Rule (DCR) for Microsoft Sentinel. The DCR specifies the expected structure, including field names and types. Mismatched field names can result in dropped fields.
{.box .warning}

To determine which Sentinel table should ingest a specific data source, review the  [Microsoft Sentinel Content Hub catalog](https://learn.microsoft.com/en-us/azure/sentinel/sentinel-solutions-catalog). For instance, the Palo Alto Sentinel Solution sends all Palo Alto Firewall data to the `CommonSecurityLog` table. In contrast, the Crowdstrike Solution uses multiple tables, including custom ones, for data ingestion.

Besides the [Azure documentation](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection-rule-overview), you can study the Cribl template to understand how DCRs are structured. For example, each table's columns are defined in a sub-element of the `resources` > `properties` > `streamDeclarations`  element. The `dataFlows` element – at the same level as `streamDeclarations` – specifies the table that a given type of data should populate, via the `outputStream` sub-element. It also specifies the query that will translate incoming fields into values in the table's columns, via the `transformKql` sub-element.

Read more about DCRs in the [Microsoft Sentinel documentation](https://learn.microsoft.com/en-us/azure/sentinel/data-transformation#data-transformation-support-for-custom-data-connectors).

> ##### Custom DCR Template Errors
>
> If you get an error such as `Table for output stream 'Microsoft-CommonSecurityLog' is not available for destination 'logAnalyticsWorkspace')`, it is probably because you don't have Sentinel assigned to your Log Analytics workspace. Please add Microsoft Sentinel to your Log Analytics Workspace to correct this error.
>
{.box .info}

## Preparing Your Azure Environment

To configure your Cribl Stream Microsoft Sentinel Destination, you must provide your log ingestion URL. This resides either in a Data Collection Endpoint, or in a Data collection Rule of `kind:Direct` and a Data Collection Rule (DCR) immutable `ID.` You configure this in **General Settings** > **Endpoint configuration**. Cribl recommends the `ID` option, as it automatically generates the correct URL format, and is more reliable than manually constructing URLs.

The ID format follows this pattern:
- **Data Collection Rule ID**: The immutable ID of your DCR.
- **Stream Name**: The specific stream within the DCR you're targeting.

In most respects, Cribl Stream's Microsoft Sentinel and Webhook Destinations are alike. This section notes the few differences, while highlighting key points about configuring Microsoft Sentinel Destinations. 

In Microsoft Sentinel Destination, **General Settings** > **Authentication** offers fewer options than the Webhook Destination. This is because OAuth is Cribl Stream's only supported option for Microsoft Sentinel, for now. Some fields that present in other Destinations always have the same values for Microsoft Sentinel, and Cribl Stream fills those out for you without exposing them in the UI.

The Microsoft Sentinel Destination does not support client-side TLS.

The value for **Advanced Settings** > **Body size limit (KB)** value defaults to `1000`. The intent here is to keep request bodies from exceeding Microsoft Sentinel's limit of 1 MB, meaning 1024 KB including headers. If necessary, you can lower the **Body size limit (KB)** even further.