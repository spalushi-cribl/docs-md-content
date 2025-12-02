# Cribl Expressions


Native Cribl Stream function methods can be found under `C.*`, and can be invoked from any Function that allows for expression evaluations. For example, to create a field that is the SHA1 of a another field's value, you can use the Eval Function with this **Evaluate Fields** pair:  

Name | Value Expression
--- | ---
myNewField | `C.Mask.sha1(myOtherField)`

> Where fields' names contain special characters, you can reference them using the `__e['<field‑name‑here>']` convention. For details, see [Fields with Non-Alphanumeric Characters](introduction-reference#special-chars).
>
{.box .success}

## C.Crypto – Data Encryption and Decryption Functions

`C.Crypto.decrypt`  
(method) `Crypto.decrypt(value: string, escape: boolean, escapeSeq: string): string`  
Decrypt all occurrences of ciphers in the given value. Instances that cannot be decrypted (for any reason) are left intact.  
@param – `value` – String in which to look for ciphers.  
@param – `escape` – Boolean, defaults to `false`. Set to `true` to escape double quotes in output after decryption. (E.g., for data encrypted in Splunk.)  
@param –  `escapeSeq` – String used to escape double quotes. The default `'"'` escapes CSV output.  
@returns – `value` with ciphers decrypted.  

`C.Crypto.encrypt`  
(method) `Crypto.encrypt(value: any, keyclass: number, keyId?: string, defaultVal?: string): string`  
Encrypt the given value with the `keyId`, or with a `keyId` picked up automatically based on `keyclass`.  
@param {string | Buffer} `value` – what to encrypt.  
@param – `keyclass` – if `keyId` isn't specified, pick one at the given `keyclass`.  
@param – `keyId` - encryption keyId, takes precedence over `keyclass`.  
@param – `defaultVal` – what to return if encryption fails for any reason; if unspecified, the original value is returned.  
@returns – if encryption succeeds, the encrypted value; otherwise, `defaultVal` if specified; otherwise, `value`.

{{< id `hmac` >}}`C.Crypto.createHmac`  
(method) `Crypto.createHmac(value: string | Buffer, secret: string, algorithm: string = 'sha256', outputFormat: 'base64' | 'hex' | 'latin1' = 'hex'): string`  
Generates an [HMAC](https://en.wikipedia.org/wiki/HMAC) that can be added to events, or can be used to validate events that contain an HMAC. (Available in Cribl Stream v.3.1.2+.)  
@param – `value` – The data to encrypt, as a string. (When the `outputFormat` is invalid or undefined, this parameter is returned as the digest, via a Buffer.)  
@param} – `secret` – The secret key used to generate the MAC, as a string.  
@param – `algorithm` – The hash algorithm used to generate the MAC, as a string. Defaults to `'sha256'`. Run `openssl list -digest-algorithms` to see the list of available algorithms.  
@param – `outputFormat` – One of `'base64'`, `'hex'`, or `'latin1'`. Defaults to `'hex'`.  
@returns – The calculated HMAC digest on success; otherwise, `value`.  


## C.Decode – Data Decoding Functions

`C.Decode.base64`  
(method) `Decode.base64(val: string, resultEnc?: string): any`  
Performs base64 decoding of the given string. Returns a string or Buffer, depending on the `resultEnc` value, which defaults to `'utf8'`.  
@param – `val` – value to base64-decode.  
@param – `resultEnc` – encoding to use to convert the binary data to a string. Defaults to `'utf8'`. Use `'utf8‑valid'` to validate that result is valid UTF8; use `'buffer'` if you need the binary data in a Buffer.  

