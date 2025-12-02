# union


The `union` operator appends one set of results to another.

Results are automatically sorted by time in ascending order if a time field is present. Otherwise, the order will be
random.

## Syntax

Using a [`let`](let) statement:

```kusto
let SubqueryName = Subquery;

MainQuery | union SubqueryName
```

Using an [inline subquery](common-examples#inline-subqueries):

```kusto
MainQuery | union (
  Subquery
  )
```

### Arguments

- **SubqueryName**: The name for the **Subquery** expression. Spaces (` `) are not allowed.
- **Subquery**: The data to append to the **MainQuery**.
- **MainQuery**: The data to which the results of **Subquery** are appended.

## Rules

The union operation includes the first 50,000 events of **SubqueryName**. The remaining events are ignored.

## Examples

Combine two sets of data.

```kusto {runSearch=true}
let dataset1 = range x from 1 to 5 step 1 | extend dataField = x * 2;
range y from 6 to 10 step 1 | extend dataField = y * 3 | union dataset1;
```

Extend the results of three searches on the [`$vt_dummy`](vt_dummy) Dataset, each filtered with `event < 10`, and
combine them using the `union` operator.

```kusto {runSearch=true}
let stage1 = search in($vt_dummy) event<10 | extend foo=42;
let stage2 = search in($vt_dummy) event<10 | extend bar=24;
search in($vt_dummy) event<10 | extend baz=84 | union stage1, stage2;
```

Append data, using an [inline subquery](common-examples#inline-subqueries).

```kusto
// main query
print x = 1 | union (
  // inline subquery
  print y = 1 | extend y = 2
  )
```

