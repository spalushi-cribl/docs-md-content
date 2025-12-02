# ipv4_netmask_suffix


The `ipv4_netmask_suffix` function returns the value of the IPv4 netmask suffix from IPv4 string address.

[Private network addresses](https://en.wikipedia.org/wiki/Private_network) were originally defined to assist in delaying IPv4 address exhaustion. IP packets originating from or addressed to a private IP address cannot be routed through the public internet.

## Syntax

    `ipv4_netmask_suffix( Expression )`

### Arguments

* **Expression**: A string expression representing an IPv4 address. Denote a subnet mask with a forward slash `/`.

## Results

* The value of the netmask suffix of the IPv4 address. If suffix is not present in the input, a value of `32` (full netmask suffix) is returned.
* `null`: If parsing of the input as IPv4 address string wasn't successful.

## Examples

This example returns `24`:

```kusto {runSearch=true}
print ipv4_netmask_suffix('192.168.1.1/24')
```

This example returns `32`:

```kusto {runSearch=true}
print ipv4_netmask_suffix('192.168.1.1')
```
