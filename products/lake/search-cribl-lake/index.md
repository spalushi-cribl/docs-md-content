# Search Cribl Lake


Search your Cribl Lake data with Cribl Search.

---

Cribl Lake appears as a preconfigured Cribl Search [Dataset Provider](/search/basic-concepts#dataset-providers) called `cribl_lake`. This enables you to use Cribl Lake [Datasets](datasets) as Cribl Search Datasets and instantly start searching them.

## Search a Lake Dataset {#search-lake}

To access Cribl Search: From your Organization's top bar, select **Products**, then select **Search**. You can then [query](/search/about#action) your Lake Datasets from Cribl Search.

## Search a Lakehouse {#search-lakehouse}

You can speed up searching your Cribl Lake data by using a [Lakehouse](/lake/lakehouse). Once a Lake Dataset is assigned to a Lakehouse, searches against that Dataset will run significantly faster.

### Verify Lakehouse Use {#verify-lakehouse}

To verify whether a search successfully used a Lakehouse,
take a look at the [tracking bar](/search/search-overview#track-the-progress-of-your-search).

If the search failed to use a Lakehouse, the bar presents information about potential reasons.
The reasons might include (as examples) the Lakehouse being disabled or misconfigured, or the query exceeding the time range of data stored in the Lakehouse.

### Search Multiple Lakehouse Datasets {#multi-lakehouse}

You can run a single query against multiple Lakehouse-assigned Datasets. For the query to execute at Lakehouse speed, all  Datasets in the query must be Lakehouse-assigned, and your query must also meet one of these conditions:

- Include only operators from the following group: [`cribl`](/search/cribl), [`centralize`](/search/centralize), [`extend`](/search/extend), [`extract`](/search/extract), [`foldkeys`](/search/foldkeys), [`limit`](/search/limit), [`mv-expand`](/search/mv-expand), [`pivot`](/search/pivot), [`project`](/search/project), [`project-away`](/search/project-away), [`project-rename`](/search/project-rename), [`search`](/search/search), and [`where`](/search/where).

- Or, if you use any other operators, insert the [`centralize`](/search/centralize) or [`limit`](/search/limit) operator before them. (Result counts will be further constrained by [usage group](/search/usage-groups#usage-limits) limits.)

If neither of the above conditions is met, or if your query includes non-Lakehouse Datasets, the query will run at standard speed.

### Cribl Search Differences with Lakehouse {#search-diffs}

Executing Cribl Search queries against a Lakehouse-assigned Dataset changes some behavior and results, compared to executing the same queries without Lakehouse caching. For details, see [Lakehouse Search Differences](/search/lakehouse-differences).

## Examples of Searching Cribl Lake {#examples}

Use these examples as starting points for your own searches.

### Basic Search into Cribl Lake {#basic}

This search specifies the Dataset (`test_dataset`) and limits the number of results.

```kusto
dataset="test_dataset"
| limit 100
```

![Cribl Search Home page showing results of a simple search over Cribl Lake](lake-search-simple-4.6.png)
{scale="100%" caption="Sample Cribl Lake Search"}


### Search Cribl Lake with a Partition {#partition}

This search uses a [Lake partition](managing-datasets#lake-partitions) named `sourcetype`
that is configured for the `partitioned` Dataset to speed up retrieval:

```kusto
dataset="partitioned" host="cribl-stream"
```

![Cribl Search Home page showing results of a search over Cribl Lake with a partition](lake-search-partitioned-4.11.png)
{scale="100%" caption="Sample Cribl Lake Search: Using partitions"}

### Export Cribl Search Results to Cribl Lake {#export}

The [`export`](/search/export/) operator lets you export Cribl Search results to a Lake Dataset.
You can later search this Dataset to extract relevant data from it.

An efficient way to search exported data is to provide the search job ID to the [`where`](/search/where/) operator:

```kusto
dataset="exported_data"
| where source contains "1713177481843.9AOqxI"
```

> You can find the search job ID in search details after running it,
> or in the [**History**](/search/search-overview#history) tab, in the **Search ID** column.
{.box .success}

You can also label exported events using the [`extend`](/search/extend/) operator
and then include the added fields in your search.
For example, during export you can include the user that performed the search:

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
