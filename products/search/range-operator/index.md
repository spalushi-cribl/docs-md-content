# range


The `range` operator generates a series of events, each containing a generated numeric field and a `_time` field. You can use this operator as syntactic sugar on the `$vt_dummy` virtual table, to generate a defined range of events with equally spaced values. 

> This is a data-generating operator, not a data-consuming operator.
>
{.box .info}

## Syntax

    `range <FieldName> from <Start> to <Stop> [step <Step>]`

### Arguments

Name|Type|Required|Description
-----|-----|-----|-----
**FieldName**|String|Yes|The name of the single column in the output table.
**Start**|Int, Long, Real, Datetime, or Timespan|Yes|The smallest value to generate.
**Stop**|Int, Long, Real, Datetime, or Timespan|Yes|The highest value to generate; or, if **Step** would generate a value exceeding this parameter, a bound on the highest value.
**Step**|Int, Long, Real, Datetime, or Timespan|No|The difference between two consecutive values in the output.

## Results

A series of events, each containing a field called `<FieldName>`, whose values are: `<Start>`, `<Start>` + `<Step>`, ... up to and including `<Stop>`.

## Examples

Generate 10 equally spaced events, numbered `1` to `10`:

```kusto {runSearch=true}
range theField from 1 to 10
```

Generate five equally spaced events, numbered `1` to `9`:

```kusto {runSearch=true}
range theField from 1 to 10 step 2
```

Generate a series of hourly events, starting three days back:

```kusto {runSearch=true}
range theField from ago(3d) to ago(2d) step 1h | project strftime(theField,'%Y-%m-%dT%H:%M:%S.%L%Z')
```

Generate a week's worth of daily events:

```kusto
range _time from ago(7d) to now() step 1d
```

Generate 10 event values from 10 to 100, incremented by 10:

```kusto
range myField from 10 to 100 step 10
```
