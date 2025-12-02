# .show objects


The `.show objects` command lists objects included in a Dataset, including files, tables, and endpoints, along with information about each object. The results are filtered by the set [time range](build-a-search#time-range).


> [Cribl Edge Datasets](edge-datasets) are not supported.
{.box .warning}

```kusto {runSearch=true}
// list all objects in the cribl_search_sample Dataset
.show objects(cribl_search_sample)
```

## Purpose

Use `.show objects` to get detailed information about the contents of a Dataset before you search it.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions |
| -------------------------------------------------------- | ----------- |
| Admin                                                    | Can see the objects of all Datasets in their organization. |
| Editor                                                   | Can see the objects of only those Datasets that they created themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |
| User                                                     | Can see the objects of only those Datasets that were shared with them. |

## Syntax

    `
.show objects(DatasetID[, ...])
             [with(reason = "InvestigationReason")]`




You can also combine this command with [operators](operators).


### Parameters

| Parameter             | Type               | Description |
| --------------------- | ------------------ | ----------- |
| `DatasetID`           | [`string`](string) | The ID of the Dataset whose objects you want to list. Separate multiple Dataset IDs with a comma `,`. Wildcards `*` are supported at the end or in the middle of an ID, as in `cribl_*_logs*`. |
| `InvestigationReason` | [`string`](string) | The reason for running the `.show objects` command, which is added to the Cribl Search audit log. |

## Returns

The `.show objects` command returns a list of files, tables, endpoints, metrics, and any other units that
the specified Dataset contains. Each object in the Dataset is represented by one row in the results table.

The details about each object vary according to which objects the Dataset contains. Depending on the Dataset, the results may include the following details:

| Field          | Description                                                                            |
| -------------- | -------------------------------------------------------------------------------------- |
| `_time`        | The `.show objects` command's execution time. It's the same for every returned object. |
| `success`      | Shows whether the command was successful.                                              |
| `dataset`      | The ID of the examined Dataset.                                                        |
| `datasetType`  | The type of the examined Dataset.                                                      |
| `name`         | The name of the object.                                                                |
| `size`         | The size of the object in bytes.                                                       |
| `lastModified` | The object's last modification time.                                                   |

## Examples

List all objects in the `cribl_internal_logs` and `cribl_search_sample` Datasets.

```kusto {runSearch=true}
.show objects(cribl_internal_logs, cribl_search_sample)
```

List the five largest objects in the `cribl_internal_logs` Dataset.

```kusto {runSearch=true}
.show objects(cribl_internal_logs) | top 5 by size
```

See how many objects larger than 50,000 bytes exist in all Datasets whose IDs start with `cribl_int`.

```kusto {runSearch=true}
.show objects(cribl_int*)
    with(reason = "Investigating number of large objects") 
    | where size > 50000 | count
```

