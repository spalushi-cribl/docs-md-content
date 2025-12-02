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




