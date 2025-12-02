# Azure Analytics/Webhook Integration


You can configure Cribl Stream to send logs through a [Webhook Destination](destinations-webhook) to the Azure Log Analytics Workspace. 

Currently, Cribl Stream supports sending data only to custom tables and certain native tables. For details, see Azure's [Supported Tables](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview#supported-tables) topic.

To send data to custom tables using this integration, you must update a DCR Template with Stream definitions. We've provided an example [DCR Template](usecase-webhook-azure-sentinel-dcr-template) file.



## Prepare the Azure Workspace

First, you must prepare the Azure Workspace to receive data from Cribl Stream, by [registering](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app) Cribl Stream as an authorized application with the Microsoft Identity Platform]. 

> Use Copy buttons, where available, to copy complete values for later use without truncation.
>
{.box .success}

### Create Credentials for a New Azure Application

1. Log into the **Azure** portal as an `Administrator`.
2. Navigate to the portal's **App registration** section.
3. Click **New registration** to register an application.
4. Register a new application with the name `Cribl Stream`.

![Registering an Application](st-usecase-webhook-azure-sentinel-ss01.a471278aa6.png)
{border="true"}

5. Click **Register**.
6. Store the **Application (client) ID** and **Directory (tenant) ID** for later steps.
7. In your newly registered application, select the **Certificates & secrets** tab.
8. Create a **New client secret**.
9. Store the secret **Value** for later steps.

![Creating a client secret](st-usecase-webhook-azure-sentinel-ss02.8d62a9c15a.png)
{border="true"}

### Create a Data Collection Endpoint {#endpoint}

1. Navigate to the Azure portal's **Monitor** service.
2. Under **Settings**, select **Data Collection Endpoints**.
3. Select **Create** to add a new endpoint named `Cribl-Stream-Ingestion`. 
4. Select the appropriate Subscription and Resource Group used by your organization.
5. Select **Review + create**, review your changes, then select **Create**.
6. In the list of **Data Collection Endpoints**, open the newly created endpoint. (You might need to refresh the page to see it.)

![Creating a Data Collection Endpoint](st-usecase-webhook-azure-sentinel-ss03.fa8d4defaa.png)
{border="true"}

7. Navigate to the **Overview** page, and copy and store the **Logs Ingestion** URL.
8. Select **JSON view**, and store the value of the **Resource ID** (gray box at the top) for later use.

### Find the Log Analytics Workspace Resource ID

1. Navigate to the Azure portal's **Log Analytics workspaces** service.
2. Select the workspace that will receive data.
3. From the **Overview** page, select **JSON view**, and store the value of the **Resource ID** (gray box at the top) for later use.

### Create Data Collection Rule

1. Navigate to the Azure portal's **Deploy a custom template** service.
2. Select **Build your own template in the editor**.
3. Select **Load file** and upload the [DCR Template](usecase-webhook-azure-sentinel-dcr-template).
4. Click **Save**.
5. Select the appropriate Subscription and Resource Group used by your organization.
6. Name the new Data Collection Rule `Cribl-Stream-Ingestion-Rule`.
7. Enter the Log Analytics **Workspace Resource ID**, and the Data Collection **Endpoint Resource ID**, that you stored in the previous steps.
8. To create the Data Collection Rule, click **Review + create**, followed by **Create**.

![Registering an Application](st-usecase-webhook-azure-sentinel-ss04.77b470508b.png)
{border="true"}

### Add Role Assignment

1. Once the template has been deployed, click **Go to resource**.
2. From the Overview page, select the **JSON view**, and store the **immutableId** from the JSON body (without quotes) for later use.
3. Click the Data Collection Rule's **Access control (IAM)**.
4. Click **Add role assignment**.
5. Select the **Monitoring Metrics Publisher** role, then click **Next**.
6. Click **+ Select members**.
7. Search for and select `Cribl Stream` (the app you created earlier), then click **Select** to confirm the selection.
8. Click **Review + assign** to review changes. 
9. Click **Review + assign** again to implement the permissions update.

![Adding a Role Assignment](st-usecase-webhook-azure-sentinel-ss05.44b5b449b3.png)
{border="true"}

## Configure Cribl Stream's Webhook Destination

From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Destinations**, then select **Webhook** from the **Manage Destinations** page's tiles or left nav. Click **New Destination** to open the **Webhook** > **New Destination** modal.

> Currently, you must configure a separate Cribl Stream Destination for each stream defined in the Azure portal's **Data Collection Rule**.
>
{.box .info}

On the modal's **Configure** > **General Settings** tab, enter or select the following values:
- **URL**: Enter the URL for the webhook, substituting the values you stored earlier into this template:  
`https://<< your logs ingestion url >>/dataCollectionRules/<< your data collection rule immutableId >>/streams/Custom-<< log analytics workspace native table name >>?api-version=2021-11-01-preview`
- **Method**: `POST`
- **Format**: `JSON Array`

Next, in the **Authentication** tab, make sure that **Authentication type** is set to `OAuth`. 

- **Login URL**:  
`https://login.microsoftonline.com/<< your tenant ID >>/oauth2/v2.0/token`
- **OAuth secret**: `<< your client secret >>`
- **OAuth Secret Parameter Name**: `client_secret`
- **Token Attribute Name**: `access_token`
- **Authorize Expression**: `` `Bearer ${token}` ``
- **Refresh interval (secs.)**: `3000`

![Example General Settings](st-usecase-webhook-azure-sentinel-ss06.d7405145dd.png)
{border="true"}

As **OAuth parameters**, enter:
- **client_id**: `` `<< your client/app ID >>` ``
- **scope**: `` `https://monitor.azure.com//.default` ``
- **grant_type**:  `` `client_credentials` ``

![Example Authentication Settings](st-usecase-webhook-azure-sentinel-ss07.13d898a9d0.png)
{border="true"}

## Verify that Azure Log Analytics Workspace Is Receiving Data

In Cribl Stream:

1. Navigate to your Webhook Destination's configuration modal.

2. Click the **Test** tab.

3. In the **Test** field, define one or more JSON payloads for the Cribl data you want to send. You can use the sample payload for a `syslog` table, below. Make sure you update the timestamps.

    ```json
    [
        {
            "Computer": "CriblStreamWorker",
            "EventTime": "2023-02-16T16:17:18.000Z",
            "Facility": "local4",
            "HostIP": "10.0.1.96",
            "HostName": "cribl-worker.lan",
            "ProcessID": "42000",
            "ProcessName": "cribl",
            "SeverityLevel": "warning",
            "SourceSystem": "destinationTest",
            "SyslogMessage": "CONNECTIVITY TEST",
            "TimeCollected": "2023-02-16T16:17:19.000Z",
            "TimeGenerated": "2023-02-16T16:17:20.000Z"
        }
    ]
    ```
4. Click **Test**.

5. Open Azure Log Analytics Workspaces, and select the workspace with the endpoint you [configured](#endpoint) earlier.
4. Search the `syslog` table for the message in the sample.

>  When you run the test for the first time, the data might take up to 15 minutes to appear in the table.
> 
>  When you write to syslog or `CommonEventLog` tables, `TimeGenerated` is a required field. If you omit it, your request will silently fail to write data into the tables.
>
{.box .warning}
