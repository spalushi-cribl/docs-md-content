# Cribl Search 4.12.2


Cribl Search 4.12.2 enables you to run Lakehouse-speed searches against multiple Datasets at once, speeds up Amazon S3
searches with Splunk SmartStore Partitioning, brings new and updated Packs, and includes numerous corrections.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl has deprecated the [Dataset Acceleration](acceleration) feature and removed it from this release. We invite you to
explore [Cribl Lakehouses](/lake/lakehouse) as a new option to speed up searches on designated Datasets. Please continue
to report Dataset Acceleration issues through normal Cribl [support channels](/support/), but assistance for this
deprecated feature might be limited.

## New Features

This release adds the following new capabilities.

### Multiple-Dataset Lakehouse Searches

A query against **multiple** [Lakehouse-assigned Datasets](set-up-cribl-lake#search-lakehouse) can now run at Lakehouse
speed (before, the Lakehouse performance boost worked only with **one** Dataset at a time).

Beyond the Lakehouse assignment, your query must also meet one of these conditions:

- Include only operators from the following group: [`cribl`](cribl), [`centralize`](centralize), [`extend`](extend),
  [`extract`](extract), [`foldkeys`](foldkeys), [`limit`](limit), [`mv-expand`](mv-expand), [`pivot`](pivot),
  [`project`](project), [`project-away`](project-away), [`project-rename`](project-rename), [`search`](search), and
  [`where`](where).

- Or, if you use any other operators, insert the [`centralize`](centralize) or [`limit`](limit) operator before them.
  (Result counts will be further constrained by [usage group](usage-groups#usage-limits) limits.)

If neither of the above conditions is met, or if your query includes non-Lakehouse Datasets, the search will run at
standard speed.

### Faster S3 Searches with Splunk SmartStore Partitioning

When you configure an Amazon S3 Dataset with [Splunk SmartStore partitioning](set-up-s3#partitioning-scheme), a new
**Advanced** section enables you to define a retrospective **Time range** over which searches will be sped up.

## Packs

This release includes new and updated Packs:

- Cribl Search Edge Monitor (new)
- [Cribl Search Sysdig](https://packs.cribl.io/packs/cribl-search-sysdig) (updated)

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| **ID**                                                                                                                              | **Description**                                                                                                                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px">SEARCH-10174<br>[Known Issue](https://docs.cribl.io/results/?query=10174&searchOption=known-issues)</div> | Search Members with the `User` Permission can again see saved searches that they created, or to which they were given access.                                                                                                                                                                               |
| SEARCH-10157<br>[Known Issue](https://docs.cribl.io/results/?query=10157&searchOption=known-issues)                                 | Search Members with the `Editor` Permission can again see Cribl Search [Packs](packs#who) and their contents.                                                                                                                                                                                               |
| SEARCH-9497<br>[Known Issue](https://docs.cribl.io/results/?query=9497&searchOption=known-issues)                                   | The [`week_of_year`](week-of-year) function now properly follows the [ISO standard](https://en.wikipedia.org/wiki/ISO_8601#Week_dates) for weekdays. All edge cases work properly now (for example, early dates in years starting after Thursday are no longer counted as the last week of the prior year). |
| SEARCH-10017                                                                                                                        | Names of lookup files can now include whitespaces. To reference these lookups, surround their file names with quotes. For example: `lookup "foo bar" on baz`.                                                                                                                                               |
| SEARCH-10254                                                                                                                        | The [`lookup`](lookup) operator now supports file extensions in lookup filenames. For example, `lookup foo.csv on bar` now works with no error.                                                                                                                                                             |
| SEARCH-8651                                                                                                                         | The [`datetime()`](datetime) function now supports quotes, as documented. For example, `datetime(2022-10-14)` now works the same as `datetime("2022-10-14")`.                                                                                                                                               |
| SEARCH-8592                                                                                                                         | When passed an object, the [`tostring()`](tostring) function now properly returns a string with the object’s JSON representation. Before, the function returned `[object Object]`.                                                                                                                          |
| SEARCH-9692                                                                                                                         | Scheduled searches using the "local timezone" option are no longer incorrectly interpreted as UTC.                                                                                                                                                                                                          |
| SEARCH-10050                                                                                                                        | The [`send`](send) operator now properly records the URL of the Cribl Stream Worker Group to which it sent the data. Before, search logs always indicated that the data was sent to the `default` Group, even though it was correctly sent to the Group configured by the `group` parameter.                |
| SEARCH-7350                                                                                                                         | The [`$vt_results`](vt_results) virtual table now correctly displays result sets of more than 10,000 events.                                                                                                                                                                                                |
| SEARCH-10204                                                                                                                        | Clicking **Open in Search** in a Dashboard scheduled search now always opens the last run of the search, as intended. Before, in certain cases, it re-ran the search.                                                                                                                                       |
| LAKE-868                                                                                                                            | Cribl Search now returns results, as intended, for DDSS-formatted Cribl Lake Datasets populated using [Direct Access](/lake/direct-access).                                                                                                                                                                 |

See also corrections in the
[Cribl Lake Release Notes](https://docs.cribl.io/lake/release-notes/release-v4122#corrections).
