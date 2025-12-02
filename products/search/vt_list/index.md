# $vt_list


The `$vt_list` virtual table lists available virtual tables.

```kusto {runSearch=true}
// returns a list of all virtual tables available to the current user
dataset="$vt_list"
```

## Purpose

Use `$vt_list` to quickly look up the names and descriptions of [virtual tables](virtual-tables) available in Cribl
Search.

## Permissions

All types of [Search Members](cloud-managing#search-member-roles) can see the list of all virtual tables.

## Syntax

```kusto
dataset="$vt_list"
```

## Returns

Returns one event for each of Cribl Search's virtual tables, containing the table's name and description.

## Examples

List virtual tables whose names start with `$vt_object`.

```kusto {runSearch=true}
dataset="$vt_list"
  | where name startswith "$vt_object"
```

