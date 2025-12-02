# set


The `set` statement configures options that affect one or more searches.

```kusto {runSearch=true}
// return no more than 1000 results in this search
set max_results_per_search=1000;
```

## Purpose

Use `set` statements to control search behavior. For example, you can configure
[how long the current search can run](#max_running_time_per_search) before getting canceled, or set the default
[maximum time range](#max_timerange_width) for each of your searches.


> Use `set` statements carefully and always make sure you understand the option that you want to change. If in doubt,
> read the docs, or contact the Cribl Support team.
{.box .warning}

To see which `set`-statement options affect your account at the moment, use the [`.show options`](show-options) command.

To disable `set`-statement options, use the [`.clear options`](clear-options) command.

## Syntax

    `
set [Scope:]OptionName=OptionValue [, ...];`

Each `set` statement needs to end with a semicolon `;`.

### Parameters

| Parameter     | Description                                                                                                                                                                                                                            |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Scope`       | The [scope](#scope-of-set-statements) to which the option will apply. Can be either `user:` or `global:` (only when set by Admin [Search Members](cloud-managing#search-member-roles)). When omitted, affects only the current search. |
| `OptionName`  | The name of the [option](#options) to set.                                                                                                                                                                     |
| `OptionValue` | The value of the [option](#available-set-statement-options) to set.                                                                                                                                                                    |

## Scope of Set Statements

You can apply `set` statements in the following scopes:

| Scope         | Example                                   | Impact                                                                                                                    | Can be set by                                              |
| ------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Single search | `set max_results_per_search=1000;`        | Applies only to the current search.<br>Depending on the permissions, can override user settings and Usage Group settings. | All [Search Members](cloud-managing#search-member-roles)   |
| `user:`       | `set user:max_results_per_search=1000;`   | Applies to every search by the current user.<br>Depending on the permissions, can override Usage Group settings.          | All [Search Members](cloud-managing#search-member-roles)   |
| `global:`     | `set global:max_results_per_search=1000;` | Applies to every search by every member of the [Usage Group](usage-groups).                                               | Admin [Search Members](cloud-managing#search-member-roles) |

Depending on your permissions and the [options](#available-set-statement-options) specified, `set` statements can also
override certain Cribl Search [settings](search-tools), [limits](limits), and [Usage Group](usage-groups)
configurations.


> A single `set` statement can apply multiple options to multiple scopes at once. For example:
>
> ```kusto
> set global:max_executors=10, user:max_bytes_read_per_search=536870912, max_results_per_search=100000;
> ```
{.box .info}

## Returns

Search results are returned as usual, but with the specified settings applied.

The settings that were applied to a search are always logged in that search's [details](search-details#details), under
**set options**.

## Examples

Set the maximum number of results to 100,000.

```kusto {runSearch=true}
set max_results_per_search=100000;

dataset="cribl_internal_logs"
```

If an analogous search ran within the last 10 minutes,
[automatically reuse](search-results#automatically-reuse-search-results) its results instead of running the same search
again.

```kusto {runSearch=true}
set allow_previous_results="10min";

dataset="cribl_internal_logs" method status
| summarize count() by method, status
```

{#available-set-statement-options}
## Available Set-Statement Options {#options}

The `set` statement accepts the following options.

### allow_incomplete_results {#allow_incomplete_results}

Enable querying uncompleted search jobs.

When you set this property to `true`, the [`$vt_results`](vt_results) virtual table will enable querying the results of jobs that ended in `failed` or `canceled` status. (By default, `$vt_results` allows reading only successfully completed jobs.) 

When you set this property to `true`, the output will contain a `jobStatus` property, in addition to the original data fields. Usage example:

```kusto
set allow_incomplete_results=true; 
dataset="$vt_results" jobId=<some-ID>
```

You can set this option to one of the following:

| Setting | Behavior |
| ------- | -------- |
| `false` or `"no"` or `0` (default) | Disable querying `failed` or `canceled` search results. |
| `true` or `"yes" or `1`            | Enable querying `failed` or `canceled` search results. |

### allow_previous_results

[Automatically reuse](search-results#automatically-reuse-search-results) the results of analogous searches that were
run in the specified time range.

You can set this option to a time [string](string) such as `1h` (1 hour), `10min` (10 minutes), or `10` (10 seconds).

```kusto
// allow this search to reuse analogous search results from the last 10 minutes
set allow_previous_results="10min";

// allow my every search to reuse analogous search results from the last 10 minutes
set user:allow_previous_results="10min";

// (admin only) allow every user to reuse analogous search results from the last 10 minutes
set global:allow_previous_results="10min";
```

### disable_previews {#disable_previews}

Disable generating preview of search results.

Certain Cribl Search functions generate preliminary results that get replaced as the search scans more data. This way,
you quickly can see initial search results. The `disable_previews` option suppresses generating those previews, which,
in certain scenarios, may speed the overall search up and get you the final results faster.

You can set this option to one of the following:

| Setting                            | Behavior                                                 |
| ---------------------------------- | -------------------------------------------------------- |
| `false` or `"no"` or `0` (default) | If possible, generate the preview of the search results. |
| `true` or `"yes"` or `1`           | Don't generate the preview of the search results.        |

For example:

```kusto
// in this search, don't generate the preview of the results
set disable_previews=true;

// in all your searches, don't generate previews
set user:disable_previews=1;

// (admin only) disable previews for all searches run by any user
set global:disable_previews="yes";
```

### job_visibility

Set the default visibility of the search and its results.

You can also [change the visibility](sharing#share-search-results) of a search in **Actions** > **Results Access**.

You can set this option to one of the following:

| Setting             | Who can see the search and its results                                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `private` (default) | Only you and the [Search Admin](cloud-managing#search-member-roles). Same as [**Private**](sharing#share-search-results). |
| `public`            | Everyone in your organization. Same as [**All Members**](sharing#share-search-results).                                   |

For example:

```kusto
// make this search visible only to you and the Search Admin
set job_visibility="private";

// make all your searches visible to everyone
set user:job_visibility="public";

// (admin only) make all users' searches visible to everyone
set global:job_visibility="public";
```

### lakehouse​

When querying a Cribl Lake Dataset linked to a [Lakehouse](/lake/lakehouse), define whether the search should use the
Lakehouse or not.

| Setting        | Behavior                                                   |
| -------------- | ---------------------------------------------------------- |
| `on` (default) | Use only Lakehouse-cached data.                            |
| `off`          | Don't use Lakehouse data. Run the search at regular speed. |

For example:

```kusto
// query the Lake Dataset using Lakehouse caching
set lakehouse="on";

// query the Dataset without Lakehouse caching
set lakehouse="off";
```

### max_bytes_read_per_search

Maximum number of bytes that can be read per search. When the limit is exceeded, the search gets canceled.

You can set this option to an [int](int) (as bytes), for example:

```kusto
// allow this search to read up to 512MB of data
set max_bytes_read_per_search=536870912;

// allow my every search to read up to 512MB of data
set user:max_bytes_read_per_search=536870912;

// (admin only) allow every user to read up to 512MB of data per search
set global:max_bytes_read_per_search=536870912;
```

Depending on your permissions, this option can override the [**Byte limit**](usage-groups#usage-limits) of the matching
[Usage Group](usage-groups):

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                        |
| -------------------------------------------------------- | -------------------------------------------------- |
| Admin                                                    | Can increase or decrease the **Byte limit** value. |
| Editor or User                                           | Can only decrease the **Byte limit** value.        |

### max_earliest_relative

How far back in time the search can go.

You can set this option to a negative time [string](string), for example: `-1h` (1 hour), `-10min` (10 minutes), or
`-10` (10 seconds).

```kusto
// allow this search to scan data as old as 90 days
set max_earliest_relative=-90d;

// allow my every search to scan data as old as 90 days
set user:max_earliest_relative=-90d;

// (admin only) allow all users to scan data as old as 90 days
set global:max_earliest_relative=-90d;
```

Depending on your permissions, this option can override the
[**Earliest relative time range**](usage-groups#usage-limits) of the matching [Usage Group](usage-groups).

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                 |
| -------------------------------------------------------- | --------------------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **Earliest relative time range**. |
| Editor or User                                           | Can only decrease the value of the **Earliest relative time range**.        |

### max_executors

Maximum number of executors that the search can use.


> More executors don’t necessarily cause better performance.
> Sometimes, more executors can lead to an overload of the coordinator, slowing search execution down.
{.box .info}

You can set this option to an [int](int) or to the string `"auto"`.

With `max_executors="auto"`, the search starts with a few executors and then dynamically scales these up or down based
on collected throughput metrics. In the [**Details**](https://docs.cribl.io/search/search-details) of the search, you'll
see an additional [metric](https://docs.cribl.io/search/search-details/#metrics) showing the **average** number of
executors.

```kusto
// allow this search to invoke up to 5 executors
set max_executors=5;
// automatically adjust the number of executors for this search
set max_executors="auto";

// allow my every search to invoke up to 5 executors
set user:max_executors=5;
// automatically adjust the number of executors for my every search
set user:max_executors="auto";

// (admin only) allow every user to invoke up to 5 executors per search
set global:max_executors=5;
// (admin only) automatically adjust the number of executors for all users
set global:max_executors="auto";
```

Depending on your permissions, this option can override the [**Executors limit**](usage-groups#usage-limits) of the
matching [Usage Group](usage-groups).

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                    |
| -------------------------------------------------------- | -------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **Executors limit**. |
| Editor or User                                           | Can only decrease the value of the **Executors limit**.        |

### max_results_per_search

Maximum number of events returned by the search.

You can set this option to an [int](int) between `1` and `10000000`, for example:

```kusto
// allow this search to store up to 100,000 result events
set max_results_per_search=100000;

// allow my every search to store up to 100,000 result events
set user:max_results_per_search=100000;

// (admin only) allow every user to store up to 100,000 result events per search
set global:max_results_per_search=100000;
```

Depending on your permissions, this option can override the [**Results limit**](usage-groups#usage-limits) of the
matching Usage Group. For details, see: [Set the Limits of Search Results](search-tools#limits).

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| Admin                                                    | Can increase or decrease the value of the **Results limit**. |
| Editor or User                                           | Can only decrease the value of the **Results limit**.        |

### max_running_time_per_search

How long the search can run. When the limit is exceeded, the search gets canceled.

You can set this option to an [int](int) (as seconds), for example:

```kusto
// allow this search to run for up to 1 hour
set max_running_time_per_search=3600;

// allow my every search to run for up to 1 hour
set user:max_running_time_per_search=3600;

// (admin only) allow every search by every user to run for up to 1 hour
set global:max_running_time_per_search=3600;
```

Depending on your permissions, this option can override the [**Running time limit**](usage-groups#usage-limits) of the
matching [Usage Group](usage-groups).

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                       |
| -------------------------------------------------------- | ----------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **Running time limit**. |
| Editor or User                                           | Can only decrease the value of the **Running time limit**.        |

> When a search does exceed the `max_running_time_per_search`, it can take up to an additional 10 seconds for the job to fully terminate. This ensures proper collection of internal logs and metrics during a graceful shutdown.
{.box .info}

### max_scheduled_searches_per_user

Maximum number of concurrent [scheduled](scheduled-searches) searches per user. If the user tries to execute more
scheduled searches than specified by this limit, the searches get queued.

You can set this option to an [int](int), for example:

```kusto
// allow yourself to run up to 2 scheduled searches at once
set max_scheduled_searches_per_user=2;
// or:
set user:max_scheduled_searches_per_user=2;

// (admin only) allow every user to run up to 2 scheduled searches at once
set global:max_scheduled_searches_per_user=2;
```

Depending on your permissions, this option can override the
[**User concurrent scheduled search limit**](usage-groups#usage-limits) of the matching [Usage Group](usage-groups):

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                           |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **User concurrent scheduled search limit**. |
| Editor or User                                           | Can only decrease the value of the **User concurrent scheduled search limit**.        |

### max_searches_per_user

Maximum number of concurrent ad-hoc searches per user. If the user tries to execute more queries than specified by this
limit, the searches get queued.

You can set this option to an [int](int), for example:

```kusto
// allow yourself to run up to 3 searches at the same time
set max_searches_per_user=3;
// or:
set user:max_searches_per_user=3;

// (admin only) allow every user to run up to 3 searches at the same time
set global:max_searches_per_user=3;
```

Depending on your permissions, this option can override the
[**User concurrent ad hoc search limit**](usage-groups#usage-limits) of the matching [Usage Group](usage-groups):

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                        |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **User concurrent ad hoc search limit**. |
| Editor or User                                           | Can only decrease the value of the **User concurrent ad hoc search limit**.        |

### max_timerange_width

Maximum [time range](build-a-search#time-range) (the value between `earliest` and `latest`) that a single search can
cover.

You can set this option to a time [string](string) such as `1h` (1 hour), `10min` (10 minutes), or `10` (10 seconds).

```kusto
// allow this search to scan up to 2 hours of data
set max_timerange_width=7200;

// allow my every search to scan up to 2 hours of data
set user:max_timerange_width=7200;

// (admin only) allow every user to scan up to 2 hours of data per search
set global:max_timerange_width=7200;
```

Depending on your permissions, this option can override the [**Time range limit**](usage-groups#usage-limits) of the
matching [Usage Group](usage-groups).

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                     |
| -------------------------------------------------------- | --------------------------------------------------------------- |
| Admin                                                    | Can increase or decrease the value of the **Time range limit**. |
| Editor or User                                           | Can only decrease the value of the **Time range limit**.        |



### usage_group

Override the current [Usage Group](usage-groups) limits with the limits of another Usage Group.

Only Admin [Search Members](cloud-managing#search-member-roles) can use this option.

You can set this option to a [string](string) (the name of the Usage Group to use), for example:

```kusto
// run this search as a Member of `someOtherGroup`
set usage_group="someOtherGroup";
```
