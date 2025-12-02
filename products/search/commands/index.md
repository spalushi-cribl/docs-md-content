# Commands


Run commands, and [`set`](set)-statement options, from the query box to manage searches.

---

Cribl Search supports the following commands:

- [`.cancel`](cancel) – Stop queued, running, or all queued and running searches.
- [`.clear options`](clear-options) – Disable [`set`](set)-statement options.
- [`.generate stats`](generate-stats) – Produce statistics about the results of a search.
- [`.show objects`](show-objects) – List the contents of a Dataset before you search it.
- [`.show options`](show-options) – View [`set`](set)-statement options configured for your account.
- [`.show queries`](show-queries) – List queued, running, or all queued and running searches run by all users or
  specific users.

## Basic Command Usage {#basic}

Unlike functions and operators, commands start with a period. For example:

```kusto
.show objects
```

How commands work depends on your [Search Member Permissions](cloud-managing#search-member-roles). For example, **User**
Search Members can manage only their own searches, but **Admin** Search Members can manage the searches of all users in
the organization.

You can combine commands with [operators](operators). For example, to show queued queries that were created more than 10
minutes ago:

```kusto {runSearch=true}
.show queued queries
 | where timeCreated < ago(10min)
```

However, you can't use commands in [subqueries](common-examples#subqueries). For example, this won't work:

```kusto
// invalid example
let stage1 = .show objects(cribl_search_sample);
```

For more transparency, you can provide a reason why you're using a particular command. The reason will be added to the
Cribl Search audit log. For example:

```kusto {runSearch=true}
.cancel running queries with(reason = "Time is up.")
```

## Manage Searches with Commands

You can manage your or your users' searches straight from the query box, using the following commands:

- [`.show queries`](show-queries) – View searches based on their IDs, status, or the users running them.
- [`.cancel`](cancel) – Cancel queued or running searches.

### View Searches {#view-searches}

To display searches based on their IDs, status, or the users running them, use the [`.show queries`](show-queries)
command.

**User** and **Editor** [Search Members](cloud-managing#search-member-roles) can view only their own searches. **Admin**
Search Members can view the searches of all users in the organization.

To view all queued or running searches:

```kusto {runSearch=true}
.show queries
```

To view all queued searches:

```kusto {runSearch=true}
.show queued queries
```

To view searches that are being run by specific users:

```kusto {runSearch=true}
.show running queries by user "Jane*"
```

The results are not affected by the set [time range](build-a-search#time-range).

### Cancel Searches {#cancel-searches}

To stop queued or running searches, use the [`.cancel`](cancel) command.

**User** and **Editor** [Search Members](cloud-managing#search-member-roles) can cancel only their own searches.
**Admin** Search Members can cancel the searches of all users in the organization.

To cancel a specific search:

```kusto
.cancel query "1693827597495.ji5y5g"
```

To cancel searches that are queued or being run by specific users:

```kusto {runSearch=true}
.cancel queries by user("John Doe", "Jane*")
```

To cancel all currently running searches:

```kusto {runSearch=true}
.cancel running queries
```

The results are not affected by the set [time range](build-a-search#time-range).

## Manage Set-Statement Options with Commands

You can manage your or your users' [`set`](set)-statement options straight from the query box, using the following
commands:

- [`.clear options`](clear-options) – Disable [`set`](set)-statement options.
- [`.show options`](show-options) – View [`set`](set)-statement options configured for your account.

### View Set-Statement Options

To see [`set`-statement options](set#available-set-statement-options) configured for your account, use the
[`.show options`](show-options) command.

To view all options configured for you:

```kusto {runSearch=true}
.show options
```

To view only those options that are **not** overridden by other settings:

```kusto {runSearch=true}
.show active options
```

### Disable Set-Statement Options

To disable [`set`-statement options](set#available-set-statement-options), use the [`.clear options`](clear-options)
command.

To disable all options configured for your own account:

```kusto
.clear options
```

As an Admin [Search Member](cloud-managing#search-member-roles), to disable all options for all users in the usage
group:

```kusto
.clear global options
```
