# C.Lookup – Inline Lookup Methods


## `C.Lookup` – Exact Lookup

```js
C.Lookup: (file: string, primaryKey?: string, otherFields: string[]=[],ignoreCase: boolean = false) => InlineLookup
```

Returns an instance of a lookup to use inline.

### Examples

In this example, `host` is the name of the primary key field:

```js
C.Lookup('lookup_name.csv', 'IP_field_name_in_lookup_file').match(host)
```

Here, the quoted `'event_field_or_string_to_match'` could be a string to match in the primary key field:

```js
C.Lookup('name_of_lookup_file.csv', 'field_in_csv_to_match').match('event_field_or_string_to_match', 'field_in_csv_to_output')
```

This example checks whether `someValue` is present or not and returns the answer as a boolean.

```js
C.Lookup('file', 'primaryKeyCol', additionalCols).match('someValue')
```

This expression returns a string – the `fieldToReturn` column from the matched lookup row:

```js
C.Lookup('file', 'primaryKeyCol', additionalCols).match('someValue','fieldToReturn')
```

This expression returns an array – the specified `fieldToReturn1` and `fieldToReturn2` columns from the matched lookup row:

```js
C.Lookup('file', 'primaryKeyCol').match('someValue', ['fieldToReturn1', 'fieldToReturn2'])
```

To return an array with all columns from the matched lookup row, you can use:

```js
C.Lookup('file', 'primaryKeyCol').match('someValue', [])
```

> `C.Lookup` can load lookup files of up to 10 MB.
>
> All inputs to Lookup methods' `match()` method must be strings. If your lookup file contains numeric fields, convert them to strings, e.g.: `.match(String(<fieldname>)`.
>
> You can use the optional `otherFields[]` argument, shown in the above `C.Lookup()` signatures and examples, to limit which columns of the lookup table will be available in a subsequent `.match()` call. If omitted, or set to `undefined`, all columns will be available.
>
> Cribl Edge 4.1 and newer support returning all columns from a matched row. Use the empty-array convention shown in the above examples: `.match('someValue',[])`.
>
{.box .warning}

## `C.LookupCIDR` – CIDR Lookup

```js
C.LookupCIDR: (file: string, primaryKey?: string, otherFields: string[]=[]) => InlineLookup
```

Returns an instance of a CIDR lookup to use inline.

## `C.LookupIgnoreCase` – Case-insensitive Lookup

```js
C.LookupIgnoreCase: (file: string, primaryKey?: string, otherFields: string[]=[]) => InlineLookup
```

Returns an instance of a lookup (ignoring case) to use inline. Works identically to `C.Lookup`, except ignores the case of lookup values. (Equivalent to calling `C.Lookup` with its fourth `ignoreCase?` parameter set to `true`).

## `C.LookupRegex` – Regex Lookup

```js
C.LookupRegex: (file: string, primaryKey?: string, otherFields: string[]=[]) => InlineLookup
```

Returns an instance of a Regex lookup to use inline.

## `C.Lookup.match()`

```js
InlineLookup.match(value: string): boolean

InlineLookup.match(value: string, fieldToReturn?: string): any
InlineLookup.match(value: string, fieldToReturn: string[]): {[key: string]: any}
```

|Parameter|Type|Description|
|---|---|---|
|`value` | string | the value to look up.
|`fieldToReturn` | string \| string [] | name of the lookup file > field to return.

### Examples

To find the last row where the value from column `foo` matches the string `abc`, you can use the following expression.
If a matching row is found, the expression returns the row's value for column `bar`.

```js
C.Lookup('lookup-exact.csv', 'foo').match('abc', 'bar')
```

To find the last row where the value from column `transport` matches the string `tcp`, you can use the following expression.
If a matching row is found, the expression returns the row's value for column `port_number`.

```js
C.Lookup('service_names_port_numbers.csv', 'transport').match('tcp', 'port_number')
```

You can find the last row where the CIDR range in `cidr` includes `192.168.1.1` with the following expression.
If a matching row is found, the expression returns the row's value for column `bar`.

```js
C.LookupCIDR('lookup-cidr.csv', 'cidr').match('192.168.1.1', 'bar')
```

The following expression finds the last row where the value from column `cidr` matches `hostIP`.
If a matching row is found, it returns the row's value for column `location`.

```js
C.LookupCIDR('lookup-cidr.csv', 'cidr').match(hostIP, 'location')
```

To find the last row where the regex in column `foo` matches the string `manchester`, use the following expression.
If a matching row is found, the expression returns the row's value for column `bar`.

```js
C.LookupRegex('lookup-regex.csv', 'foo').match('manchester', 'bar')
```

> With `C.LookupRegex`, ensure that your lookup file contains no empty lines – not even at the bottom. Any empty rows will cause `C.LookupRegex().match()` to always return `true`.
>
{.box .warning}
