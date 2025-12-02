# DNS Lookup


The DNS Lookup Function offers two operations useful in enriching security and other data:

- DNS lookups based on host name as text, resolving to A record (IP address) or to other record types.

- Reverse DNS Lookup. (This duplicates the behavior of Cribl Stream's prior [Reverse DNS](reverse-dns-function) Function, which was deprecated as of v.2.4, and has been removed as of v.4.0.)

To reduce DNS lookups and minimize latency, the DNS Lookup Function incorporates a configurable DNS cache (including resolved and unresolved lookups). If you need additional caching, consider enabling OS-level DNS caching on each Cribl Stream Worker that will execute this Function. (OS-level caching options include DNSMasq, nscd, systemd‑resolved, etc.)

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

### DNS Lookup Fields Section {#lookup}

**Lookup field name**: Name of the field containing the domain to look up.

**Resource record type**: DNS record type (RR) to return. Defaults to `A`' record.

**Output field name**: Lookup result(s) will be added to this field. Leave blank to overwrite the original field specified in **Lookup field name**.

### Reverse DNS Lookup Field(s) Section {#reverse}

**Lookup field name**: Name of the field containing the IP address to look up.

> If the field value is not in IPv4 or IPv6 format, the lookup is skipped.
>
{.box .warning}

**Output field name**: Name of the field in which to add the resolved hostname. Leave blank to overwrite the original field specified in **Lookup field name**.

### Fallback Resolving Methods

This section contains settings for resolving DNS short names.

**Use /etc/resolv.conf**: Toggle to `Yes` if you want Cribl Stream to resolve DNS short names using the search or domain directive from `/etc/resolv.conf`. Defaults to `No`. Not applicable for Windows hosts.

**Use search or domain fallback(s)**: Add fallback value(s) for the DNS resolver to use when it cannot resolve a DNS short name.

**Fall back to DNS.lookup()**: Toggle to `Yes` if you want Cribl Stream to make a `DNS.lookup()` call to resolve a DNS short name it can't resolve by other methods.

If you enable all of these methods at once. Cribl Stream will attempt to resolve short names by trying each method in the following order:

1. **Use /etc/resolv.conf**.
1. **Use search or domain fallback(s)**.
1. **Fall back to DNS.lookup()**.

If Cribl Stream can't resolve the short name after trying all of these methods, it will drop the output field from the event.

> ##### Potential Performance Impact
>
> Making a `DNS.lookup()` call might degrade performance in unrelated areas of Cribl Stream. Use this method only if you have not been able to resolve DNS short names using other settings.
>
{.box .warning}

### Advanced Settings

**DNS server(s) overrides**: IP address(es), in [RFC 5952](https://tools.ietf.org/html/rfc5952) format, of the DNS server(s) to use for resolution. IPv4 examples: `1.1.1.1`, `4.2.2.2:53`. IPv6 examples: `[2001:4860:4860::8888]`, `[2001:4860:4860::8888]:1053`. If this field is not specified, Cribl Stream will use the system's DNS server.

**Cache time to live (minutes)**: Determines the interval on which the DNS cache will expire, and its contents will be refetched. Defaults to `30` minutes. Use `0` to disable cache expiration/refresh behavior.



**Maximum cache size**: Maximum number of DNS resolutions to cache locally. Before changing the default `5000`, consider the implications for your system. Higher values will increase memory usage. Highest allowed value is `100000`.

## Example

This example Pipeline chains two Functions. First, we have an **Eval** Function that defines key-value pairs for two alphabetical domain names and two numeric IP addresses.

![DNS Lookup: Eval Function](dns-lookup-eval.ed1b1d203d.png)
{border="true"}

Next, the DNS Lookup Function looks up several record types for the two domain names, placing each retrieved record type in its own output field.

![DNS Lookup: multiple record types](dns-lookup-direct.f67e634147.png)
{border="true"}

Finally, the same Function's **Reverse DNS lookup** section retrieves domain names for the two IP addresses.

![DNS Lookup: reverse lookups](dns-lookup-reverse.65912b4090.png)
{border="true"}

### Working with Multi-value Results

DNS records can contain single values mixed together with multi-value results. While this can be challenging to work with, the right code can adjust your results to the desired values.

In the screenshot below, the DNS lookup for `google.com` (event 1) returns a single record, while the `cribl.cloud` lookup (event 2) returns four addresses.

![DNS lookup with multi-value result](dns-multi-first.5127fd830b.png)
{border="true"}

Sometimes a receiver is unable to consume the data in an array format. One solution to this problem is to pick the first result and convert it into a string. Here's a JavaScript eval to do this:

```javascript
Array.isArray(host) ? host[0] : host;
```

This function checks whether the value of the `host` field is an array.

* If true, the result is the left side of the ternary expression – in this case, `host[0]`, which is the first element in the array, converted into a string. See event 2 in the screenshot below.
* If false, the result is the single-value string in the `host` field. See event 1.

![DNS lookup multi-value result converted into a single value](dns-multi-second.7ac6a2e284.png)
{border="true"}

Another option is to **randomly** pick an address from the result. To do this, use a modified version of the JavaScript eval:

```javascript
Array.isArray(host) ? host[Math.floor(Math.random() * host.length)] : host
```

Here, instead of picking the first element, the code uses `Math.floor(Math.random() * host.length)` to randomly pick an element from the array.



