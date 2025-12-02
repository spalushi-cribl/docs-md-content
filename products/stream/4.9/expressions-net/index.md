# C.Net – Network Methods


## `C.Net.cidrMatch()`

```js
Net.cidrMatch(cidrIpRange: string, ipAddress: string): boolean
```

Determines whether the supplied IPv4 or IPv6 `ipAddress` is inside the range of addresses identified by `cidrIpRange`.

|Parameter|Type|Description|
|---|---|---|
|`cidrIpRange` | string | IP address range in CIDR format. E.g., `10.0.0.0/24`.
|`ipAddress` | string | The IP address to test for inclusion in `cidrIpRange`.

### Examples

(IPv4): `C.Net.cidrMatch('10.0.0.0/24', '10.0.0.100')` returns `true`.

(IPv6): `C.Net.cidrMatch('2001:db8::/32', '2001:db8:0:0:0:ffff:0:0')` returns `true.`

## `C.Net.communityIDv1()`

```js
Net.communityIDv1(sourceIP: string, destIP: string, sourcePort: number | undefined, destPort: number | undefined, proto: string | number, seed: number = 0): string
```

Calculates the [Community ID](https://github.com/corelight/community-id-spec) for a given network flow. A Community ID is a unique identifier that is reversible – it is the same for both sides of a connection. It is calculated based on source and destination IP addresses, ports, a protocol, and (optionally) a seed (explained below). When the protocol is ICMP, the Community ID will be reversible provided that the network flow uses types with a clear request/response relationship (for example, [Echo](https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml#icmp-parameters-codes-8) and [Echo Reply](https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml#icmp-parameters-codes-0)).

### Usage

This method returns a Base64-encoded string.

Provide IP addresses – whether IPv4 or IPv6 – as strings.

When the protocol is ICMP (v4 or v6), `sourcePort` corresponds to message type and `destPort` corresponds to message code. For other Layer 3 protocols, `sourcePort` and `destPort` should be `undefined`.
  
You can specify `proto` either as a number (recommended, to avoid ambiguity) or a string. 
- If a string, `proto` must be the desired protocol's keyword as specified in the [IANA protocol registry](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml).
- For network flows coming from the Cribl Stream [NetFlow Source](sources-netflow), use the protocol name as-is – the Source will provide it in correct form.

The optional `seed` parameter defaults to `0`. If you want to generate different Community IDs for different use cases within the same flow, change `seed` to a different value for each use case. 

|Parameter|Type|Description|
|---|---|---|
| sourceIP   | string              | Source IPv4 or IPv6 address |
| destIP     | string              | Destination IPv4 or IPv6 address |
| sourcePort | number or undefined | Source port. For ICMP, use the ICMP type. For other portless protocols, use `undefined`. |
| destPort   | number or undefined | Destination port. For ICMP, use the ICMP code. For other portless protocols, use `undefined`. |
| proto      | number or string    | IP protocol number, such as `6`, `17`, or `1`. May also be the protocol name, such as `'tcp'`, `'udp'`, or `'icmp'`. |
| seed       | number              | Optional seed value; defaults to `0` |

### Examples

Bidirectional TCP flow using the protocol number.

```js
// Protocol number 6 is TCP

C.Net.communityIDv1('1.2.3.4','5.6.7.8',1122,3344,6)
// '1:wCb3OG7yAFWelaUydu0D+125CLM='

C.Net.communityIDv1('5.6.7.8','1.2.3.4',3344,1122,6)
// '1:wCb3OG7yAFWelaUydu0D+125CLM='
```

Bidirectional UDP flow using the protocol name.

```js
C.Net.communityIDv1('1.2.3.4','5.6.7.8',1122,3344,'udp')
// '1:0Mu9InQx6z4ZiCZM/7HXi2WMhOg='

C.Net.communityIDv1('5.6.7.8','1.2.3.4',3344,1122,'udp')
// '1:0Mu9InQx6z4ZiCZM/7HXi2WMhOg='
```

ICMP Echo/Echo Reply flow.

```js
// Protocol number 1 is ICMP
// Echo is Type=8, Code=0
// Echo Reply is Type=0, Code=0

// Echo (equivalent of request)
C.Net.communityIDv1('1.2.3.4','5.6.7.8',8,0,1)
// '1:crodRHL2FEsHjbv3UkRrfbs4bZ0='

// Echo Reply (equivalent of response)
C.Net.communityIDv1('5.6.7.8','1.2.3.4',0,0,1)
// '1:crodRHL2FEsHjbv3UkRrfbs4bZ0='
```

## `C.Net.isIp()`

```js
Net.isIp(value: string): boolean
```

Determines whether the value supplied is an IP address (either IPv4 or IPv6).

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The IP address to test.

## `C.Net.isIpV4()`

```js
Net.isIpV4(value: string): boolean
```

Determines whether the value supplied is an IPv4 address.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The IP address to test.

## `C.Net.isIpV6()`

```js
Net.isIpV6(value: string): boolean
```

Determines whether the value supplied is an IPv6 address.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The IP address to test.

## `C.Net.isIPV4Cidr()`

```js
Net.isIPV4Cidr(value: string): boolean
```

Determines whether a value supplied is an IPv4 CIDR range.

Returns `true` if the value is a valid IPv4 CIDR range; otherwise, `false`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String value that may be an IPv4 CIDR range.

## `C.Net.isIPV6Cidr()`

```js
Net.isIPV6Cidr(value: string): boolean
```

Determine whether a value supplied is an IPv6 CIDR range.

Returns `true` if the value is a valid IPv6 CIDR range; otherwise, `false`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String value that may be an IPv6 CIDR range.

## `C.Net.isIpV4AllInterfaces()`

```js
Net.isIpV4AllInterfaces(value): boolean
```

Determines whether the value supplied is the IPv4 all-interfaces address, typically used to bind to all IPv4 interfaces – i.e., '0.0.0.0'.

|Parameter|Type|Description|
|---|---|---|
|`value` | any | The IP address to test.

## `C.Net.isIpV6AllInterfaces()`

```js
Net.isIpV6AllInterfaces(value): boolean
```

Determines whether the value supplied is an IPv6 all-interfaces address, typically used to bind to all IPv6 interfaces – one of: '::', '0:0:0:0:0:0:0:0', or '0000:0000:0000:0000:0000:0000:0000:0000'.

|Parameter|Type|Description|
|---|---|---|
|`value` | any | the IP address to test.

## `C.Net.ipv6Normalize()`

```js
Net.ipv6Normalize(address: string): string
```

Normalizes an IPV6 address based on [RFC draft-ietf-6man-text-addr-representation-04](https://tools.ietf.org/html/draft-ietf-6man-text-addr-representation-04).

|Parameter|Type|Description|
|---|---|---|
|`address` | string | The IPV6 address to normalize.

## `C.Net.isPrivate()`

```js
Net.isPrivate(address: string): boolean
```

Determines whether the supplied IPv4 address is in the range of private addresses per [RFC1918](https://tools.ietf.org/html/rfc1918).

|Parameter|Type|Description|
|---|---|---|
|`address` | string | The IP address to test.

## `C.Net.parseAddressOrRangeString()`

```js
Net.parseAddressOrRangeString(source: string, mask?: string | number)
```

Parses a string representation of an IP or IP range.

Returns an `IPv4` or `IPv6` object.
When the `source` argument is an IP range, this method returns the first IP in the range.

|Parameter|Type|Description|
|---|---|---|
|`source` | string | The string to parse.
|`mask` | string \| number | Optional parameter containing the CIDR mask. Used only when `source` is a valid IP string.

## `C.Net.IPv4_CIDR_REGEX`

```js
Net.IPv4_CIDR_REGEX: RegExp
```

The regular expressions for matching IP CIDR.
