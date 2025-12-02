# .show queries


The `.show queries` command lists queued, running, or all pending (queued and running) searches. You can limit the list to only searches run by a specific user.

`.show queries` ignores the set [time range](build-a-search#time-range).

```kusto {runSearch=true}
// show all queued and running searches
.show queries
```

## Purpose

Use `.show queries` to [monitor](commands#view-searches) search status, track individual and overall search performance, and identify searches that are taking longer than expected. This command is especially useful in multi-user environments.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions |
| -------------------------------------------------------- | ----------- |
| Admin                                                    | Can see all searches in their Organization. |
| Editor or User                                           | Can see only those searches that they ran themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |

## Syntax

    `
.show [Status] queries
               [by user User]
               [with(reason = "InvestigationReason")]`




You can also combine this command with [operators](operators).


### Parameters

| Parameter             | Type               | Description |
| --------------------- | ------------------ | ----------- |
| `Status`              | [`string`](string) | The status of the queries to list:<ul><li>`queued`: Show only queued searches.</li><li>`running`: Show only running searches.</li><li>`all` or `*` (default): Show both queued and running searches.</li></ul> |
| `User`                | [`string`](string) | The names of the user whose queries you want to list. |
| `InvestigationReason` | [`string`](string) | The reason for running the `.show queries` command, which is added to the Cribl Search audit log. |

## Returns

Returns the [details](search-details#details) of the specified searches.

## Examples

Each of these queries demonstrates runnable syntax, but this command will return results only if you have pending queries in a state matching the `Status` parameter.

Show all running searches:

```kusto {runSearch=true}
.show running queries with(reason = "Checking what's taking so long.")
```

Show searches queued by specific users:

```kusto
.show queued queries by user "John*" with(reason = "Just checking.")
```

Show searches being run by specific users:

```kusto
.show running queries by user "John*" with(reason = "Thinking of resources.")
```
