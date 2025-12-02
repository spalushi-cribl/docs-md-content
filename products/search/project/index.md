# project



The `project` operator keeps only the fields specified, and can also rename fields and insert new computed fields.

The order of the [arguments](#arguments) determines the order of the fields in the result.

See also [`extend`](extend) and [`print`](print).

> Unlike [`project-rename`](project-rename), `project` drops all fields that were not specified.
{.box .info}

## Syntax

* `Scope | project FieldName [= Expression] [, ...]`

* `Scope | project [FieldName | (FieldName[,]) =] Expression [, ...]`

### Arguments

* **Scope**: The events to search.
* **FieldName**: Optional if there's an **Expression**. The name of the field to appear in the output. If omitted, the name will be generated.
    * If there is no **Expression**, then **FieldName** is mandatory and a field of that name must appear in the input. 
    * If **Expression** returns more than one field, a list of field names can be specified in parentheses. In this case **Expression**'s output fields will be given the specified names, dropping all the rest of the output fields, if there are any. If a list of the field names is not specified, all **Expression**'s output fields with generated names will be added to the output.
* **Expression**: Optional if there's a **FieldName**. A scalar expression referencing the input fields.

> You can return a new calculated field with the same name as an existing field in the input.
>
{.box .info}

## Results

Returns a table that has the fields named as arguments, and as many rows as the input table.

## Examples

Show only two fields, `cost` and `price`.

```kusto
dataset=myDataset
| project cost=price*quantity, price
```

The following example shows several kinds of manipulations that can be done using the `project` operator. The Dataset has three fields of type `int`: `A`, `B`, and `C`.

```kusto
dataset=myDataset
| project
    X=C,                       // Rename field C to X
    A=2*B,                     // Calculate a new field A from the old B
    C=strcat("-",tostring(C)), // Calculate a new field C from the old C
    B=2*B                      // Calculate a new field B from the old B
```

You can also use `project` with nested data, using dot notation. In the following example, two new fields (`user` and `query`) are created based on an existing nested field (`stats`).

```kusto {runSearch=true}
dataset="cribl_internal_logs" message="search finished"
| project _time, user=stats.userDisplayName, query=stats.query // show only the `_time` field, plus two calculated fields: `user` and `query`
```

Show only `_time`, `event`, and `parity` fields.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| project _time, event, parity 
```

Show `_time`, `event`, and `parity` fields and evaluate two new fields, `foo` and `bar`.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| project _time, event, parity, foo=42, bar=foo*2
```

