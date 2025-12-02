# .show options


The `.show options` command lists [`set`](set)-statement options configured for your account.

```kusto {runSearch=true}
// list all set-statement options (both active and overridden) configured for you
.show options
```

## Purpose

Use `.show options` to get a list of [`set`-statement options](set#available-set-statement-options) that you, or an
Admin [Search Member](cloud-managing#search-member-roles), have configured for your account. This command lets you:

- See the values of the options.
- Find out which options are active and which are overridden by other settings.
- Identify which options were set by you and which were set the [Search Admin](cloud-managing#search-member-roles).

## Permissions

All [Search Members](cloud-managing#search-member-roles) can see all [`set`](set)-statement options that apply to their
own account.

## Syntax

    `
.show [Scope] options
              [with(reason = "InvestigationReason")]`

### Parameters

| Parameter             | Type               | Description |
| --------------------- | ------------------ | ----------- |
| `Scope`               | [`string`](string) | [Scope](set#scope-of-set-statements) for listing the options:<ul><li>`all` or `*`: List all options configured for your account.</li><li>`active`: List only those options that may affect your searches at the moment (that is, options that are not being overridden by other settings).</li></ul> |
| `InvestigationReason` | [`string`](string) | The reason for running the `.show options` command, which is added to the Cribl Search audit log. |

## Returns

Returns a list of [`set`](set)-statement options configured for your account. The results include the following details:

| Field     | Description |
| --------- | ----------- |
| `_time`   | Execution time of the `.show options` command. |
| `success` | `true` if the `.show options` command was successful. |
| `option`  | The name of the [`set`-statement option](set#available-set-statement-options). |
| `value`   | The value of the option. |
| `source`  | The [scope](set#scope-of-set-statements) in which the option was set. |
| `active`  | `true` if the option affects your searches. `false` if the option doesn't affect you at the moment because it's being overridden by another setting. |

## Examples

See all [`set`](set)-statement options that affect you at the moment.

```kusto {runSearch=true}
.show active options
             with(reason = "Just checking options active for my account.")
```

