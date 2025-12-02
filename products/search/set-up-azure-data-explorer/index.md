# Connect Cribl Search to Azure Data Explorer


Configure Cribl Search to query Azure Data Explorer.

---

Cribl Search allows you to search database tables from
[Azure Data Explorer (ADX)](https://learn.microsoft.com/en-us/azure/data-explorer/data-explorer-overview). ADX stores
log data in databases, which in turn contain one or more of the tables defined in the Azure namespace.


> Azure Data Explorer has a 500,000 result limit per search.
{.box .info}

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) database tables in Azure Data Explorer.

## Add an Azure Data Explorer Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Azure
Data Explorer Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Azure Data Explorer**.
1. The following inputs are retrieved from Azure Data Explorer after creating and registering a Microsoft Entra service
   principal and then authorizing it to access an Azure Data Explorer database. See Microsoft's guide -
   [Create a Microsoft Entra application registration in Azure Data Explorer](https://learn.microsoft.com/en-us/azure/data-explorer/provision-entra-id-app)
   for details.
   - **Tenant ID** is the ID of the Directory (tenant) in Azure Data Explorer to retrieve information from.
   - **Client ID** is the ID of the Application (client) in Azure Data Explorer.
   - **Client Secret** is the client secret that will be used as the secret in the connection to the application.
1. Select **Save** when finished.

## Add an Azure Data Explorer Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of an [Azure Data Explorer provider](#provider).
   - Enter the **Cluster** name and select its region from the **Location** drop-down.
   - **Database** is the database name.
   - **Table or Query** supports the
     [Kusto Query Language](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/). You can specify the
     name of a table to query, like `logs`, or enter a Kusto query, such as
     `StormEvents | summarize PropertyDamage = sum(DamageProperty) by State | join kind=innerunique PopulationData on State`.
   - **Timestamp field** is the event field with a timestamp. You need to specify a timestamp field to query by
     [date and time](build-a-search#time-range) in Cribl Search.
   - **Timestamp field contents** is the data type of the timestamp field. Defaults to Kusto's
     [datetime](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/scalar-data-types/datetime) data type.
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see [Datatypes](datatypes).
1. Select **Save** when finished.

## Search Azure Data Explorer

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

It can take a few minutes for a search to start returning results. Here's an example of an initial exploratory search:

```kusto
dataset="adx" | limit 1000
```

