# Microsoft Graph API Collection


The [Microsoft Graph API](https://docs.microsoft.com/en-us/graph/overview?view=graph-rest-1.0) provides access to data in the Microsoft Cloud. This page explains how to configure a Cribl Stream [REST/API Collector](collectors-rest) to ingest data using the Microsoft Graph API. 

Before you start, you'll need to do the following in the Azure portal:

* Register the app you'll use to interact with Graph API.
* Generate a **Client Secret** for the app.
* Write down the **Application ID**, **Tenant ID**, and **Secret Value** defined for the app.
* Decide which API calls you expect to use, then, for each of those API calls:
  * Configure the corresponding API permissions for your app.
  * Examine how the Graph API structures events, and decide whether you want to change that structure using an [Event Breaker](event-breakers).

For example, if you plan to call the [Audit Logs API](https://docs.microsoft.com/en-us/graph/api/resources/directoryaudit?view=graph-rest-1.0) (as demonstrated in this walkthrough), do the following:

* Configure your app with `AuditLog.Read.All` API permissions, which Audit Logs API calls require.
* Notice that the API structures Microsoft Entra ID Audit Logs data as a single JSON object with events nested under the `value` field. With an Event Breaker, you can [split](#event-breakers) this into individual events for each AD Audit Log activity.

## How the REST Collector Interacts with Microsoft APIs

To retrieve data using the Microsoft Graph API, your Collector first obtains a Bearer token by sending an HTTP `POST` request to the Microsoft identity platform. Once it has the Bearer token, your Collector can send an HTTP `GET` request to the Graph API, which responds with the data you requested. Configuring your Collector will be more intuitive if you keep this pattern in mind.

## Configuring a Graph API REST Collector

From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Sources**, then select **Collectors** > **REST** from the **Manage Sources** page's tiles or left nav. Click **New Collector** to open the **REST** > **New Collector** modal, which provides the following options and fields.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
>
{.box .info}

## Collector Settings

These settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. E.g., `ms_graph_42`.

### Collect Settings

**Collect URL**: An expression that produces a URL for the collect operation. The URL can be any of the Graph API [endpoints](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0). In this example, we want to retrieve Microsoft Entra ID Audit logs, so we use the Audit Logs endpoint:

`` `https://graph.microsoft.com/v1.0/auditLogs/directoryAudits` ``

**Collect method**: `GET`.

**Collect headers**: Add the following header.
- **Name**: `content-type`.
- **Value**: `application/x-www-form-urlencoded`

To authenticate against the Microsoft Graph API, the REST Collector uses a POST call whose body is a template. The `content-type` header tells the Graph API that the template is a URL-encoded (not JSON-encoded) form. See **Authentication Settings** > **POST Body** below.

### Authentication Settings

Use the **Authentication method** buttons to select **Login**, then enter the following information:

**Login URL**: The token API endpoint for the Microsoft identity platform. Use the following string substituting your Microsoft Entra ID tenant ID for `<tenant_id>`:
```
`https://login.microsoftonline.com/<tenant_id>/oauth2/v2.0/token`
```

**Login username**: The **Application (Client) ID** listed on your app's **Overview** page in Azure.

**Login password**: The **Secret Value** listed on your app's **Certificate & Secret** page in Azure.

**POST body**: The template for the token request that the Collector sends to the Microsoft identity platform. Use the following string:
``` 
`client_secret=${password}&scope=https://graph.microsoft.com/.default&client_id=${username}&grant_type=client_credentials`
```
When the Collector sends the request, it will populate the template with the **Username** and **Password** that you entered above.

**Token attribute**: The field that contains the Bearer token, within the Microsoft identity platform's response to the token request. Use `access_token`.

**Authorize expression**: The JavaScript expression that computes the `Authorization` header the Collector uses in calls to the Graph API. Use the default `` `Bearer ${token}` ``. The Collector will populate the `${token}` variable with the token obtained from the Microsoft identity platform. 

## Result Settings

As explained above, the Microsoft Graph API delivers Azure Active Directory Audit Logs data as a single JSON object, with events nested under the `value` field. Cribl recommends using an Event Breaker to split this into separate name/value pairs.

You'll create the Event Breaker in the **Result Settings** > **Event Breakers** tab. But first, you must save a sample file, and create a ruleset for the Event Breaker to use.

### Saving a Sample File

On the **Manage REST Collectors** page, click **Run** beside the REST Collector you configured. This opens the **Run configuration** modal, in **Preview** mode. Click **Run** to open the **Capture Sample Data** modal. 

When data appears, notice that it's a large JSON object with multiple events nested under the `value` field:

![Audit Logs data in original form](rest-ms-graph-1.4249c0d9fb.png)

Click **Save as Sample File** and note the file name and location.

### Creating an Event Breaker Ruleset

From the top menu, open **Processing** > **Knowledge** > **Event Breaker Rules**, then click **New Ruleset** to open the **New Ruleset** modal. Enter an ID and a Description for the ruleset, then click **Rules** > **+ Add Rule** to open the **Rules** modal.

Here's what we want the ruleset to do:

1. Remove the leading `@odata.context` statement. For this example, we'll assume that [OData](https://docs.microsoft.com/en-us/odata/overview) is not important for your use case. 
1. Structure each of the fields nested within the `value` element as a separate event, where the value of the `activityDateTime` field becomes the value of the `_timestamp` field.

Name the ruleset, then:

* From the **Event Breaker Type** drop-down, choose **JSON Array**.
 This detects that the `value` element is a JSON array, and breaks it into key-value pairs. It also discards the `@odata.context` element, because that falls outside the detected JSON array.
* In the **Timestamp Anchor** field, enter `activityDateTime` between the slashes.
* Upload your sample file.
* Compare the **In** and **Out** panes. You should see output similar to this:

  ![Audit Logs data broken into individual events](rest-ms-graph-1.5.97ac061556.png)
  {border="true"}
* Click **OK** to return to the **New Ruleset** modal.
* You should see a result similar the screenshot below. Click **Save**. 

![Creating an Event Breaker ruleset for Audit Logs data](rest-ms-graph-2.e34001e81e.png)

### Event Breakers {#event-breakers}

In the Collector's **Result Settings** > **Event Breakers** tab:

* From the **Event Breakers** drop-down, select the ruleset that you created and saved.
* Click **Save**.

Run the Collector again. Now you should see an individual JSON-formatted event for each AD Audit Log activity.

![Audit Logs data from the finished Collector](rest-ms-graph-3.995c9509a3.png)
