# take



The `take` operator returns up to the specified number of events.

Alias: [`limit`](limit) ([`limit`](limit) and `take` are synonyms.)

> `take` doesn't guarantee any consistency in its results when executing multiple times, even if the data set hasn't changed.
>
{.box .info}

## Syntax

    `Scope | take Integer`

### Arguments

* **Scope**: The events to search.
* **Integer**: A positive integer.

## Example

```kusto
dataset=myDataset
| take 1000
```

Return only 42 events.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| take 42
```

