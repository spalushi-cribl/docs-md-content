# ipv6_compare


The `ipv6_compare` function compares two IPv6 or IPv4 network address strings. The two IPv6 strings are parsed and compared while accounting for the combined IP-prefix mask calculated from argument prefixes, and the optional **PrefixMask** argument.

## Syntax

    `ipv6_compare( Expr1, Expr2 [, PrefixMask ] )`

### Arguments

* **Expr1, Expr2**: A string expression representing an IPv4 or IPv6 address. Denote a subnet mask with a forward slash `/`.
* **PrefixMask**: An integer from 0 to 128 representing the number of most-significant bits that are taken into account.

## Results

* `0`: If the long representation of the first IPv6 string argument is equal to the second IPv6 string argument.
* `1`: If the long representation of the first IPv6 string argument is greater than the second IPv6 string argument.
* `-1`: If the long representation of the first IPv6 string argument is less than the second IPv6 string argument.
* `null`: If conversion for one of the two IPv6 strings wasn't successful.

## Examples

This example returns `0`:

```kusto {runSearch=true}
print ipv6_compare('::ffff:7f00:1', '127.0.0.1')
```

This example returns `-1`:

```kusto {runSearch=true}
print ipv6_compare('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995')
```

This example returns `0`:

```kusto {runSearch=true}
print ipv6_compare('192.168.1.1/24', '192.168.1.255/24')
```

This example returns `0`:

```kusto {runSearch=true}
print ipv6_compare('fe80::85d:e82c:9446:7994/127', 'fe80::85d:e82c:9446:7995/127')
```

This example returns `0`:

```kusto {runSearch=true}
print ipv6_compare('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995', 127)
```
