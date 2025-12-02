# .cancel


The `.cancel` command stops queued, running, or all queued and running searches.

`.cancel` ignores the set [time range](build-a-search#time-range).

```kusto {runSearch=true}
// cancel all running searches
.cancel running queries with(reason = "Time is up.")
```

## Purpose

Use `.cancel` to manage system resources and optimize performance by terminating queries that are no longer needed, taking too long, consuming too many resources, or made in error.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                    |
| -------------------------------------------------------- | ---------------------------------------------- |
| Admin                                                    | Can cancel all searches in their Organization. |
| Editor or User                                           | Can cancel only their own searches.            |

## Syntax

    `
.cancel query SearchId
              [with(reason = CancellationReason)]`

Or:

```kusto
.cancel Status queries
               [by user (User[, ...])]
               [with(reason = CancellationReason)]
```




You can also combine this command with [operators](operators).


### Parameters

| Parameter            | Type               | Description |
| -------------------- | ------------------ | ----------- |
| `SearchId`           | [`string`](string) | The ID of the search to cancel. |
| `CancellationReason` | [`string`](string) | The reason for running the `.cancel` command, which is added to the Cribl Search audit log. |
| `Status`             | [`string`](string) | The status of the queries to cancel:<ul><li>`queued`: Cancel only queued searches.</li><li>`running`: Cancel only running searches.</li><li>`all` or `*`: Cancel both queued and running searches.</li></ul> | |
| `User`               | [`string`](string) | The name of the user whose searches you want to cancel. |

## Returns

Returns information about the canceled searches.

## Examples

Cancel a specific search.

```kusto
.cancel query "1693827597495.ji5y5g"
```

Cancel searches queued or being run by specific users.

```kusto
.cancel queries by user("John Doe", "Jane*") with(reason = "I don't need these searches.")
```

Cancel all searches started by users whose names start with `John` or `Jane`.

```kusto {runSearch=true}
.cancel queries by user("John*", "Jane*") with(reason = "Cleaning up.")
```

Cancel queued searches that were created more than 10 minutes ago.

```kusto {runSearch=true}
.cancel queued queries
 | where timeCreated < ago(10min)
```

