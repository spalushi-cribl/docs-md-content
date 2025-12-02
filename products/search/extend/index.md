# extend



The `extend` operator calculates one or more expressions and assigns the results to fields.

## Syntax

    `Scope | extend [FieldName | (FieldName[, ...]) =] Expression [, ...]`

### Arguments

* **Scope**: The events to search.
* **FieldName**: Optional. The name of the field to add or update. If omitted, the name will be generated. 
    * If **Expression** returns more than one field, a list of field names can be specified in parentheses. In this case, **Expression**'s output fields will be given the specified names, dropping the rest of the output fields, if there are any. 
    * If a list of the field names is not specified, all **Expression**'s output fields with generated names will be added to the output.
* **Expression**: A calculation over the fields of the input.

## Results

Resulting fields are returned in one of the following ways:

* Field names noted by `extend` that already exist in the input are removed and appended as their new calculated values.
* Field names noted by `extend` that do not exist in the input are appended as their new calculated values.

## Examples

To calculate and add fields called `Duration` and `IsSevere`:

  ```kusto
  dataset=myDataset 
  | extend Duration = CreatedOn - CompletedOn
  , IsSevere = Level == "Critical" or Level == "Error"
  ```

### Monitoring Disk Operations by Ratio and Status


If you're using [Cribl Edge](/edge/), you can use `extend` to calculate the ratio of read and write operations on each [Edge Node](set-up-edge). Based on the ratio, you can use the [iif](iif) function to give each Node a status, indicating any imbalances or I/O issues:

```kusto {runSearch=true}
dataset="cribl_edge_metrics"
| extend Disk_Operation_Ratio = node_disk_reads_completed_all_total / node_disk_writes_completed_all_total
| extend operation_status = iif(Disk_Operation_Ratio > 2, 'High Read', iif(Disk_Operation_Ratio < 0.5, 'High Write', 'Balanced'))
| summarize count() by operation_status
```

### Monitoring CPU Usage by Level

You can use `extend` to monitor CPU usage on each [Edge Node](set-up-edge). The following query breaks CPU usage into two categories: `High` for CPU usage greater than 70%, and `Low` for anything below that threshold. Then, the [`timestats`](timestats) operator allows you to observe how these categories change over time.

```kusto {runSearch=true}
dataset=cribl_edge_metrics
| extend CPU_Usage_Per_Host = iif(node_cpu_percent_active_all > 70, 'High', 'Low'), HostName=host
| timestats by CPU_Usage_Per_Host
```

```kusto {runSearch=true}
dataset=$vt_dummy event<10
| extend answer=42, parity=iif(event%2==0, 'even', 'odd')
```

