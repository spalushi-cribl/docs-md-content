# $vt_dummy


The `$vt_dummy` virtual table generates dummy events.

```kusto {runSearch=true}
// returns 10 dummy events
dataset="$vt_dummy" event<10
```

## Purpose

Use `$vt_dummy` to test your queries with dummy data.

The `event` predicate lets you specify the number of events, and the `second` predicate lets you simulate a long-running
query. Using both lets you simulate a burst of events over a specific time range.

## Permissions

All types of [Search Members](cloud-managing#search-member-roles) can use the `$vt_dummy` virtual table.

## Syntax

```kusto
dataset="$vt_dummy" event<5 second<3
```

### Parameters

| Name             | Type         | Required | Description                                                                                                                                                                                                      |
| ---------------- | ------------ | :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NumberOfEvents` | [`int`](int) |    No    | The number of events to generate.<br><br>If used together with `SearchRuntime`, generates the `NumberOfEvents` per second, over the course of the `SearchRuntime`.<br><br>If not specified, generates one event. |
| `SearchRuntime`  | [`int`](int) |    No    | The desired runtime of the simulated search, in seconds.                                                                                                                                                         |

## Returns

Returns dummy events according to the specified criteria. Each event contains the following fields:

| Field   | Description                 |
| ------- | --------------------------- |
| `_time` | The event's timestamp.      |
| `event` | The event's ordinal number. |

## Examples

Generate one dummy event, with an additional field `foo` set to `42`.

```kusto {runSearch=true}
dataset="$vt_dummy"
  | extend foo=42
```

Generate five dummy events.

```kusto {runSearch=true}
dataset="$vt_dummy" event<5
```

Generate one event per second, over the course of 10 seconds.

```kusto {runSearch=true}
dataset="$vt_dummy" second<10
```

Generate three events per second, over the course of 10 seconds.

```kusto {runSearch=true}
dataset="$vt_dummy" event<3 and second<10
```

