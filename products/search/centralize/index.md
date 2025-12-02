# centralize


The `centralize` operator enforces the execution of operators on the coordinator rather than on federated executors. This is useful where executors lack network visibility to the destination. You can centralize the results on a coordinator before [sending](send) them. This operator overrides the normal division of labor, in which:

* Executors process as close to your data as possible, on the remote end. They scan data – for example, reading from S3, decompressing, filtering, and projecting.
* Coordinators receive the partial results from the executors and perform post-aggregation and merging of the results.

## Syntax

    `... | centralize`

## Examples

Centralize your data on a coordinator before sending it to a Cribl Stream [Destination](https://docs.cribl.io/stream/destinations).

```kusto {runSearch=true}
dataset=$vt_dummy event<100
| extend parity=iif(event%2==0, 'even', 'odd')
| centralize
| send
```

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| where dstport <= 1024 and action == REJECT 
| centralize 
| send
```

```kusto
dataset=myDataset method or status 
| where status > 299 
| centralize 
| send
```
