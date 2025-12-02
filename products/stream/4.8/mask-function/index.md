# Mask


The Mask Function masks, or replaces, patterns in events. This is especially useful for redacting PII (personally identifiable information) and other sensitive data.

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Optionally, enter a simple description of this Function.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Masking Rules**: This table defines pairs of patterns to match and replace. Defaults to empty. Click **Add Rule** to add each rule. Each row of the resulting table provides the following options:

  - **Match Regex**: Pattern to replace. Supports capture groups. Use `/g` to replace all matches, e.g.: `/foo(bar)/g`
  - **Replace Expression**: A JavaScript expression or literal to replace all matching content. Capture groups can be referenced with `g` and the group number – e.g., `g2` to reference the second capture group.
  - **Enabled**: Toggle this off to disable matching individual rules, while retaining their configuration in the table. Useful for iterative development and debugging.

**Apply to Fields**: Fields on which to apply the masking rules. Defaults to `_raw`. Add more fields by typing in their names, separated by hard returns. Supports wildcards (`*`) and nested addressing. Supports double-underscore (`__`) internal fields only if individually enumerated – not via wildcards.

> Negated terms are supported. When you negate field names, the fields list is order-sensitive. E.g., `!foobar` before `foo*` means "Apply to all fields that start with `foo`, except `foobar`." However, `!foo*` before `*` means "Apply to all fields, except for those that start with `foo`."
>
{.box .info}

### Advanced Settings

**Evaluate fields**: Optionally, specify fields to add to events in which one or more of the **Masking Rules** were matched. These fields can be useful in downstream processing and reporting. You specify the fields as key–value expression pairs, like those in the [Eval Function](eval-function).

  * **Name**: Field name.
  * **Value Expression**: JavaScript expression to compute the value (can be a constant).



## Evaluating the Replace Expression

The **Replace expression** field accepts a full JS expression that evaluates to a value, so you're not necessarily limited to what's under `C.Mask`. For example, you can do conditional replacement: `` g1%2==1 ? `fieldA="odd"` : `fieldA="even"` ``

The **Replace expression** can reference other event fields as `event.<fieldName>`. For example, `` `${g1}${event.source}` ``. Note that this is slightly different from other expression inputs, where event fields are referenced without `event.` Here, we require the `event.` prefix for the following reasons: 

* We don't expect this to be a common case.
* Expanding the event in the replace context would have a high performance hit on the common path.
* There is a slight chance that there might be a `gN` field in the event.

> The output of the Mask Function is a string even when the **Replace expression** produces a number.
> 
> For example, suppose a **Replace expression** that generates a random integer is applied to a field named `bytes` and produces the number `8403`. Cribl Stream will output an event where the value of the `bytes` field is the **string** `8403`.
> 
> If you need to convert the string value back to a number, you could add a [Numerify Function](numerify-function) to your Pipeline.
>
{.box .info}

## Examples

### Example 1: Transform a String

Here, we'll simply search for the string `dfhgdfgj`, and replace that value (if found) with `Trans AM`. This will help close America’s muscle-car gap:

![Event before masking](mask-initial.c40f76d67c.png)
{scale="75%" border="true"}

Configure the Mask Function > Masking Rules as follows:

Match Regex: `dfhgdfgj`
Replace Expression: `Trans AM`

![Mask Function configuration](mask-config.482b12efc7.png)
{border="true"}

Result: Vroom vroom!

![Event after masking](mask-result.b111d26bb1.png)
{scale="60%" border="true"}

### Example 2: Mask Sensitive Data

Assume that you're ingesting data whose `_raw` fields contain unredacted Social Security numbers in the Key=Value pattern `social=#########`.

![Event with unredacted SSNs](ssn-event-raw.c8a9eb4072.png)
{border="true"}

You can use a Mask Function to run an md5 hash of the `social` keys' numeric values, replacing the original values with the hashed values. Configure the Masking Rules as follows:

Match Regex: `(social=)(\d+)`
Replace Expression: `` `${g1}${C.Mask.md5(g2)}` ``

In the first example everything in the Match regex field was replaced by the Replace Expression. However if that isn't desired then you can use capture groups in the Match Regex to define individual string components for manipulation or, alternatively, use string literals in the Replace expression for retaining any static text. Any content matching the Match Regex that is not inserted into the Replace expression will not be retained. 

In this example, `social=` is assigned to capture group g1 for later reference. The value of `social=` will be hashed by referencing it as g2 in the md5 function. If we didn't make `social=` its own capture group (or specified `social=` as a literal in the Replace Expression) then we cannot reference it using g1 in the Replace expression, the value of `social=` would instead be assigned to g1, and the entire `social=#########` string would be replaced with a hash of the social security number, which probably isn't desired because no one would know the value being hashed without a field name preceding it.

![Mask Function configuration](ssn-mask-config.2d5535ae00.png)
{border="true"}

Result: The sensitive values are replaced by their md5 hashes.

![Event with hashed SSNs](ssn-event-masked.5dd5e52d95.png)
{border="true"}

> In scenarios where you need to send unmodified values to certain Destinations (such as archival stores), you can narrow the Mask Function's scope by setting the associated [Route](routes)'s **Output** field.
> 
> For further masking examples, see [Masking and Obfuscation](/stream/usecase-masking-and-obfuscation).
>
{.box .info}

### Example 3: Replace with an Event Field

In this example, we'll replace the IP address `127.0.0.1` in the `_raw` field with the IP address `192.168.123.25` from an existing field named `source_ip`. The following is a snippet of `_raw`:

```js
ProcessId=0x0 IpAddress=127.0.0.1 IpPort=0
```

To match the IP address, we'll define three capture groups. Set **Match Regex** to: 

```js
/(IpAddress=)((?:\d{1,3}\.){3}\d{1,3})(\s)/
```

In the **Replace Expression**, we'll reference the `source_ip` field by prepending `event` to it, like this: 

```js
${g1}${event.source_ip}${g3}
``` 

Note that we've also referenced the first and third capture groups, using `g1` and `g3`. This changes `_raw` to:

```js
ProcessId=0x0 IpAddress=192.168.123.25 IpPort=0
```