# Searching Cribl Lake


Learn how to search your Cribl Lake data.

---



This page provides example searches that you can run with [Cribl Lake](/lake/) and [Lakehouses](/lake/lakehouse). 

## Setting Up {#seting-up}

To begin integrating Cribl Search with these data lakes, first see [Connect to Cribl Lake](set-up-cribl-lake).

Certain operators, functions, and data types behave differently when you search Cribl Lake Datasets with (versus without) Lakehouse caching. To avoid unexpected search results, see [Lakehouse Search Differences](lakehouse-differences).

## Examples {#examples}

Use these examples as starting points for your own searches.

### Basic Search {#basic}

This search specifies a Dataset (`test_dataset`) and limits the number of results.

```kusto
dataset="test_dataset"
| limit 100
```


![Cribl Search Home page showing results of a simple search over Cribl Lake](lake-search-simple-4.6.png)
{scale="100%" caption="Sample Cribl Lake Search"}


### Lake Partition {#partition}

This search uses a [Lake partition](/lake/managing-datasets#lake-partitions) named `sourcetype`
that is configured for the `partitioned` Dataset to speed up retrieval:

```kusto
dataset="partitioned" host="cribl-stream"
```


![Cribl Search Home page showing results of a search over Cribl Lake with a partition](lake-search-partitioned-4.11.png)
{scale="100%" caption="Sample Cribl Lake Search: Using partitions"}

### Exported Data {#export}

The [`export`](export) operator lets you export search results to a Cribl Lake Dataset. You can later search this
Dataset to extract relevant data from it.

An efficient way to search exported data is to provide the search job ID to the [`where`](where) operator:

```kusto
dataset="exported_data"
| where source contains "1713177481843.9AOqxI"
```


> You can find the search job ID in search details after running it,
> or in [**History**](search-overview#history), in the **Search ID** column.
{.box .success}

You can also label exported events using the [`extend`](extend) operator and then include the added fields in your
search. For example, during export you can include the user that performed the search:

```kusto {runSearch=true}
dataset="cribl_search_sample"
| extend user = user()
| export to lake exported_data
```

You can then search for data by this user:

```kusto
dataset="exported_data"
| where user == "John Doe"
```
