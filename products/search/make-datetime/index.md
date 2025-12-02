# make_datetime


The `make_datetime` function converts the specified date and time into a [`datetime`](datetime) value (Unix time in
seconds).





> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}


## Syntax

`make_datetime( Year, Month, Day [, Hour] [, Minute] [, Second])`

### Arguments

- **Year**: year (an integer value, from 0 to 9999)
- **Month**: month (an integer value, from 1 to 12)
- **Day**: day (an integer value, from 1 to 28–31)
- **Hour**: hour (an integer value, from 0 to 23)
- **Minute**: minute (an integer value, from 0 to 59)
- **Second**: second (a real value, from 0 to 59.9999999)

## Returns

If creation is successful, result will be a [`datetime`](datetime) value; otherwise, the result will be `null`.

## Example

This example returns `1506729600`:

```kusto {runSearch=true}
print year_month_day = make_datetime(2017,10,0)
```
