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

## `C.Net.isIpV4()`

```js
Net.isIpV4(value: string): boolean
```

Determines if the value supplied is an IPv4 address.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The IP address to test.

## `C.Net.isIpV6()`

```js
Net.isIpV6(value: string): boolean
```

Determines if the value supplied is an IPv6 address.

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

Determines if the value supplied is the IPv4 all-interfaces address, typically used to bind to all IPv4 interfaces – i.e., '0.0.0.0'.

|Parameter|Type|Description|
|---|---|---|
|`value` | any | The IP address to test.

## `C.Net.isIpV6AllInterfaces()`

```js
Net.isIpV6AllInterfaces(value): boolean
```

Determines if the value supplied is an IPv6 all-interfaces address, typically used to bind to all IPv6 interfaces – one of: '::', '0:0:0:0:0:0:0:0', or '0000:0000:0000:0000:0000:0000:0000:0000'.

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
