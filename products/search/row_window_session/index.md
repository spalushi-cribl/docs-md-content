# row_window_session


The `row_window_session` function identifies the value at the beginning of each session for a specified field within the results.

The function has the following conceptual calculation model:
1. Goes over the input sequence of **Expr** values in order.
1. For every value, determines if it establishes a new session.
1. If it establishes a new session, it emits the value of **Expr**. Otherwise, emits the previous value of **Expr**.

## Syntax

    `row_window_session( Expr , MaxDistanceFromFirst , MaxDistanceBetweenNeighbors [, Restart] )`

### Arguments

* **Expr**: An expression whose values are grouped together in sessions. Null values produce null values, and the next value starts a new session. **Expr** must be a scalar expression of type [`datetime`](datetime).

* **MaxDistanceFromFirst**: Establishes one criterion for starting a new session: The maximum distance between the current value of **Expr** and the value of **Expr** at the beginning of the session. It's a scalar constant of type [`timespan`](timespan).

* **MaxDistanceBetweenNeighbors**: Establishes a second criterion for starting a new session: The maximum distance from one value of **Expr** to the next. It's a scalar constant of type [`timespan`](timespan).

* **Restart**: An expression that returns a [`bool`](bool) value to indicate when the operation should restart. If specified, every value that evaluates to `true` will immediately restart the session.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

## Example



```kusto
| sort by ID asc, Timestamp asc
| extend SessionStarted = row_window_session(Timestamp, 1h, 5m, ID != prev(ID))
```
