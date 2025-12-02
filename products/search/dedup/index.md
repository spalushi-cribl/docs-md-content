# dedup


The `dedup` operator filters out duplicate events.

You need to specify the field that you want to inspect for duplicates (**FieldName**). When that field has the same value in multiple events, those events are considered duplicates.

You can also specify multiple fields. In that case, events are considered duplicates only if **all** of the specified fields are duplicated. For example, if you specify fields named `Phone` and `Email`, events are considered duplicates only if they have an identical `Phone` value **and** an identical `Email` value.

By default, the `dedup` operator outputs only the first duplicate it finds, and drops the rest. You can change this behavior by specifying the number of duplicates to keep (**NumberOfDuplicatesToKeep**).

When looking for duplicates, the `dedup` operator compares events timed within 30 seconds of each other. You can also specify a different time window (**TimeWindow**).

## Syntax

`Scope | dedup [time_window=TimeWindow] [num_duplicates=NumberOfDuplicatesToKeep] by FieldName [, ...]`

### Arguments

* **TimeWindow** ([int](int)): Sets a time interval, across event timestamps, over which this operator will apply the **NumberOfDuplicatesToKeep** limit on the specified **FieldName**(s). For event pairs outside this time interval, `dedup` will release the limit, allowing the next duplicate event to filter through to results. Default: `30` seconds.
* **NumberOfDuplicatesToKeep** ([int](int) or expression): The number of duplicate events to keep. Default: `1`.
* **FieldName**: The name of the field that you want to inspect for duplicates. Allowed formats: `fieldName` or `[“field name”]`. Separate multiple fields with a comma: `fieldName1, [“field name 2”], ...`.

## Results

Filters out events identified as duplicates.

## Examples

Filter out events that contain duplicate instances of a random number:

```kusto {runSearch=true}
dataset=$vt_dummy event<1000
| extend randomNumber=rand(10)
| dedup by randomNumber
```

Filter out events that have the same value in the `Name` field.

```kusto
dataset=myDataset
| dedup by Name
```

Filter out events that have the same values in the corresponding `Name`, `Home address`, and `Work address` fields.

```kusto
dataset=myDataset
| dedup by Name, ['Home address'], ["Work address"]
```

Filter out events that contain `Name` duplicates, and that were found within a minute of each other. If there are more than 5 such events, keep only the first 5.

```kusto
dataset=myDataset
| dedup time_window=60 num_duplicates=5 by Name
```
