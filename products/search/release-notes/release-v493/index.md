# Cribl Search 4.9.3


With Cribl Search 4.9.3, you gain access to new string operators, wider support for regex literals, simpler time
extraction in Cribl Lake exports, and more.

## New String Operators: `!has_all` and `!has_any`

The existing [`has_all`](has-all) and [`has_any`](has-any) string operators now have their negative counterparts:
[`!has_all`](not-has-all) and [`!has_any`](not-has-any).

## `matches regex` Gets Support for Regex Literals

In addition to strings (`"K.\*S"`), the [`matches regex`](matches-regex) operator now also accepts ECMA-style regular
expressions (`/K.*S/`). This allows you to use flags, for example, to specify case sensitivity:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=*
 | limit 1000
 | where method matches regex /^po.*/i
```

## New History Filtering

On the Search Home page, a new drop-down enables Search Admins to filter the displayed history by **My History** (the
default) versus **All History**.

## Simpler Time Extraction For Cribl Lake Events

When [exporting from Cribl Search to Cribl Lake](search-cribl-lake#export), `_time` data is now handled more predictably and consistently:

- Since `_time` is handled automatically, [Datatype timestamp settings](datatypes#timestamp-settings) no longer
  apply to Cribl Lake events.
- If the source event has no `_time` field, Cribl Lake adds this field, setting its value to [`now()`](now) at write time.

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| Reference                                                                                       | Description                                                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px">SEARCH-8016<br>[Known issue](known-issues#SEARCH-8016)</div>          | When configuring an Amazon S3 or AWS API Dataset, you can now select the correct Region from a drop-down.<br>(You no longer need to type in the exact AWS Region name, with no validation or error message.)      |
| SEARCH-7100<br>[Known issue](known-issues#SEARCH-7100)                                          | Empty prefixes in path definitions no longer cause Cribl Search to return the same results multiple times.                                                                                                        |
| SEARCH-8158                                                                                     | Cribl Search no longer logs spurious warnings about objects supposedly being skipped during search.                                                                                                               |
| SEARCH-8004                                                                                     | Improved the accuracy of past searches' status, as displayed on the **History** tab.                                                                                                                              |
| SEARCH-8011                                                                                     | Improved the [field browser](search-overview#browser)'s display of very long field values.                                                                                                                        |
| SEARCH-7900                                                                                     | Configuring a Generic HTTP API Dataset Provider now enforces filling out the mandatory **Name** tab.                                                                                                              |
| SEARCH-7917                                                                                     | Corrected users' inability to run any searches when the [concurrent scheduled search limit](usage-groups#usage-limits) ([`max_scheduled_searches_per_user`](set#max_scheduled_searches_per_user)) was set to `0`. |
| SEARCH-8138                                                                                     | The list of default Datasets is now editable.                                                                                                                                                                     |
| SEARCH-7966                                                                                     | Corrected the [`bin`](bin) function's rounding of values.                                                                                                                                                         |
| SEARCH-4175                                                                                     | Search Admins now have full access to editing Usage Groups.                                                                                                                                                       |
