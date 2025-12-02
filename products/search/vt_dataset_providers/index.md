# $vt_dataset_providers


The `$vt_dataset_providers` virtual table lists available Dataset Providers.

```kusto {runSearch=true}
// returns a list of all Dataset Providers available to the current user
dataset="$vt_dataset_providers"
```

## Purpose

Use `$vt_dataset_providers` for a quick overview of your [Dataset Providers](basic-concepts#dataset-providers).

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions                                                                                                                                      |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Admin                                                    | Can see all Dataset Providers in their organization.                                                                                             |
| Editor                                                   | Can see only those Dataset Providers that they created themselves or that were [shared](sharing#share-datasets-and-dataset-providers) with them. |
| User                                                     | Can see only those Dataset Providers that were shared with them.                                                                                 |

## Syntax

```kusto {runSearch=true}
dataset="$vt_dataset_providers"
```

## Returns

Returns one event for each available Dataset Provider. Each event contains the following fields:

| Field         | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `id`          | The ID of the Dataset Provider.                              |
| `description` | The description of the Dataset Provider.                     |
| `type`        | The type of the Dataset Provider. For example, `cribl_edge`. |

## Examples

List all your S3 Dataset Providers.

```kusto {runSearch=true}
dataset="$vt_dataset_providers"
  | where type == "s3"
```

List all built-in Dataset Providers.

```kusto {runSearch=true}
dataset="$vt_dataset_providers"
  | where type startswith "cribl"
```

