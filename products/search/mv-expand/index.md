# mv-expand



Expand an array or object within an event into multiple events, where each of the generated events contains a single value of the original array or object. If you're curious, mv stands for multiple value.

## Syntax

* `Scope | mv-expand [ bagexpansion=ExpansionMode ] [ with_itemindex=IndexFieldName ] FieldName [, FieldName... ] [ limit Rowlimit ]`

* `Scope | mv-expand [ bagexpansion=ExpansionMode ] [ Name=NewName ] ArrayExpression [, [ Name=NewName ] ArrayExpression... ] [ limit Rowlimit ]`

### Arguments

|Name|Type|Required|Description|
|--|--|--|--|
|**ExpansionMode**|string|No|`bag` (default) expands into single-entry property bags <br /><br />`array` expands into two-element `[key,value]` array structures, allowing uniform access to keys and values.|
|**FieldName**, **ArrayExpression**|string|Yes|A field reference, or a scalar expression with a value of type `dynamic` that holds an array or a property bag. The individual top-level elements of the array or property bag get expanded into multiple events. <br /><br />When **ArrayExpression** is used and **NewName** doesn't equal any input field name, the expanded value is extended into a new field in the output. Otherwise, the existing **FieldName** is replaced. <br /><br />Wildcards are not supported.|
|**IndexFieldName**|string|No|If `with_itemindex` is specified, the output includes another field named **IndexFieldName** that contains the index starting at 0 of the item in the original expanded collection.|
|**NewName**|string|No|A name for the new field.|
|**RowLimit**|int|No|The maximum number of rows generated from each original row. The default is `9007199254740991`.|

## Returns

For each event in the input, the operator returns zero, one, or many events in the output,
as determined in the following way:

1. Input fields that aren't expanded appear in the output with their original value.
   If a single input event is expanded into multiple output events, the value is duplicated
   to all events.

1. For each **FieldName** or **ArrayExpression** that is expanded, the number of output events
   is determined for each value based on the **ExpansionMode**. For each input event, the maximum number of output events is calculated. All arrays or property bags are expanded in parallel.
   so that missing values (if any) are replaced by null values. Elements are expanded into rows in the order that they appear in the original array/bag.

1. If the dynamic value is null, then a single event is produced for that value (null).
   If the dynamic value is an empty array or property bag, no event is produced for that value.
   Otherwise, as many events are produced as there are elements in the dynamic value.

> The expanded fields are of type `dynamic`.
>
{.box .info}

## Examples

### Single field - expansion

```kusto
dataset=myDataset
| mv-expand b
```

### Single field - bag expansion to key-value pairs

A simple bag expansion to key-value pairs:

```kusto
dataset=myDataset
| mv-expand bagexpansion=array b 
| extend key = b[0], val=b[1]
```

### Cartesian product of two fields

If you want to get a Cartesian product of expanding two fields, expand one after the other:

```kusto
dataset=myDataset
| mv-expand b
| mv-expand c
```

### Using with_itemindex

Expansion of an array with `with_itemindex`:

```kusto
| extend x=dynamic([17, 3]), y=dynamic({'foo': 'bar', 'cribl': 'search'}) 
| mv-expand bagexpansion=array with_itemindex=i x, z=y limit 3
```

### Filter a nested array

Expand a nested array and filter by its values:

```kusto
dataset="myDataset"
| mv-expand events
| mv-expand events.parameters
| where events.parameters.name = 'suspicious' and events.parameters.boolValue == true
```

Create an event for each of the values of field `method`. You should see 200 events total.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend method=split("GET,POST", ",") 
| mv-expand method
```

