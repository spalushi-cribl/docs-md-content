# C.Text – Text Methods


## `C.Text.entropy()`

```js
C.Text.entropy(bytes: Buffer | string): number
```

Computes the Shannon entropy of the given Buffer or string.

Returns the entropy value; or `-1` in case of an error.

|Parameter|Type|Description|
|---|---|---|
|`bytes` | Buffer \| string | value to undergo Shannon entropy computation. |

## `C.Text.hashCode()`

```js
C.Text.hashCode(val: string | Buffer | number): number
```

Computes hashcode (djb2) of the given value.

Returns hashcode value.

|Parameter|Type|Description|
|---|---|---|
|`val` | string \| Buffer \| number | value to be hashed.

## `C.Text.isASCII()`

```js
C.Text.isASCII(bytes: Buffer | string): boolean
```

Checks whether all bytes or chars are in the ASCII printable range.

Returns `true` if all chars/bytes are within ASCII printable range; otherwise, `false`.

|Parameter|Type|Description|
|---|---|---|
|`bytes` | string \| Buffer | value to check for character range.

## `C.Text.isUTF8()`

```js
C.Text.isUTF8(bytes: Buffer | string): boolean
```

Checks whether the given Buffer contains valid UTF8.

Returns `true` if bytes are UTF8; otherwise, `false`.

|Parameter|Type|Description|
|---|---|---|
|`bytes` | Buffer \| string | bytes to check.

## `C.Text.parseWinEvent()` {#parse-win-event}

```js
C.Text.parseWinEvent(xml: string, nonValues: string[] = C.Text._WIN_EVENT_NON_VALUES): any
```

Parses an XML string representing a Windows event into a compact, prettified JSON object. Works like [`C.Text.parseXml`](#parse-xml), but with Windows events, produces more-compact output. For a usage example, see [Reducing Windows XML Events](/stream/usecase-win-xml).

Returns an object representing the parsed Windows Event; or `undefined` if the input could not be parsed.

|Parameter|Type|Description|
|---|---|---|
|`xml` | string | an XML string; or an event field containing the XML.
|`nonValues` | string[] | array of string values. Elements whose value equals any of the values in this array will be omitted from the returned object. Defaults to `['-']`, meaning that elements whose value equals `-` will be discarded.

## `C.Text.parseXml()` {#parse-xml}

```js
C.Text.parseXml(xml: string, keepAttr: boolean = true, keepMetadata: boolean = false, nonValues: string[] = []): any
```

Parses an XML string and returns a JSON object. Can be used with [Eval](eval-function) Function to parse XML fields contained in an event, or with ad hoc XML.

Returns an object representing the parsed XML; or `undefined` if the input could not be parsed. An input collection of elements will be parsed into an array of objects.

|Parameter|Type|Description|
|---|---|---|
|`xml` | string | XML string, or an event field containing the XML.
|`keepAttr` | boolean | whether or not to include attributes in the returned object. Defaults to `true`.
|`keepMetadata` | boolean | whether or not to include metadata found in the XML. The `keepAttr` parameter must be set to `true` for this to work. Defaults to `false`. (Eligible metadata includes namespace definitions and prefixes, and XML declaration attributes such as encoding, version, etc.)
|`nonValues` | string[] | array of string values. Elements whose value equals any of the values in this array will be omitted from the returned object. Defaults to `[]` (empty array), meaning discard no elements.

## `C.Text.relativeEntropy()` {#relative-entropy}

```js
C.Text.relativeEntropy(bytes: Buffer | string, modelName: string = 'top_domains'): number
```

Computes the relative entropy of the given Buffer or string.

Returns the relative entropy value, or `-1` in case of an error.

|Parameter|Type|Description|
|---|---|---|
|`bytes` | Buffer \| string | Value whose relative entropy to compute.
|`modelName` | string | Optionally, override the default `$CRIBL_HOME/data/lookups/model_relative_entropy_top_domains.csv` model used to test the input. Create a custom lookup file with the same column and value structure as the default, and store it in the same path, as `model_relative_entropy_<custom‑name>.csv`. To reference it, pass your `<custom‑name>` substring as the `modelName` parameter.

> When using `modelName` in a [distributed deployment](/stream/deploy-distributed), the corresponding paths are `$CRIBL_HOME/groups/<worker–group‑name>/data/lookups/`. Creating your custom lookup file [via Cribl Edge's UI](lookups-library) will automatically set the appropriate paths.
>
{.box .success}