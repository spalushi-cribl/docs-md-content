# ipv4_is_match


The `ipv4_is_match` function matches two IPv4 strings. The two IPv4 strings are parsed and compared while accounting for the combined IP-prefix mask calculated from argument prefixes, and the optional **PrefixMask** argument.
## Syntax

    `ipv4_is_match( Expr1, Expr2 [, PrefixMask ] )`

### Arguments

* **Expr1, Expr2**: A string expression representing an IPv4 address. Denote a subnet mask with a forward slash `/`.
* **PrefixMask**: An integer from 0 to 32 representing the number of most-significant bits that are taken into account.

## Results

* `true`: If the long representation of the first IPv4 string argument is equal to the second IPv4 string argument.
* `false`: Otherwise.
* `null`: If conversion for one of the two IPv4 strings wasn't successful.

## Examples

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_match("127.0.0.1", "127.0.0.1")
```

This example returns `false`:

```kusto {runSearch=true}
print ipv4_is_match('192.168.1.1', '192.168.1.255')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_match('192.168.1.1/24', '192.168.1.255/24')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_match('192.168.1.1', '192.168.1.255', 24)
```
