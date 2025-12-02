# render


The `render` operator enforces a specific visualization of the search results. It's useful when you want to override the default rendering of a query.

The `render` operator supports the following modes:

* `event` – renders the results as a list of events, under the [**Events** tab](search-overview#events).
* `table` – renders the results as a [table](results-table-settings), under the [**Chart** tab](search-overview#charts).

## Syntax

    `Scope | render RenderMode`

### Arguments

* **Scope**: The events to search.
* **RenderMode**: The rendering mode to use. Can be `event` or `table`.

## Examples

```kusto
dataset=myDataset
| render table
```

Render a table of enumerated fields.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| project event, parity 
| render table 
```

