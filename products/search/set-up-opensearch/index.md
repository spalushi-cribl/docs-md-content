# Connect Cribl Search to OpenSearch


Configure Cribl Search to query an OpenSearch index.

---

[OpenSearch](https://opensearch.org/) is an open-source search engine for data-intensive applications.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) an OpenSearch index.

## Add an OpenSearch Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an OpenSearch
Dataset Provider.

To add a new Dataset Provider:

1. From the sidebar, select **Data**, then **Dataset Providers**.
2. Select the **Add Provider** button on the right.



Next, set the following configurations in the **New Dataset Provider** modal:

1. In **ID**, enter a unique identifier for the Dataset Provider. This is how you'll reference it when assigning
   Datasets to it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. Optionally, add **Description** to quickly describe the Dataset Provider.
1. Set **Dataset Provider Type** to **OpenSearch**.
1. Enter the following:
   - **Username**: Your OpenSearch account name.
   - **Password**: Your password for OpenSearch.
   - **Endpoint**: URL for the OpenSearch endpoint, for example: `https://opensearch.thecriblgoat.farm`.
1. Select **Save** when finished.

## Add an OpenSearch Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.



Next, set the following configuration in the resulting **New Dataset** modal:

1. In **ID**, enter a unique identifier for the Dataset. You'll use this to specify the Dataset in a query's
   [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. Optionally, add **Description** to quickly describe the Dataset.
1. Set **Dataset Provider** to the **ID** of an [OpenSearch Dataset Provider](#provider).
1. In **Index**, point to the identifier of the OpenSearch index you want to search. If you have rolling indices
   (indices with dynamic naming), use the name of the index and the wildcard character `*`. For example, if your indices
   are named `my-index-00001`, `my-index-00002`, and so on, use `my-index*` in this field.
1. Make sure **Timestamp field** contains the name of the field that contains event timestamps. In most cases, this will
   be `@timestamp` for time series data in OpenSearch.
1. Select **Save** when finished.

## Search OpenSearch

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

### OpenSearch Results Limit {#limit}

The number of OpenSearch results is capped by the
[`max_result_window` setting of your OpenSearch index](https://opensearch.org/docs/latest/install-and-configure/configuring-opensearch/index-settings/#dynamic-index-level-index-settings).
By default, it's 10,000.

If you need to raise this limit, make sure you research and test for potential side effects.

