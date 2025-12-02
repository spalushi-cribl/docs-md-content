# ipv4_is_in_range


The `ipv4_is_in_range` function checks if IPv4 string address is in IPv4-prefix notation range.

## Syntax

    `ipv4_is_in_range( Ipv4Address, Ipv4Range )`

### Arguments

* **Ipv4Address**: A string expression representing an IPv4 address.
* **Ipv4Range**: A string expression representing an IPv4 address with a subnet mask denoted with a forward slash `/`.

## Results

* `true`: If the long representation of the first IPv4 string argument is in range of the second IPv4 string argument.
* `false`: Otherwise.
* `null`: If conversion for one of the two IPv4 strings wasn't successful.

## Examples

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_in_range("127.0.0.1", "127.0.0.1")
```

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_in_range('192.168.1.6', '192.168.1.1/24')
```

This example returns `false`:

```kusto {runSearch=true}
print ipv4_is_in_range('192.168.1.1', '192.168.2.1/24')
```