`C.Decode.gzip`  
(method) `Decode.gzip(value: any, encoding?: string): string`  
Gunzip the supplied value.  
@param – `value` – the value to gunzip.  
@param – `encoding` – encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`. Default is `'base64'`. If data is received as Buffer (from gzip with encoding:`'none'`), decoding is skipped.  

`C.Decode.hex`  
(method) `Decode.hex(val: string): number`  
Performs hex to number conversion. (Returns `NaN` if value cannot be converted to a number.)  
@param – `val` – hex string to parse to a number (e.g., "0xcafe").  

`C.Decode.uri`  
(method) `Decode.uri(val: string): string`  
Performs URI-decoding of the given string.  
@param – `val` – value to URI-decode.  


## C.Encode – Data Encoding Functions

`C.Encode.base64`  
(method) `Encode.base64(val: any, trimTrailEq?: boolean): string`  
Returns a base64 representation of the given string or Buffer.  
@param – `val` – value to base64-encode.  
@param – `trimTrailEq` – whether to trim any trailing `=`.  

`C.Encode.gzip`  
(method) `Encode.gzip(value: string, encoding?: string): any`  
Gzip, and optionally base64-encode, the supplied value.  
@param – `value` – the value to gzip.  
@param – `encoding` – encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`, `'none'`. Default is `'base64'`. If `'none'` is specified, data will be returned as a Buffer.  

`C.Encode.hex`  
(method) `Encode.hex(val: string | number): string`  
Rounds the number to an integer and returns its hex representation (lowercase). If a string is provided,  it will be parsed into a number or `NaN`.  
@param – `val` – value to convert to hex.  

`C.Encode.uri`  
(method) `Encode.uri(val: string): string`  
Returns the URI-encoded representation of the given string.  
@param – `val` – value to uri encode.  


## C.env – Environment

`C.env`  
(property) `env: {[key: string]: string;}`  
Returns an object containing Cribl Stream's environment variables.  


## C.Lookup – Inline Lookup Functions {#lookup}

`C.Lookup` – Exact Lookup   
(property) `Lookup: (file: string, primaryKey?: string, otherFields?: string[], ignoreCase?: boolean) => InlineLookup`  
Returns an instance of a lookup to use inline.   

Example invocation, where `host` is the name of the primary key field:   
`C.Lookup('lookup_name.csv', 'IP_field_name_in_lookup_file').match(host)`
  
Expanded example, where the quoted `'event_field_or_string_to_match'` could be a string to match in the primary key field:   
`C.Lookup('name_of_lookup_file.csv', 'field_in_csv_to_match').match('event_field_or_string_to_match', 'field_in_csv_to_output')`  

Example that returns a boolean – is `someValue` there or not?:  
`C.Lookup('file','primaryKeyCol',additionalCols).match('someValue')`

Example that returns a string – the `fieldToReturn` column from the matched lookup row:  
`C.Lookup('file','primaryKeyCol',additionalCols).match('someValue','fieldToReturn')`

> `C.Lookup` can load lookup files of up to 10 MB.
> 
> All inputs to Lookup functions' `match()` method must be strings. If your lookup file contains numeric fields, convert them to strings, e.g.: `.match(String(<fieldname>)`.
> 
> You can use the optional `otherFields[]` argument, shown in the above `C.Lookup()` signatures and examples, to limit which columns of the lookup table will be available in a subsequent `.match()` call. If omitted, or set to `undefined`, all columns will be available.
>
{.box .warning}

`C.LookupCIDR` – CIDR Lookup   
(property) `LookupCIDR: (file: string, primaryKey?: string, otherFields?: string[]) => InlineLookup`  
Returns an instance of a CIDR lookup to use inline.  

`C.LookupIgnoreCase` – Case-insensitive Lookup  
(property) `LookupIgnoreCase: (file: string, primaryKey?: string, otherFields?: string[]) => InlineLookup`  
Returns an instance of a lookup (ignoring case) to use inline. Works identically to `C.Lookup`, except ignores the case of lookup values. (Equivalent to calling `C.Lookup` with its fourth `ignoreCase?` parameter set to `true`).

`C.LookupRegex` – Regex Lookup   
(property) `LookupRegex: (file: string, primaryKey?: string, otherFields?: string[]) => InlineLookup`  
Returns an instance of a Regex lookup to use inline.


(method) `InlineLookup.match(value: string, fieldToReturn?: string): any`  
@param – `value` – the value to look up.  
@param – `fieldToReturn` – name of the lookup file > field to return.  

E.g., `C.Lookup('lookup-exact.csv', 'foo').match('abc', 'bar')`   
Return the value of field **bar** in the lookup table if field **foo** matches `abc`.   

Example 1: `C.LookupCIDR('lookup-cidr.csv', 'foo').match('192.168.1.1', 'bar')`   
Return the value of field **bar** in the lookup table if the CIDR range in **foo** includes `192.168.1.1`.   

