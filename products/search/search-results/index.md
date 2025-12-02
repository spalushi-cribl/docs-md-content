# Use Search Results


See how to export, reuse, and manage search results.

---

When you obtain each search's results, you have several options to export the results from Cribl Search, and to reuse them within the product. You can also control how long Cribl Search [retains your results](#manage).

## Export Search Results {#export}

Use the [`export`](export) operator to export search results to either a lookup table or to a
[Cribl Lake Dataset](/lake/datasets).

```kusto
dataset="cribl_search_sample"
 | export to lake my_lake_dataset
```

## Reuse Search Results {#reuse}

You can reuse the results of previous searches. This might let you avoid running the same search multiple times, saving you time and resources. Your options include:

- [View the Cached Results of a Previous Search](#view)
- [Search the Result Set of a Previous Search](#search-the-results)
- [Use the Results of a Previous Search in a New Query](#reuse-query)
- [Rerun a Search](#rerun)
- [Automatically Reuse Search Results](#automatically-reuse-search-results)

You can access previous search results for as long as they're kept in the system. For retention details, see:

- [Manage Search Results](#manage)

### View the Cached Results of a Previous Search {#view}

You can quickly display the cached results of any search kept in [**History**](search-overview#history). This doesn't
consume any [credits](https://cribl.io/pricing/search/).

1. In Cribl Search, select **History**.
1. Select the row of the search you're interested in.

>This displays the cached results of the search.

> You can also select **View Results** in the [**Details**](search-details) of each search.
{.box .info}

### Search the Result Set of a Previous Search {#search-the-results}

You can quickly query the results of a previous search. This doesn’t [rerun](#rerun) the original query itself, but runs
an actual search on the result set, so can lead to minor credit consumption.

1. In Cribl Search, select **History**.
1. In the row of the search you're interested in, select the Actions ![](actions-dropdown.light.svg) button at the far right.
1. Select **Search the Results**.
 
> In a new window, Cribl Search runs a search on the result set, using the
   [`$vt_results`](vt_results) virtual table. You can now [modify the query](#reuse-query) for further
   analysis.

> You can also select **Search the Results** in the [**Details**](search-details) of each search. Read more about [virtual tables here](virtual-tables).
{.box .info}

### Reuse the Results of a Previous Search in a New Query {#reuse-query}

When writing a query, you can reference the result set of a previous search, using the [`$vt_results`](vt_results)
[virtual table](virtual-tables). This lets you treat the results as a regular Dataset.

While doing this doesn’t [rerun](#rerun) the original query itself, it runs an actual search on the result set, so can
lead to minor credit consumption.

Get the ID of the search you're interested in (you'll find it in the [**Details**](search-details) tab), and use the
`jobId` predicate:

```kusto
dataset="$vt_results" jobId="1704236905683.wgocax"
```

You can also load the results of multiple searches at once, for example:

```kusto
dataset="$vt_results"
 | where jobId > "1704236905600" and jobId < "1704236906000"
```

To reuse the results of a [saved](search-overview#saved) or [scheduled](scheduled-searches) search, add the search name
by using the `jobName` predicate. This will load the results of the latest execution of the saved search. For example:

```kusto
dataset="$vt_results" jobName="mySavedSearch"

// or, for example
dataset="$vt_results"
 | where jobName startswith "my"
```

To load a specific execution of a saved search, use the `execution` parameter, for example:

```kusto
// load the last run of mySavedSearch (default)
dataset="$vt_results" jobName="mySavedSearch" execution = 0

// load the run before last
dataset="$vt_results" jobName="mySavedSearch" execution = -1
```

For more information and examples, see the [`$vt_results`](vt_results) page.

### Rerun a Search {#rerun}

You can easily run a new search that uses exactly the same query text and settings as a previous search.

1. In Cribl Search, select **History**.
1. In the row of the search you're interested in, select the Actions ![](actions-dropdown.light.svg) button at the far right.
1. Select **Rerun**.

You can also select **Rerun** in the [**Details**](search-details) of each search.


> In certain cases, rerunning the search may not be necessary, and you'll find it faster and cheaper to
> [query the result set](#search-the-results) or simply [view the cached results](#view).
{.box .success}

### Automatically Reuse Search Results

When writing a query, you can allow Cribl Search to automatically reuse the results of analogous searches that were run
recently in your organization. This is especially useful for configuring [Dashboards](dashboards), or when you run the
same query multiple times, potentially producing the same results.

Cribl Search treats two searches as analogous when they share the same:

- [Search plan](search-details#search-plan): both searches can be broken down into the same set of pipelines. For
  example, `sort by x desc | limit 10` has the same search plan as `top 10 by x`.
- [Datasets](basic-concepts#datasets): both searches touch (and have access to) the same Datasets.
- [`set`](set) statements that can affect results.
- [Relative time range](cribl#time-syntax).
- [Sample rate](build-a-search#sampling).

When a search reuses previous results, the [**Details**](search-details#details) tab describes such a search as an
**alias job**, and provides the ID of the job that produced the original results. To hide potentially sensitive
information, you can't access the [logs](search-details#logs), [metrics](search-details#metrics), and
[diagnostics](search-details#diagnostics) of the alias job or the original job.

To allow for automatic reuse, at the beginning of your query add a [`set`](set) statement with the
`allow_previous_results` option set to a time interval.

For example, to allow Cribl Search to reuse results from the last 10 minutes, add the following statement:

```kusto
set allow_previous_results="10min";

// here, add your query
```

## Manage Search Results {#manage}

Search results are stored temporarily on your Organization's Leader. We use an industry-standard AES-256 algorithm to encrypt your data at rest.

You can control how long Cribl retains your search results at **Settings** > **Search** > **Limits** > **Search history TTL**. For details, see [Limits](limits).
