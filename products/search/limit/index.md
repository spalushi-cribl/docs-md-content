# limit



The `limit` operator returns up to the specified number of events (or fewer, if fewer events match the query). You can use this operator to size your result sets, and to control resource usage.

Alias: [`take`](take) (`limit` and `take` are synonyms).

## Syntax

    `Scope | limit Integer`

### Arguments

* **Scope**: The events to search.
* **Integer**: A positive integer.

## Usage {#usage}

Bear in mind the following execution details:

- `limit` doesn't guarantee any consistency in its results when executing multiple times, even if the Dataset hasn't changed.

- In interactive searches (from the query box), query execution with `limit` displays previews of the results. These previews reflect only a sample of events, up to the `limit` threshold.

- Scheduled and programmatic searches omit these previews.

- In [Lakehouse](/search-cribl-lake#searching-lakehouse) searches, `limit` constrains the number of output rows, but the query continues iterating over the whole population of events. Here, Cribl Search will true up aggregations as the query completes.

- Outside of Lakehouse searches, the `limit` operator's argument sets a hard threshold. Query execution will end when Cribl Search finds a matching number of events, or can be canceled during a preview. So aggregations will be accurate for the `limit` sample, but not necessarily for the overall population of events.

## Example

```kusto
dataset=myDataset
| limit 1000
```

Return only 42 events.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| limit 42
```
