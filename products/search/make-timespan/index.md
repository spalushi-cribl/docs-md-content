# make_timespan


The `make_timespan` function converts the specified time period into a number of seconds.


> This output format differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/make-timespan-function) `make_timespan` function, which returns a timespan.
> 
> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}

## Syntax

`make_timespan( [ Day ,] Hour, Minute [, Second])`

### Arguments

- **Day**: day (an integer value, from 1 to 28–31)
- **Hour**: hour (an integer value, from 0 to 23)
- **Minute**: minute (an integer value, from 0 to 59)
- **Second**: second (a real value, from 0 to 59.9999999)

## Returns

If creation is successful, the result will be the number of seconds in the interval. Otherwise, the result will be `null`.

{#example}
## Examples

This example returns `131455.123`:

```kusto {runSearch=true}
print make_timespan(1,12,30,55.123)
```

This example returns `3600`:

```kusto {runSearch=true}
print make_timespan(1, 0)
```
