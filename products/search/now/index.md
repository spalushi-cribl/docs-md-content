# now


The `now` function returns the current UTC clock time as a [datetime](datetime) value (Unix time in seconds), optionally
offset by a given timespan.

You can use `now` multiple times in a statement and the clock time being referenced will be the same for all instances.





> To convert [`datetime`](datetime) (Unix time) into a [`string`](string), or vice versa, use the [`strftime`](strftime) and [`strptime`](strptime) functions.
{.box .info}


## Syntax

`now( [ Offset ] )`

### Arguments

- **Offset**: A timespan, added to the current UTC clock time. Default: 0.

## Returns

The current UTC clock time as a [`datetime`](datetime).

## Examples

This returns the current time, in Unix epoch time:

```kusto {runSearch=true}
print now()
```

This returns the current time two days earlier, in Unix epoch time: 

```kusto {runSearch=true}
print now(-2d)
```
