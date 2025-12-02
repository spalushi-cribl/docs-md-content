# ipv4_is_private



The `ipv4_is_private` function checks if IPv4 string address belongs to a set of private network IPs.

[Private network addresses](https://en.wikipedia.org/wiki/Private_network) were originally defined to assist in delaying IPv4 address exhaustion. IP packets originating from or addressed to a private IP address cannot be routed through the public internet.

## Syntax

    `ipv4_is_private( Expression )`

### Arguments

* **Expression**: A string expression representing an IPv4 address. Denote a subnet mask with a forward slash `/`.

## Results

* `true`: If the IPv4 address belongs to any of the private network ranges.
* `false`: Otherwise.
* `null`: If parsing of the input as IPv4 address string wasn't successful.

## Examples

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_private('192.168.1.1/24')
```

This example returns `true`:

```kusto {runSearch=true}
print ipv4_is_private('10.1.2.3/24')
```

This example returns `false`:

```kusto {runSearch=true}
print ipv4_is_private('202.1.2.3')
```

This example returns `false`:

```kusto {runSearch=true}
print ipv4_is_private("127.0.0.1")
```
