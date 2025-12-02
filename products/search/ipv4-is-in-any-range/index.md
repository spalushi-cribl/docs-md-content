# ipv4_is_in_any_range


The `ipv4_is_in_any_range` function checks whether IPv4 string address is in any of the specified IPv4 address ranges.

## Syntax

    `ipv4_is_in_any_range( Ipv4Address , Ipv4Range [, Ipv4Range ...] )`

### Arguments

* **Ipv4Address**: A string expression representing an IPv4 address.
* **Ipv4Range**: A string expression representing an IPv4 address with a subnet mask denoted with a forward slash `/`.

## Results

* `true`: If the IPv4 address is in the range of any of the specified IPv4 networks.
* `false`: Otherwise.
* `null`: If conversion for one of the two IPv4 strings wasn't successful.

## Examples

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_in_any_range('192.168.1.6', '192.168.1.1/24', '10.0.0.1/8', '127.1.0.1/16')
```

This example returns `false`:

```kusto {runSearch=true}
print ipv4_is_in_any_range('192.168.1.1', '192.168.2.1/24', '10.0.0.1/8', '127.1.0.1/16')
```