Example 2: `C.LookupCIDR('lookup-cidr.csv', 'cidr').match(hostIP, 'location')`  
Return the value of column **location** in the lookup table if the **location** includes `hostIP`.

Example 3: `C.LookupRegex('lookup-regex.csv', 'foo').match('manchester', 'bar')`   
Return the value of field **bar** in the lookup table if the regex in **foo** matches the string `manchester`.   

> With `C.LookupRegex`, ensure that your lookup file contains no empty lines – not even at the bottom. Any empty row will cause `C.LookupRegex().match()` to always return `true`.
>
{.box .warning}

## C.Mask – Data Masking Functions

`C.Mask.CC`
(method) `Mask.CC(value: string, unmasked?: number, maskChar?: string): string`  
Check whether a value could be a valid credit card number, and mask a subset of the value. By default, all digits except the last 4 will be replaced with `X`.  
@param – `value` – a string whose digits should be masked, **if and only if** it could be a valid credit card number.  
@param – `unmasked` – number of digits to leave unmasked: positive for left, negative for right, `0` for none.  
@param – `maskChar` – a string/char to replace a digit with.  

`C.Mask.IMEI`
(method) `Mask.IMEI(value: string, unmasked?: number, maskChar?: string): string`  
Check whether a value could be a valid IMEI number, and mask a subset of the value. By default, all digits except the last 4 will be replaced with `X`.  
@param – `value` – a string whose digits should be masked, **if and only if** it could be a valid IMEI number.  
@param – `unmasked` – number of digits to leave unmasked: positive for left, negative for right, `0` for none.  
@param – `maskChar` – a string/char to replace a digit with.

`C.Mask.isCC`  
(method) `Mask.isCC(value: string): boolean`  
Checks whether the given value could be a valid credit card number, by computing the string's Lunh's checksum modulo 10 == `0`.  
@param – `value` – a string to check for being a valid credit card number.

`C.Mask.isIMEI`  
(method) `Mask.isIMEI(value: string): boolean`  
Checks whether the given value could be a valid IMEI number, by computing the string's Lunh's checksum modulo 10 == `0`.  
@param – `value` – a string to check for being a valid IMEI number

`C.Mask.luhn`  
(method) `Mask.luhn(value: string, unmasked?: number, maskChar?: string): string`  
Check that value Lunh's checksum mod 10 is `0`, and mask a subset of the value. By default, all digits except the last 4 will be replaced with `X`. If the value's Lunh's checksum mod 10 is not `0`, then the value is returned unmodified.  
@param – `value` – a string whose digits should be masked, **if and only if** the value's Lunh's checksum mod 10 is `0`.  
@param – `unmasked` – number of digits to leave unmasked: positive for left, negative for right, `0` for none.  
@param – `maskChar` – a string/char to replace a digit with.

`C.Mask.LUHN_SUB`
(property) `Mask.LUHN_SUB: any`

`C.Mask.luhnChecksum`  
(method) `Mask.luhnChecksum(value: string, mod?: number): number`  
Generates the Luhn checksum (used to validate certain credit card numbers, IMEIs, etc.). By default, the mod 10 of the checksum is returned. Pass mod = `0` to get the actual checksum.  
@param – `value` – a string whose digits you want to perform the Lunh checksum on.  
@param – `mod` – return checksum modulo this number. If `0`, skip modulo. Default is `10`.

`C.Mask.md5`  
(method) `Mask.md5(value: string, len?: string | number): string`  
Generate MD5 hash of a given value.  
@param – `value` – compute the hash of this.  
@param – `len` – length of hash to return: 0 for full hash, a +number for left or a -number for right substring. If a string is passed it's length will be used.

`C.Mask.random`  
(method) `Mask.random(len?: string | number): string`  
Generates a random alphanumeric string.  
@param – `len` – a number indicating the length of the result; or, if a string, use its length.  

`C.Mask.REDACTED`  
(property) `Mask.REDACTED: string`  
The literal `'REDACTED'`.

`C.Mask.repeat`  
(method) `Mask.repeat(len?: string | number, char?: string): string`  
Generates a repeating char/string pattern, e.g., `XXXX`.  
@param – `len` – a number indicating the length of the result; or, if a string, use its length.  
@param – `char` – pattern to repeat `len` times.  

