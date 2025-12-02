# Reverse DNS (deprecated)


The Reverse DNS Function resolves hostnames from a numeric IP address, using a reverse DNS lookup.

> This Function was deprecated as of v.2.4, and has been removed from Cribl Stream as of v.4.0. Please instead use the [DNS Lookup](dns-lookup-function#reverse) Function's reverse lookup feature.
>
{.box .warning}

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

### Lookup Fields

**Lookup field name**: Name of the field containing the IP address to look up.

> If the field value is not in IPv4 or IPv6 format, the lookup is skipped.
>
{.box .warning}

**Output field name**: Name of the field in which to add the resolved hostname. Leave blank to overwrite the lookup field.

**Reload period (minutes)**: How often to refresh the DNS cache. Use `0` to disable refreshes. Defaults to `60` minutes.

## Example

**Lookup field name**: `dest_ip`
**Output field name**: `dest_host`
**Result**: See the `dest_ip` field, and the newly created `dest_host` field, in the events.

![](reverse-dns-result.d72c3b3baa.png)
{border="true"}
