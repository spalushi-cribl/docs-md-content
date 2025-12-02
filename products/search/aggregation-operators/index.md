# Aggregation Operators


A list of aggregation operators supported by Cribl Search.

---

Aggregation operators summarize data by grouping it based on specified fields and applying aggregation functions like
[`sum`](sum), [`avg`](avg), or [`max`](max) to produce meaningful insights.


| Name                       | Description                                                                |
| -------------------------- | -------------------------------------------------------------------------- |
| [`count`](operators-count) | Returns the number of all input events.                                    |
| [`eventstats`](eventstats) | Aggregates events and adds the results as new fields to the source events. |
| [`summarize`](summarize)   | Produces a table that aggregates the input data.                           |
| [`timestats`](timestats)   | Aggregates events by time periods or bins..                                |

