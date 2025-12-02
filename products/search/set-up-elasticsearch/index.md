# Connect Cribl Search to Elasticsearch


Configure Cribl Search to query an Elasticsearch index.

---

[Elasticsearch](https://www.elastic.co/elasticsearch) is a proprietary search engine for data-intensive applications.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) an Elasticsearch index.

## Add an Elasticsearch Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Elasticsearch Dataset Provider.

To add a new Dataset Provider:

1. From the sidebar, select **Data**, then **Dataset Providers**.
2. Select the **Add Provider** button on the right.

Next, set the following configurations in the **New Dataset Provider** modal:

1. In **ID**, enter a unique identifier for the Dataset Provider. This is how you'll reference it when assigning
   Datasets to it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. Optionally, add **Description** to quickly describe the Dataset Provider.
1. Set **Dataset Provider Type** to **Elasticsearch**.
1. Enter the following:
   - **Username**: Your Elasticsearch account name.
   - **Password**: Your password for Elasticsearch.
   - **Endpoint**: URL for the Elasticsearch endpoint, for example: `https://es.mycompany.com`.
1. Select **Save** when finished.

## Add an Elasticsearch Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Next, set the following configuration in the resulting **New Dataset** modal:

1. In **ID**, enter a unique identifier for the Dataset. You'll use this to specify the Dataset in a query's
   [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. Optionally, add **Description** to quickly describe the Dataset.
1. Set **Dataset Provider** to the **ID** of an [Elasticsearch Dataset Provider](#provider).
1. In **Index**, point to the identifier of the Elasticsearch index you want to search. If you have rolling indices
   (indices with dynamic naming), use the name of the index and the wildcard character `*`. For example, if your indices
   are named `my-index-00001`, `my-index-00002`, and so on, use `my-index*` in this field.
1. Make sure **Timestamp field** contains the name of the field that contains event timestamps. In most cases, this will
   be `@timestamp` for time series data in Elasticsearch.
1. Select **Save** when finished.

## Search Elasticsearch

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

### Elasticsearch Results Limit {#limit}

The number of Elasticsearch results is capped by the
[`max_result_window` setting of your Elasticsearch index](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#dynamic-index-settings).
By default, it's 10,000.

If you need to raise this limit, make sure you research and test for potential side effects.

