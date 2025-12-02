# project-away



The `project-away` operator excludes specific fields from the results.

## Syntax

    `Scope | project-away ColumnNameOrPattern [, ...]`

### Arguments

* **Scope**: The events to search.
* **ColumnNameOrPattern**: The name of the field or field wildcard-pattern to be removed from the results.
    * Wildcards `*` represent zero or more characters in a field name. For example, `Sta*`.
    * Wildcards are not supported in the middle of a field name and consecutive wildcards are not allowed. For example, `St*us` and `S**s` are invalid.

## Results

A table with fields that were not named as arguments. Contains the same number of rows from the data piped to the operation.

## Example

```kusto
dataset=myDataset
| project-away price, quantity, zz*
```

Exclude all fields that start with the letter "p".

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| project-away p*
```

