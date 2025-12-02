# Cribl Functions


# ![](page/agg-icon.svg) Cribl Functions

Cribl Search supports additional functions that you can use together with the [`summarize`](summarize),
[`eventstats`](eventstats), and [`timestats`](timestats) operators to aggregate your data. We refer to those additional
functions as **Cribl** functions, since they're specific to Cribl Search.

Cribl Search supports the following Cribl functions:


| Name                               | Description                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| [`findearliest`](findearliest)     | Returns the earliest value of an expression across the group.                                                |
| [`findearliestif`](findearliestif) | Returns the earliest value of an expression across the group for which a predicate evalutes to `true`.       |
| [`findfirst`](findfirst)           | Returns the first observed value of an expression across the group.                                          |
| [`findfirstif`](findfirstif)       | Returns the first observed value of an expression across the group for which a predicate evalutes to `true`. |
| [`findlast`](findlast)             | Returns the last observed value of an expression across the group.                                           |
| [`findlastif`](findlastif)         | Returns the last observed value of an expression across the group for which a predicate evalutes to `true`.  |
| [`findlatest`](findlatest)         | Returns the latest value of an expression across the group.                                                  |
| [`findlatestif`](findlatestif)     | Returns the latest value of an expression across the group for which a predicate evalutes to `true`.         |
| [`list`](list)                     | Returns the list of values of an expression across the group                                                 |
| [`median`](median)                 | Returns the middle value of an expression across the group.                                                  |
| [`medianif`](medianif)             | Returns the middle value of an expression across the group for which a predicate evalutes to `true`.         |
| [`persecond`](persecond)           | Returns the per-second rate of an expression across the group                                                |
| [`persecondif`](persecondif)       | Returns the per-second rate of an expression across the group for which a predicate evalutes to `true`.      |
| [`rate`](rate)                     | Returns the rate observed value of an expression across the group.                                           |
| [`rateif`](rateif)                 | Returns the rate observed value of an expression across the group for which a predicate evalutes to `true`.  |
| [`sumsq`](sumsq)                   | Returns the sum of squares of an expression across the group.                                                |
| [`sumsqif`](sumsqif)               | Returns the sum of squares of an expression across the group for which a predicate evalutes to `true`.       |
| [`values`](values)                 | Returns all of the distinct values of an expression across the group.                                        |

