# count



The `count` operator returns the number of all input events.

> `count` is a short form of `summarize count=count()`.
>
{.box .info}

## Syntax

    `Scope | count`

### Arguments

- **Scope**: The events to search.

## Results

Returns a single record and column of type long. The value is the number of records in the scope.

## Example

- Count events where `status=200` and `response_time>2`.

```kusto
dataset=myDataset status=200 response_time>2
| count
```

- Count the number of events.

```kusto {runSearch=true}
dataset=$vt_dummy event<10 
| count
```

- Show the number of search jobs by their status and the user that created it. [`$vt_jobs`](vt_jobs) requires **Admin** [Permissions](cloud-managing#search-member-roles).

```kusto {runSearch=true}
dataset=$vt_jobs
| summarize jobs=count() by status, user
```

