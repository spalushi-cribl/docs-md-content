# parse_ipv4


The `parse_ipv4` function converts IPv4 string to long (signed 64-bit) number representation in big-endian order.

## Syntax

`parse_ipv4( Expression )`

### Arguments

- **Expression**: String expression representing IPv4 that will be converted to long. Denote a subnet mask with a
  forward slash `/`.

## Results

If the conversion is successful, returns a [`long`](long) number.

If the conversion fails, returns `null`.

## Examples

This example returns `3232235778`:

```kusto {runSearch=true}
print theData='192.168.1.2' | extend parsed_data=parse_ipv4(theData) | render event
```

This example also returns `3232235776`:

```kusto {runSearch=true}
print theData='192.168.1.2/24' | extend parsed_data=parse_ipv4(theData) | render event
```

This example returns `2130706433`:

```kusto
parse_ipv4("127.0.0.1")`
```

This example returns `2130706432`:

```kusto
parse_ipv4("127.0.0.1/24")
```

This example returns `true`:

```kusto
parse_ipv4('192.1.168.1') < parse_ipv4('192.1.168.2')
```