`C.Mask.sha1`
(method) `Mask.sha1(value: string, len?: string | number): string`
Generate SHA1 hash of given value.  
@param – `value` - compute the hash of this.  
@param – `len` - length of hash to return: `0` for full hash, a +number for left, or a -number for right.  
substring. If a string is passed, its length will be used

## C.Misc – Miscellaneous Utility Functions

`C.Misc.zip()`  
(method) `Misc.zip(keys: string[], values: any[], dest?: any): any`  
Set the given keys to the corresponding values on the given `dest` object. If `dest` is not provided, a new object will be constructed.  
@param – `keys` – field names corresponding to keys.  
@param – `values` – values corresponding to values.  
@param – `dest` – object on which to set field values.  
@returns – object on which the fields were set.

E.g., `people = C.Misc.zip(titles, names)`   
Sample data: `titles=['ceo', 'svp', 'vp']`, `names=['foo', 'bar', 'baz']`  
Create an object called `people`, with key names from elements in `titles`, and with corresponding values from elements in `names`.  
Result: `"people": {"ceo": "foo", "svp": "bar", "vp": "baz"}`

## C.Net – Network Functions

`C.Net.cidrMatch()`  
(method) `Net.cidrMatch(cidrIpRange: string, ipAddress: string): boolean`  
Determines whether the supplied IPv4 `ipAddress` is inside the range of addresses identified by `cidrIpRange`. For example: `C.Net.cidrMatch ('10.0.0.0/24', '10.0.0.100')` returns `true`.  
@param – `cidrIpRange` – IPv4 address range in CIDR format. E.g., `10.0.0.0/24`.  
@param – `ipAddress` – The IPv4 IP address to test for inclusion in `cidrIpRange`.  

`C.Net.isIpV4()`  
(method) `Net.isIpV4(value: string): boolean`  
Determine if the value supplied is an IPv4 address. (Available in Cribl Stream v.3.1.2+.)  
@param – `value` – the IP address to test.

`C.Net.isIpV6()`  
(method) `Net.isIpV6(value: string): boolean`  
Determine if the value supplied is an IPv6 address. (Available in Cribl Stream v.3.1.2+.)  
@param – `value` – the IP address to test.


`C.Net.isIpV4AllInterfaces()`  
(method) `Net.isIpV4AllInterfaces(value: string): boolean`  
Determine if the value supplied is the IPv4 all-interfaces address, typically used to bind to all IPv4 interfaces – i.e., '0.0.0.0'. (Available in Cribl Stream v.3.1.2+.)
@param – `value` – the IP address to test.

`C.Net.isIpV6AllInterfaces()`  
(method) `Net.isIpV6AllInterfaces(value: string): boolean`  
Determine if the value supplied is an IPv6 all-interfaces address, typically used to bind to all IPv6 interfaces – one of: '::', '0:0:0:0:0:0:0:0', or '0000:0000:0000:0000:0000:0000:0000:0000'. (Available in Cribl Stream v.3.1.2+.)  
@param – `value` – the IP address to test.

