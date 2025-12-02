# project-rename


The `project-rename` operator renames fields.

> Unlike [`project`](project), `project-rename` keeps all fields that were not specified.
{.box .info}

## Syntax

    `Scope | project-rename NewFieldName = ExistingFieldName [, ...]`

### Arguments

* **Scope**: The events to search.
* **NewFieldName**: The new field name.
* **ExistingFieldName**: The name of the existing field to rename.

## Results

A table that has the fields in the same order as in an existing table, with its fields renamed.

## Example

```kusto
dataset=myDataset
| project-rename new_b=b, new_a=a
```

Rename the field `parity` to `oddEven`.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| project-rename oddEven=parity
```

