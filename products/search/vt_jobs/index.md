# $vt_jobs


The `$vt_jobs` virtual table lists previously executed searches.

```kusto {runSearch=true}
// returns a list of all searches that were run within the past hour and are available to the current user
dataset="$vt_jobs" earliest=-1h
```

## Purpose

Use `$vt_jobs` for troubleshooting or performance analysis, or to monitor search activity in your organization.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                                                                         |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Admin                                                    | Can see all searches in their organization.                                                                                         |
| Editor or User                                           | Can see only those searches that they ran themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |

## Syntax

```kusto
dataset="$vt_jobs"
```

## Returns

Returns one event for every search run within the specified [time range](build-a-search#time-range), filtered through
any additional [criteria](build-a-search) you specify.

Each event contains fields with the search's [details](search-details#details).

## Examples

Aggregate searches from the past seven days by their status.

```kusto {runSearch=true}
dataset="$vt_jobs" earliest=-7d
  | summarize count() by status
```

