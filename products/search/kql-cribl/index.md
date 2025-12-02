# KQL Extensions


Understand Cribl KQL extensions and variations from the Kusto language.

---

Cribl Search KQL is built on top of the Microsoft [Kusto Query Language](https://learn.microsoft.com/en-us/kusto/query/), with additional extensions and operators. 

This page lists known areas where Cribl KQL operators, functions, and types differ from their similarly named Kusto counterparts. 

These differences might require changes when you programmatically manage Cribl Search using API requests (or other automation) that was originally written around those Kusto counterparts. 

- The [`dayofweek`](dayofweek) function returns an integer between `0` and `6`, representing the day of the week, beginning on Sunday. This differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/day-of-week-function) `dayofweek` function, which returns a timespan.

- The [`make_timespan`](make-timespan) function converts the specified time period into a number of seconds. This output format differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/make-timespan-function) `make_timespan` function, which returns a timespan.

- The [`timespan`](timespan) data type represents a time interval, in seconds. This representation differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/scalar-data-types/timespan) `timespan` type, which represents a literal timespan.

- The [`totimespan`](totimespan) function converts the input expression into a time interval, in seconds. This output format differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/totimespan-function) `totimespan` function, which returns a timespan.

- The [`tostring`](tostring) function, when applied to a `null` input value, returns `null`. This output format differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/tostring-function) `tostring` function, which returns an empty string for `null` values.
