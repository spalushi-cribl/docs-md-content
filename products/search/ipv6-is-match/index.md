# ipv6_is_match


The `ipv6_is_match` function matches two IPv6 or IPv4 network address strings. The two IPv6/IPv4 strings are parsed and compared while accounting for the combined IP-prefix mask calculated from argument prefixes, and the optional **PrefixMask** argument.
## Syntax

    `ipv6_is_match( Expr1, Expr2 [, PrefixMask ] )`

### Arguments

* **Expr1, Expr2**: A string expression representing an IPv6 or IPv4 address. Denote a subnet mask with a forward slash `/`.
* **PrefixMask**: An integer from 0 to 128 representing the number of most-significant bits that are taken into account.

## Results

* `true`: If the long representation of the first IPv6/IPv4 string argument is equal to the second IPv6/IPv4 string argument.
* `false`: Otherwise.
* `null`: If conversion for one of the two IP strings wasn't successful.

## Examples

This example returns `true`:

```kusto {runSearch=true}
print ipv6_is_match('::ffff:7f00:1', '127.0.0.1')
```

This example returns `false`:

```kusto {runSearch=true}
print ipv6_is_match('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv6_is_match('192.168.1.1/24', '192.168.1.255/24')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv6_is_match('fe80::85d:e82c:9446:7994/127', 'fe80::85d:e82c:9446:7995/127')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv6_is_match('fe80::85d:e82c:9446:7994', 'fe80::85d:e82c:9446:7995', 127)
```
