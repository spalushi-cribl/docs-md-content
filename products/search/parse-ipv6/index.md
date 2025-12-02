# parse_ipv6


The `parse_ipv6` function converts IPv6 or IPv4 string to a canonical IPv6 string representation.

## Syntax

`parse_ipv6( Expression )`

### Arguments

- **Expression**: String expression representing IPv6/IPv4 network address that will be converted to canonical IPv6
  representation. Denote a subnet mask with a forward slash `/`.

## Results

If the conversion is successful, returns a [`string`](string) representing a canonical IPv6 network address.

If the conversion fails, returns `null`.

## Examples

```kusto {runSearch=true}
print theData='192.168.1.2'  | extend parsed_data=parse_ipv6(theData) | render event
```

```kusto {runSearch=true}
print theData='abcd::ffff:c0a8:0102'  | extend parsed_data=parse_ipv6(theData) | render event
```

This example returns `'0000:0000:0000:0000:0000:ffff:7f00:0001'`:

```kusto
parse_ipv6("127.0.0.1")
```

This example returns `'fe80:0000:0000:0000:085d:e82c:9446:7994'`:

```kusto
parse_ipv6(":fe80::85d:e82c:9446:7994")
```
