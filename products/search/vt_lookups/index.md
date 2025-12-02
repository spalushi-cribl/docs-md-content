# $vt_lookups


The `$vt_lookups` virtual table lists available lookups, or gets the contents of a specific lookup.

```kusto {runSearch=true}
// returns the contents of the service_names_port_numbers.csv lookup file
dataset="$vt_lookups" lookupFile="service_names_port_numbers"
```

## Purpose

Use `$vt_lookups` to:

- Get a quick overview of all [lookup](lookups) files available to you.
- Search the contents of a specific lookup.

## Permissions

All types of Search Members can see the list of all lookup files in the organization. For details about who can export to `$vt_lookups`, see [Search Member Permissions](cloud-managing#member-roles).

## Syntax

```kusto
dataset="$vt_lookups"
  // optionally, get the contents of a specific lookup file:
  lookupFile="LookupName"
```

### Parameters

| Name         | Type               | Description                                                                                     |
| ------------ | ------------------ | ----------------------------------------------------------------------------------------------- |
| `LookupName` | [`string`](string) | The name of the lookup file whose content you want to access. Don't include the file extension. |

## Returns

Returns one event for each available lookup. Each event contains the following fields:

| Field         | Description                                                                        |
| ------------- | ---------------------------------------------------------------------------------- |
| `name`        | The name of the lookup.                                                            |
| `description` | The description of the lookup.                                                     |
| `rowCount`    | The number of rows in the lookup.                                                  |
| `size`        | The size of the lookup file, in bytes.                                             |
| `compressed`  | `true`: the lookup file is compressed. `false`: the lookup file is not compressed. |

## Examples

List all lookups available to the current user.

```kusto {runSearch=true}
dataset="$vt_lookups"
```

List available lookups that have more than 10,000 rows.

```kusto {runSearch=true}
dataset="$vt_lookups"
  | where rowCount>10000
```

Get only specific entries of the `service_names_port_numbers.csv` lookup file.

```kusto {runSearch=true}
dataset="$vt_lookups" lookupFile="service_names_port_numbers"
 | where transport == "udp"
```

