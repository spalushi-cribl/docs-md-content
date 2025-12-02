# distinct



The `distinct` operator identifies unique values within each of the specified fields. It's used to retrieve distinct values for each field independently. `distinct` finds unique values within each provided field separately, helping you to analyze and explore data uniqueness within individual fields.

## Syntax

    `Scope | distinct [ MaxCombinations ] [ MaxDepth ] FieldName [, ...]`

### Arguments

* **Scope**: The events to search.
* **MaxCombinations**: The maximum number of distinct combinations. Defaults to `10000`.
* **MaxDepth**: The maximum depth for searching unique combinations of field values. This controls how deep the search goes when identifying distinct combinations within nested or hierarchical data structures. Defaults to `15`.
* **FieldName**: One or more fields you want to find distinct values for. This operator allows you to identify unique values within the specified field(s) from your Dataset. A wildcard `*` indicates all available fields.

## Examples

Find the distinct values for all fields in your Dataset.

```kusto
  dataset=myDataset
  | distinct *
```

Find unique values within the `host` and `port` fields.

```kusto
  dataset=myDataset
  | distinct host, port
```

```kusto {runSearch=true}
dataset=$vt_dummy event<1000
| extend randomNumber=rand(10)
| distinct randomNumber
```

