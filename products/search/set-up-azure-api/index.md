# Connect Cribl Search to Azure API


Configure Cribl Search to query an Azure API endpoint.

---

[Microsoft Azure](https://azure.microsoft.com/en-us/) is a public cloud computing platform that offers a range of
services that include Infrastructure as a Service (IaaS), Platform as a Service (PaaS), and Software as a Service
(SaaS).

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the [disks](https://learn.microsoft.com/en-us/rest/api/compute/disks/list?tabs=HTTP),
[networkSecurityGroups](https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-security-groups/list-all?tabs=HTTP),
[virtualMachines](https://learn.microsoft.com/en-us/rest/api/azure/devops/distributedtask/virtualmachines/list?view=azure-devops-rest-6.0),
and [webapps](https://learn.microsoft.com/en-us/rest/api/appservice/app-service-plans/list-web-apps) endpoints in the
[Azure API](https://learn.microsoft.com/en-us/rest/api/azure/).

## Azure API Authorization

Set up an Azure service account with a client secret credential for Search. For details, see
[Create a Microsoft Entra application and service principal that can access resources](https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
You will need the account credentials to [Create a Dataset Provider](#provider).

Also, you can assign the built-in role of
[Reader](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#reader) to the application so
it has read access to all endpoints. To limit access to the current Cribl Search endpoints (listed below), create a
custom role:

- [Microsoft.Compute/disks](https://learn.microsoft.com/en-us/rest/api/compute/disks/list?tabs=HTTP)
- [Microsoft.Network/networkSecurityGroups](https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-security-groups/list-all?tabs=HTTP)
- [Microsoft.Compute/virtualMachines](https://learn.microsoft.com/en-us/rest/api/compute/virtual-machines/list-all)
- [Microsoft.Web/sites](https://learn.microsoft.com/en-us/rest/api/appservice/web-apps/list?view=rest-appservice-2022-03-01)

You can modify permissions as the application adds more endpoints. For details, see
[Create an Azure custom role](https://learn.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell).

## Add an Azure API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Azure API
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Azure API**.
1. Select **Add Configuration** to specify your Azure account(s).
   - **Account Name** is the Azure account name.
   - **Tenant ID** is the ID of the
     [Microsoft Entra ID](https://learn.microsoft.com/en-us/partner-center/find-ids-and-domain-names) to retrieve
     information from.
   - **Client ID** is the ID of the application that will connect to Microsoft Entra ID Active Directory. For details,
     see
     [Register an application](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app#register-an-application).
   - **Client Secret** is the key that will be used as the secret in the connection to Microsoft Entra ID. For details,
     see
     [Add a client secret](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app#add-a-client-secret).
1. Select **Save** when finished.

## Add an Azure API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of an [Azure API Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. Your options are:
   - [virtualMachines](https://learn.microsoft.com/en-us/rest/api/azure/devops/distributedtask/virtualmachines/list?view=azure-devops-rest-6.0)
   - [disks](https://learn.microsoft.com/en-us/rest/api/compute/disks/list?tabs=HTTP)
   - [networkSecurityGroups](https://learn.microsoft.com/en-us/rest/api/virtualnetwork/network-security-groups/list-all?tabs=HTTP)
   - [webapps](https://learn.microsoft.com/en-us/rest/api/appservice/app-service-plans/list-web-apps)
1. **Subscription IDs** is a list of the
   [Subscription IDs](https://learn.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id#find-your-azure-subscription)
   within the tenant to query with this Dataset.
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Azure API

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

