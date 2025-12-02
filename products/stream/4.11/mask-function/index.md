# Mask


The Mask Function replaces patterns in events, a process called *masking*. Masking is especially useful for redacting PII (personally identifiable information) and other sensitive data.

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Optionally, enter a simple description of this Function.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Masking rules**: This table defines pairs of patterns to match and replace. Defaults to empty. Select **Add Rule** to add each rule. Each row of the resulting table provides the following options:

  - **Match Regex**: Pattern to replace. Supports capture groups. Use `/g` to replace all matches, like so: `/foo(bar)/g`
  - **Replace Expression**: A JavaScript expression or literal to replace all matching content. Capture groups can be referenced with `g` and the group number – for example, `g2` to reference the second capture group.
  - **Enabled**: Toggle this off to disable matching individual rules, while retaining their configuration in the table. Useful for iterative development and debugging.

**Apply to fields**: Fields on which to apply the masking rules. Defaults to `_raw`. Add more fields by typing in their names, separated by hard returns. Supports wildcards (`*`) and nested addressing. Supports double-underscore (`__`) internal fields only if individually enumerated – not via wildcards.

> Negated terms are supported. When you negate field names, the fields list is order-sensitive. For example, `!foobar` before `foo*` means "Apply to all fields that start with `foo`, except `foobar`." However, `!foo*` before `*` means "Apply to all fields, except for those that start with `foo`."
>
{.box .info}

### Advanced Settings

**Evaluate fields**: Optionally, specify fields to add to events in which one or more of the **Masking rules** were matched. These fields can be useful in downstream processing and reporting. You specify the fields as key–value expression pairs, like those in the [Eval Function](eval-function).

  * **Name**: Field name.
  * **Value Expression**: JavaScript expression to compute the value (can be a constant).

**Depth**: The depth to which the Function will traverse nested properties in an event. Defaults to 5.

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

Configure the Mask Function > Masking rules as follows:

Match Regex: `dfhgdfgj`
Replace Expression: `Trans AM`

![Mask Function configuration](mask-config-4101.png)
{border="true"}

Result: Vroom vroom!

![Event after masking](mask-result.b111d26bb1.png)
{scale="60%" border="true"}

### Example 2: Mask Sensitive Data

This section demonstrates how to use Mask Functions to anonymize sensitive data, with examples for [Social Security numbers](#mask-social-security-numbers) and [credit card numbers](#mask-credit-card-numbers).

In these examples, the Mask Function runs an md5 hash of the values for the specified keys and replaces the original values with the hashed values. Everything in the **Match Regex** field is replaced by the expression defined in the **Replace Expression** field. If you prefer, you can use capture groups in **Match Regex** to define individual string components for manipulation. You can also use string literals in **Replace Expression** to retain static text. Cribl does not retain any content that matches the **Match Regex** that is not inserted into the **Replace Expression**.

> If you need to send the unmodified values to a Destination, such as an archival stores, you can use the associated [Route](routes)'s **Output** field to narrow the scope of the Mask Function.
>
{.box .info}

#### Mask Social Security Numbers

This example assumes that you're ingesting data whose `_raw` fields contain unredacted Social Security numbers in the Key=Value pattern `social=#########`.

![Event with unredacted SSNs](ssn-event-raw.c8a9eb4072.png)
{border="true"}

To use a Mask Function to replace the original numeric values for all `social` keys with hashed values, enter the following values into the **Masking rules** fields:

* **Match Regex**: `(social=)(\d+)`
* **Replace Expression**: `` `${g1}${C.Mask.md5(g2)}` ``

Everything in **Match Regex** is replaced by the expression defined in the **Replace Expression** field. `social=` is assigned to capture group `g1` for later reference. The value of `social=` will be hashed because it is referenced as `g2` in the md5 function.

If you don't make `social=` its own capture group (or specify `social=` as a literal in **Replace Expression**) then you cannot reference it using `g1` in **Replace Expression**. The value will be assigned to `g1` along with `social=`, and the entire `social=#########` string will be replaced with a hash of the Social Security number. Without the key name preceding the hashed value, it isn't clear what value is being hashed.

![Mask Function configuration for masking Social Security numbers](ssn-mask-config-4101.png)
{border="true"}

Result: The sensitive values of `social=` are replaced by their md5 hashes.

![Event with hashed Social Security numbers](ssn-event-masked.5dd5e52d95.png)
{border="true"}

#### Mask Credit Card Numbers

This example assumes that you're ingesting data whose `_raw` fields contain unredacted credit card numbers in the values of the `cardNumber` key. The `cardNumber` values may be digits alone or may include hyphens or spaces as separators.

![Event with unredacted credit card numbers](ccnumber-event-raw.4f7a2b9c1d.png)
{border="true"}

To use a Mask Function to replace the original numeric values for all `cardNumber` keys with hashed values, configure the **Masking rules** fields:

* **Match Regex**: `(cardNumber=)(\b(?:\d[ -]*?){13,16}\b)`
* **Replace Expression**: `` `${g1}${C.Mask.md5(g2)}` ``

Everything in **Match Regex** is replaced by the expression defined in the **Replace Expression** field. `cardNumber=` is assigned to capture group `g1` for later reference. The value of `cardNumber=` will be hashed because it is referenced as `g2` in the md5 function.

If you don't make `cardNumber=` its own capture group (or specify `cardNumber=` as a literal in **Replace Expression**) then you cannot reference it using `g1` in **Replace Expression**. The value will be assigned to `g1` along with `cardNumber=`, and the entire string, including `cardNumber=`, will be replaced with a hash of the credit card number. Without the key name preceding the hashed value, it isn't clear what value is being hashed.

![Mask Function configuration for masking credit card numbers](ccnumber-mask-config-4101.png)
{border="true"}

Result: The sensitive values of `cardNumber=` are replaced by their md5 hashes.

![Event with hashed credit card numbers](ccnumber-event-masked.d7c9e4f1a2.png)
{border="true"}

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

## See Also {#see-also}

For further usage examples, see:

- [Masking and Obfuscation](/stream/usecase-masking-and-obfuscation/) illustrates random, repeat, string, and hash masking.

- The [Cribl Knowledge Pack](https://packs.cribl.io/packs/cribl-knowledge-learning-pack) provides sample Pipelines that demonstrate using Mask to both encode and decode data, convert hex values to numbers, and create md5 hashes of values using multiple wildcards.