`C.Net.ipv6Normalize()`  
(method) `Net.ipv6Normalize(address: string): string`  
Normalize an IPV6 address based on [RFC draft-ietf-6man-text-addr-representation-04](https://tools.ietf.org/html/draft-ietf-6man-text-addr-representation-04).  
@param – `address` – the IPV6 address to normalize.  

`C.Net.isPrivate()`  
(method) `Net.isPrivate(address: string): string`  
Determine whether the supplied IPv4 address is in the range of private addresses per [RFC1918](https://tools.ietf.org/html/rfc1918).  
@param – `address` –  address to test.

## C.os – System Functions

`C.os.hostname()`  
Returns hostname of the system running this Cribl Stream instance.  

## C.Schema – Schema Functions

`C.Schema()`  
(property) `Schema: (id: string) => SchemaValidator`  
(method) `SchemaValidator.validate(data: any): boolean`  
Validates the given object against the schema.  
@param – `data` – object to be validated.  
@returns – `true` when schema is valid; otherwise, `false`.  
Example: `C.Schema('schema1').validate(myField)` will validate if `myField` object conforms to `schema1`.

See [Schema Library](schema-library) for more details.


##  C.Secret – Secrets‑Management Functions  {#secret}


`C.Secret()`  
(method) `Secret: (id: string, type?: string): ISecret`  
(method) `Secret(id: string, type: 'keypair') => IPairSecret`  
(method) `Secret(id: string, type: 'text') => ITextSecret`  
(method) `Secret(id: string, type: 'credentials') => ICredentialsSecret`  
Returns a secret matching the specified ID.  
@param `id` – ID of the secret.  
@param `type` – optional type of the secret.  

Examples: 
- `C.Secret('victorias', 'text')` will return a text secret with ID `victorias` (or with undefined, if no such secret exists).
- `C.Secret('api_key', 'keypair').secretKey`  
- `C.Secret('secret_hash', 'text').value`  
- `C.Secret('user_pass', 'credentials').password`

Common returned attributes for `ISecret` objects:  
- `secretType` – one of `keypair`, `text`, or `credentials`.  
- `description` (optional) – the secret description.  
- `tags` (optional) – a comma separated list of tags.

Additional returned attributes for `IPairSecret` objects:  
- `apiKey` – the API key value  
- `secretKey` – the Secret key value  

Additional returned attributes for `ITextSecret` objects:  
- `value` – the text value

Additional returned attributes for `ICredentialsSecret` objects:  
- `username` – the username value  
- `password` – the password value

See [Securing Cribl Stream > Secrets](/stream/securing-and-monitoring#secrets) for more details.

## C.Text – Text Functions

`C.Text.entropy()`  
(method) `Text.entropy(bytes: any): number`  
Computes the Shannon entropy of the given buffer or string.  
@param – `bytes` – value to undergo Shannon entropy computation.  
@returns – the entropy value; or `-1` in case of an error.  

`C.Text.hashCode()`  
(method) `Text.hashCode(val: string | Buffer | number): number`  
Computes hashcode (djb2) of the given value.  
@param – `val` - value to be hashed.  
@returns – hashcode value.  

`C.Text.isASCII()`  
(method) `Text.isASCII(bytes: any): boolean`  
Checks whether all bytes or chars are in the ASCII printable range.  
@param – `bytes` – value to check for character range.  
@returns – `true` if all chars/bytes are within ASCII printable range; otherwise, `false`.

`C.Text.isUTF8()`  
(method) `Text.isUTF8(bytes: any): boolean`  
Checks whether the given Buffer contains valid UTF8.  
@param – `bytes` – bytes to check.  
@returns – `true` if bytes are UTF8; otherwise, `false`.


{{< id `parse-win-xml` >}}`C.Text.parseWinEvent`  
(method) `C.Text.parseWinEvent(xml: string, nonValues?: string[]): any`  
Parses an XML string representing a Windows event into a compact, prettified JSON object. Works like [`C.Text.parseXml`](#parse-xml), but with Windows events, produces more-compact output. For a usage example, see [Reducing Windows XML Events](/stream/usecase-win-xml).  
@param – `xml` – an XML string; or an event field containing the XML.  
@param – `nonValues` – array of string values. Elements whose value equals any of the values in this array will be omitted from the returned object. Defaults to `['-']`, meaning that elements whose value equals `-` will be discarded.  
@returns – an object representing the parsed Windows Event; or `undefined` if the input could not be parsed.


{{< id `parse-xml` >}}`C.Text.parseXml`  
(method) `C.Text.parseXml(xml:string, keepAttr?:boolean, keepMetadata?:boolean, nonValues?:string[]): any`  
Parses an XML string and returns a JSON object. Can be used with [Eval](eval-function) Function to parse XML fields contained in an event, or with ad hoc XML.  
@param – `xml` – XML string, or an event field containing the XML.  
@param – `keepAttr` – whether or not to include attributes in the returned object. Defaults to `true`.  
@param – `keepMetadata` – whether or not to include metadata found in the XML. The `keepAttr` parameter must be set to `true` for this to work. Defaults to `false`. (Eligible metadata includes namespace definitions and prefixes, and XML declaration attributes such as encoding, version, etc.)  
@param – `nonValues` – array of string values. Elements whose value equals any of the values in this array will be omitted from the returned object. Defaults to `[]` (empty array), meaning discard no elements.  
@returns – an object representing the parsed XML; or `undefined` if the input could not be parsed. An input collection of elements will be parsed into an array of objects.

{{< id `relative-entropy` >}}`C.Text.relativeEntropy()`  
(method) `Text.relativeEntropy(bytes: any, modelName?: string): number`  
Computes the relative entropy of the given buffer or string.  
@param – `bytes` – Value whose relative entropy to compute.  
@param – `modelName` – Optionally, override the default `$CRIBL_HOME/data/lookups/model_relative_entropy_top_domains.csv` model used to test the input. Create a custom lookup file with the same column and value structure as the default, and store it in the same path, as `model_relative_entropy_<custom‑name>.csv`. To reference it, pass your `<custom‑name>` substring as the `modelName` parameter.   
@returns – The relative entropy value, or `-1` in case of an error.

> When using `modelName` in a [distributed deployment](/stream/deploy-distributed), the corresponding paths are `$CRIBL_HOME/groups/<worker–group‑name>/data/lookups/`. Creating your custom lookup file [via Cribl Stream's UI](lookups-library) will automatically set the appropriate paths.
>
{.box .success}

## C.Time – Time Functions {#time}

`C.Time.adjustTZ()`  
(method) `Time.adjustTZ(epochTime: number, tzTo: string, tzFrom?: string): number`  
Adjust a timestamp from one timezone to another.  
@param – `epochTime` – UNIX epoch time.  
@param – `tzTo` – timezone to adjust to.  
@param – `tzFrom` – optional timezone of the timestamp.  
@returns – the adjusted timestamp, in UNIX epoch time (ms).

> Cribl relies on [this list of TZ Database Time Zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List).
>
{.box .success}


{{< id `clamp` >}}`C.Time.clamp()`  
(method) `Time.clamp(date, earliest, latest, defaultDate?): number`  
Constrains an event's parsed timestamp to realistic earliest and latest boundaries.  
@param – `date` – Timestamp originally parsed from event, in UNIX epoch time (ms) or JavaScript Date format.  
@param – `earliest` – earliest allowable timestamp, in UNIX epoch time (ms) or JS Date format.  
@param – `latest` – latest allowable timestamp, in UNIX epoch time (ms) or JS Date format.  
@param – `defaultDate` – optional default date, in UNIX epoch time (ms) or JS Date format, to substitute for values outside the `earliest` or `latest` boundaries.

`C.Time.strftime()`  
(method) `Time.strftime(date: number | Date, format: string, utc?: boolean): string`  
Format a [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object or number as a time string, using [strftime specifier](https://github.com/d3/d3-time-format#api-reference).  
@param – `date` – [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object or number (seconds since epoch) to format.  
@param – `format` – specifier to use to format the date.  
@param – `utc` – whether to output the time in UTC, rather than in local timezone.  
@returns – representation of the given date.

`C.Time.strptime()`  
(method) `Time.strptime(str: string, format: string, utc?: boolean, strict?: boolean): Date`  
Extract time from a string using [strptime specifier](https://github.com/d3/d3-time-format#locale_format).  
@param – `str` – string to parse to a timestamp (see strict flag).  
@param - `format` – `strptime` specifier.  
@param – `utc` – whether to interpret times as UTC, rather than as local time.  
@param – `strict` – whether to return `null` if there are any extra characters after timestamp.  
@returns – a parsed [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) object, if successful; otherwise, `null` if the specifier did not match.

{{< id `time-finder` >}}`C.Time.timestampFinder()`  
(method) `Time.timestampFinder(utc?: boolean).find(<source‑field>): AutoTimeParser`  
Extract time from the specified `<source‑field>`, using the same algorithm as the [Auto Timestamp](auto-timestamp-function) Function and the [Event Breaker](event-breakers#timestamp-settings) Function.  
@param – `utc` – whether to output the time in UTC, rather than in local timezone.  
@param – `<source‑field>` – the field in which to search for the time.  
@returns – representation of the extracted time; truncates timestamps to three-digit (milliseconds) resolution, omitting trailing zeros. 

## C.vars – Global Variables 

See [Global Variables Library](global-variables-library) for more details. 

## C.version – Cribl Stream Versions

`C.version`   
(property) `version: string`  
Returns the Cribl Stream version currently running.  

`C.confVersion`  
(property) `version: string`  
Returns the commit hash of the Worker Node's current config version. (Evaluates only against live data sent through Worker Nodes. Values will be undefined in the Leader's Preview pane.)
