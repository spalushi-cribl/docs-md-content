# parse_ipv4_mask


The `parse_ipv4_mask` function converts the input string of IPv4 and netmask to a signed, 64-bit wide, long number
representation in big-endian order.

## Syntax

`parse_ipv4_mask( Expression, PrefixMask )`

### Arguments

- **Expression**: String expression representing IPv4 that will be converted to long.
- **PrefixMask**: An integer from 0 to 32 representing the number of most-significant bits that are taken into account.

## Results

If the type conversion is successful, returns a [`long`](long) number.

If the conversion fails, returns `null`.

## Examples

This example returns `3232235776`:

```kusto {runSearch=true}
print theData='192.168.1.2'  | extend theMask=24, parsed_data=parse_ipv4_mask(theData,theMask) | render event
```

This example returns `2130706432`:

```kusto
parse_ipv4_mask("127.0.0.1", 24)
```

This example returns `parse_ipv4_mask('192.1.168.3', 31)`:

```kusto
parse_ipv4_mask('192.1.168.2', 31)` returns as `parse_ipv4_mask('192.1.168.3', 31)
```
