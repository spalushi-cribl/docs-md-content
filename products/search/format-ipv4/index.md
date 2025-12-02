# format_ipv4


The `format_ipv4` function parses input with a netmask and returns string representing IPv4 address.

## Syntax

`format_ipv4( Expression [, PrefixMask ] )`

### Arguments

- **Expression**: A string or number representation (in big-endian order) of the IPv4 address.
- **PrefixMask**: An integer from 0 to 32 representing the number of most-significant bits that are taken into account.
  If argument isn't specified, all bit-masks are used (32).

## Results

If the conversion is successful, returns a [`string`](string) representing IPv4 address.

If the conversion fails, returns an empty [`string`](string).

## Examples

This example returns `"192.168.1.0"`:

```kusto {runSearch=true}
print format_ipv4('192.168.1.255', 24)
```

This example returns `"192.168.1.0"`:

```kusto {runSearch=true}
print format_ipv4(3232236031, 24)
```
