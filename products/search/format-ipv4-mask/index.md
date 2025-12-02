# format_ipv4_mask


The `format_ipv4_mask` function parses input with a netmask and returns string representing IPv4 address as CIDR
notation.

## Syntax

`format_ipv4_mask( Expression [, PrefixMask ] )`

### Arguments

- **Expression**: A string or number representation (in big-endian order) of the IPv4 address as CIDR notation.
- **PrefixMask**: An integer from 0 to 32 representing the number of most-significant bits that are taken into account.
  If argument isn't specified, all bit-masks are used (32).

## Results

If the conversion is successful, returns a [`string`](string) representing the IPv4 address in CIDR notation.

If the conversion fails, returns an empty [`string`](string).

## Examples

This example returns `"192.168.1.0/24"`:

```kusto {runSearch=true}
print format_ipv4_mask('192.168.1.255', 24)
```

This example returns `"192.168.1.0/24"`:

```kusto {runSearch=true}
print format_ipv4_mask(3232236031, 24)
```
