# not


The `not` function reverses the value of its boolean argument.

## Syntax

`not( Expression )`

### Arguments

- **Expression**: A boolean expression to be reversed.

## Results

Returns the reversed logical value of its boolean argument.

If the argument is [`null`](null), or cannot be converted to [`bool`](bool) (using the [`tobool`](tobool) function), returns [`null`](null).

## Examples {#examples}

These examples return `false`:

```kusto {runSearch=true}
print not( true )
```

```kusto {runSearch=true}
print not( "true" )
```

```kusto {runSearch=true}
print not( 1 )
```

```kusto {runSearch=true}
print not( 100 )
```

These examples return `true`:


```kusto {runSearch=true}
print not( false )
```

```kusto {runSearch=true}
print not( "false" )
```

```kusto {runSearch=true}
print not( 0 )
```

These examples return [`null`](null):

```kusto {runSearch=true}
print not( "abc" )
```

```kusto {runSearch=true}
print not( null )
```
