# $vt_results


The `$vt_results` virtual table retrieves the result sets of searches completed (or attempted) in the past.

```kusto {runSearch=true}
// get the results of the search 1704236905683.wgocax, if available to the current user
dataset="$vt_results" jobId="1704236905683.wgocax"
```

## Purpose

Use `$vt_results` to access the [results of previously executed searches](search-results#reuse) without having to rerun
them. This can be particularly useful for iterative analysis or when you want to reference the results of multiple
searches in a single query.

Mind that although `$vt_results` doesn't rerun the actual query, it **searches** the results of the specified searches,
which can lead to minor [credit](https://cribl.io/pricing/search) consumption. This is different than
[opening a search from **History**](search-results#view), which only loads the cached results.

## Permissions and Limits

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                                                                                           |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Admin                                                    | Can access the results of all searches in the organization.                                                                                           |
| Editor or User                                           | Can access the results of only those searches that they ran themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |

You can access previous search results for as long as they're kept in the system. By default it's 7 days, but you can
change this in [**Settings**](settings) > **Search** > **[**Limits**](limits) > **Search history TTL**.

`$vt_results` cannot access the results of searches that are still running.

## Variations {#variations}

By default, `$vt_results` can access only the results of successfully completed searches. However, you precede this Dataset with the `set` statement's [`allow_incomplete_results`](set#allow_incomplete_results) option to also access the results of failed or canceled searches. This option is `false` by default. Set it to `true` using syntax like this:

```kusto
set allow_incomplete_results=true; 
dataset="$vt_results" jobId=<some-ID>
```

## Syntax

```kusto
dataset="$vt_results"
  // either:
  jobId="SearchId"
  // or:
  jobName="SavedSearchName" [execution > -NumberOfPreviousRuns]
```

### Parameters

| Name                   | Type                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SearchId`             | [`string`](string)                | The ID of the search whose results you want to access. You can find the ID of every search in its [**Details**](search-details#details), or in [**History**](search-overview#history).<br><br>For multiple IDs, use square brackets and commas:<br>`jobId=["SearchId1", "SearchId2"]`.                                                                                                                                                  |
| `SavedSearchName`      | [`string`](string)                | The **name** (not ID) of the [saved search](search-overview#saved) whose results you want to access. By default, you'll get the results of the latest execution that saved search, unless you use the the `execution` parameter.<br><br>For multiple names, use square brackets and commas, for example:<br>`jobName=["SavedSearchName1", "SavedSearchName2"]`.                                                                         |
| `NumberOfPreviousRuns` | [`int`](int)<br><br>or<br><br>`*` | Which of the past executions of the saved search to load. If not specified, only the latest run is loaded.<br><br>`execution = 0` (default) loads the last run.<br>`execution = -1` loads the run before last.<br>`execution > -1` loads the last two runs together.<br>`execution > -2` loads the last three runs together.<br>`execution = *` loads all runs kept in history.<br><br>Works only with the `SavedSearchName` specified. |

## Returns

Returns the results of the specified searches, in their original format.

## Examples

### Get the Results of Multiple Previous Searches {#multiple}

Get the results of all completed searches whose IDs are between `1704236905600` and `1704236906000`.

```kusto {runSearch=true}
dataset="$vt_results"
  | where jobId>"1704236905600" and jobId<"1704236906000"
```

### Get the Results of a Previous Run of a Saved Search {#previous-run}

Get the results of the last run of a saved search named `mySavedSearch`.

```kusto {runSearch=true}
dataset="$vt_results" jobName="mySavedSearch" execution = 0

// or simply:

dataset="$vt_results" jobName="mySavedSearch"
```

Get the run before last:

```kusto {runSearch=true}
dataset="$vt_results" jobName="mySavedSearch" execution = -1
```

### Get the Results of Multiple Previous Runs of a Saved Search {#multiple-previous-runs}

Get the results of the last two executions of a saved search named `mySavedSearch`.

```kusto {runSearch=true}
dataset="$vt_results" jobName="mySavedSearch" execution > -1
```

Get the last three runs:

```kusto {runSearch=true}
dataset="$vt_results" jobName="mySavedSearch" execution > -2
```
