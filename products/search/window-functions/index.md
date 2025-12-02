# Window Functions


# Window Functions

Window functions operate on multiple rows (events) in a row set at a time. Unlike aggregation functions, window
functions require that the rows in the row set be ordered or sorted.

Cribl Search supports window functions on queries that use the `extend` operator. With this combination, Cribl Search
implicitly applies the `centralize` operator. Cribl Search does not support window functions on queries with the
`project` or `where` operator.

The following window functions are available:


| Name                                       | Description                                                                                     |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| [`next`](next)                             | Returns the value of a specific field in a subsequent row.                                      |
| [`prev`](prev)                             | Returns the value of a specific field in a previous row.                                        |
| [`row_cumsum`](row_cumsum)                 | Calculates the cumulative sum for a specified field across all previous rows.                   |
| [`row_number`](row_number)                 | Assigns a unique row number to each row within the results.                                     |
| [`row_rank_dense`](row_rank_dense)         | Assigns a unique numerical position (rank) to each row within the results                       |
| [`row_rank_min`](row_rank_min)             | Assigns a minimal numerical position (rank) to each row within the results                      |
| [`row_window_session`](row_window_session) | Identifies the value at the beginning of each session for a specified field within the results. |

