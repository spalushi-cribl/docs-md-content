# datetime_part


The `datetime_part` function extracts the requested date part as an integer value.

## Syntax

    `datetime_part( Part, Datetime )`

### Arguments

* **Part**: Possible values are: `Year`, `Quarter`, `Month`, `week_of_year`, `Day`, `DayOfYear`, `Hour`, `Minute`, `Second`, `Millisecond`, `Microsecond`, `Nanosecond`.
* **Datetime**: A datetime.

## Returns

An integer representing the extracted part.

> `week_of_year` returns an integer that represents the week number. The week number is calculated from the first week of a year, which is the one that includes the first Thursday.
>
{.box .info}

## Examples

The following examples illustrate calling this function with different values for the `Part` argument:

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print year = datetime_part("year", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print quarter = datetime_part("quarter", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print month = datetime_part("month", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print weekOfYear = datetime_part("week_of_year", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print day = datetime_part("day", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print dayOfYear = datetime_part("dayOfYear", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print hour = datetime_part("hour", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print minute = datetime_part("minute", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print second = datetime_part("second", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print millisecond = datetime_part("millisecond", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print microsecond = datetime_part("microsecond", dt)
```

```kusto {runSearch=true}
let dt = datetime(2017-10-30 01:02:03.7654321);
print nanosecond = datetime_part("nanosecond", dt)
```
