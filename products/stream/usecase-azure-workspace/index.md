# Prepare the Azure Workspace for Cribl Integrations

This guide walks you through configuring Azure to receive data from Cribl Stream. Currently, Cribl Stream supports sending data to  **custom tables** and some **native tables** in Azure Log Analytics workspaces. For more information, see Azure's [Supported Tables](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/logs-ingestion-api-overview#supported-tables) topic.  

## Before You Begin

To send data to custom tables, you need to configure a Data Collection Rule (DCR) with Stream definitions. We provide example DCR templates in our [Cribl-Microsoft GitHub repository](https://github.com/criblio/Cribl-Microsoft) to simplify this process.

## Choose Your Data Collection Method

Decide which method best suits your deployment:

* **Method 1: DCR kind:direct (Recommended)**: This is the simplest approach. The `logIngestion` URL is embedded directly in the DCR, so a separate Data Collection Endpoint (DCE) is not needed. This is ideal if you don't require Private Link connectivity or complex network routing.

* **Method 2: Data Collection Endpoint (DCE)**: Use this method if you need **Private Link connectivity** for network isolation, specific data routing, or a single ingestion endpoint for multiple DCRs.

> For most deployments, DCR `kind:direct` is the recommended approach due to its simplicity. Only use a DCE if you have specific networking requirements such as Private Link connectivity, network isolation, or centralized endpoint management across multiple regions.
>
{.box .info}

## Deployment Approaches

This guide covers **manual configuration** using the Azure portal. For automation and templates, refer to the [Cribl-Microsoft GitHub repository](https://github.com/criblio/Cribl-Microsoft). The GitHub repository contains various resources and approaches that can simplify and automate the deployment process, which is particularly useful for production environments or repeated deployments.

The following sections walk through manual Azure portal configuration for those who prefer or need to understand the step-by-step process.

### Prepare the Azure Workspace

First, you must prepare the Azure Workspace to receive data from Cribl Stream by [registering](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app) Cribl Stream as an authorized application with the [Microsoft Identity Platform](https://learn.microsoft.com/en-us/azure/active-directory/develop/v2-overview).  

> Use Copy buttons, where available, to copy complete values for later use without truncation.
>
{.box .success}

#### Common Setup Steps

Regardless of which configuration path you choose, you'll need to complete these initial steps.

##### Create Credentials for a New Azure Application

Register Cribl Stream as an authorized application with the Microsoft Identity Platform.

1. Log into the **Azure** portal as an `Administrator`.
2. Navigate to the portal's **App registration** section.
3. Click **New registration** to register an application.
4. Register a new application with the name `Cribl Stream`.

![Registering an Application](st-usecase-webhook-azure-sentinel-ss01.a471278aa6.png)
{border="true"}

5. Click **Register**.
6. Store the **Application (client) ID** and **Directory (tenant) ID** for later steps.
7. In your newly registered application, select the **Certificates & secrets** tab.
8. Create a **New client secret**.
9. Store the secret **Value** for later steps.

![Creating a client secret](st-usecase-webhook-azure-sentinel-ss02.8d62a9c15a.png)
{border="true"}

##### Find the Log Analytics Workspace Resource ID {#workspace-id}

1. Navigate to the Azure portal's **Log Analytics workspaces** service.
1. Select the workspace that will receive data.
1. From the **Overview** page, select **JSON view**, and store the value of the **Resource ID** (gray box at the top) for later use.

#### Configure Data Collection (Choose Your Path)

##### Path A: Using DCR `kind:direct` (Recommended)
If you chose the DCR `kind:direct` method, you can skip directly to  [Create and Configure the Data Creation Rule](#create-dcr). The `logIngestion` URL will be embedded automatically.

##### Path B: Using a DCE

If you need a DCE, create it first.
1.  Navigate to the Azure portal's Monitor service.
1.  Under **Settings**, select **Data Collection Endpoints**.
1.  Select **Create** and name the endpoint `Cribl-Stream-Ingestion`. 
1. Select the appropriate Subscription and Resource Group used by your organization.
1. Select **Review + create**, review your changes, then select **Create**.
1. In the list of **Data Collection Endpoints**, open the newly created endpoint. (You might need to refresh the page to see it.)
1. Navigate to the **Overview** page, and copy and store the **Logs Ingestion** URL.
1. Select **JSON view**, and store the value of the **Resource ID** (gray box at the top) for later use.

![Creating a DCE](st-usecase-webhook-azure-sentinel-ss03.fa8d4defaa.png)
{border="true"}

After creating the DCE, proceed to **Create and Configure the Data Collection Rule**.

### Create and Configure the Data Collection Rule {#create-dcr}

Both configuration paths require a DCR, but they use different templates:
- **DCR `kind:direct`**: Creates a DCR with `kind:direct` that includes an embedded `logIngestion URL`.
- **DCE method**: Creates a standard DCR that references the DCE.

Both templates are available in the [Cribl-Microsoft GitHub repository](https://github.com/criblio/Cribl-Microsoft). Browse to the appropriate template for your use case.

#### Deploy the DCR Template
1.  Navigate to **Deploy a custom template**.
1.  Select **Build your own template in the editor**, then **Load file** to upload the appropriate DCR Template from the [Cribl-Microsoft repository](https://github.com/criblio/Cribl-Microsoft).
1.  Click **Save**.
1.  Select the appropriate Subscription and Resource Group used by your organization.
1.  Name the rule `Cribl-Stream-Ingestion-Rule`.
1. Enter the required parameters based on your configuration path:
   - **For DCR kind:direct configuration:** Enter only the Log Analytics **Workspace Resource ID**.
   - **For DCE configuration:** Enter the Log Analytics **Workspace Resource ID**, and the **Endpoint Resource ID**.
1. To create the Data Collection Rule, select **Review + create**, followed by **Create**.

![Deploy the DCR Template](st-usecase-webhook-azure-sentinel-ss04.77b470508b.png)
{border="true"}

### Configure Permissions: Add Role Assignment

1. Once the template has been deployed, select **Go to resource**.
1. From the Overview page, select the **JSON view**, and store the **immutableId** from the JSON body (without quotes) for later use.
1. Click the Data Collection Rule's **Access control (IAM)**.
1. Click **Add role assignment**.
1. Select the **Monitoring Metrics Publisher** role, then select **Next**.
1. Click **+ Select members**.
1. Search for and select `Cribl Stream` (the app you created earlier), then select **Select** to confirm the selection.
1. Click **Review + assign** to review changes. 
1. Click **Review + assign** again to implement the permissions update.

![Adding a Role Assignment](st-usecase-webhook-azure-sentinel-ss05.44b5b449b3.png)
{border="true"}

## Required Fields for Cribl Stream Configuration

After completing this setup, configure your [Cribl Stream Azure Destination](/stream/azure-destinations/) using the values in the following fields:
- `Application (client) ID`
- `Directory (tenant) ID`
- `Client secret value`
- `Log Analytics Workspace Resource ID`
- `Data Collection Rule immutableId`

Additional fields for DCE configurations:
- `Data Collection Endpoint Resource ID`
- `Logs Ingestion URL`

### Next Steps

1. Configure your Cribl Stream Azure Destination using the collected credentials.
1. Test the Pipeline.
1. Monitor data flow in your Log Analytics workspace.

## Additional Resources

- [Cribl-Microsoft GitHub Repository](https://github.com/criblio/Cribl-Microsoft) - DCR templates and integration artifacts
- [Azure Monitor data collection](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection)
- [Data Collection Rules overview](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection-rule-overview)
