# .generate metadata (Deprecated)



> ##### Deprecated Feature
>
> Cribl has removed this command from Cribl Search. We invite you to explore [Lakehouses](/lake/lakehouse/) as a new option to speed up searches on designated Datasets.
{.box .warning}

The `.generate metadata` command prescans selected portions of your data to generate metadata.

```kusto {runSearch=true}
// prescan all fields in the cribl_search_sample Dataset
.generate metadata(cribl_search_sample)
```

## Purpose

Use `.generate metadata` to [improve search performance](commands#generate-metadata) for specific
Datasets.

Running `.generate metadata` on large Datasets can take quite a while, but it can significantly improve performance when
searching the prescanned fields. When the command is running, the results display periodic status updates. If you plan to run `.generate metadata` on large Datasets, consider adjusting the [**Running time limit**](usage-groups#usage-limits).


> To automatically prescan your data on a regular basis, use [Dataset Acceleration](acceleration).
{.box .info}

You can use `.generate metadata` with the following [Dataset Provider](basic-concepts#dataset-providers) types:

- [Amazon S3](set-up-s3)
- [Azure Blob Storage](set-up-azure-blob)
- [Google Cloud Storage](set-up-google-cloud-storage)


> To see which Datasets already contain Cribl Search metadata, run [`dataset=$vt_object_list_summary`](vt_object_list_summary).
{.box .info}

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions |
| -------------------------------------------------------- | ----------- |
| Admin                                                    | Can generate metadata for all Datasets in their Organization. |
| Editor                                                   | Can generate metadata only for those Datasets that they created themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |
| User                                                     | Can generate metadata only for those Datasets that were shared with them. |

## Syntax

`
.generate metadata(DatasetID)
                  [on Field[, ...]]
                  [with ([earliest=TimeString] [latest=TimeString] [mode=Mode] [reason=Reason])]`




You can also combine this command with [operators](operators).


### Parameters

| Parameter    | Type               | Description |
| ------------ | ------------------ | ----------- |
| `DatasetID`  | [`string`](string) | The ID of the Dataset that you want to prescan. Wildcards `*` are not supported. |
| `Field`      | [`string`](string) | The name of the field to prescan. If not specified, `.generate metadata` prescans only the `_time` field in the specified Dataset. Separate multiple fields with a comma `,`. |
| `TimeString` | [`string`](string) | A relative or absolute timestamp. See [time syntax](cribl#time-syntax) for details. |
| `Mode`       | [`string`](string) | [Scan mode](acceleration#modes). Can be `detailed` (default) or `quick`. |
| `Reason`     | [`string`](string) | The reason for running the `.generate metadata` command, which is added to the Cribl Search audit log. |

## Returns

Returns a table that lets you track the analysis progress.

When the command finishes running, each prescanned event is enriched with a new nested field: `statistics`.

For each field specified in the command (and always for the `_time` field), the `statistics` field contains the
following metadata:

- `min`: The minimum value of the field in a given Dataset.
- `max`: The maximum value of the field in a given Dataset.

## Examples

Prescan the `cribl_search_sample` Dataset for the last 2 weeks. Use human-readable [time syntax format](cribl#time-syntax) for **relative** timestamps.

```kusto {runSearch=true}
.generate metadata(cribl_search_sample)
                  with (earliest=-2w reason="Analyzing data from the last two weeks")
```

Prescan the `cribl_search_sample` Dataset for the month of January 2024. Use [Unix time](https://en.wikipedia.org/wiki/Unix_time) for **absolute** timestamps.

```kusto {runSearch=true}
.generate metadata(cribl_search_sample)
                  with (earliest=1704067200 latest=1706745599 reason="Analyzing January 2024 data")
```

Prescan the `bytes` field and the `_time` field in the `cribl_search_sample` Dataset.

```kusto {runSearch=true}
.generate metadata(cribl_search_sample) on bytes
```
