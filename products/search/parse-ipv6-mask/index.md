# parse_ipv6_mask


The `parse_ipv6_mask` function converts an IPv6/IPv4 string and netmask to a canonical IPv6 string representation.

## Syntax

`parse_ipv6_mask( Expression, PrefixMask )`

### Arguments

- **Expression**: String expression representing IPv6 that will be converted to long.
- **PrefixMask**: An integer from 0 to 128 representing the number of most-significant bits that are taken into account.

## Results

If the conversion is successful, returns a [`string`](string) representing a canonical IPv4 or IPv6 network address.

If the conversion fails, returns `null`.

## Examples

```kusto {runSearch=true}
print theData='abcd::ffff:c0a8:0102'  | extend theMask=24, parsed_data=parse_ipv6_mask(theData,theMask) | render event
```

This example returns `'0000:0000:0000:0000:0000:ffff:7f00:0000'`:

```kusto
parse_ipv6_mask("127.0.0.1", 24)
```

This example returns `'fe80:0000:0000:0000:085d:e82c:9446:7900'`:

```kusto
parse_ipv6_mask(":fe80::85d:e82c:9446:7994", 120)
```
