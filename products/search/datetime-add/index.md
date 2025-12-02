# datetime_add



The `datetime_add` function calculates a new datetime from a specified datepart multiplied by a specified amount, added to a specified datetime.

## Syntax

    `datetime_add( Period, Amount, Datetime )`

### Arguments

* **Period**: Possible values are: `Year`, `Quarter`, `Month`, `Week`, `Day`, `Hour`, `Minute`, `Second`, `Millisecond`, `Microsecond`, `Nanosecond`.
* **Amount**: An integer.
* **Datetime**: A datetime.

## Returns

A date after a certain time/date interval has been added, represented as [`datetime`](datetime) (Unix time).

## Examples

This example adds two days to the specified date:

```kusto {runSearch=true}
print newDate=datetime_add( 'day', 2, make_datetime( 2025,12,31 ) ) 
| extend prettyDate = strftime( newDate, '%x'  )
```

The following examples illustrate calling this function with different values for the `Period` argument:

```kusto {runSearch=true}
print year = datetime_add('year',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print month = datetime_add('month',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print quarter = datetime_add('quarter',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print week = datetime_add('week',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print day = datetime_add('day',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print hour = datetime_add('hour',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print minute = datetime_add('minute',1,make_datetime(2025,1,1))
```

```kusto {runSearch=true}
print second = datetime_add('second',1,make_datetime(2025,1,1))
```
