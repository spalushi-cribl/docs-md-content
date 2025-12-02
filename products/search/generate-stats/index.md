# .generate stats


The `.generate stats` command produces statistics about the results of a search.

```kusto
// produce statistics for the search 1749594773597.yzoXZy, if available to the current user
.generate stats job("1749594773597.yzoXZy")
```

## Purpose

A standard Cribl Search query generates two types of statistics about its result set:

- **Timeline stats**: Distribution of data over time, such as the number of events per time bucket, or the earliest and
  latest timestamps in the result set. You can see these stats in the [histogram](search-overview#histogram) of the
  search results.
- **Field stats**: Distribution of data across fields, such as the number of unique values in a field, or the minimum
  and maximum values of a field. You can see these stats in the [field browser](search-overview#browser) of the search
  results.

Certain types of searches (for example, Dashboard or scheduled searches) don't generate these statistics by default, to
optimize performance.

The `.generate stats` command allows you to generate these statistics for any search, without having to rerun the search
itself.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                                         |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Admin                                                    | Can generate stats for all searches in the Organization.                                            |
| Editor or User                                           | Can generate stats for [public](set#job_visibility) searches, or searches that they ran themselves. |

## Syntax

```
.generate [timeline|field|all] stats("SearchId") [force] [with(reason = "InvestigationReason")]
```

### Parameters

| Parameter                      | Required | Description                                                                                                                                   |
| ------------------------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `timeline` or `field` or `all` | No       | Type of statistics to generate. Use `timeline` for timeline stats, `field` for field stats, or `all` for both. If omitted, defaults to `all`. |
| `SearchId`                     | Yes      | The ID of the search for which to generate statistics. Doesn't support wildcards.                                                             |
| `force`                        | No       | Force the (re)generation of stats even if they were already generated.                                                                        |
| `Investigation Reason`         | No       | The reason for running the command, added to the Cribl Search audit log.                                                                      |

## Returns

Returns a message indicating whether the statistics were successfully generated or not.

If the search ID is invalid or the user doesn't have permission to access the search, returns an error message.

## Examples

Generate timeline statistics for a search, overwriting any existing stats.

```kusto
.generate timeline stats("1749594773597.yzoXZy") force with (reason="I have my reasons");
```
