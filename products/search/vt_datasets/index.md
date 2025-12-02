# $vt_datasets


The `$vt_datasets` virtual table lists available Datasets.

```kusto {runSearch=true}
// returns a list of all Datasets available to the current user
dataset="$vt_datasets"
```

## Purpose

Use `$vt_datasets` to quickly find [Datasets](basic-concepts#datasets) of a specific type or with specific name patterns.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                                                                             |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Admin                                                    | Can see all Datasets in their organization.                                                                                             |
| Editor                                                   | Can see only those Datasets that they created themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |
| User                                                     | Can see only those Datasets that were shared with them.                                                                                 |

## Syntax

```kusto {runSearch=true}
dataset="$vt_datasets"
```

## Returns

Returns one event for each available Dataset. Each event contains the following fields:

| Field         | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| `id`          | The Dataset ID.                                                                     |
| `description` | The Dataset description.                                                            |
| `provider`    | The `id`, `description`, and `type` of the [Dataset Provider](basic-concepts#dataset-providers). |

## Examples

List all your S3 Datasets.

```kusto {runSearch=true}
dataset="$vt_datasets"
  | where provider.type == "s3"
```

Count the number of Datasets of each type.

```kusto {runSearch=true}
dataset="$vt_datasets"
  | summarize count() by provider.type
```

