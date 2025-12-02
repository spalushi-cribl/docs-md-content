# pivot


The `pivot` operator turns each unique field **value** into a field **name** (rendered as a column header).

## Syntax

`Scope | pivot DataFields over ColumnFields [by LabelField]`

### Arguments

* **Scope**: The events to search.
* **DataFields**: One or more fields to populate the cells of the output table. Wildcards are not supported for field names.
* **ColumnFields**: One or more fields whose unique **values** become column headers (field **names**).
* **LabelField**: An additional field added to the left-most column of the output table, used to label the rows (events).
  If not specified, defaults to [`_time`](search-overview#built-in).

> You can specify nested fields by using dot notation (`foo.bar`).
{.box .info}

## Results

For each unique **value(s)** combination of the **ColumnFields**, `pivot` returns a column whose:

* Column header (field name) is taken from that **ColumnFields** value(s).
* Column cells (field values) are populated by the **DataFields**, calculated separately for each returned event.

## Examples

Turn each unique value of the `colLabel` field (`Red`, `Blue`, and `Green`) into a column,
and populate the columns' cells with random 1-100 numbers:

```kusto {runSearch=true}
dataset=$vt_dummy event < 10
| extend dataField = floor(rand() * 100),
         colLabel = case(event % 3 == 0, 'Red', event % 3 == 1, 'Blue', 'Green')
| pivot dataField over colLabel by event
```

Perform a pivot on two fields, `colLabel1` and `colLabel2`:
 
```kusto {runSearch=true}
dataset=$vt_dummy event < 10
| extend dataField1 = floor(rand() * 100),
         dataField2 = 200 - dataField1,
         colLabel1 = case(event % 3 == 0, 'Red', event % 3 == 1, 'Blue', 'Green'),
         colLabel2 = iif(event % 2 == 0, 'yes', 'no')
| pivot dataField1, dataField2 over colLabel1, colLabel2 by event
```

