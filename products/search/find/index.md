# find



The `find` operator finds specific events.

> We recommend using the [`cribl`](cribl) operator, which has easier syntax and more capabilities. 
> 
> Either `cribl` or `find` can be used to initiate a query expression.
>
{.box .success}

## Syntax

* `find [withsource=FieldName] [in (Dataset [, Dataset, ...])] where Predicate [project-smart | project FieldName [:FieldType] [, FieldName[:FieldType], ...]`

* `find Predicate [project-smart | project FieldName[:FieldType] [, FieldName[:FieldType], ...]`

### Arguments

* `withsource=`**FieldName**: Optional. By default, the output will include a field called `dataset` whose values indicate which Dataset has contributed to each result. If specified, **FieldName** will be used instead of `dataset`.
* **Predicate**: An expression over the fields of the Datasets that returns a [`bool`](bool) value. For a summary of some filtering functions, see the [`where`](where) operator.
* **Dataset**: Optional. By default, find will look in all the Datasets for:
    * The name of a Dataset, such as `Events`.
    * A query expression, such as `(Events | where id==42)`.
* `project-smart | project`: If not specified, `project-smart` will be used by default. For more information, see [output-schema](#output-schema) below.

## Output Schema

#### `dataset` field

The `find` operator output will always include a `dataset` field with the Dataset name. The field can be renamed using the `withsource` parameter.

#### Results fields

Datasets that don't contain any fields used by the predicate evaluation will be filtered out.

When using `project-smart`, the fields that will appear in the output will be:

* Fields that appear explicitly in the predicate.
* Fields that are common to all the filtered Datasets.

The rest of the fields will be packed into a property bag and will appear in an additional `pack_` field. A field that is referenced explicitly by the predicate and has multiple types will have a different field in the result schema for each such type. Each of the field names will be constructed from the original field name and type, separated by an underscore.

When using `project FieldName[:FieldType] [, FieldName[:FieldType], ...]:

The results will include the fields specified in the list. If a Dataset doesn't contain a certain field, the values in the corresponding rows will be null.
When specifying a **FieldType** with a **FieldName**, this field will have the given type, and the values will be cast to that type if needed. The casting won't have an effect on the field type when evaluating the predicate.

#### `pack_` field

This field will contain a property bag with the data from all the fields that don't appear in the output schema. The Dataset name will serve as the property name and the field value will serve as the property value.

## Examples

### Term lookup across the `default` Dataset {#lookup}

The query finds all events from the `default` Dataset where any field includes the word `Goat`.

```kusto
find "Goat"
```

### Term lookup across the `default` Dataset matching a name pattern {#pattern}

The query finds all events from the `default` Dataset whose name starts with `G`, and in which any field includes the word `Cribl`.

```kusto
find in ("cribl_search_sample") where _raw has "GET" 
| limit 100
```

## Examples of find output results

The following examples show how find can be used over two Datasets: EventsTable1 and EventsTable2. Assume we have the next content of these two Datasets:

### EventsTable1

Session_Id|Level|EventText|Version
-----|-----|-----|-----
acbd207d-51aa-4df7-bfa7-be70eb68f04e|Information|Some Text1|v1.0.0
acbd207d-51aa-4df7-bfa7-be70eb68f04e|Error|Some Text2|v1.0.0
28b8e46e-3c31-43cf-83cb-48921c3986fc|Error|Some Text3|v1.0.1
8f057b11-3281-45c3-a856-05ebb18a3c59|Information|Some Text4|v1.1.0

### EventsTable2

Session_Id|Level|EventText|EventName
-----|-----|-----|-----
f7d5f95f-f580-4ea6-830b-5776c8d64fdd|Information|Some Other Text1|Event1
acbd207d-51aa-4df7-bfa7-be70eb68f04e|Information|Some Other Text2|Event2
acbd207d-51aa-4df7-bfa7-be70eb68f04e|Error|Some Other Text3|Event3
15eaeab5-8576-4b58-8fc6-478f75d8fee4|Error|Some Other Text4|Event4

### Search in common fields, project common and uncommon fields, and pack the rest

```kusto
find in (EventsTable1, EventsTable2) 
     where Session_Id == 'acbd207d-51aa-4df7-bfa7-be70eb68f04e' and Level == 'Error' 
     project EventText, Version, EventName
```

dataset|EventText|Version|EventName|pack\_
-----|-----|-----|-----|-----
EventsTable1|Some Text2|v1.0.0| |{"Session\_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Error"}
EventsTable2|Some Other Text3| |Event3|{"Session\_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Error"}

### Search in common and uncommon fields

```kusto
find Version == 'v1.0.0' or EventName == 'Event1' project Session_Id, EventText, Version, EventName
```

dataset|Session_Id|EventText|Version|EventName
-----|-----|-----|-----|-----
EventsTable1|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Some Text1|v1.0.0| 
EventsTable1|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Some Text2|v1.0.0| 
EventsTable2|f7d5f95f-f580-4ea6-830b-5776c8d64fdd|Some Other Text1| |Event1

> In practice, EventsTable1 rows will be filtered with Version == 'v1.0.0' predicate and EventsTable2 rows will be filtered with EventName == 'Event1' predicate.
>
{.box .info}

### Use abbreviated notation to search across the `default` Dataset

```kusto
find Session_Id == 'acbd207d-51aa-4df7-bfa7-be70eb68f04e'
```

dataset|Session_Id|Level|EventText|pack_
-----|-----|-----|-----|-----
EventsTable1|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Information|Some Text1|{"Version":"v1.0.0"}
EventsTable1|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Error|Some Text2|{"Version":"v1.0.0"}
EventsTable2|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Information|Some Other Text2|{"EventName":"Event2"}
EventsTable2|acbd207d-51aa-4df7-bfa7-be70eb68f04e|Error|Some Other Text3|{"EventName":"Event3"}

### Return the results from each row as a property bag

```kusto
find Session_Id == 'acbd207d-51aa-4df7-bfa7-be70eb68f04e'
```

dataset|pack_
-----|-----
EventsTable1|{"Session_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Information", "EventText":"Some Text1", "Version":"v1.0.0"}
EventsTable1|{"Session_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Error", "EventText":"Some Text2", "Version":"v1.0.0"}
EventsTable2|{"Session_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Information", "EventText":"Some Other Text2", "EventName":"Event2"}
EventsTable2|{"Session_Id":"acbd207d-51aa-4df7-bfa7-be70eb68f04e", "Level":"Error", "EventText":"Some Other Text3", "EventName":"Event3"}

Return results that match "test event".

```kusto {runSearch=true}
dataset=$vt_dummy event<10 
| extend _raw=iif(event%2>0, "This is a test event", "This is another event") 
| find where * has "test event"
```
