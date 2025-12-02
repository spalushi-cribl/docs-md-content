# datetime_diff


The `datetime_diff` function calculates the number of the specified periods between two datetime values.

## Syntax

    `datetime_diff( Period, Datetime1, Datetime2 )`

### Arguments

* **Period**: Possible values are: `Year`, `Quarter`, `Month`, `Week`, `Day`, `Hour`, `Minute`, `Second`, `Millisecond`, `Microsecond`, `Nanosecond`.
* **DatetimeX**: A datetime.

## Returns

An integer, which represents amount of **Period**s in the result of subtraction `(Datetime1 - Datetime2)`.

## Examples

The following examples illustrate calling this function with different values for the `Period` argument:

```kusto {runSearch=true}
printyear = datetime_diff('year',datetime(2017-01-01),datetime(2000-12-31))
```

```kusto {runSearch=true}
printquarter = datetime_diff('quarter',datetime(2017-07-01),datetime(2017-03-30))
```

```kusto {runSearch=true}
printmonth = datetime_diff('month',datetime(2017-01-01),datetime(2015-12-30))
```

```kusto {runSearch=true}
printweek = datetime_diff('week',datetime(2017-10-29 00:00),datetime(2017-09-30 23:59))
```

```kusto {runSearch=true}
printday = datetime_diff('day',datetime(2017-10-29 00:00),datetime(2017-09-30 23:59))
```

```kusto {runSearch=true}
printhour = datetime_diff('hour',datetime(2017-10-31 01:00),datetime(2017-10-30 23:59))
```

```kusto {runSearch=true}
printminute = datetime_diff('minute',datetime(2017-10-30 23:05:01),datetime(2017-10-30 23:00:59))
```

```kusto {runSearch=true}
printsecond = datetime_diff('second',datetime(2017-10-30 23:00:10.100),datetime(2017-10-30 23:00:00.900))
```

```kusto {runSearch=true}
printmillisecond = datetime_diff('millisecond',datetime(2017-10-30 23:00:00.200100),datetime(2017-10-30 23:00:00.100900))
```

```kusto {runSearch=true}
printmicrosecond = datetime_diff('microsecond',datetime(2017-10-30 23:00:00.1009001),datetime(2017-10-30 23:00:00.1008009))
```

```kusto {runSearch=true}
printnanosecond = datetime_diff('nanosecond',datetime(2017-10-30 23:00:00.0000000),datetime(2017-10-30 23:00:00.0000007))
```
