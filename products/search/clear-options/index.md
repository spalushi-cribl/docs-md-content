# .clear options


The `.clear options` command disables [`set`](set)-statement options.

```kusto {runSearch=true}
// disable all set-statement options configured for your account
.clear options
```

## Purpose

Use `.clear options` to disable [options](set#available-set-statement-options) that were configured by [`set`](set)
statements.

To see which [`set`](set)-statement options affect your account at the moment before running `.clear options`, use the
[`.show options`](show-options) command.

## Permissions

| [Search Member Type](cloud-managing#search-member-roles) | Permissions |
| -------------------------------------------------------- | ----------- |
| Admin                                                    | Can disable any options configured for any [scope](set#scope-of-set-statements). |
| Editor or User                                           | Can disable only those options that they configured themselves. |

## Syntax

    `
.clear [Scope] options
               [with(reason="DeactivationReason")]`

### Parameters

| Parameter            | Type               | Description |
| -------------------- | ------------------ | ----------- |
| `Scope`              | [`string`](string) | [Scope](set#scope-of-set-statements) of deactivation:<ul><li>`user` (default): Deactivate all options that the current user set for their own account.</li><li>`global` (Admin only): Deactivate all options set for all users in the matching Usage Group.</li><li>`all` (Admin only): Deactivate according to both the `user` and `global` scopes.</li></ul> |
| `DeactivationReason` | [`string`](string) | A [`string`](string) that provides a reason for disabling the options, to add to the Cribl Search audit log. |

## Returns

Returns a single event that confirms the options are disabled.

## Examples

Disable all [`set`](set)-statement options configured for your own account.

```kusto
.clear user options
            with(reason = "Clearing all my options.")
```

Disable all [`set`](set)-statement options of all users in the Usage Group.

```kusto
// Admin only
.clear global options
              with(reason = "Clearing all options for my users.")
```

Disable all [`set`](set)-statement options, both for your own account and for all users in the Usage Group.

```kusto
// Admin only
.clear all options
           with(reason = "Clearing all options for me and my users.")
```

